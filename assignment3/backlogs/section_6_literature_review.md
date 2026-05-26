# 6. Literature Review

Bus mapping is an instance of the broader **entity resolution / network alignment**
problem. Before committing to a matching method, we surveyed the relevant literature
to understand the landscape of established techniques, the tradeoffs each makes, and
where our problem sits within it. This section synthesizes six papers spanning the
two relevant traditions — record-linkage / entity resolution (papers 1–3) and graph
alignment (papers 4–6) — and explains which ideas we borrow, which we adapt, and
which we deliberately set aside given the structure of *this specific* problem.

The short version: we are not in a regime where any single published method dominates,
because our problem has an unusual mix of *very strong* signals (deterministic
attribute encoding, near-exact transformer impedance agreement) and *very weak*
signals (geographic coordinates disagree by km, line impedances disagree by a
constant factor). The literature gives us a vocabulary and a set of design patterns;
the right method is a layered combination tuned to which signals actually exist in
our data, not an off-the-shelf algorithm.

---

## 6.1 Entity resolution: the record-linkage tradition

### Christen (2012) — *A Survey of Indexing Techniques for Scalable Record Linkage and Deduplication*

Christen catalogs six families of blocking/indexing techniques: traditional blocking,
sorted-neighborhood (array and inverted-index variants), q-gram indexing,
suffix-array indexing, canopy clustering, and string-map indexing. The core message
is that the **comparison step is the bottleneck**, so any practical record-linkage
pipeline must first reduce the candidate pair space via blocking. Christen reports
that no single blocking technique dominates across datasets; multi-key blocking
(running several different blocking keys and unioning their candidate sets) is the
robust default because it tolerates errors in any one key. Phonetic encodings
(Soundex, NYSIIS, Double-Metaphone) are the workhorse pre-processing step for
person-name data.

**What we take:** the multi-key blocking philosophy. Our equivalent of a blocking key
is the combination `(Dayzer ZONE → Panorama zone) ∧ floor(kV) match`. From
Section 3.5 this filter retains ~75% of true matches in a tight candidate set, with
the remaining 25% recoverable via a fallback prefix-only block.

**What we skip:** phonetic encoding. Bus names are not person names — they are
operator-coded station identifiers with deterministic suffix conventions
(Section 3.2). Soundex would destroy the very signal we want to use. We do, however,
use Christen's *sorted neighborhood* idea implicitly: prefix-equal candidates are
naturally ordered.

### Köpcke & Rahm (2010) — *Frameworks for entity matching: A comparison*

A comparative study of eleven entity-matching frameworks (FEBRL, MARLIN, MOMA, SERF,
TAILOR, etc.), organized by whether the framework requires training, what kinds of
matchers (attribute vs. context) it supports, and how matchers are combined
(numerical, rule-based, workflow-based). Reported F-scores on bibliographic datasets
cluster between 0.85 and 0.99, but Köpcke & Rahm emphasize that these numbers are
**not directly comparable** because the test problems differ wildly in difficulty.
A consistent finding: combinations of matchers outperform any single matcher,
and rule-based combinations are easier to debug than learned numerical ones.

**What we take:** the layered, rule + numerical hybrid pattern. Our pipeline is a
sequence of cascading filters (zone, kV, name) followed by a numerical scoring layer
(impedance distance, geographic proximity, signature Jaccard). This is the
*workflow* + *numerical* combination Köpcke & Rahm describe.

**What we skip:** supervised training. Every framework in Köpcke & Rahm's survey that
uses training requires labeled match/non-match pairs — and the paper is candid that
selecting representative training data is itself a hard manual problem. We have *no
ground truth* and only a multi-hour time budget; a fully unsupervised, interpretable
pipeline is the right choice.

### Bhattacharya & Getoor (2007) — *Collective Entity Resolution in Relational Data*

The canonical formulation of **collective** entity resolution: when references
co-occur (e.g., co-authors on a paper, or in our case, buses connected by a branch),
resolving them jointly is more accurate than resolving them independently. The
authors propose a relational clustering algorithm where the similarity between two
candidate clusters is a weighted sum of attribute similarity *and* neighborhood
similarity, where neighborhood means the *current cluster labels* of the
co-occurring references. Similarities are recomputed each iteration as cluster
assignments evolve — hence "collective." They show that the Adamic-Adar measure
weighted by neighbor *uniqueness* (rare names carry more evidence than common ones)
outperforms naive shared-neighbor counts when ambiguity is high.

**What we take:** the principle that **neighborhood structure carries identity
information** when attributes alone are ambiguous. Our neighbor-kV signature
analysis in Section 4.3 is a discrete, single-pass version of this idea: a bus's
identity is partially encoded by the kV-distribution of its branch neighbors.
We also use the related "uncommon-neighbor-is-more-informative" intuition implicitly
when we treat a cross-tier 345-138 transformer connection as a stronger anchor than
a generic 138-138 line.

**What we skip:** the iterative collective procedure itself. Bhattacharya & Getoor's
algorithm is O(nk log n) but the hidden constant is large because each merge
triggers similarity recomputation across an entire neighborhood. For our 10,413 ×
10,233 problem with iteration counts in the hundreds, this would consume the entire
time budget. More importantly, our problem has a *near-deterministic anchor signal*
(transformer impedance, Section 4.2) that single-pass attribute methods do not —
so we are not in the high-ambiguity, attribute-only regime where collective
resolution most pays off.

---

## 6.2 Graph alignment: the network-science tradition

### Koutra & Tong (2013) — *BIG-ALIGN: Fast Bipartite Graph Alignment*

The first paper to explicitly address bipartite graph alignment. The key conceptual
move is to **relax permutation matrices to soft, sparse correspondence matrices**:
instead of requiring each node in A to map to exactly one node in B, allow a
probability distribution over candidates. This relaxation has two benefits relevant
to us: (a) the optimization problem becomes tractable via alternating projected
gradient descent, and (b) the input graphs need not have the same number of nodes.
BIG-ALIGN's NET-INIT initialization aligns high-degree hubs first (using a
"scree-plot" knee detection) and propagates outward via degree similarity.

**What we take:** the NET-INIT idea — *resolve high-confidence, structurally
distinctive nodes first, then use them to anchor everything else*. This is exactly
our planned Section 7 ordering: lock down cross-tier transformer-connected buses
first (deterministic impedance match), then propagate via branch connectivity to
ambiguous buses.

**What we skip:** the optimization itself. BIG-ALIGN's update rule operates on an
n_A × n_B correspondence matrix; even with the rank-r approximation our problem is
O(n²) in memory and we lose the diagnostic transparency of a layered pipeline
(every wrong match in BIG-ALIGN requires backing out gradient updates to
understand). The brief explicitly values approach over accuracy, which we read as
favoring transparency. The bigger reason: BIG-ALIGN assumes the graphs are
near-isomorphic up to a permutation. Ours are *not* — the line-impedance ratio
analysis in Section 4.1 shows Dayzer breaks parallel circuits where Panorama
collapses them, so the graphs differ by *more than a permutation*.

### Zhang & Tong (2016) — *FINAL: Fast Attributed Network Alignment*

The closest published analog to our problem. FINAL extends IsoRank to attributed
networks: alignment scores satisfy a fixed-point equation
`s = α·W̃·s + (1-α)·h`, where the propagation matrix `W̃` encodes (1) topology
consistency, (2) node attribute consistency, and (3) edge attribute consistency,
combined multiplicatively. The prior `h` encodes domain knowledge about likely
correspondences. A low-rank approximation reduces the cost from O(mn) to O(n²),
and an on-query variant achieves O(n) for single-node queries.

**What we take:** the multiplicative combination of topology, node-attribute, and
edge-attribute consistency. Our scoring function combines (topology = branch
endpoints agree) × (node attribute = kV exact match, name prefix similarity) ×
(edge attribute = impedance distance). This is structurally what FINAL does,
implemented as a discrete rule cascade rather than a continuous fixed-point
iteration.

**What we skip:** running the actual FINAL iteration. Three reasons. First, FINAL
requires a strong prior `h` to converge to good solutions — building that prior
from our name and zone matching *is* the bulk of the work, and once we have it, the
remaining propagation gain is marginal. Second, FINAL's edge-attribute consistency
assumes edges in the two networks have *comparable* attribute values; our line
impedances differ by a 0.55× ratio (Section 4.1) due to parallel-circuit
aggregation, so we'd need a base-invariant edge similarity (R/X ratio) rather than
absolute impedance — which we already plan to use directly. Third, our problem has
many ~unmatchable buckets (566-bus universe gap, 663 Dayzer-only 34.5 kV buses,
~1044 Pano satellite-component buses): an iterative method will produce confident
but wrong matches for these, while a layered method produces "no match" or "low
confidence" honestly.

### Heimann et al. (2018) — *REGAL: Representation Learning-based Graph Alignment*

The representation-learning answer to graph alignment: learn an embedding per node
such that structurally similar nodes (across both graphs) land near each other in
embedding space, then align by nearest-neighbor lookup in a k-d tree. The
embedding method, xNetMF, encodes each node by the *degree distribution of its
k-hop neighborhood*, factorized via a Nyström low-rank approximation of a
similarity matrix. The structural-identity framing — two nodes are similar if their
neighborhoods look alike, regardless of where they sit in the graph — is the
philosophical opposite of proximity-based methods like node2vec, which would fail
across disjoint graphs.

**What we take:** the structural-identity intuition. Our neighbor-kV signature
(Section 4.3) is a discrete analog of REGAL's degree-distribution-per-hop encoding,
restricted to kV (which carries more information than degree in a power grid).
Our use of the signature as a *confirmation* signal echoes REGAL's k-NN matching
logic.

**What we skip:** the embedding-and-match pipeline. REGAL is designed for the
regime where the graphs are *only* known by their structure — no shared node
identifiers, no attribute information. We are in the opposite regime: bus names
and kVs are highly informative, and learning structural embeddings would discard
that information. REGAL is also stochastic enough at the embedding step that
results vary run-to-run, which is undesirable for a deliverable we want to be
reproducible and explainable.

---

## 6.3 Synthesis: how the literature informs our method

Across these six papers, three design principles consistently emerge:

1. **Block first, score later.** Christen and Köpcke & Rahm both treat blocking as
   non-negotiable for any non-trivial problem. Our zone + kV pre-filter does this.

2. **Combine signals; do not rely on any one.** Every framework Köpcke & Rahm
   surveys uses multiple matchers. Bhattacharya & Getoor combine attribute and
   relational. FINAL combines topology, node, and edge consistency. We combine name,
   kV, zone, impedance, topology, and (softly) geography.

3. **Anchor on what is deterministic; propagate outward.** BIG-ALIGN's NET-INIT
   aligns hubs first; FINAL needs a strong prior; Bhattacharya & Getoor bootstrap
   with exact attribute matches before doing relational refinement. Our equivalent
   anchor is the cross-tier transformer impedance match (Section 4.2), which is
   near-deterministic and unambiguous when it exists.

Where our problem differs from the literature, in ways that justify a simpler,
more transparent method than the published algorithms:

- **Attribute encoding is deterministic, not noisy.** Person names are misspelled;
  bus name suffixes are operator-coded and follow rules with 96–100% reliability
  (Section 3.2). String-similarity machinery designed for noisy human-entered names
  is overkill.

- **One signal (transformer impedance) is essentially ground truth when it applies.**
  No published method exploits this, because most graph-alignment benchmarks
  (social networks, citation networks) do not have an analog. We use it as the
  primary anchor.

- **No ground truth, no training data.** This rules out the supervised methods in
  Köpcke & Rahm's survey, and means evaluation must be done via stratified
  confidence reporting and manual spot-checking (Section 9), not held-out accuracy.

- **The brief explicitly values approach over accuracy.** This means a
  transparent, debuggable pipeline that produces calibrated confidence tiers is
  worth more than a black-box optimization that produces a single accuracy number.

Section 7 implements this layered, anchor-then-propagate method.

---

## References

- Bhattacharya, I., & Getoor, L. (2007). Collective entity resolution in relational
  data. *ACM Transactions on Knowledge Discovery from Data*, 1(1), Article 5.

- Christen, P. (2012). A survey of indexing techniques for scalable record linkage
  and deduplication. *IEEE Transactions on Knowledge and Data Engineering*,
  24(9), 1537–1555.

- Heimann, M., Shen, H., Safavi, T., & Koutra, D. (2018). REGAL: Representation
  learning-based graph alignment. *CIKM '18*, 117–126.

- Köpcke, H., & Rahm, E. (2010). Frameworks for entity matching: A comparison.
  *Data & Knowledge Engineering*, 69(2), 197–210.

- Koutra, D., Tong, H., & Lubensky, D. (2013). BIG-ALIGN: Fast bipartite graph
  alignment. *ICDM '13*, 389–398.

- Zhang, S., & Tong, H. (2016). FINAL: Fast attributed network alignment.
  *KDD '16*, 1345–1354.
