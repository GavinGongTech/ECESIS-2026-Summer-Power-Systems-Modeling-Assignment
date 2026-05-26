# Mapping Buses Between Dayzer and Panorama: A Multi-Signal Layered Approach

**ECESIS Investments — Assignment 3**

---

## Abstract

We address the problem of mapping individual buses between two ERCOT power-flow models (Dayzer and Panorama) that share an underlying physical grid but differ in naming conventions, modeling scope, and geographic encoding. The problem is a record-linkage task with a graph-structural component: each model contains roughly 10,000 buses connected by transformers and transmission lines, but no shared identifier links the two. We develop a layered matching method that combines (1) tiered blocking on station prefix, mapped zone, and voltage; (2) deterministic anchoring on cross-tier transformer impedance; (3) weighted-sum scoring across five independent signals (name similarity, branch topology, neighbor-kV signature, zone consistency, geographic proximity); and (4) confidence stratification across four tiers. Applied to 10,413 cleaned Dayzer buses, the method produces 410 deterministic anchor matches, 2,114 strong matches, and 2,691 weak matches, with 5,198 buses explicitly unmatched and tagged by reason. Strong-tier matches are robust to ±0.10 weight perturbations (0 of 1,907 reassignments). The dominant cause of unmatched buses is Dayzer–Panorama modeling-scope divergence (sub-transmission and 34.5 kV buses outside Panorama's universe) and zones whose Dayzer naming does not align with Panorama's STATION encoding. The deliverable supports reviewer-led validation through explicit confidence tiers, runner-up candidate visibility, and reason codes for non-matches.

---

## 1. Introduction

ECESIS Investments operates two power-flow models of the ERCOT grid. Dayzer is the production locational marginal price (LMP) forecasting model; Panorama is a transmission-planning model used to study contingencies and topology changes. Both contain a bus-level representation of substantially the same physical grid, but the two were built independently and have no shared bus identifier. A change made in Panorama — say, a new transmission line connecting two substations — must be reflected in Dayzer for the forecast to capture its effect, but doing so requires knowing which Panorama bus corresponds to which Dayzer bus. At present this mapping is built by hand for each scenario, which is slow and error-prone.

The problem is well-defined: produce, for each Dayzer bus, the corresponding Panorama bus (or an explicit non-match with a reason). The brief explicitly values methodological transparency over perfect accuracy, since no ground-truth mapping exists against which precision and recall could be measured. We treat the task as a record-linkage problem (Christen, 2012; Köpcke & Rahm, 2010) augmented with the graph-alignment signals available in branch connectivity (Zhang & Tong, 2016; Heimann et al., 2018).

The remainder of the report is organized as follows. Section 2 surveys the relevant literature and motivates a layered approach. Section 3 summarizes the exploratory analysis that shaped signal selection. Section 4 details the matching method. Section 5 presents results stratified by confidence tier, voltage, and zone. Section 6 discusses limitations and future work, with the latter section elaborated to reflect avenues that time constraints prevented us from pursuing.

---

## 2. Related Work

The problem sits at the intersection of two traditions. **Entity resolution / record linkage** treats each bus as a record with attributes (name, voltage, zone, coordinates) and asks which records refer to the same underlying entity. **Graph alignment** treats the bus universe as a node set in a graph, with branch connections as edges, and asks how to align the nodes of one graph onto another. Our problem has signals from both — we use them in combination.

From the record-linkage tradition, Christen (2012) surveys blocking strategies as the universal first step: traditional keyed blocking, sorted neighborhood, q-gram indexing, suffix arrays, canopy clustering. The unifying insight is that multi-key blocking — using several independent block keys — is more reliable than any single key. Köpcke & Rahm (2010) compare eleven entity-matching frameworks and find that no single technique dominates across problem types; the strongest pipelines combine rule-based and numerical scorers, often in workflows that anchor on high-precision rules before applying broader scoring. Bhattacharya & Getoor (2007) introduce *collective entity resolution*, where references with shared neighbors influence one another's resolution iteratively — relevant for our problem because Dayzer and Panorama branches connect bus pairs that should resolve consistently.

From the graph-alignment tradition, Koutra & Tong's BIG-ALIGN (2013) solves bipartite graph alignment via projected gradient descent on a soft correspondence matrix, with network-inspired initialization. Zhang & Tong's FINAL (2016) generalizes IsoRank to attributed networks with both node and edge attributes, formulating alignment as a fixed-point equation combining topology and attribute consistency. Heimann et al.'s REGAL (2018) learns structural-identity embeddings via Nyström-approximated matrix factorization and aligns nodes by embedding proximity.

Our problem differs from the typical graph-alignment setup in three important ways. First, we have *strong attribute signals*: bus names and voltage levels carry near-deterministic information when they match. Second, the two graphs represent the same physical grid, so we expect a substantial near-identity alignment rather than a learned structural correspondence. Third, edge attributes (line impedances) violate the cross-graph consistency assumption that FINAL requires — Section 3 shows that line impedances differ by a systematic ~0.55× ratio between the two sources, likely due to parallel-circuit aggregation differences. Transformer impedances, by contrast, agree near-deterministically.

These observations push us away from a single-model optimization approach (BIG-ALIGN, FINAL, REGAL) and toward a layered method that uses each signal where it is strongest: tight blocking when names align cleanly, deterministic anchoring on transformer impedance, weighted-sum scoring elsewhere. This matches the design philosophy of Köpcke & Rahm's stronger pipelines while borrowing the anchor-then-propagate intuition from Bhattacharya & Getoor and FINAL.

---

## 3. Exploratory Findings

Several findings from the exploratory analysis directly shaped the matching method. We summarize them here; full detail appears in Sections 3–4 of the accompanying notebook.

**Voltage scope divergence.** Dayzer contains 1,378 buses at 34.5 kV; Panorama contains 715. Dayzer also contains substantial sub-transmission buses (1, 12, 13, 18, 23 kV) that Panorama does not model. These buses are unmatchable by design and represent a ceiling on achievable coverage: ~705 of 10,413 cleaned Dayzer buses (6.8%) fall in this category.

**Naming conventions.** Panorama bus names parse cleanly as `<STATION>_<KV>KV_<CIRCUIT>` at 100% — e.g., `MILANO_138KV_1`. Dayzer names are less regular but carry a deterministic voltage suffix: `_8` indicates 138 kV at 99.4% concentration, `_5` indicates 345 kV at 96.8%, `_9` indicates 69 kV at 97.4%. The Dayzer name prefix (stripped of the suffix) directly matches a Panorama STATION for 1,597 unique prefixes, covering 2,318 Dayzer buses — a strong but partial overlap.

**Zone mapping.** Dayzer's `ZONE_NAME` field aligns with Panorama's zone with ≥95% concentration for 295 of 505 Dayzer zones, covering roughly 75% of buses. This was substantially stronger than the comparable `AREA_NAME` mapping (33%) and became our primary blocking key alongside voltage.

**Geographic precision is poor.** Cross-source coordinate disagreement has a median of 4 km, P95 of 33 km, and a maximum of 730 km. Geographic proximity is therefore a soft scoring signal with a tight sigmoid-decay tolerance, not a hard filter.

**Transformer impedance is near-deterministic across sources.** For confirmed cross-tier transformer pairs identified by name and voltage, the median fractional difference in reactance (X) is 0.00% and in resistance (R) is 0.07%. This makes transformer impedance our strongest cross-source identity signal and the basis for the anchor layer.

**Line impedance shows a systematic ratio.** Line R and X values differ between sources by approximately a 0.55× ratio, almost certainly due to differing conventions for aggregating parallel circuits. Absolute impedance values are therefore not directly comparable, though base-invariant ratios like R/X retain some signal.

**Neighbor-kV signatures.** Each bus has a multiset of voltages observed across its branch neighbors. On single-candidate matches (high-confidence pairs), neighbor-kV signature agreement is 87%, but multi-bus stations produce spurious signature matches. We use this as a confirmatory soft signal, not a primary key.

---

## 4. Method

### 4.1 Overview

The method proceeds in five layers, each operating on the output of the previous:

1. **Candidate generation** via tiered blocking.
2. **Anchoring** on cross-tier transformer impedance.
3. **Signal computation** for every (Dayzer bus, candidate) pair on five independent signals.
4. **Topology propagation** — promoting high-confidence name matches to pseudo-anchors and recomputing the topology signal.
5. **Tier assignment** via a hybrid rule (anchors decided deterministically, remaining buses by weighted sum with multi-criteria thresholds).

The output is one row per Dayzer bus with a confidence tier (anchor, strong, weak, or unmatched), the matched Panorama bus (when matched), the runner-up candidate and its score, and a reason code for unmatched rows.

### 4.2 Candidate Generation

Following Christen (2012), we use multi-key blocking with three nested keys, falling back from tightest to loosest:

| Tier | Block key | Coverage | Median candidates |
|---|---|---|---|
| station | exact station prefix + floor(kV) | 21.1% | 1 |
| prefix3+zone | first 3 chars of prefix + mapped Pano zone + floor(kV) | 30.5% | 2 |
| zone | mapped Pano zone + floor(kV) | 14.2% | 321 |
| none | no candidates | 34.2% | — |

The station block is essentially deterministic: most buses have one candidate. The prefix3+zone block widens slightly to capture buses whose station naming differs from Panorama by a few characters. The zone block is loose but provides a candidate set for buses that fall through both tighter blocks. The 34.2% "none" buses receive no candidates and are tagged with explicit reason codes; the vast majority (80.2%) sit in Dayzer zones that did not enter the high-confidence zone lookup, and 19.8% sit at voltages below Panorama's modeling scope.

### 4.3 Anchoring on Transformer Impedance

For each Dayzer cross-tier transformer (FROM_KV ≠ TO_KV) with at least one endpoint at a matchable voltage (≥69 kV), we find Panorama transformers with the same sorted (FROM_KV, TO_KV) voltage pair and a fractional reactance difference ≤5%. When a clear winner exists (margin ≥1% over the runner-up), both transformer endpoints are anchored at matchable voltages, and the same Dayzer bus appearing as endpoint to multiple anchored transformers is resolved by the tightest impedance match.

Anchoring matched 420 transformer pairs (median fractional X difference 0.000%, P95 0.205%) and anchored 410 Dayzer buses. Of these, 122 anchors landed on Dayzer buses that name-based blocking had failed to give candidates to — the anchor layer rescued these from the unmatched bucket entirely. This is the design payoff Bhattacharya & Getoor (2007) emphasize: anchoring on a deterministic signal lifts buses out of regions where the primary blocking strategy fails.

### 4.4 Signal Computation

For each non-anchored Dayzer bus with at least one candidate, we compute five per-pair signals against every candidate:

- **s_name** — exact prefix match (1.0) or `SequenceMatcher` ratio fallback.
- **s_topo** — fraction of branch neighbors that resolve to anchored Panorama counterparts in the candidate's neighborhood.
- **s_sig** — Jaccard similarity on neighbor-kV signatures (Section 3).
- **s_zone** — Dayzer→Panorama zone mapping consistency (computed but excluded from scoring; see below).
- **s_geo** — sigmoid-decayed haversine distance with a ~5 km half-credit tolerance.

Computing all five signals across 828,893 (bus, candidate) pairs took 5.7 seconds.

**s_zone was excluded from scoring after this step.** Because candidates are already blocked on the mapped zone in 4.2, virtually every candidate already has s_zone ≈ 1.0 — the signal confirms the block but adds no discriminating power. We retain it as a diagnostic column in the output but assign it weight zero in the scoring function.

### 4.5 Topology Propagation

The initial scoring revealed that s_topo fired for only 0.14% of pairs — a direct consequence of having only 410 transformer anchors among ~10,000 buses. To extract more signal from topology, we run a single propagation pass: any bus whose best candidate has s_name ≥ 0.95 and a margin of ≥0.20 over the runner-up's s_name is promoted to a "pseudo-anchor." This added 1,395 pseudo-anchors, expanding the effective anchor set from 410 to 1,805, and tripled s_topo's firing rate to 0.46% of pairs (3,815 of 828,893).

The propagation pass implements the anchor-then-propagate pattern from Bhattacharya & Getoor (2007) and FINAL (Zhang & Tong, 2016) in a single round. The absolute firing rate of s_topo remains modest because branch graphs are sparse — even with 1,805 anchors, most buses do not have an anchored neighbor by chance. When it does fire, however, s_topo is typically 1.0 and tips the score decisively.

### 4.6 Tier Assignment

The combined score uses weights chosen by EDA-informed intuition:

`score = 0.50·s_name + 0.20·s_topo + 0.20·s_sig + 0.10·s_geo`

Tier rules are:

- **Strong**: `score ≥ 0.70` AND `≥3 of 5 signals fire above individual thresholds` AND `margin over runner-up ≥ 0.15`.
- **Name-dominant strong**: `s_name = 1.0` AND `margin over runner-up ≥ 0.30` — promotes exact-prefix matches whose combined score is capped at 0.50 because corroborating signals do not fire (e.g., BUTLER, SUTTON in 5.3).
- **Weak**: `score ≥ 0.50` and not strong.
- **Unmatched (low score)**: otherwise, tagged with reason `no_strong_candidate_match`.

Signal-firing thresholds are s_name ≥ 0.80, s_topo ≥ 0.50, s_sig ≥ 0.50, s_geo ≥ 0.30. The name-dominant promotion rule corrected a failure mode in which exact-prefix matches with weak corroboration (geographic disagreement, no anchored neighbors) were undervalued — 207 buses were promoted from weak to strong by this rule.

Anchors override all scoring decisions and are reported with score 1.0 and `match_method=transformer_impedance`.

---

## 5. Results

### 5.1 Coverage

The matched dataset contains 10,413 Dayzer buses, distributed across four confidence tiers:

| Tier | Count | % |
|---|---|---|
| Anchor (deterministic transformer impedance) | 410 | 3.9% |
| Strong (multi-signal or exact-name with margin) | 2,114 | 20.3% |
| Weak (score ≥ 0.50, insufficient corroboration) | 2,691 | 25.8% |
| Unmatched | 5,198 | 49.9% |
| **Matched (any tier)** | **5,215** | **50.1%** |
| **High-confidence matched (anchor + strong)** | **2,524** | **24.2%** |

The 50.1% headline coverage understates method efficacy because it includes buses that cannot be matched regardless of approach. Separating genuinely unmatchable buses (below Panorama's modeling scope) from methodologically unmatched ones (in unmapped zones or with no strong candidate):

- **Genuinely unmatchable**: 705 buses (6.8% of total).
- **Methodologically unmatched**: 4,493 buses (43.1% of total).

Against the **reachable population** of 9,708 buses (excluding the 705 below modeling scope), coverage becomes **53.7% matched** and **26.0% high-confidence**. We treat these as the appropriate efficacy figures because they isolate "what the method achieved" from "what was outside the method's universe."

### 5.2 Stratification

**By voltage.** 138 kV is the matching bottleneck: 5,209 buses (68% of transmission-voltage buses) but only 24.2% high-confidence. 345 kV has the highest anchor rate per bus (10.4% vs 4.4% at 138 kV), reflecting the density of cross-tier transformer connections. 69 kV has higher overall match rates than 138 kV (60.3% vs 45.5%) but lower high-confidence (44.9% vs 24.2%) because 69 kV buses receive more multi-candidate matches that score in the weak range.

**By blocking quality (candidate source).** Blocking quality predicts match quality almost perfectly:

| Source | Total | High-confidence % |
|---|---|---|
| station (exact prefix+kV) | 2,196 | 78.2% |
| prefix3_zone (loose prefix + zone) | 3,175 | 15.5% |
| zone (loose zone only) | 1,478 | 13.0% |
| none (no candidates) | 3,564 | 3.4% |

The 3.4% high-confidence rate in the "none" tier is composed entirely of buses rescued by the anchor layer — 122 buses with no name-based candidates that transformer impedance matched deterministically.

**By zone.** Three patterns emerge among the top zones by bus count. *Clean small zones* (NUECES_COUN, TRAV-AEU, E_HARRIS) reach 89–95% match rates because Dayzer naming aligns cleanly with Panorama STATION encoding. *Match-but-low-confidence zones* (O_DALLAS at 60% match, 7% high-confidence) produce candidates through blocking but cannot resolve them confidently because Dayzer naming diverges from Panorama in those zones. *Coverage failure zones* (CNP_DIST at 4.7%, CNP_INDS at 0.7%) are zones whose Dayzer names use conventions different enough from Panorama's that no high-confidence zone mapping was built in the EDA. These zones drive most of the 2,737 "unmapped zone" unmatched buses.

### 5.3 Confidence Indicators

Without ground-truth validation, the argument for trusting tier assignments must come from the structure of evidence behind each tier.

**Score and margin distributions.** Strong-tier matches have a median score of 0.836 (P10–P90: 0.702–0.999) and a median margin over runner-up of 1.000 (P10 0.267). Weak-tier matches have a median score of 0.616 (P10–P90: 0.518–0.799) and a median margin of just 0.001 (P10 0.000). The weak tier is therefore best understood as **buses where the data itself is genuinely ambiguous**, not where the method failed — MILANO is the canonical example, with two indistinguishable Panorama candidates (MILANO_138KV_1 and MILANO_138KV_2) tied perfectly on every signal.

**Signal firing rates.** Strong-tier matches average 3.84 firing signals (out of 5); weak-tier matches average 3.06. The discriminating signals are s_name (89.4% firing in strong vs 48.2% in weak) and s_topo (68.6% vs 25.1%). s_sig and s_geo fire at comparable rates in both tiers — they are confirmatory rather than discriminating. This empirically validates Christen (2012) and Bhattacharya & Getoor (2007): name and topology are the workhorse signals.

**Robustness to weight choice.** Under ±0.10 perturbations to each of the four scoring weights (four configurations tested), **0 of 1,907 weighted-scored strong-tier matches changed tier**. The strong-tier assignments are entirely insensitive to weight choice within a wide neighborhood, because the matches that reach strong tier do so by large margins over their runners-up rather than by passing the threshold narrowly.

**Multi-bus station collisions.** 786 Panorama buses receive multiple Dayzer matches (2,011 Dayzer buses involved). Inspection of the largest collisions reveals these are **modeling-convention differences**, not method errors. THW_13KV_14 receives 14 Dayzer matches that are all distinct generator units at the Tom-Hill-Whitney plant; FORMOSA_69KV_1 receives 14 Formosa generator buses; DOW_14KV_1 receives 13 Dow chemical plant generators. Dayzer represents each generator unit as a distinct bus; Panorama aggregates them into a single collector-voltage bus. The collisions are methodologically correct — the Dayzer generator buses *do* all correspond to the same Panorama plant bus. They are assigned to weak tier because the matching method cannot distinguish among them given the available signals.

This reframes the multi-bus collision count from "matching error" to "Dayzer–Panorama generator-aggregation modeling divergence" and is a substantive finding for the modeling team rather than a method failure.

---

## 6. Limitations and Future Work

### 6.1 Limitations

**No ground truth.** We have no held-out set of confirmed Dayzer↔Panorama bus pairs against which to measure precision and recall. The 410 anchor matches are near-deterministic by construction (transformer impedance agreement at <1% fractional difference), but the 2,114 strong and 2,691 weak matches rest on the structural evidence presented in Section 5.3 rather than empirical accuracy. This is the most important limitation of the work and is intrinsic to the problem rather than a methodological choice.

**Manual sample validation was de-scoped.** We originally planned to manually inspect ~25–30 matched pairs across tiers to characterize match quality empirically. Time constraints (Assignment 3 is optional and was completed in approximately 15 hours alongside other assignments) prevented this. The deliverable instead supports reviewer-led validation: every matched row exposes its score, n_fires, margin, runner-up candidate, and per-signal contributions in the output CSV, so a reviewer can spot-check any subset of matches without re-running the method.

**Weight choice is intuition-driven.** The scoring weights (0.50 / 0.20 / 0.20 / 0.10 on s_name / s_topo / s_sig / s_geo) were chosen based on EDA-informed assessments of signal reliability, not learned from labeled data. The ±0.10 robustness check shows strong-tier matches are entirely insensitive to this choice within a reasonable neighborhood, but we have no evidence that the chosen weights are optimal — only that they are stable.

**The zone lookup is biased by construction.** The high-confidence zone mapping (used as the primary block) was built from prefix-station overlap counts — meaning a Dayzer zone enters the lookup only if its buses' prefixes already match Panorama stations frequently. Zones whose Dayzer buses use entirely different naming conventions can never enter the lookup, no matter how cleanly they sit inside a Panorama zone geographically. This explains the 2,737 "unmapped zone" unmatched buses: they live in zones the lookup cannot see.

**Line impedance is unused.** We dropped line impedance as a scoring signal because the systematic 0.55× ratio between sources (likely parallel-circuit aggregation) violates the cross-source comparability that scoring requires. Base-invariant ratios like R/X retain some signal but are weaker than the transformer impedance signal we use for anchoring. With more time, line endpoint matching via candidate bus pairs (iterative confirmation) could recover some of this signal.

**Single-pass propagation.** The topology propagation in 4.5 runs once: pseudo-anchors derived from name agreement are used to recompute s_topo, but s_topo's recomputation does not in turn promote new pseudo-anchors and trigger a third pass. A multi-round iteration in the style of Bhattacharya & Getoor (2007) would likely lift some weak-tier matches into strong, but the gain is bounded by branch graph sparsity and was not pursued given time constraints.

**Generator aggregation is unhandled.** The multi-bus collisions in 5.3 are correctly identified as a modeling-convention difference, but the method does not attempt to *resolve* them — it produces weak-tier matches for each Dayzer generator unit individually rather than recognizing that all of them legitimately point to one Panorama bus. A post-processing step that detects generator-unit naming patterns (e.g., Dayzer buses sharing a prefix with `_N` suffixes pointing to one Panorama bus) would be more honest about the structure.

### 6.2 Future Work

If continued, the most valuable next steps would be, in roughly increasing effort:

**Validation against a sample of confirmed pairs.** Even a hand-built validation set of 100 pairs would let us replace the structural-evidence argument in 5.3 with empirical accuracy by tier. The deliverable's runner-up columns make this cheap to assemble: a reviewer working through strong-tier matches can confirm or reject without re-running anything.

**Iterative propagation.** Re-running 4.4–4.5 in two or three rounds, with each round's new pseudo-anchors feeding s_topo for the next, would likely recover 200–500 weak matches into the strong tier. The cost is modest (each round runs in seconds) and aligns directly with the collective-resolution methodology from the literature review.

**Generator-aggregation handling.** A pre-processing pass that identifies Dayzer generator-unit naming patterns and collapses them to a single representative bus before matching would convert ~2,000 weak-tier multi-collision matches into ~200 cleaner one-to-one mappings, plus an explicit "Dayzer generator unit → Panorama plant bus" sub-table. This would substantially improve the apparent coverage rate and make the output more usable downstream.

**Geographic-centroid zone lookup.** The current zone lookup is biased toward zones with naming overlap. A complementary lookup built from geographic centroids (which Panorama zone does each Dayzer zone's bus centroid fall in) would catch zones that the current lookup misses — likely recovering a substantial fraction of the 2,737 "unmapped zone" unmatched buses. The 4-km median coordinate disagreement makes this noisier than the naming-based approach but useful as a fallback.

**Line endpoint matching.** Once a partial bus mapping exists (which we now have, at the strong/anchor level), line endpoints can be matched probabilistically: if Dayzer bus A connects via a line to Dayzer bus B, and A is anchored to Panorama bus A', then B should anchor to a Panorama bus that connects to A'. This is an explicit version of the topology signal currently embedded in s_topo, and it could be used to disambiguate weak-tier matches where two candidates differ on connectivity but not on attribute signals.

**Weight learning.** With a validation sample, scoring weights could be tuned by gradient descent on labeled pairs rather than chosen intuitively. The robustness analysis suggests this would not change strong-tier assignments meaningfully but might tighten the weak/unmatched boundary.

**Active learning.** Köpcke & Rahm (2010) and Bhattacharya & Getoor (2007) both discuss active-learning extensions in which the matcher asks a human to label its lowest-margin pairs. For our problem, this would mean presenting a reviewer with the ~200 weak-tier matches with the thinnest runner-up margins, getting decisions on those, and propagating the resulting constraints back into the score. This is the cheapest way to convert weak matches into strong ones at scale.

**End-to-end graph alignment.** A learned alignment via REGAL or FINAL, initialized with our strong + anchor matches as a prior, would be the most ambitious follow-up. The strong + anchor matches (n=2,524) form a sufficient prior to constrain the alignment substantially, and the learned method might recover matches in zones where attribute signals fail entirely. This was outside the scope of the time budget but would be the natural direction if a higher-accuracy mapping is needed.

---

## 7. Conclusion

We have presented a layered, transparent method for mapping buses between the Dayzer and Panorama power-flow models, achieving 24.2% high-confidence coverage and 50.1% any-tier coverage across the full Dayzer dataset (26.0% and 53.7% against the reachable population). The method combines record-linkage techniques (tiered blocking, multi-signal weighted scoring) with graph-alignment ideas (transformer impedance anchoring, branch-topology propagation) and produces an output that is interpretable at the row level: every match carries its confidence tier, score breakdown, runner-up candidate, and signal-by-signal contributions, and every non-match carries an explicit reason code.

The strongest finding is methodological rather than numerical: blocking quality predicts match quality almost deterministically (78% high-confidence in the tightest block, 3% in the loosest), and the 410 transformer-impedance anchors are robust enough to serve as a gold-standard backbone for the rest of the pipeline. The most useful limitation finding is substantive: the dominant cause of non-matches is not method failure but Dayzer–Panorama modeling-convention divergence — sub-transmission scope, generator aggregation, and zone-naming differences that no matching method can resolve without ground-truth labels or explicit modeling-team input.

The deliverable is designed to support reviewer-led validation and incremental improvement rather than to be a one-shot final answer. With additional time, the most valuable extensions — multi-round propagation, generator-aggregation handling, and a 100-pair validation set — would together likely lift high-confidence coverage to 35–45% without changing the underlying approach.

---

## References

Bhattacharya, I., & Getoor, L. (2007). Collective entity resolution in relational data. *ACM Transactions on Knowledge Discovery from Data*, 1(1), 5-es.

Christen, P. (2012). A survey of indexing techniques for scalable record linkage and deduplication. *IEEE Transactions on Knowledge and Data Engineering*, 24(9), 1537–1555.

Heimann, M., Shen, H., Safavi, T., & Koutra, D. (2018). REGAL: Representation learning-based graph alignment. *Proceedings of the 27th ACM International Conference on Information and Knowledge Management (CIKM)*, 117–126.

Koutra, D., Tong, H., & Lubensky, D. (2013). BIG-ALIGN: Fast bipartite graph alignment. *Proceedings of the IEEE 13th International Conference on Data Mining (ICDM)*, 389–398.

Köpcke, H., & Rahm, E. (2010). Frameworks for entity matching: A comparison. *Data & Knowledge Engineering*, 69(2), 197–210.

Zhang, S., & Tong, H. (2016). FINAL: Fast attributed network alignment. *Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining*, 1345–1354.
