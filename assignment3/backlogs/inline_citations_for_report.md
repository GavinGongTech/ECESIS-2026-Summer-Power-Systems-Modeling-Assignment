# Inline citation snippets for the report

These are pre-built sentences that cite the literature review papers, keyed to
the design decisions in the matching method. Drop them into the report's
"Method" section where appropriate — they're already in defensible academic
prose, and tying each decision to a citation will read well in interview.

## Blocking / candidate generation

> Following the multi-key blocking strategy advocated by Christen (2012), we
> first partition the candidate space by `(zone, floor(kV))` rather than
> compare all Dayzer-Panorama bus pairs. This block retains roughly 75% of
> true correspondences with a candidate-set size reduction of two orders of
> magnitude (Section 4).

> Christen (2012) notes that multi-key blocking — running several blocking
> definitions and unioning their candidates — is the robust default when any
> single key may suffer errors. We use a primary block on `(zone, floor(kV))`
> and a fallback block on `(station_prefix, floor(kV))` to recover
> cross-zone matches.

## Multi-signal scoring

> The matching score combines name similarity, kV agreement, geographic
> proximity, and topological signals (transformer impedance and neighbor-kV
> signature). This layered combination follows the multi-matcher pattern
> consistently identified as best practice in Köpcke & Rahm's (2010) survey
> of eleven entity-matching frameworks: no single matcher dominates, and
> rule-based combinations are easier to debug than learned numerical ones.

## Transformer-impedance anchor

> The cross-tier transformer impedance signal serves as our primary anchor.
> This is conceptually similar to the high-confidence bootstrapping step in
> Bhattacharya & Getoor's (2007) collective entity resolution, where exact
> attribute matches initialize clusters before relational refinement begins.
> In our problem, transformer impedance is more than a bootstrap signal — it
> is near-deterministic (median fractional difference 0.07% on R, 0.00% on X;
> Section 4.2), making it stronger than any single attribute available to
> the methods Bhattacharya & Getoor study.

## Topology / neighborhood signature

> The neighbor-kV signature (the multiset of kV values among a bus's branch
> neighbors) is used as a confirmation signal. This is a discrete analog of
> the structural-identity embeddings learned by REGAL (Heimann et al., 2018):
> two nodes are similar if their k-hop neighborhood structure looks alike,
> independent of where they sit in the overall graph. We use kV instead of
> degree because, in a transmission network, kV carries more identity
> information.

## Not running FINAL / BIG-ALIGN / REGAL end-to-end

> Modern attributed graph alignment methods (FINAL [Zhang & Tong, 2016];
> BIG-ALIGN [Koutra & Tong, 2013]; REGAL [Heimann et al., 2018]) formulate
> the problem as an optimization or representation-learning problem over an
> n × n correspondence matrix. For our 10K × 10K problem these are feasible
> but offer two disadvantages relative to a layered pipeline: (1) the
> end-to-end objective is not easily decomposable, making it hard to assign
> confidence tiers to individual matches; (2) they assume the two graphs are
> near-isomorphic up to a permutation, while Section 4.1 shows our two
> graphs differ by more than a permutation (Dayzer breaks parallel circuits
> that Panorama collapses, producing a systematic ~0.55× line-impedance
> ratio). A discrete, layered method handles this asymmetry honestly: buses
> in the "split parallel circuit" set are flagged as low-confidence rather
> than forced into a one-to-one alignment.

## Confidence stratification

> Because no ground-truth bus mapping is available, we report match coverage
> stratified by confidence tier — anchor matches (cross-tier transformer
> impedance agreement), strong matches (name+zone+kV+impedance ≥ 3
> independent signals), and weak matches (name+kV only, no topological
> confirmation) — rather than precision/recall against an unavailable label
> set. This stratified evaluation follows the practice recommended by
> Köpcke & Rahm (2010), who observe that aggregate F-scores across the
> eleven frameworks they survey are not directly comparable because test
> problems differ wildly in inherent difficulty.

---

## Suggested citation order in the report

If you want to economize on the number of citations: the four that carry the
most weight in interview discussion are

1. **Köpcke & Rahm (2010)** — for the multi-signal combination pattern;
   justifies the whole layered approach.
2. **Christen (2012)** — for blocking; justifies the zone+kV pre-filter.
3. **Bhattacharya & Getoor (2007)** — for the anchor-then-propagate pattern;
   justifies starting with transformer-impedance matches.
4. **Zhang & Tong (2016) (FINAL)** — for *not* running an end-to-end
   attributed-graph-alignment method despite this being the closest published
   analog to our problem. Citing the closest analog and then explaining why
   you didn't use it is the strongest move.

BIG-ALIGN and REGAL can be cited collectively as "more recent
optimization-based and representation-learning approaches in the same
family" without a deep treatment, if space is tight.
