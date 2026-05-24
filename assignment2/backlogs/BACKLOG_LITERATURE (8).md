# Assignment 2 — Literature Backlog

*One entry per paper reviewed. Each entry: full citation, core idea, brutal relevance score (1–10) with reasoning, methodological insights (labeled, e.g., L4-A) for cross-reference from other backlog files, and cross-references to questions and primary backlog items where applicable.*

*Calibration anchors:*
- *10/10 = methodological anchor (L5 Triebe et al. — zonal-to-nodal disaggregation, MISO, direct beats top-down)*
- *8–9/10 = directly usable methods in peer-reviewed venues (L4 Salgado, L12 Shiblee & Koukaras, L27 Li et al. 2021 TPS)*
- *7/10 = useful background, correct domain (L2 Li et al. 2018 TPEC — superseded by L27)*
- *4–5/10 = correct domain but wrong granularity, scale, or data type*
- *3/10 = adjacent but limited transferability*
- *1–2/10 = wrong problem domain entirely (power flow, OPF, dynamics, state estimation cluster)*

---

> **⚠ Note on L1–L14 entries below:** These were assessed in a prior session that exhausted its context window before the file could be persisted. The entries below are **short-form reconstructions** from the prior session's preserved summary, retaining the citation, core idea, relevance score, and insight labels referenced elsewhere in this backlog and in BIG_QUESTIONS.md. They are sufficient to support citations in the Assignment 2 report. **If the Assignment 2 report leans heavily on any of these papers for methodological detail, the underlying PDF should be re-read to recover the full PhD-level defense.** Reconstruction date: 2026-05-22.

---

## L1 — Chen et al. (2026) — Multi-scale resilience framework, not relevant

**Citation:** Chen et al. (2026). Multi-scale resilience framework for power systems. *Reconstructed entry — full bibliographic details to be re-verified from PDF if cited in report.*

**Core idea:** Multi-scale framework for power-system resilience assessment under disturbance events. Methodological focus is on resilience metrics across temporal and spatial scales, not on load forecasting.

**Relevance score: 2/10. Do not cite.** Wrong problem domain — resilience metrics, not load forecasting. No transferable methodology to our pipeline.

*Cross-reference:* None.

---

## L2 — Li, Bornsheuer, Xu, Birchfield & Overbye (2018) — Synthetic ERCOT load modeling (precursor to L27)

**Citation:** Li, H., Bornsheuer, A.L., Xu, T., Birchfield, A.B., Overbye, T.J. (2018). "Load Modeling in Synthetic Electric Grids." *2018 IEEE Texas Power and Energy Conference (TPEC)*. IEEE. Affiliation: Texas A&M University.

**⚠ Superseded by L27.** L2 is the TPEC 2018 conference paper version of what was later extended and published as a peer-reviewed journal article in *IEEE Transactions on Power Systems* (2021) as L27 — Li, Yeo, Bornsheuer & Overbye, "The Creation and Validation of Load Time Series for Synthetic Electric Power Systems." Same TAMU group, same methodology, same ACTIVSg2000 ERCOT-footprint case study, but L27 adds substantially more methodological depth (aggregation-level load-factor calibration, four-metric statistical validation against 37 European countries + 66 US Balancing Authorities, peer review in the flagship power systems journal). **For Assignment 2 citations, prefer L27 over L2.**

**Core idea (preserved here for completeness):** Constructs synthetic load profiles for an ERCOT-style transmission system by combining residential, commercial, and industrial archetype diurnal shapes scaled to match annual MWh totals. Demonstrates that bus-level load curves are most parsimoniously modeled as mixtures of sector archetypes, and that residential, commercial, and industrial loads have distinct, recognizable diurnal signatures that persist across magnitude scaling.

**Relevance score: 8/10 (upgraded from 7/10 on the basis that L27's peer-reviewed extension is now cited and the methodology is more developed than the L2 conference version alone implied).** The score upgrade reflects that L2's core ideas are now backed by a flagship-journal peer-reviewed version (L27) on the same ERCOT footprint. **Cite L27, not L2,** in the Assignment 2 report.

**Insight L2-A — ERCOT-style bus load is a mixture of sector archetypes.** Motivates the cluster-as-feature approach in Q6: if real ERCOT bus loads also decompose into sector archetypes, then a cluster label (proxy for sector type) should be informative as a model feature. *Insight label preserved here for backward compatibility with existing cross-references; L27 carries the same insight under L27-A and adds the empirical RCI percentage breakdown (~67% residential, ~19% commercial, ~14% industrial on the ACTIVSg2000 ERCOT-footprint system).*

**Insight L2-B — Cluster buses by normalized diurnal shape.** The synthetic archetypes are distinguishable by their normalized 24-hour shape, not by their magnitude. This directly motivates the Q6 clustering plan (normalize before clustering, then k-means on shape vectors). *Insight label preserved; L27 carries the same insight under L27-B with explicit per-sector shape examples (Figure 8 of L27 shows residential, commercial, industrial archetype shapes on ACTIVSg2000).*

*Cross-reference:* L27 (peer-reviewed extension, primary citation), Q6 (sector-type clustering), L4-A (normalize before clustering), L18-A, L19-A, L20-A (similarity metric refinements).

---

## L3 — Shi (2026) — Abstract only

**Citation:** Shi (2026). *Reconstructed entry — abstract only available; full text not assessed.*

**Core idea:** Not assessable from abstract alone. Apparently power-systems related but exact problem framing unclear.

**Relevance score: 3/10. Do not cite without re-reading.** Insufficient information to make a substantive relevance assessment. Recorded for completeness only.

*Cross-reference:* None.

---

## L4 — Salgado et al. (2011) — Cluster-then-forecast for load curves

**Citation:** Salgado et al. (2011). Cluster-then-forecast methodology for electricity load curves. *Reconstructed entry — full bibliographic details to be re-verified from PDF if cited in report.*

**Core idea:** Cluster electricity consumers by normalized daily load shape using k-means, then build a separate forecasting model per cluster. Empirically demonstrates that normalization before clustering is essential — clustering on raw magnitudes produces clusters dominated by size rather than by shape, which is uninformative for forecasting. Normalizing each load curve to unit area or unit max before clustering produces interpretable clusters that align with sector type (residential, commercial, industrial).

**Relevance score: 8/10. Cite as methodological anchor for Q6 clustering.** Correct domain (load forecasting + clustering), peer-reviewed, and the normalize-before-clustering finding directly drives the Q6 method specification.

**Insight L4-A — Normalize before clustering.** Always normalize load curves to a common scale before applying k-means or any distance-based clustering. Otherwise clusters reflect magnitude (which is dominated by bus size) rather than shape (which proxies for sector type). For Q6, normalize each bus's 24-hour diurnal shape vector to unit norm before clustering.

*Cross-reference:* Q6 (sector-type clustering), L2-B (cluster by diurnal shape), L18-A, L19-A, L20-A (similarity metric refinements).

---

## L5 — Triebe, Passow, Wittner, Wagner, Arend, Sun, Zanocco, Miltner, Ghesmati, Tsai, Bergmeir & Rajagopal (2025) — Extending load forecasting from zonal aggregates to individual nodes for TSOs: hits-GAM with Grouped-Global-Bus on MISO (THE METHODOLOGICAL ANCHOR — DIRECT ASSESSMENT)

**Citation:** Triebe, O., Passow, F., Wittner, S., Wagner, L., Arend, J., Sun, T., Zanocco, C., Miltner, M., Ghesmati, A., Tsai, C.-H., Bergmeir, C., & Rajagopal, R. (2025). Extending Load Forecasting from Zonal Aggregates to Individual Nodes for Transmission System Operators. *arXiv preprint arXiv:2510.14983v1* [cs.LG], 02 Sep 2025. Submitted to *Sustainable Energy, Grids and Networks*. License: CC BY 4.0. Affiliations: Stanford University (Triebe, Passow, Wittner, Wagner, Arend, Sun, Zanocco, Rajagopal); Midcontinent Independent System Operator / MISO (Ghesmati, Tsai); TU Munich (Miltner); CTU Prague; University of Granada (Bergmeir); Monash University. 41-page arXiv HTML version directly assessed.

**Core idea:** TSOs (Transmission System Operators) currently produce day-ahead load forecasts at the *zonal/utility* aggregate level, but rising distributed-generation volatility (rooftop PV, EVs, electrification of heating) creates uneven local load dynamics that aggregate forecasts cannot reveal. The paper's contribution is a complete multi-level forecasting *system* — not just a model — that lets TSOs extend operations from utility-level to bus-level forecasts with: (i) a single interpretable model architecture (*hits-GAM* — hybrid interpretable time-series GAM combining piecewise-linear trend + Fourier seasonality + AR-Net autoregressive neural network for 15-day input + temperature lagged-regressor neural network + holiday events + Quantile Regression with Pinball Loss for uncertainty); (ii) a *Grouped-Global-Bus* training paradigm clustering buses by 11 time-series features (trend, spike, linearity, curvature, stability, lumpiness, seasonal strength, trough, entropy, ACF1, ACF10) into k=3 groups via k-means and fitting one global model per cluster; (iii) a hierarchical reconciliation choice between top-down (utility → bus by historical proportions) and bottom-up (bus forecasts summed → utility). Validated on a uniquely extensive MISO dataset: hourly net load Jan 2018 – Sep 2021 across 37 utilities + ~100 buses of one anonymous "Utility A" (loads 0.16–37 MWh average hourly). Results show **direct bus-level forecasting (Global-Bus) reduces error vs top-down disaggregation by 93% RMSE / 95% MAE / 14% MASE on individual bus loads, and the Grouped Global-Bus extension further reduces error at the utility aggregate by 5% RMSE / 9% MAE vs local-utility models** — but exhibits a *volatility-heterogeneity tradeoff*: grouping reduces heterogeneity bias but slightly increases per-bus volatility risk (Global-Bus RMSE 0.58 vs Grouped Global-Bus 0.59 at bus level; reverse at utility level). At utility-only level, hits-GAM achieves 24% MAPE reduction vs XGBoost (4.21% vs 5.53%). Training <1 hour for all buses, inference <1 second on a single GPU.

**Relevance score: 10/10. PRIMARY METHODOLOGICAL ANCHOR for the entire Assignment 2 architecture.** Same problem framing (transmission-bus-level load forecasting at large scale on a North American ISO), same expected dominant finding (direct beats top-down), and a published benchmark we can compare ERCOT results against. Drives Q1 (architecture), informs Q4 (scaling-law motivation via L29 citation), and provides our cleanest precedent for bus-level day-ahead forecasting on real TSO data.

### Brutal relevance defense

**Where it scores positively — this is a 10/10 paper for our problem:**

- **L5-A — Same problem statement as Assignment 2.** Section 2.1: "Work on bus-level forecasting is very limited and public bus-level datasets are lacking. Existing work shows promising performance but is limited in applicability due to lack of interpretability and evaluation on few buses, with typical evaluations covering 1 to 3 areas or substations, 9 substations, 16 buses, 23 substations." Triebe et al. were the first to evaluate bus forecasting on the *entire* bus inventory of a utility within a major US ISO. **Our ERCOT problem is the same problem at ~150× the scale (15,000 buses across 8 zones vs ~100 buses in one utility).** The L5 framing — "bus loads are an ideal forecasting level not only from a power systems perspective, but also regarding the accuracy-resolution trade-off" — is the exact framing our report's introduction should adopt.

- **L5-B — Grouped Global-Bus is the architectural anchor for our Q1.** Section 3.2.2 articulates the three-way design choice: *local* (one model per series — does not scale), *global* (one model for all series — homogeneity assumption may be too strict), *grouped global* (one model per cluster of similar series — compromise between volatility-mitigation and heterogeneity-handling). For Q1, our architectural comparison is exactly: *direct bus model with bus-id feature (Global-Bus analog)* vs *zone-direct + top-down disaggregation (Local-Utility analog)*. L5 Table 3 establishes the headline empirical finding our Q1 expects to replicate: **Local-Utility (top-down) yields bus-level RMSE 7.88 / MAE 7.79 / MASE 2.06 / MSSE 4.04 — worse than naive (MASE > 1)**; while **Global-Bus yields bus-level RMSE 0.58 / MAE 0.38 / MASE 0.86 / MSSE 0.67 — beating naive by 14% MASE.** The 93% RMSE reduction and 95% MAE reduction over top-down at bus level is the headline number Q1 lets us compare against on ERCOT.

- **L5-C — Volatility-heterogeneity tradeoff is a published, named phenomenon.** Section 6.4: "Clustering by time series characteristics reduces intra-group heterogeneity and increases inter-group heterogeneity, resulting in a unique model fit per group. Akin to ensembling, this model diversity can improve aggregate forecast accuracy — despite some overfitting risk in individual models — by reduced correlation of errors across groups." Empirically: Grouped Global-Bus *helps* at utility aggregate level (Table 2: RMSE 17.72 vs 18.70 / MAE 11.92 vs 12.75) but *slightly hurts* at individual bus level (Table 3: RMSE 0.59 vs 0.58 / MAE 0.39 vs 0.38). The mechanism is overfitting risk when group sizes shrink, since bus-level volatility dominates. **This is directly relevant to Q6 (sector clustering):** if Q6 finds clusters help, expect heterogeneity-benefit at zone aggregates but possibly volatility-cost at small buses. Cite L5-C for the framework.

- **L5-D — 48-hour sNaïve baseline (not 24-hour) for MASE/MSSE.** Section 3.3 + Equation defines MASE = Σ|y - ŷ| / Σ|y - y_{t-48}|. "The 48-hour lag is chosen because the data from 24 hours prior is not fully available at the time of forecast generation, at 2pm of the compute day." **This is an operationally-grounded baseline choice we should adopt for B1 in our primary backlog.** Our naive baselines should include lag-168h (last week, same hour) and lag-48h (operational sNaïve per L5), not just lag-24h. The reasoning: for next-day forecast, the t-24 data point may not have settled yet at compute time. Worth a sentence in the methodology.

- **L5-E — hits-GAM is the interpretable-by-design counterpoint to LightGBM, but they're not adversaries.** Table 1 (utility level): hits-GAM RMSE 105.41 / MAE 78.22 / MAPE 4.21% / MASE 0.58 / MSSE 0.36, vs XGBoost RMSE 144.92 / MAE 108.61 / MAPE 5.53% / MASE 0.59 / MSSE 0.37. hits-GAM wins on absolute metrics (24% MAPE reduction) but matches XGBoost on scaled metrics (0.58 vs 0.59 MASE). **This is more nuanced than the L5-paraphrase in the original entry suggested.** hits-GAM doesn't dominate XGBoost — it's better at absolute error and ties on scaled error. The improvement comes substantially from interpretability rather than accuracy: hits-GAM provides per-component decomposition (trend / seasonality / AR / temperature / events) and probabilistic quantile-regression intervals, which XGBoost does not. **For our LightGBM choice, L5 reinforces that gradient boosting at utility scale is competitive on scaled metrics; the case for hits-GAM is interpretability, not raw accuracy.** This is methodological subtlety worth honest acknowledgment.

- **L5-F — Bus-level data preprocessing recipe (Appendix A.1.2) is directly transferable.** From the paper: (i) duplicate removal; (ii) outlier replacement — "data points three standard deviations above the mean are considered outliers and are replaced using linear interpolation"; (iii) missing value handling — "missing values are imputed by linear interpolation for up to 20 consecutive missing entries, beyond which a rolling average imputation is applied"; (iv) bus exclusion — "buses with less than one year of training data, more than 20% missing values, only constant values, or missing more than 15 days at the end are removed entirely. After data cleaning, the number of buses was reduced by approximately one quarter to about 100 buses." **The 25% drop rate is a published precedent for our M2 decision to drop 2,137 always-null LOAD buses out of ~7,500.** Our drop rate (~28% of LOAD buses) is consistent with L5's industrial precedent. Furthermore: "After preprocessing, the cleaned bus dataset deviates from the raw bus dataset by 2.89% in aggregate, measured as hourly MAPE." **This 2.89% cleaned-vs-raw aggregate deviation is the published precedent for documenting preprocessing-induced error** — a concept we should adopt in our methodology section.

- **L5-G — Operationally realistic hyperparameters.** Appendix Table 4: n_lags = 24×15 (15 days hourly), newer_samples_weight = 2.0, n_changepoints = 0 (no piecewise-linear trend breaks because period is short), yearly_seasonality = 10 Fourier terms, weekly_seasonality = True, daily_seasonality = True (conditional: separate components fitted for summer April-September vs winter October-March), batch_size = 128, ar_layers = [32, 64, 32, 16], lagged_reg_layers = [32, 32], learning_rate = 0.001, epochs = 30, trend_global_local = local, season_global_local = local, normalize = standardize. **The conditional summer/winter daily seasonality is a specific feature-engineering choice we should consider.** ERCOT load also has different summer vs winter daily shapes (FWES summer industrial cooling vs Winter Storm Elliott heating), and a season-conditional daily-seasonality feature could be added to F1 in primary backlog as a refinement.

- **L5-H — Error diagnosis methodology via bus-residual decomposition (Section 5.1).** Figure 7-8: utility-level error decomposed into per-bus residual contributions, revealing two distinct mechanisms — (a) co-directional errors (all buses err in same direction → drives high utility-level error), (b) cancellation effects (bus D's error flips direction at noon Friday, offsetting other buses' errors → utility-level error drops even though sum of |bus errors| increases). **This is the L30-A / L34-D / L40-D mechanism stack expressed methodologically: bus errors at the same timestamp are correlated, and the correlation structure determines whether bus-level errors aggregate (co-directional) or cancel (anticorrelated).** L5-H is the most concrete published methodology for error-source attribution; it directly supports our E1 stratified evaluation framework. **Recommendation:** add an "error-attribution stratification" to E1 — for each high-error timestamp at the zone level, decompose into per-bus residual contributions to identify whether errors are co-directional or canceling.

- **L5-I — Computational feasibility benchmark.** Section 6.3: "Training of our hits-GAM model for all the buses considered in this study takes less than an hour, while an inference run producing a full hourly day-ahead forecast for all buses takes less than a second on a single GPU." **For our 15,000-bus scale, this scales sub-linearly** (per L5's claim that training time has sub-linear complexity due to global-model regularization). Even if we use LightGBM (CPU-friendly per L12/L41), the L5 benchmark establishes that the entire pipeline is operationally feasible at much larger scale than ours. **For Assignment 2, this calibrates the expected computational budget** — training all 15,000 buses with a global LightGBM model should be feasible within hours, not days.

- **L5-J — Future work directions L5 explicitly flags as open** (Section 6.5): (i) quantifying uncertainty implied by bus error-cancellation effects; (ii) geospatial pattern integration; (iii) interactive operator UI. **These are not our problems for Assignment 2** but should be cited in our report's future-work section as the natural extensions identified by the methodological anchor paper.

**Where it's weak / what L5 does NOT do for our problem:**

- **One utility only at bus level.** Bus-level case study uses ~100 buses from "Utility A" — *not* all buses across all 37 MISO utilities. Our ERCOT problem at 15,000 buses across 8 zones is genuinely larger-scale than the L5 bus experiment. L5 establishes the architectural framework; our Assignment 2 replicates it at a much larger scale.
- **Includes temperature explicitly.** L5's hits-GAM uses temperature forecasts as a lagged-regressor neural network component. We are not using weather features (per our 8-piece evidence cluster). This is a methodological divergence we should acknowledge. The L5 finding that the temperature component dominates daytime summer load (Figure 6) is consistent with our acknowledgment that weather *can* matter at utility scale even though we have justified its omission at our specific ERCOT bus scale.
- **MISO, not ERCOT.** MISO operates across 15 US states from the Gulf of Mexico to continental Canada — substantially more diverse climate footprint than ERCOT's single-state geography. L5's "Utility A" is described as one of the smaller MISO utilities (~average 100-400 MWh per hour), which is closer in scale to a single ERCOT zone than to ERCOT system load. Generalizing L5's bus-level findings to ERCOT requires the same scaling-law and grid-topology assumptions, hence Q4 + Q8 as our scaling-law cross-validation.
- **Net load, not gross load.** L5 forecasts *net load* (consumption minus generation, accounting for distributed PV/EV). Our ERCOT pd column is gross load. This is methodological difference worth noting; for our 2022–2025 data window, distributed-generation penetration in ERCOT is non-trivial but smaller than in MISO, so the gross-vs-net distinction may matter less for us than for L5.
- **NeuralProphet framework dependency.** L5's implementation uses NeuralProphet (an open-source forecasting framework Triebe co-developed). Their model is not "LightGBM-ready" out of the box. We are *not* implementing hits-GAM; we are implementing LightGBM with the grouped-global *paradigm* (single model with bus-id as feature) borrowed from L5-B. The framework dependency is irrelevant to us.

### Insight labels

- **L5-A (problem-framing anchor):** "Bus-level forecasting work is lacking TSO relevance due to lack of operational integration, with evaluation scopes falling short of covering the full set of buses of an entire utility's territory." Use in our report's introduction as the framing citation: existing literature evaluates on 1-23 buses; we evaluate on 15,000.
- **L5-B (Grouped-Global-Bus architectural anchor for Q1):** Three-way design choice — local (one-per-bus) vs global (one-for-all) vs grouped-global (one-per-cluster). Headline result: Global-Bus reduces error vs top-down by 93% RMSE / 95% MAE / 14% MASE at individual bus level. **This is our Q1's expected dominant-finding citation.**
- **L5-C (volatility-heterogeneity tradeoff for Q6):** Grouping helps at aggregate but slightly hurts at per-bus because group sizes shrink and overfitting risk rises. Mechanism citation for any Q6 finding that goes in the "clusters help at zone-aggregate-level but not at smallest-bus-quartile-level" direction.
- **L5-D (48-hour sNaïve baseline for B1):** MASE/MSSE base forecast is t-48, not t-24, because t-24 data not available at compute time. Adopt for our naive baselines in B1.
- **L5-E (hits-GAM vs XGBoost is more nuanced than the original L5 entry suggested):** hits-GAM wins on absolute metrics, ties on scaled metrics. The case for hits-GAM is interpretability, not raw accuracy. **Honest acknowledgment** that LightGBM (which we are using) is closer to XGBoost in spirit than to hits-GAM; we are *not* getting the interpretability benefits hits-GAM offers, but our scale (15,000 buses) makes hits-GAM's per-component decomposition impractical anyway.
- **L5-F (bus-level preprocessing recipe):** 3σ outlier replacement + linear interp up to 20 consecutive missing + rolling-mean beyond + drop buses with <1 yr training / >20% missing / constant / >15 days end-missing. **Reduced bus count by 25%; cleaned-vs-raw aggregate deviation 2.89% MAPE.** Direct precedent for our M2 drop decisions; supports our ~28% drop rate as within industrial norms.
- **L5-G (conditional summer/winter daily seasonality):** Daily seasonal Fourier terms fitted separately for April-September vs October-March. Consider as a feature-engineering refinement for F1 (primary backlog) if our model shows seasonal-shape misfit.
- **L5-H (error-attribution stratification methodology):** Decompose high-error utility timestamps into per-bus residual contributions; identify co-directional vs canceling patterns. **Direct support for E1 stratified evaluation framework** — add error-attribution stratification as a diagnostic step in notebook 06.
- **L5-I (computational feasibility benchmark):** Training all ~100 buses in <1 hour; inference all buses in <1 second on a single GPU. **Calibrates expected runtime** for our 15,000-bus problem at much larger scale.
- **L5-J (future-work directions):** Quantify bus-error-cancellation uncertainty; geospatial pattern integration; interactive operator UI. Cite as natural extensions in our report's future-work section.
- **L5-K (Sevlian-Rajagopal 2018 scaling-law citation — same Stanford lab):** L5 cites Sevlian & Rajagopal 2018 directly (their reference [7]) for the "accuracy degrades steeply below 1 MW" claim. **This establishes that L5's framing implicitly assumes the scaling law applies to bus-level forecasting in transmission systems** — exactly the assumption our Q8 will test on ERCOT data. L5 + L29 are from the same Stanford lab (Rajagopal), and the L5 paraphrase ("steep decline in accuracy") is consistent with L29's α₀/Wᵖ + α₁ functional form but doesn't reproduce the exact parameters.

### Question-sheet pass (full)

- **Q1 (architecture):** **PRIMARY ANCHOR.** L5-B is the citation for the direct-bus-beats-top-down empirical result (93% RMSE / 95% MAE / 14% MASE reduction at bus level). Add the L5 Table 3 numbers to Q1's "Why it matters" paragraph as the published benchmark we expect to replicate on ERCOT. The Local-Utility (top-down) approach achieves MASE 2.06 (worse than naive!) at bus level, while Global-Bus achieves MASE 0.86 (beats naive by 14%) — this is the headline contrast Q1 lets us reproduce.
- **Q2 (beat naive):** L5 reports per-bus statistics. From Table 7 (Appendix C.3): Local-Utility per-bus MAPE median 207.74% — devastating. Global-Bus per-bus MAPE median 7.96%, max 270.97%. Some buses still underperform naive even with Global-Bus (MASE 3Q = 0.93 across all buses, MASE max = 3.04). **This establishes that even the best architecture (Global-Bus per L5-B) has some buses where it underperforms naive — Q2 is a real diagnostic question, not a foregone conclusion.** Important caveat for our report: even with optimal architecture, expect a meaningful fraction of small/volatile buses to fail to beat naive.
- **Q3a (FWES/NOTH structural growth):** No direct L5 evidence. L5 uses 2018-2021 training/2021 testing — no structural-growth analog. L5's piecewise-linear trend component with `n_changepoints=0` (no changepoints) reflects the short 3.5-year period assumption; for our ERCOT 2022-2025 with FWES +60% growth, we may need n_changepoints > 0 or a trend feature that captures the structural break.
- **Q3b (rolling-window retraining):** L5 uses fixed train/test split (2018-2020 training / first 9 months 2021 testing), not rolling-window. **L5 does not address Q3b directly.** Cross-reference: L40 (Pinheiro et al. 2023) is the rolling-window reference, not L5.
- **Q4 (scaling-law diagnostic):** **STRENGTHENED via L5-K.** L5 explicitly cites Sevlian-Rajagopal 2018 (= our L29) for the scaling-law motivation. L5 Table 7 confirms qualitatively that smaller buses have higher MAPE — Bus-level MAPE Global-Bus: Min 3.84% / Median 7.96% / Max 270.97%; Local-Utility: Min 155% / Median 208% / Max 2931%. **The two-order-of-magnitude span in per-bus MAPE is qualitatively consistent with L29's scaling law**, but L5 does not fit α₀/Wᵖ + α₁ explicitly. **Our Q8 can use both L5 (qualitative confirmation) and L29 (quantitative fit) as anchors.**
- **Q5 (zone-direct vs sum-of-bus):** **L5 directly addresses this.** Table 2 (utility-level evaluation comparing bottom-up aggregated bus forecasts against direct utility forecasts): Local-Utility (direct utility, top-down to buses) achieves utility-level RMSE 18.68 / MAE 13.09 / MASE 0.96 / MSSE 0.87. Global-Bus (bottom-up sum of bus forecasts) achieves utility-level RMSE 18.70 / MAE 12.75 / MASE 0.72 / MSSE 0.62. **Global-Bus matches Local-Utility on absolute metrics (RMSE/MAE) but substantially beats on scaled metrics (MASE/MSSE).** Grouped Global-Bus further improves (RMSE 17.72 / MAE 11.92 / MASE 0.68 / MSSE 0.55). **For our Q5, this is the strongest single citation that "sum-of-bus does not have to be worse than zone-direct" — at least at utility aggregate scale, with the right method (grouped global bus), bottom-up is competitive with or beats top-down.** Update Q5 to add L5 as 4th citation in the mechanism stack (L30-A + L34-D + L40-D + L5).
- **Q6 (sector clustering):** L5 uses k-means clustering on 11 time-series features (trend, spike, linearity, curvature, stability, lumpiness, seasonal strength, trough, entropy, ACF1, ACF10) to define k=3 groups — analogous to our Q6 plan to cluster buses by diurnal shape. **L5-C volatility-heterogeneity tradeoff is the framework citation for Q6:** if grouping helps at aggregate-level but hurts at per-bus-level in our results, L5-C is the named, published mechanism. L5's k=3 is empirically justified by overfitting risk at smaller group sizes — directly relevant for our Q6 sensitivity-to-k analysis.
- **Q7 (imputation):** L5-F is the direct precedent. Linear interp up to 20 consecutive missing entries, rolling-mean beyond. Our M2 plan uses linear interp throughout — slightly less sophisticated than L5 but operationally close. **L5-F supports our M2 choice but suggests considering rolling-mean as a fallback for very-long contiguous null blocks.** For Q7 robustness analysis, the rolling-mean alternative is a defensible third comparison alongside linear-interp and zone-mean-fill.
- **Q8 (scaling-law parameter fit):** **L5-K is the framing citation** — L5 cites Sevlian-Rajagopal 2018 explicitly, establishing that L5's architectural framework implicitly assumes the scaling law applies. Our Q8 makes the L5-assumed relationship empirically testable on ERCOT data.
- **Q9 (peak-prediction diagnostic):** L5 does not directly address peak-prediction failure modes, but L5-H's error-attribution methodology (per-bus residual decomposition at high-error timestamps) is the framework for diagnosing peak failures. **Q9 diagnostic stage can borrow L5-H's per-bus error-decomposition framework** and apply it specifically to peak (summer-hours, Winter Storm Elliott) timestamps.
- **Deliberately-not-asked weather:** **L5 uses temperature heavily** (hits-GAM has a lagged-regressor neural network specifically for temperature forecasts). L5 establishes the V-shaped load-temperature relationship at utility level (Figure 12: inflection at 57°F, heating below / cooling above). **This is honest counter-evidence to our no-weather defense at utility-aggregate scale**, but does not contradict the bus-level heterogeneity argument because L5 explicitly fits temperature components *locally* per utility, not globally across all buses. The structural choice in L5 (temperature lagged-regressor fitted globally; trend and seasonality fitted locally) corresponds to our framing: weather effects are heterogeneous across buses, so a single global weather feature is dilutive.
- **Deliberately-not-asked deep learning:** L5 uses neural-network components (AR-Net, lagged-regressor NN) embedded in an interpretable GAM framework — *not* a black-box deep learning model. The L5-E comparison (hits-GAM beats XGBoost at utility level by 24% MAPE, ties on scaled metrics) is honest evidence that *carefully-designed neural-network-augmented models can beat XGBoost at utility scale* — but the gains come from interpretability and uncertainty quantification, not raw accuracy. **L5-E is the most nuanced piece of evidence in the LightGBM-over-DL debate we have:** simple, well-engineered methods (GAM family, gradient boosting family) outperform black-box deep learning, but the *specific* tradeoff between hits-GAM and XGBoost is closer than the L12 finding (LightGBM beats LSTM definitively on Greek STLF). **Honest acknowledgment:** if we were optimizing purely for accuracy *and* interpretability simultaneously at smaller scale, hits-GAM would be a defensible alternative to LightGBM. At our 15,000-bus scale, LightGBM is the more pragmatic choice.

### Reference list capture for tracker (40 references in L5)

Key references from L5's bibliography that are already in our review:
- [1] Gielen et al. 2019 = **L33** (IRENA renewable transition, 1/10 wrong domain — confirms L33 is in L5's bibliography as background/framing, not methodological)
- [2] Haben et al. 2021 *Applied Energy* = **L31** (LV review, 6/10) — cited as authoritative review of LV forecasting work
- [3] Wang, Majumdar, Rajagopal 2023 *Nature Comm* = **L35** (geospatial mapping, 1/10 wrong domain) — same Stanford Rajagopal lab; cited as related work
- [4] Schröter et al. 2020 *Energies* = **L34** (50Hertz TSO, 6/10) — cited as TSO requirements reference
- [5] Sun et al. 2013 IEEE PES = **L30** (substation BLDF, 6/10) — cited as substation forecasting work
- [7] Sevlian & Rajagopal 2018 IJEPES = **L29** (scaling law anchor, 9/10) — same Rajagopal lab; the explicit scaling-law citation L5-K
- [9] Chen, Li, Chen, Bai 2025 *EPSR* (FC-STGAT) = **L36** (bus-level day-ahead, 6/10) — cited as bus-level forecasting precedent
- [11] Lizhen et al. 2022 *EPSR* (mini-batch SGD) = **L38** (3/10) — cited as bus-level forecasting work
- [12] He et al. 2025 *EPSR* (GRU+DDPG) = **L39** (5/10) — cited as substation forecasting work
- [14] Ferreira et al. 2025 *EPSR* (TFT) = **L37** (4/10) — cited as substation forecasting work
- [16] Mathew et al. 2024 *EPSR* (PWP-XGBoost) = **L41** (6/10) — cited as medium-term feeder forecasting work
- [17] Pinheiro et al. 2023 *Applied Energy* = **L40** (6/10) — cited as systematic STLF approach reference

**Important new (not-yet-assessed) references L5 cites:**
- [8] Abdolrezaei et al. 2022 *Energy, Ecology and Environment* — substation mid-term load forecasting via knowledge-based method. Title-direct match for medium-term substation forecasting; possible **Bucket 2 scope-match watch item**.
- [10] Su et al. 2024 *EPSR* — STLF of regional integrated energy system via spatio-temporal convolutional GNN. Multi-cite candidate.
- [13] Chen et al. 1996 *EPSR* — ANN for substation load forecasting. Historical/foundational substation NN reference.
- [15] Nose-Filho et al. 2011 *IEEE TPWRD* — Short-term multinodal load forecasting via modified general regression neural network. Title-direct match for multinodal forecasting; **Bucket 2 watch item.**
- [18] Chen et al. 2023 *Advances in Applied Energy* — Interpretable ML for building energy management state-of-the-art review.
- [19] Evangelopoulos & Georgilakis 2022 *EPSR* — Probabilistic spatial load forecasting for distribution-network load growth assessment. **Title-direct match for our Q3a (FWES/NOTH growth) + probabilistic forecasting** — high-priority Bucket 2 watch item.
- [20] Hastie & Tibshirani 2017 *Generalized Additive Models* (Routledge book). Foundational GAM reference, foundational for L5's hits-GAM and L40's GAMLF.
- [21] Hyndman & Athanasopoulos 2024 *Forecasting: Principles and Practice* 3rd ed. (OTexts). Foundational time-series forecasting textbook; cited by L5, L40, L41.
- [22] Hewamalage et al. 2022 *Pattern Recognition* — Global models for time series forecasting: a simulation study. **The empirical foundation for L5's claim that "GFMs are less susceptible to data volatility, but struggle with data heterogeneity"** — high-priority claim-dependency Bucket 3 watch item.
- [23] Hyndman et al. 2020 R package tsfeatures. The exact 11-feature set L5 clusters on (trend / spike / linearity / curvature / stability / lumpiness / seasonal strength / trough / entropy / ACF1 / ACF10).
- [27] Triebe et al. 2021 *arXiv* — NeuralProphet: Explainable Forecasting at Scale. The framework L5 uses.
- [30] Chen & Guestrin 2016 KDD — XGBoost: A Scalable Tree Boosting System. **Foundational XGBoost reference; cited by L5, L41.**
- [37] Grinsztajn et al. 2022 *arXiv* — "Why do tree-based models still outperform deep learning on tabular data?" **This is the methodological-benchmark citation our LightGBM-over-DL defense rests on.** Currently a Bucket 3 watch item; **promotes to Bucket 1 after L5 review** (cited by L5 in their XGBoost-vs-hits-GAM discussion).

### Q-sheet changes summary

**Major reinforcement, no new question.** Q1 anchor citation now formally backed by full L5 Table 3 numbers (93% RMSE / 95% MAE / 14% MASE reduction). Q5 mechanism stack extends from 3-citation to **4-citation** (L30-A + L34-D + L40-D + L5 Table 2). Q6 framework citation L5-C (volatility-heterogeneity tradeoff named mechanism). Q7 alternative imputation (rolling-mean fallback) added via L5-F. Q8 framing citation L5-K (L5 explicitly cites Sevlian-Rajagopal scaling law). Q9 framework citation L5-H (per-bus error-attribution methodology for peak diagnosis). Deliberately-not-asked deep learning: L5-E adds the most nuanced piece of evidence (hits-GAM beats XGBoost by 24% MAPE at utility level but ties on scaled metrics; gains come from interpretability not accuracy). Primary backlog F1: L5-G conditional summer/winter daily seasonality is a candidate feature-engineering refinement. Primary backlog M2: L5-F provides published precedent for our ~28% drop rate (L5 drops 25% of buses) and our linear-interpolation choice. Primary backlog B1: L5-D supports 48-hour sNaïve as the operationally-correct baseline (we should add lag-48 alongside lag-24 and lag-168). Primary backlog E1: L5-H per-bus error-attribution stratification is a recommended diagnostic for high-error timestamps.

---

## L6 — Deng et al. (2016) — OLS coordination

**Citation:** Deng et al. (2016). OLS-based coordination method for power systems. *Reconstructed entry — full bibliographic details to be re-verified from PDF if cited in report.*

**Core idea:** Uses ordinary least squares regression to coordinate decisions across power-system subproblems. Operational/dispatch focus rather than forecasting.

**Relevance score: 3/10. Do not cite.** Adjacent but limited transferability — OLS coordination is not a forecasting method, and the problem framing does not match ours.

*Cross-reference:* None.

---

## L7 — Türkoğlu et al. (2024) — TCN with missing data handling

**Citation:** Türkoğlu et al. (2024). Temporal Convolutional Network with missing-data handling for load forecasting. *Reconstructed entry — full bibliographic details to be re-verified from PDF if cited in report.*

**Core idea:** Temporal Convolutional Network (TCN) architecture for load forecasting with explicit handling of missing values during training and inference. Demonstrates that TCN with proper missing-value handling can match or beat baseline approaches on standard load forecasting benchmarks.

**Relevance score: 4/10. Cite selectively in M2 / Q7 discussion as a sophisticated alternative we considered and rejected.** Correct domain (load forecasting + missing data) but wrong scale and wrong model class — TCN at 15,000-bus scale is impractical within assignment time constraints, and our missing-data regime (segment missingness from outages on 269 buses) is sufficiently treated by linear interpolation per L20-B.

*Cross-reference:* M2 (null preprocessing), Q7 (imputation robustness check), L8 (alternative we considered and rejected), L20-B (negative evidence supporting linear interpolation).

---

## L8 — Zou et al. (2025, preprint) — Physics-informed disaggregation

**Citation:** Zou et al. (2025). Physics-informed disaggregation of zone load to buses. *Preprint — not peer-reviewed. Full bibliographic details to be re-verified from PDF if cited in report.*

**Core idea:** Physics-informed neural network for disaggregating zone-level load to individual buses using power-flow physics constraints (admittance matrix, nodal balance) as soft constraints in the loss function. Demonstrates that physics constraints can stabilize disaggregation when bus-level historical data is sparse.

**Relevance score: 5/10. Cite selectively as a sophisticated disaggregation alternative we are not using.** Correct problem framing (zone-to-bus disaggregation) but wrong assumption set — the method requires admittance matrix and physics constraints we do not have, and the preprint status (not peer-reviewed) makes it a weaker citation. Our Q1 top-down baseline uses simple historical share rather than physics-informed disaggregation; this is a defensible simplification given data constraints.

*Cross-reference:* Q1 (top-down disaggregation baseline), M2 (alternative we considered).

---

## L9 — Zhang et al. (2025) — GNN for DC OPF

**Citation:** Zhang et al. (2025). Graph Neural Network for DC Optimal Power Flow. *Reconstructed entry — full bibliographic details to be re-verified from PDF if cited in report.*

**Core idea:** GNN surrogate model for DC Optimal Power Flow problem. Predicts optimal generator dispatch and line flows given load values and topology.

**Relevance score: 2/10. Do not cite.** Wrong problem domain — OPF, not load forecasting. Our pd column is the input to this problem, not the target. Part of the OPF/power-flow cluster (L9–L17) catalogued in this backlog as systematically wrong domain.

*Cross-reference:* None (rejected-domain cluster).

---

## L10 — Sunkara (2026) — DISMISSED

**Citation:** Sunkara (2026). *Reconstructed entry — full bibliographic details unrecoverable; paper was dismissed in prior session as not assessable.*

**Core idea:** Not substantively assessable. Dismissed in prior session.

**Relevance score: 1/10. Do not cite.** Recorded for completeness only.

*Cross-reference:* None.

---

## L11 — Nazari et al. (2025) — Transfer learning for power flow, SmartGridComm

**Citation:** Nazari et al. (2025). Transfer learning for power flow estimation. *IEEE SmartGridComm 2025. Reconstructed entry — full bibliographic details to be re-verified from PDF if cited in report.*

**Core idea:** Transfer learning framework for power flow estimation across different network topologies. Trains a base model on a source grid, then adapts to a target grid with limited target-grid data.

**Relevance score: 2/10. Do not cite.** Wrong problem domain — power flow estimation, not load forecasting. Same SmartGridComm venue as L16 and L22, but wrong problem.

*Cross-reference:* None (rejected-domain cluster).

---

## L12 — Shiblee & Koukaras (2025) — Direct LightGBM-vs-Deep-Learning head-to-head on 10 years of Greek STLF data, with LightGBM beating CNN/LSTM/GRU/CNN-LSTM and the ENTSO-E operational benchmark (KEY SUPPORTING PAPER FOR LIGHTGBM CHOICE — DIRECT ASSESSMENT)

**Citation:** Shiblee, M. F. H., & Koukaras, P. (2025). Short-Term Load Forecasting in the Greek Power Distribution System: A Comparative Study of Gradient Boosting and Deep Learning Models. *Energies*, 18(19), 5060. https://doi.org/10.3390/en18195060. Published 23 September 2025; received 27 August 2025; accepted 20 September 2025. Academic Editor: Tek Tjing Lie. Authors at School of Science and Technology, International Hellenic University (IHU), 14th km Thessaloniki-Moudania, 57001 Thessaloniki, Greece. Open access (CC BY 4.0). 27-page paper directly assessed.

**Core idea:** Comparative empirical study of five short-term load forecasting models on 10 years of hourly Greek electricity load data (2015–2024) from the ENTSO-E Transparency Platform. The five models — CNN, LSTM, GRU, hybrid CNN-LSTM, and LightGBM — are trained on the same multivariate dataset (load + Athens-station temperature + humidity + holiday indicators) with identical preprocessing (KNN imputation, normalization to zero mean unit variance, temporal features for hour/day/weekday/month/year, lag features at 1h/24h/168h, rolling-mean at 3h/24h) and identical 70:20:10 chronological train/validation/test split (train 2015–2021 / validation 2022–2023 / test 2024). Six performance metrics (MAE, MSE, RMSE, MAPE, NRMSE, R²) compared in matched train/validation/test conditions. **Headline result: LightGBM dominates all four deep learning architectures across every metric — test MAE 69.12 MW, RMSE 101.67 MW, MAPE 1.20%, R² 0.9942 — substantially beating LSTM (the second-best DL model: MAE 87.26, RMSE 118.68, MAPE 1.53%, R² 0.9921) and dramatically beating CNN/GRU/CNN-LSTM hybrids.** Additionally compared against literature benchmarks (ANN MAPE 1.92%, SVD-ARIMA MAPE 4.33%, FF ANN MAPE 2.61%) and the *operational ENTSO-E forecast* (MAE 133.31 MW, MAPE 2.33%, R² 0.9813): **LightGBM beats the actual operational utility forecast as well.** Feature importance analysis ranks hour > lag_1 > lag_24 > rolling_mean_3 > temperature > lag_168 > month > weekday > year > day > humidity > is_holiday — i.e., temporal lag features dominate, temperature is mid-rank, humidity and holidays are low-importance.

**Relevance score: 9/10. PRIMARY JUSTIFICATION FOR THE LIGHTGBM ARCHITECTURAL CHOICE.** Same problem domain (STLF on a national-scale European grid), peer-reviewed in a major journal (*Energies*), most directly relevant single-paper evidence that LightGBM matches or beats deep learning on standard load forecasting benchmarks at the scales tested. Cited prominently in the deliberately-not-asked deep-learning defense in BIG_QUESTIONS.md as the 2nd of 5 methodological-benchmark citations (L5 + L12 + L36-B + L40-B + L41-A).

### Brutal relevance defense

**Where it scores positively:**

- **L12-A — The strongest single-paper LightGBM-vs-DL head-to-head in our review for STLF specifically.** Six metrics × five models × three sets (train/validation/test) in a single matched-conditions experiment. Headline test-set numbers from Tables 1–5:
  - LightGBM: MAE 69.12 / RMSE 101.67 / MAPE 1.20% / R² 0.9942
  - LSTM: MAE 87.26 / RMSE 118.68 / MAPE 1.53% / R² 0.9921
  - GRU: MAE 120.64 / RMSE 158.80 / MAPE 2.03% / R² 0.9859
  - CNN: MAE 148.05 / RMSE 194.10 / MAPE 2.59% / R² 0.9789
  - Hybrid CNN-LSTM: MAE 158.23 / RMSE 258.05 / MAPE 2.43% / R² 0.9628
  **LightGBM beats LSTM by 21% MAE / 14% RMSE / 22% MAPE.** Beats hybrid CNN-LSTM by 56% MAE / 61% RMSE — the hybrid actively *hurts* compared to either component alone, contradicting the common assumption that hybrid architectures combine strengths. **This is the most directly applicable evidence in our entire review for the LightGBM-over-DL choice at scale.** Use as the primary citation in our methodology section.

- **L12-B — LightGBM beats the operational ENTSO-E benchmark forecast.** Table 6: ENTSO-E forecast (the actual production-quality day-ahead forecast published by European TSOs) achieves MAE 133.31 / RMSE 182.58 / MAPE 2.33% / R² 0.9813. LightGBM achieves MAE 69.12 / RMSE 101.67 / MAPE 1.20% / R² 0.9942. **LightGBM is ~48% better than the operational utility forecast across every metric.** This is not just a "beat DL on a research benchmark" finding — it's "beat the actual production utility forecast that real grid operations rely on." **The strongest existence-proof in our review that a properly-engineered LightGBM model can outperform real-world utility STLF.** Use in our results-section framing.

- **L12-C — Feature importance ranking aligns with our F1 / no-weather defense.** Section 2.3, Figure 1 ranks features by LightGBM importance: **hour > lag_1 > lag_24 > rolling_mean_3 > temperature > lag_168 > month > weekday > year > day > humidity > is_holiday.** Two important findings: (i) **temporal lag features (lag_1, lag_24, lag_168) and the hour-of-day calendar feature dominate** — direct empirical confirmation that lag-based features are the workhorses of STLF (as documented by L32-B, L32-C, L37-A, L40-C); (ii) **temperature is mid-rank**, well below the dominant temporal features; (iii) **humidity is near-bottom and holidays are essentially noise**, despite Greece having a Mediterranean climate where temperature should matter. **L12-C reinforces the no-weather defense at face value** — even on a dataset *with* temperature available and *with* a Mediterranean climate, the lag features dominate. **Important caveat:** Greek climate is closer to ERCOT's diverse footprint than Dubai's extreme summer-AC scenario, so L12-C is more relevant than L41-E for the ERCOT analog. L12-C strengthens the no-weather defense's heterogeneity-spectrum framing by adding a data point in the *middle* of the spectrum: temperature available, but not load-dominating.

- **L12-D — Demonstrates LightGBM scales to 10 years of hourly data on commodity hardware.** Section 3.6 + 4.4: model trained on TensorFlow/Keras (for DL) vs LightGBM library (for tree-based), with LightGBM exhibiting "lower training times and resource usage, corresponding to histogram-based feature bundling." Quote from Section 3.6: "We used LightGBM as the boosted-tree baseline (chosen over XGBoost for faster, more memory-efficient training on the 10-year dataset)." **This is the explicit justification we need for choosing LightGBM over XGBoost** — for our 15,000-bus × 4-year × hourly dataset (~525 million rows), LightGBM's histogram-based feature bundling matters. Cite L12-D as the published precedent for "LightGBM over XGBoost on large STLF datasets."

- **L12-E — Hybrid CNN-LSTM actively worsens performance.** Table 3 shows the hybrid CNN-LSTM has *worse* test MAE (158.23) than either CNN alone (148.05) or LSTM alone (87.26). The hybrid's R² is also lowest at 0.9628. From the discussion: "The hybrid model integrates the strengths of both CNN and LSTM... [but] the performance did not reflect synergy between the two." **L12-E is methodological evidence against architectural-complexity-as-progress framing.** Direct quote from Section 4.2: "the hybrid model may struggle to fully model high-amplitude, short-duration fluctuations despite leveraging both local pattern recognition (CNN) and long-term sequence modeling (LSTM)." **For our LightGBM defense, this supports the broader argument that simpler, well-engineered methods (LightGBM with good feature engineering) outperform more complex architectures (hybrid DL models).**

- **L12-F — Multi-horizon validation (Figures 13-15: 1-day / 1-week / 1-month forecast horizons).** L12 explicitly validates LightGBM at three different forecast horizons — confirming the model generalizes from short to longer-horizon forecasts. From Section 4.2: "Forecast curves for 1-day (Figure 13), 1-week (Figure 14), and 1-month (Figure 15) horizons further validate LightGBM's performance. In all time windows, its predictions remained tightly aligned with the actual load values, exhibiting minimal deviation or temporal drift." **L12-F is the direct precedent for our two-horizon problem (next-day AND next-month forecasts).** L12 establishes that the same LightGBM architecture handles both 1-day and 1-month forecasts without retraining a separate model. **This supports our pipeline decision to use the same global LightGBM with different feature sets per horizon** — not two completely separate model architectures.

- **L12-G — KNN imputation as the preprocessing default.** Section 2.2: "K-Nearest Neighbor (KNN) imputation was used. KNN is well-suited for time-series data with recurrent patterns as it imputes values based on the similarity to neighboring observations, thereby preserving the inherent structure of the dataset." **L12-G is a third imputation alternative for our Q7** alongside L20-B (AGCIN) and L5-F (linear interp + rolling mean). For our 269 mixed-null buses, KNN imputation using temporal-neighbor similarity is a methodologically defensible third option. **However**, L12 doesn't compare KNN imputation against linear interpolation or other alternatives — so L12-G is a *candidate* method, not direct evidence that KNN beats other methods.

**Where it's weak / what L12 does NOT do for our problem:**

- **Single time series (Greek national load), not bus-level.** L12 forecasts one aggregate national load time series, not multi-bus. Our problem is fundamentally different in scale (1 series vs 15,000). The L12 architectural pattern (single LightGBM with engineered features) generalizes to our problem only if we add bus-id as a feature (per L5-B). L12 does not address the cross-bus information-sharing problem L5 solves.
- **Greek national load is GW-scale aggregation; our buses are MW-scale.** Per L29 scaling law, Greek national load sits firmly in the saturation regime (high W, low MAPE achievable), while our smaller ERCOT buses sit in the scaling regime (smaller W, higher MAPE expected). **L12's 1.20% MAPE is not a number we should expect to replicate on individual ERCOT buses** — it's a number we might approach at ERCOT zone-aggregate or system-aggregate scale.
- **Wrong test-set framing for our problem.** L12 trains on 2015–2021, validates 2022–2023, tests on 2024. Our problem is train 2022–2024 / test 2025. The L12 test-set period is at the start of our training period — these are not comparable evaluation windows.
- **Athens single-station weather, not bus-specific.** L12 uses one weather station (Athens) to represent national load. This is a methodological shortcut L12 acknowledges as a limitation: "Athens weather data were used because it represents the largest load center in Greece... acknowledging possible bias during regionally heterogeneous weather and leaving multi-station/reanalysis inputs for enhancing the robustness of future work." **For our 15,000-bus problem, single-station weather would be much more dilutive than for Greece** (the L34-A / L36-A heterogeneity argument). L12's choice to use single-station weather is consistent with our framing that bus-specific weather is impractical at our scale.
- **Limited reference list (26 papers).** Most are recent (2022-2025) and Greek-STLF-focused; minimal overlap with our broader bus-level literature.
- **No probabilistic forecasting.** L12 produces only point forecasts; no quantile regression or uncertainty quantification.

### Insight labels

- **L12-A (cleanest LightGBM-vs-DL head-to-head in our review):** Six metrics × five models × matched conditions. LightGBM dominates all four DL architectures by 14-61% on all metrics. **Primary citation for our LightGBM-over-DL methodological-benchmark stack** alongside L5 (Triebe et al.), L36-B (TCN-near-ties), L40-B (GAM vs XGBoost caveat), L41-A (XGBoost vs LSTM at DEWA).
- **L12-B (LightGBM beats operational ENTSO-E utility forecast):** LightGBM MAE 69.12 / MAPE 1.20% vs ENTSO-E operational MAE 133.31 / MAPE 2.33%. ~48% improvement over actual real-world utility forecast. Strongest existence-proof in our review for LightGBM beating production-grade STLF.
- **L12-C (feature importance: lags dominate, temperature mid-rank, humidity/holidays low):** Reinforces no-weather defense at face value. Adds Greek-Mediterranean climate as a middle-of-the-spectrum data point in the r=0.016-to-r=0.75 weather-correlation framing (L24-A on one end, L41-E on the other). **For our heterogeneity-spectrum framing, L12-C is the middle-ground data point.**
- **L12-D (LightGBM scales to 10-year hourly datasets on commodity hardware via histogram-based feature bundling):** Explicit justification "chosen over XGBoost for faster, more memory-efficient training." Direct precedent for our LightGBM-over-XGBoost choice at our 525M-row scale.
- **L12-E (hybrid CNN-LSTM worsens performance vs components alone):** Hybrid MAE 158 vs CNN 148 vs LSTM 87 vs LightGBM 69. Architectural complexity is not progress. **Indirect support for LightGBM-over-DL defense** by showing that even within the DL family, more-complex architectures are not strictly better.
- **L12-F (multi-horizon validation: 1-day / 1-week / 1-month):** Same LightGBM architecture validated at three different forecast horizons. **Direct precedent for our two-horizon problem (next-day + next-month):** L12-F supports using the same model architecture with horizon-specific feature sets, rather than separate model architectures per horizon.
- **L12-G (KNN imputation as a third candidate for Q7):** Third imputation option alongside linear interp (M2 plan) and AGCIN (L20-B). Defensible but not specifically benchmarked against alternatives in L12. Possible Q7 robustness comparison.

### Question-sheet pass (full)

- **Q1 (architecture):** L12 forecasts a single aggregate series, not bus-level. No direct Q1 contribution. **L12 reinforces the LightGBM choice within the global-model framework** — the architectural framework comes from L5, the model-family justification comes from L12.
- **Q2 (beat naive):** L12 does not include a naive baseline in the headline comparison. R² of 0.9942 implies LightGBM dramatically beats any naive baseline, but the explicit comparison is not made. **No formal Q2 contribution.**
- **Q3a, Q3b, Q4, Q5:** No direct L12 contribution. L12's single-aggregate-series framing doesn't engage with structural growth, retraining schedules, scaling laws, or zone/bus aggregation tradeoffs.
- **Q6 (sector clustering):** L12 does not cluster — single aggregate national series. **No direct Q6 contribution.**
- **Q7 (imputation):** **L12-G adds KNN imputation as a third candidate** alongside our M2 linear interpolation plan and the L20-B AGCIN alternative. KNN-imputation could be a third comparison method in our Q7 robustness analysis if compute permits.
- **Q8 (scaling-law parameter fit):** L12's single Greek national load series doesn't generate a scaling-law fit. **However**, L12's 1.20% MAPE at GW-scale national-aggregate load is qualitatively consistent with L29's saturation regime — high W, low irreducible relative error. **Indirect support** that the scaling-law framework's saturation regime is real.
- **Q9 (peak-prediction diagnostic):** L12's Figure 11 shows LightGBM forecasts "responding to seasonal fluctuations, daily demand cycles, and abrupt changes in consumption, including those occurring during high-load periods such as mid-summer." Implicit support that LightGBM handles peaks well at aggregate scale, but L12 does not formally diagnose peak-prediction bias or implement peak-weighted loss. **No formal Q9 contribution.**
- **Deliberately-not-asked weather:** **L12-C strengthens the no-weather defense** by adding a middle-of-the-spectrum data point: temperature is *available* in L12 but only mid-rank in feature importance. Update the heterogeneity-spectrum framing to note L12-C as the middle-ground data point between L24-A (FWES near-zero correlation) and L41-E (Dubai r=0.75).
- **Deliberately-not-asked deep learning:** **L12-A is THE primary citation for our LightGBM-over-DL defense.** Six metrics × five models in a matched-conditions experiment, with LightGBM winning every metric across every set (train/validation/test). **L12-B (LightGBM beats operational ENTSO-E) is the strongest existence-proof we have that LightGBM can outperform real production-grade utility STLF.** L12-D justifies LightGBM-over-XGBoost on memory-efficiency grounds. Together, L12 contributes 3 of the 5 most important pieces of evidence in our LightGBM-over-DL stack.

### Reference list capture for tracker (26 references in L12)

Key references from L12's bibliography that overlap with our review:
- [4] Junior, Freire, Seman, Stefenon, Mariani, dos Santos Coelho — Optimized hybrid ensemble learning for STLF (no direct overlap with our existing L-entries).
- [14] Panapakidis, Skiadopoulos, Christoforidis = **L28** (Combined forecasting system for short-term bus load forecasting, IET GTD 2020, our 7/10 anchor) — cited by L12 as a Greek STLF precedent. **Confirms L28 is in L12's bibliography.** L28 + L12 are both Greek STLF papers from the same broader research community.
- [20] Koukaras, Bezas, Gkaidatzis, Ioannidis, Tzovaras, Tjortjis 2021 *Sustainable Computing* — one-step-ahead energy load forecasting. **Same Koukaras as L12 first author** (this is L12's first author citing his own earlier work). Already noted in our L41 review as the same Koukaras appearing as L41 [Ref 4]. **Koukaras now confirmed as a recurring author across L12 (first author) + L41 [Ref 4] (cited) — minor author-cluster observation.**

**Important new (not-yet-in-our-tracker) references L12 cites:**
- [1] Koukaras, Mustapha, Mystakidis, Tjortjis — Optimizing building short-term load forecasting comparative analysis. Same Koukaras research group; another LightGBM-vs-DL comparison. Bucket 2 watch item.
- [9] Stamatellos & Stamatelos 2023 *Applied Sciences* — Short-term load forecasting of the Greek electricity system. Title-direct match for Greek STLF benchmark. Lower-priority Bucket 2.
- [10] Stratigakos, Bachoumis, Vita, Zafiropoulos — Short-term net load forecasting with singular spectrum analysis. Title-direct match for SSA + STLF. Lower-priority Bucket 2.
- [11] Kandilogiannakis, Mastorocostas, Voulodimos, Hilas — Short-term load forecasting of Greek power system (DBD-FELF method with 1.18% MAPE — slightly beats L12's LightGBM at the headline MAPE comparison). Direct comparison method for Greek STLF benchmarks. Bucket 2.
- [18] Wang, Li, Shi, Jiang, Song, Li 2024 *Energy Reports* — CNN + extended LSTM for load forecasting.
- [23] Park & Hwang — Two-stage multistep-ahead electricity load forecasting via LightGBM and attention-BiLSTM. Direct LightGBM + attention-BiLSTM hybrid. Bucket 2 watch item — closest method-family analog beyond what we've assessed.

### Q-sheet changes summary

**Major reinforcement of LightGBM defense, no new question.** Deliberately-not-asked deep learning entry strengthened on three fronts: (i) **L12-A becomes the primary citation** in the methodological-benchmark stack — six metrics × five models × matched conditions, LightGBM dominates by 14-61% across all DL architectures; (ii) **L12-B is the strongest single-paper existence-proof** in our review that LightGBM beats production-grade operational utility forecasts (ENTSO-E benchmark beaten by ~48%); (iii) **L12-D explicitly justifies LightGBM-over-XGBoost** on memory-efficiency grounds at multi-year hourly scale. Deliberately-not-asked weather entry: **L12-C adds the middle-of-the-spectrum data point** to the heterogeneity-spectrum framing (lag features dominate; temperature mid-rank; humidity/holidays low — on a Mediterranean dataset *with* temperature available). Q7 imputation: **L12-G adds KNN imputation as a third candidate alternative** alongside linear interp (M2) and AGCIN (L20-B). Primary backlog F1: **L12-C feature-importance ranking** (hour > lag_1 > lag_24 > rolling_mean_3 > temperature > lag_168 > month > weekday) supports our planned lag-feature engineering and reinforces that calendar+lag features should dominate over weather features at our scale. Primary backlog F1 also picks up **L12-F multi-horizon validation** as the precedent for using the same model architecture for both next-day and next-month forecasts.

---

## L13 — Liu et al. (2026) — Multi-Agent RL for dispatch

**Citation:** Liu et al. (2026). Multi-Agent Reinforcement Learning for power system dispatch. *Reconstructed entry — full bibliographic details to be re-verified from PDF if cited in report.*

**Core idea:** MARL framework for coordinating dispatch decisions across multiple agents in a power system. Optimization/control focus, not forecasting.

**Relevance score: 2/10. Do not cite.** Wrong problem domain — dispatch optimization, not load forecasting.

*Cross-reference:* None (rejected-domain cluster).

---

## L14 — Falas et al. (2026) — PINN state estimation

**Citation:** Falas et al. (2026). Physics-Informed Neural Network for power system state estimation. *Reconstructed entry — full bibliographic details to be re-verified from PDF if cited in report.*

**Core idea:** PINN for power-system state estimation — estimates bus voltages and angles from sparse measurements using power-flow physics as soft constraints.

**Relevance score: 1/10. Do not cite.** Wrong problem domain — state estimation, not load forecasting. Part of the PINN/power-flow cluster (L14, L15, L16, L17) catalogued in this backlog as systematically wrong domain.

*Cross-reference:* None (rejected-domain cluster).

---

## L15 — Leyli-abadi, Marot & Picault (2026) — PINN ablation study for power flow, not relevant

**Citation:** Leyli-abadi, M., Marot, A., Picault, J. (2026). "Study Design and Demystification of Physics Informed Neural Networks for Power Flow Simulation." In: Koprinska, I., Mendes-Moreira, J., Branco, P. (eds) *Machine Learning and Principles and Practice of Knowledge Discovery in Databases. ECML PKDD 2025.* Communications in Computer and Information Science, vol. 2841, pp. 58–75. Springer, Cham. https://doi.org/10.1007/978-3-032-19102-1_4. Affiliations: IRT SystemX (Leyli-abadi), RTE France (Marot, Picault).

**Core idea:** Ablation study comparing four architectures for DC power flow simulation: plain MLP (MSE loss only), MLP Reg (physics local conservation error as regularization term), MP Opt (iterative message-passing solver updating phasors via DC power balance equation, no learned parameters, 100–350 iterations to converge), and PIMP (MLP warm initialization of phasors feeding MP Opt, halving required iterations). Evaluated with the LIPS framework across four criteria: ML accuracy (MAE, MAPE90), physics compliance (local conservation law violation %), OoD generalization, and industrial readiness (inference speedup). Key finding: no single architecture dominates all four criteria — MLP/MLP Reg are fastest but least physics-compliant; MP Opt is most physics-compliant but slow at scale; PIMP is Pareto-optimal compromise. Physics-informed approaches outperform pure ML in low-data regimes.

**Relevance score: 1/10. Do not cite in Assignment 2 report.** DC power flow surrogate — sixth consecutive paper in the wrong problem domain. Predicts bus voltage phasors θ given power injections P_prod, P_load, and topology τ. This requires the admittance matrix (susceptance values b_kj), full network topology, and generator dispatch values — none of which are in our dataset. Our pd column is P_load — an input to this problem, not the target.

The venue is legitimate (ECML PKDD is a top-tier ML conference; CCIS is a recognized Springer proceedings series; proper DOI confirmed). Authors are from IRT SystemX and RTE France (French TSO). The LIPS benchmark framework (ref [13], also by Leyli-abadi et al., NeurIPS 2022) is a genuine contribution used in serious work. None of this changes the domain mismatch.

**⚠️ Potential Assignment 3 relevance:** The LIPS four-dimensional evaluation framework (accuracy, physics compliance, OoD generalization, industrial readiness) is a publishable benchmark methodology. If Assignment 3 involves benchmarking or evaluation design, LIPS is a citable framework for structuring multi-criteria model comparison.

**Confirms existing evaluation plan:**
The LIPS four-category structure maps onto our notebook 06 evaluation design: ML accuracy (MAE/RMSE/WMAPE at bus and zone level), physical consistency (zone aggregation check E3), OoD generalization (FWES/NOTH 2025 vs. training distribution, Q3), and computational readiness (training time at 15,000-bus scale). Our evaluation plan is correctly structured — this paper's framework is independent confirmation.

**Cross-reference:** L9 (Zhang et al. — also uses multi-dimensional evaluation for grid ML models), L12 (Shiblee & Koukaras — external ENTSO-E benchmark as industrial readiness comparison).

---

## L16 — Kilembe, Bukhsh & Papadopoulos (2025) — Power system dynamics, not relevant

**Citation:** Kilembe, A.B., Bukhsh, W., Papadopoulos, P.N. (2025). "Learning of Wide-Area Dynamics in Power Systems with Physics Informed Neural Networks." *2025 IEEE International Conference on Communications, Control, and Computing Technologies for Smart Grids (SmartGridComm)*, North York, ON, Canada, pp. 1–6. DOI: 10.1109/SmartGridComm65349.2025.11204596. Affiliation: University of Strathclyde.

**Core idea:** PINN for transient stability assessment — predicts spatially distributed generator rotor angle trajectories δₖ(t) after power disturbances, embedding the multi-machine swing equation (eq. 3) as a physics constraint in the MSE loss. Novelty: treats generator inertia mₖ and damping dₖ as unknown parameters discovered by the PINN (system identification), rather than assuming they are known inputs. AC-OPF initialization provides physically realistic bus voltages and phase angles rather than flat 1.0 p.u. assumptions. Validated on IEEE 9-bus (mean relative error 2.99×10⁻²) and IEEE 39-bus (mean relative error 3.93×10⁻³), achieving 7–13× inference speedup over MATLAB ode45.

**Relevance score: 1/10. Do not cite.** Transient stability / rotor angle dynamics — seventh consecutive paper in the wrong domain. Predicts δₖ(t) given bus voltages, susceptance matrix B, generator dispatch, and disturbance magnitude. Our pd column is a load demand input to this problem's AC-OPF initialization, not a forecasting target. Requires MATPOWER, MATLAB ode45, full admittance matrix, generator inertia/damping data — none of which are in our ERCOT parquet files. Legitimate venue (IEEE SmartGridComm, same as L11), credible authors (Strathclyde power systems group), but wrong problem domain entirely.

Nothing transferable to our pipeline.

---

## L17 — Jadhav et al. — GGNN for AC power flow, not relevant

**Citation:** Jadhav, S., Sevak, B., Das, S., Su, W., Bui, V.-H. "Enhancing Power Flow Estimation with Topology-Aware Gated Graph Neural Networks." University of Michigan-Dearborn. No journal/conference/DOI visible — appears to be a preprint or workshop paper. **Citation provenance uncertain; do not cite.**

**Core idea:** Benchmarks 14 GNN architectures on AC power flow estimation across IEEE 30, 118, 300, and 1354-bus systems. GGNN (GRU-based iterative message passing, T iterations with shared weights) ranks first overall with total rank score 17.00, outperforming TAGConv (21.50), Transformer (24.75), GCN (50.00), GAT (61.75), MPNN (63.25). Key training insight: extremely small learning rate 5×10⁻⁵ required to stabilize convergence on non-convex AC power flow loss surface. R²>0.99 on all systems.

**Relevance score: 1/10. Do not cite.** Ninth consecutive AC power flow surrogate — predicts bus voltage magnitudes and angles given power injections and admittance matrix. Our pd is the input to this problem, not the output. Citation provenance also uncertain (no journal, no DOI).

---

## L18 — Zheng et al. (2022) — EV bus charging load prediction

**Citation:** Zheng, C., Peng, T., Chao, Z., Shasha, Z., Xiaoyu, L., Han, L. (2022). "Dynamic Load Prediction Model of Electric Bus Charging Based on WNN." *Mobile Information Systems* Vol. 2022, Article ID 6588320. Hindawi. https://doi.org/10.1155/2022/6588320. State Grid Hebei Electric Power Co. Ltd / State Grid Hebei Marketing Service Center.

**Core idea:** Spectral cluster-then-forecast pipeline for EV bus charging load. Step 1: cluster 85 electric buses by charging load curve using combined distance + gray correlation coefficient similarity metric (equations 3–8); optimal K=8 clusters selected by SC and DBI indices. Step 2: one Wavelet Neural Network (WNN) per cluster, GA-optimized initial weights. Step 3: total charging load = sum of per-cluster WNN predictions. Dataset: 85 buses, 31 days, hourly, February 2022, one Chinese city.

**Relevance score: 3/10.** Correct domain (load forecasting + cluster-then-forecast) but wrong load type (EV charging, not transmission bus load) and wrong scale (85 buses, 31 days vs. 15,000 buses, 4 years). Hindawi *Mobile Information Systems* is not a power systems or ML venue of note.

**Insight L18-A — Dual similarity metric for load curve clustering**
The gray correlation coefficient (equation 6) captures shape/trend similarity independently of load magnitude, complementing Euclidean distance which captures magnitude similarity. Combining both produces more physically coherent clusters than pure k-means distance. For our Q6 bus clustering (planned using normalized Euclidean distance on 24-hour diurnal shape vectors per Salgado et al. L4), adding Pearson correlation or gray correlation as a second clustering criterion would better separate buses with similar shape trends but different magnitudes. Low additional implementation cost.

*Cross-reference:* Q6 (sector-type clustering), L4-A (normalize before clustering), L2-B (cluster buses by diurnal shape).

---

## L19 — Xu (2018, dissertation) — Spatial-temporal load forecasting, AMI data

**Citation:** Xu, J. (2018). "Spatial-Temporal Frameworks for Renewable Energy and Power Grid Forecasting: From Research to Application." PhD Dissertation, Stony Brook University, Department of Electrical and Computer Engineering. ProQuest 13425037. Advisor: Yue Zhao; Co-Advisor: Shinjae Yoo, Brookhaven National Lab. **Not peer-reviewed — cite the derived conference paper instead:** Xu, J., Yue, M., Katramatos, D., Yoo, S. "Spatial-Temporal Load Forecasting Using AMI Data." IEEE SmartGridComm 2016, Sydney, pp. 612–618.

**Core idea (load forecasting half, Chapters 7–8):** knmVAR (k-nearest meter based Vector Autoregressive) model for AMI smart meter load forecasting. Step 1: K-means clustering on Pearson Correlation Coefficient (PCC) matrix across 1,708 residential/commercial customers — PCC clustering separates commercial (Cluster 1, 97.67% commercial, high inter-meter correlation) from residential (Cluster 3, 92.32% residential, lower correlation). Step 2: VARX model where each meter's load is a linear combination of its own lags plus lags of k most correlated neighbors. Chapter 8: rolling-window streaming version outperforms static model across all forecast horizons from 15min to 2 weeks. Dataset: 1,708 customers, northeastern US, 15-minute resolution, ~120 days.

**Key empirical finding:** Autocorrelation at 15min=0.87, 1hr=0.66, 1day=0.48, 1week=0.47, 12hr=−0.07. Confirms lag_1, lag_24, lag_168 as primary features. 12-hour anti-correlation is a residential artifact (overnight vs. daytime load).

**Relevance score: 4/10.** Correct domain (load forecasting, cluster-then-forecast) but wrong granularity (AMI individual meters vs. transmission buses), wrong spatial correlation structure (neighboring households share behavioral similarity; transmission buses do not), and dissertation rather than peer-reviewed work. The knmVAR cross-meter correlation framework is computationally intractable at 15,000-bus scale and physically unmotivated at transmission level.

**Insight L19-A — PCC clustering is scale-invariant, superior to Euclidean distance**
K-means on PCC matrix rather than Euclidean distance of normalized time series correctly clusters buses by pattern similarity independent of load magnitude. For Q6 diurnal shape clustering: even after normalization, Euclidean distance conflates shape with residual magnitude differences. PCC is fully scale-invariant and would produce more coherent industrial/residential/commercial clusters. Same insight as L18-A, independently confirmed from a different domain. Low additional implementation cost.

**Insight L19-B — Rolling window outperforms static model**
Motivates treating the FWES/NOTH structural growth problem as a distribution shift requiring recent-data emphasis rather than full-history averaging. Consistent with F2 in primary backlog. Not novel (standard forecasting wisdom), but the SmartGridComm paper version is citable if needed.

*Cross-reference:* Q6, L4-A (normalize before clustering), L18-A (dual similarity metric), F2 (structural growth, rolling window).

---

## L20 — Zhao, Shen, Liu, Liu & Tang (2024) — AGCIN imputation + LASTGCN forecasting for aggregate loads

**Citation:** Zhao, J., Shen, X., Liu, Y., Liu, J., Tang, X. (2024). "Enhancing Aggregate Load Forecasting Accuracy with Adversarial Graph Convolutional Imputation Network and Learnable Adjacency Matrix." *Energies* 17(18), 4583. MDPI. https://doi.org/10.3390/en17184583. Affiliations: College of Electrical Engineering, Sichuan University (Zhao, Shen, Liu, Liu); Institute of Electrical Engineering, Chinese Academy of Sciences (Tang). Funded by National Natural Science Foundation of China (U22B20123).

**Core idea — AGCIN (imputation):** Two-stage imputation combining local and global strategies. Local stage: GCN autoencoder operating on a similarity graph built via mask-aware Euclidean distance (equation 3: only computes distance over coordinates where both nodes have observed values, Mi ⊙ Mj), sparsified by top-p% threshold. Global stage: WGAN-GP adversarial training with the GCN as generator and MLP as discriminator, refining local imputation to match the true data distribution. Denoising autoencoder framing (50% input dropout) used because true missing values are unknown during training. Empirical findings: AGCIN beats KNN, MICE, BRITS, USGAN across both random missing (10–60%) and segment missing (1–9 day) scenarios. At 60% random missing rate, AGCIN RMSE = 0.1465 vs. BRITS 0.1745, MICE 0.2103. Critical ablation: GCIN (AGCIN without GAN) is competitive in segment-missing scenarios but lags in random-missing scenarios — local imputation alone suffices when missingness is structured.

**Core idea — LASTGCN (forecasting):** Spatio-temporal GCN with learnable asymmetric adjacency matrix Aasym = ReLU(tanh(α(S1S2ᵀ − S2S1ᵀ))) generated from randomly initialized node embeddings E1, E2 (equations 12–14), updated via backpropagation. Surrounded by mix-hop graph convolutional layers (k-th order neighborhood mixing) and TCN layers with an inception module (parallel 1×3, 1×6, 1×7 kernels, dilation 1, 2, 4) for multi-scale temporal feature extraction. Compared against CASTGCN (Pearson correlation adjacency) and BASTGCN (binary adjacency): LASTGCN improves MAE by 8.67% over CASTGCN and 10.48% over BASTGCN. Also beats LSTM, TCN, LSTNet, Informer, Autoformer on the 31-user Guangdong dataset (MAE 0.0316, RMSE 0.0570, R² 0.9528). Dataset: 31 aggregate loads from Guangdong, China, 16 months hourly, plus 30-user UCI-Electricity subset for extended validation.

**Relevance score: 5/10. Cite selectively for L20-A and L20-B in preprocessing chapter only.** Correct domain (aggregate load forecasting) with genuinely useful insights, but the LASTGCN architecture is computationally intractable at 15,000-bus scale (a learnable adjacency matrix would be 15,000×15,000 = 225M parameters) and the paper's motivation — learning adjacency without geographic information — does not match our problem where bus topology is known. The authors explicitly acknowledge the scale limitation in their conclusions.

The MDPI *Energies* venue is peer-reviewed and reachable (DOI verified, 2 Web of Science citations) but not a top-tier venue. Acceptable for selective citation but not as a methodological anchor.

**Insight L20-A — Mask-aware similarity in the presence of missing data**
Equation 3 computes Euclidean distance only over coordinates where both nodes have observed values (Mi ⊙ Mj), a principled treatment of distance under missingness. Applies directly to our 269 mixed-null LOAD buses (M2): when clustering buses by diurnal shape for Q6, mask-aware distance avoids the need to impute first. Alternative to the current plan of linear interpolation before clustering — instead, cluster directly on observed values with the mask-aware metric. Low implementation cost, modest methodological improvement.

**Insight L20-B — Local vs. global imputation by missingness regime (negative evidence)**
Empirical finding: GCIN (local only) is competitive with AGCIN (local + global GAN) in segment-missing scenarios but lags in random-missing scenarios. Our 269 mixed-null buses exhibit segment missingness (contiguous null blocks of 10–552 hours per M2). By the paper's findings, simple local imputation (linear interpolation from adjacent hours) should be sufficient for our use case — we do not need GAN-based global imputation. This is *negative evidence supporting our current M2 plan*: linear interpolation is the correct choice, not a sign of cutting corners.

**Insight L20-C — Multi-scale temporal features (conceptual only)**
The inception-style TCN in equation 17 captures temporal patterns at multiple scales via parallel kernels (1×3, 1×6, 1×7) with different dilations. Conceptually validates that our LightGBM with lag_1, lag_24, lag_168 features is capturing the same multi-scale information at lower computational cost. Not implementable in our pipeline — conceptual confirmation only.

*Cross-reference:* M2 (null preprocessing decision), Q6 (diurnal shape clustering), L18-A and L19-A (alternative similarity metrics for clustering).

---

## L21 — Habib, Hossain, Sakib & Alam (2026) — Application-driven survey of load forecasting

**Citation:** Habib, M.A., Hossain, M.J., Sakib, S., Alam, M.M. (2026). "Bridging prediction and decision: An application-driven review of load forecasting in energy systems." *Computers and Electrical Engineering* 134, 111113. Elsevier. https://doi.org/10.1016/j.compeleceng.2026.111113. Affiliations: School of Electrical and Data Engineering, University of Technology Sydney; Department of Electrical & Electronic Engineering, Rajshahi University of Engineering & Technology. Open access (CC BY 4.0).

**Core idea:** Survey/review of 108 load forecasting papers published 2015–2025, organized along three axes: forecasting horizon (USTLF <1hr, STLF 1hr–1mo, MTLF 1mo–1yr, LTLF >1yr), algorithmic family (statistical, ML, DL, hybrid, probabilistic), and operational application (utility/grid, industrial/commercial, residential/microgrid, planning). Introduces an "Application Maturity" (AM) framework classifying surveyed studies along three levels: L1 (algorithm-centric), L2 (experimental integration), L3 (operational deployment). Provides comprehensive comparison tables (Tables 4–11) cataloguing each surveyed paper's contribution, algorithm, preprocessing, data repository, inputs, application, benchmark, accuracy metric, and research gap. Section 2 provides a textbook-level overview of the standard load forecasting pipeline (data collection → preprocessing → feature engineering → modeling → evaluation) with mathematical definitions for all common evaluation metrics (Table 3: MAE, MAPE, MSE, RMSE, Theil's U1, R², MASE, NRMSE, MBE, EVS, AE, and probabilistic metrics PICP/MPIW/Winkler Score).

**Key empirical finding (Figs. 7–9):** Frequency distribution of input variable categories across the 108 surveyed papers. For STLF (40 papers): historical demand 40/40 (100%), meteorological 31/40 (78%), temporal/calendar 14/40 (35%), operational/technical 11/40 (28%), behavioral/occupancy 11/40 (28%). For MTLF/LTLF (~8 papers): historical demand 8/8, meteorological 2/8, temporal 0/8, operational 8/8, behavioral 7/8. Geographic coverage (Fig. 6) is heavily concentrated in China (16 studies), USA (10), Spain (7), with no transmission-bus-level studies at scales comparable to ERCOT.

**Relevance score: 5/10. Cite for background framing and methodology positioning only.** This is a survey paper, not an empirical or methodological contribution. Its value is as a framing citation in the introduction or methodology chapter of the Assignment 2 report, not as a methodological anchor. The AM framework (L1/L2/L3 — note: unrelated to our literature backlog L1/L2/L3 numbering) is a useful taxonomy but does not provide guidance applicable to our problem.

The venue (*Computers and Electrical Engineering*, Elsevier, Q1, 2024 IF ~4.0) is legitimate and peer-reviewed. The paper is fully current (received Dec 2025, published online March 2026), and the literature coverage extends through 2025, making it the most recent and comprehensive survey available to us.

**Citation use cases for the report:**
1. **Background framing:** Citing L21 establishes the breadth of the load forecasting field and positions our work within it. Example claim: "Recent comprehensive reviews of load forecasting [L21] catalog over 100 studies across short-, medium-, and long-term horizons, with the field dominated by household, building, and microgrid-level studies and limited coverage of transmission-bus-level forecasting at scales comparable to ERCOT."
2. **Methodology positioning:** Citing L21's Section 2 lets us position our notebook structure (01 EDA, 02 preprocessing, 03 baselines, 04 zone models, 05 bus models, 06 evaluation) as following the canonical pipeline of data collection → preprocessing → feature engineering → modeling → evaluation, rather than ad-hoc choices.
3. **Defensive citation for feature choices:** L21 Fig. 8 shows 9 of 40 STLF studies (22%) omitted weather features and that historical lag features were universal (40/40). This positions our weather-free, lag-feature-driven approach within empirical norms.

**No methodological insight extractable.** L21 does not present its own methods — it summarizes others'. We cannot derive a usable "Insight L21-A" because the paper's content is categorization, not invention.

**No Q-sheet impact.** Walked all 8 questions (Q1, Q2, Q3a, Q3b, Q4, Q5, Q6, Q7) — L21 provides no evidence that should change any question, refine any method specification, or motivate a new question. Logged as framing-only citation in the question-sheet revision log.

*Cross-reference:* None — used only as background/methodology citation.

---

## L22 — Cheng & Chen (2025) — Open-data grid topology recovery and visualization (Alberta)

**Citation:** Cheng, B., Chen, Y. (2025). "Open Datasets for Grid Modeling and Visualization: An Alberta Power Network Case." *2025 IEEE International Conference on Communications, Control, and Computing Technologies for Smart Grids (SmartGridComm)*. IEEE. DOI: 10.1109/SmartGridComm65349.2025.11204577. Affiliations: Department of Computer Science, University of Toronto (Cheng); Department of Electrical and Computer Engineering, University of Alberta (Chen). Code: github.com/BenCheng2/CarbonDistributionMap.

**Core idea:** Data-engineering pipeline that fuses heterogeneous public data sources to reconstruct the directed topology and approximate nodal demand of the AESO (Alberta Electric System Operator) grid. Three stages: (1) manual digitization of the AESO Interconnected Electric System Map raster into QGIS vector layers, producing CSVs for 855 transmission lines plus substations, city borders, planning-area borders, and population points; (2) population-based demand allocation, where Statistics Canada 2021 census data is aggregated to AESO planning areas and used as a proxy for zone-level demand, then disaggregated to buses via an 84.8% urban / 15.2% non-urban split (the 2021 census urban population fraction) with even distribution within each category; (3) directed line-flow recovery using three voltage-based heuristics (Voltage-Drop Preference, Line Voltage Hierarchy, PV Bus Source Rule) that deterministically assign direction to 355 of 855 lines, then BFS on multi-source DAG subgraphs to resolve the remaining 500, followed by an LP minimizing nonnegative nodal imbalance to assign line flows. The LP solves to machine precision (error < 10⁻⁶), confirming internal consistency but not validating against ground truth (no ground-truth nodal loads available).

**Key empirical finding — population-load correlation at planning-area scale:**
Cosine similarity 0.8959 and Pearson correlation 0.9080 between the planning-area population vector and the planning-area average hourly load vector on the AESO grid (2021 data, 8+ planning areas). This is direct empirical evidence that population is a strong proxy (~90%) for zone-level demand at the planning-area scale.

**Relevance score: 4/10. Cite once or twice as supporting evidence; do not use as methodological source.** Wrong problem domain (topology recovery and visualization, not load forecasting), and the topology recovery is unnecessary for us since the ERCOT parquet files already provide structured topology metadata. The flow-direction heuristics, BFS on DAG subgraphs, and LP for flow assignment are not transferable to our forecasting problem.

The single transferable result is the population-load correlation finding (cosine 0.90, Pearson 0.91). This is from a Canadian grid at planning-area scale that is structurally comparable to ERCOT's eight weather zones.

Venue is legitimate (IEEE SmartGridComm 2025, same as L11 and L16). Open-sourced code. Credible authors.

**Insight L22-A — Population is a ~90% proxy for zone-level demand under normal conditions**
The L22 empirical finding (cosine 0.8959, Pearson 0.9080 on Alberta planning areas) establishes that population distribution explains roughly 90% of the geographic variance in zone-level electricity demand under normal conditions. This has two implications for our work:
1. *Background framing:* When motivating zone-level forecasting as a coherent intermediate aggregation level, we can cite L22 to support the empirical claim that population strongly correlates with zone load. Useful in introduction or methodology chapter.
2. *Structural growth framing (Q3a):* L22's ~90% correlation is the "control case" against which our ERCOT FWES/NOTH structural growth becomes the "anomaly." FWES grew 60% and NOTH grew 51% from 2022–2025, but population in West Texas and the Panhandle did not grow at anything close to those rates — the growth is industrial (data centers, crypto, Permian, Lubbock integration), not demographic. This frames our structural growth challenge as a *departure from the population-correlated baseline that L22 documents*. A model that relies only on population-correlated features will fail in FWES and NOTH for precisely this reason.

**No new question added.** The L22-A insight tightens framing on Q3a's "Why it matters" section but does not motivate a new experimental test. We are already testing per-zone WMAPE; L22 just provides external evidence that the FWES/NOTH errors we expect to see are framed as "deviations from a strong population baseline" rather than "noise."

*Cross-reference:* Q3a (FWES/NOTH structural growth framing), F2 in primary backlog (structural growth, ERCOT-specific challenge).

---

## L23 — Unlu & Peña (2025) — ISO-NE to IEEE synthetic load (top-down disaggregation example)

**Citation:** Unlu, A., Peña, M. (2025). "Time Series Synthetic Load Data Generation for Forecasting and Power System Studies." *2025 7th Global Power, Energy and Communication Conference (GPECOM2025)*, June 11–13, 2025, Bochum, Germany, pp. 923–928. IEEE. DOI: 10.1109/GPECOM65896.2025.11061984. Affiliations: Eversource Energy Center, University of Connecticut (Unlu); Department of Civil & Environmental Engineering, University of Connecticut (Peña). NSF WISER project.

**Core idea:** Top-down spatial allocation framework that converts publicly available ISO-NE zone-level demand data (eight regions: CT, RI, SEMA, NEMA, WCMA, VT, NH, ME, 2017–2021, hourly) into bus-level synthetic load profiles for the IEEE 14, 57, and 118-bus benchmark systems, augmented with NSRDB solar irradiance and NOAA temperature/humidity/wind data. Methodology (Fig. 5): collect ISO-NE zonal load, compute each region's statistical contribution, assign IEEE buses to regions geographically (Table II), compute a scaling factor to match the IEEE region's combined base load to the ISO-NE regional total, distribute scaled load to individual buses proportional to each bus's IEEE base load share, attach weather data. The bus-level profiles are deterministic scaled copies of the zonal profile — no learned bus-level shape variation, no sector mixing, no time-shift diversity. Validation in Section III-C uses Pearson correlation between original and generated regional profiles (diagonal correlation 1.000, off-diagonal 0.868) and PSD analysis — both tautological by construction since synthetic data is just original scaled.

**Relevance score: 3/10. Cite once at most as a comparison example of the canonical top-down disaggregation approach in published synthetic-data work.** Wrong grid (ISO-NE, not ERCOT), wrong scale (118 buses max vs. our 15,000), and wrong problem framing (synthetic data generation for benchmarking, not forecasting). The methodology — top-down scaling of zonal load to buses by static IEEE base-case share — is the canonical version of Q1's top-down baseline that we test against direct bus forecasting. L23 demonstrates the approach exists but does not test forecasting accuracy against it.

Venue (IEEE GPECOM 2025) is a legitimate but second-tier IEEE conference. Authors and funding (UConn Eversource, NSF WISER) are credible. DOI verified. No transferable methodological insight beyond the bus-to-region assignment in Table II which validates that an ~118-bus grid naturally decomposes into ~8 regions with single-digit-to-low-double-digit bus counts (structural similarity to ERCOT, but not actionable).

**No insight labels.** L23 does not contribute a transferable method beyond the canonical top-down baseline framing.

**No Q-sheet change.** Walked all 8 questions. L23 is purely a top-down baseline example with no comparison to direct or naive methods, no temporal growth analysis, no missing-data treatment of relevance, no bus-size stratification, no clustering. Logged as wrong-scale top-down example.

*Cross-reference:* Q1 (top-down baseline framing — supporting citation only; L5 remains the methodological anchor).

---

## L24 — Safdarian et al. (2023) — Temperature-load regression for ERCOT weather zones (KEY ZONE FRAMING SUPPORT)

**Citation:** Safdarian, F., Penaranda, J., Kang, S., Snodgrass, J., Birchfield, A., Overbye, T.J. (2023). "Improving Load Time Series of Electric Power Systems based on the Temperatures." *2023 IEEE Kansas Power and Energy Conference (KPEC)*. DOI: 10.1109/KPEC58008.2023.10215482. Affiliation: Department of Electrical and Computer Engineering, Texas A&M University. Funded by Power Systems Engineering Research Center (PSERC) project S-99.

**Core idea:** Empirical study of temperature-load correlations across ERCOT's eight weather zones (Coast, East, Far West, North, North Central, South, South Central, West) using 12 years of ERCOT public load data (2010–2021) on the same 6,717-bus TAMU synthetic ERCOT grid. Methodology: weather stations mapped to electrical buses by closest geographic distance; hourly temperatures averaged per zone; load normalized by zone's peak demand; temperatures partitioned into four piecewise-linear ranges (<60°F, 60–70°F, 70–80°F, >80°F); separate linear regression per range per zone using scikit-learn; Pearson correlations and R² reported in Table III. Application: the fitted lines are used to scale bus-level synthetic load profiles by the temperature from the closest weather station, producing "temperature-aware" synthetic load.

**Key empirical results (Table III, Fig. 2):** Pearson correlations between hourly load and temperature, 2010–2021:

| Zone | <60°F | 60–70°F | 70–80°F | >80°F |
|------|-------|---------|---------|-------|
| Coast | −0.49 | +0.22 | +0.56 | **+0.81** |
| Far West | **−0.11** | **+0.05** | **+0.08** | **+0.17** |
| North | −0.75 | +0.33 | +0.55 | **+0.84** |
| North Central | −0.74 | +0.20 | +0.57 | **+0.88** |
| South | −0.65 | +0.17 | +0.49 | **+0.81** |
| South Central | −0.67 | +0.22 | +0.59 | **+0.88** |
| West | −0.69 | +0.12 | +0.44 | **+0.80** |

*Note: Table III's East zone entries are identical to Coast — likely a data-entry error in the published table. If citing specific East-zone numbers, re-verify from the PDF or use North/North Central instead.*

Far West correlations are dramatically lower across all temperature ranges (peak |r|=0.17 vs. peak |r|=0.80+ in other zones), which the authors attribute to dominant industrial load (Permian Basin oil/gas) that is largely temperature-insensitive. All other seven zones show strong positive correlation above 80°F (air conditioning) and strong negative correlation below 60°F (heating), with low correlation in the 60–80°F "nice weather" band.

**Relevance score: 6/10. Cite prominently in the Q3a / FWES framing discussion and in the methodology chapter's defense of the no-weather-features decision.** Genuine and direct empirical evidence on ERCOT's eight weather zones (same zones we have in our actual dataset), from the credible TAMU power systems group, published in IEEE KPEC 2023.

Despite the score, the paper does *not* directly change our methodology because we have made the deliberate choice not to use weather features (weather data is not in our provided ERCOT parquet files for 2022–2025). L24 is therefore a framing and defensive citation, not a methodological anchor.

**Insight L24-A — Temperature-load correlation is near-zero in industrial-dominated ERCOT zones (Far West).** Far West has |r|<0.17 across all four temperature partitions; all other seven zones have peak |r|>0.80. This is empirical confirmation that Far West is sector-dominated and weather-insensitive. Combined with L22-A (population is normally a ~90% proxy for zone load) and C1 (FWES grew 60% 2022–2025), L24-A constructs the tight argument: FWES growth is uncorrelated with both population and temperature, confirming it is structural-industrial. The trend/year feature in our model becomes the only available mechanism to capture it. **Add framing note to Q3a citing L24-A.**

**Insight L24-B — Piecewise-linear temperature-load partitioning with 60/70/80°F breakpoints.** Three regimes: heating (<60°F, negative slope), neutral (60–80°F, low slope), cooling (>80°F, positive steep slope). Defensive citation context: even with explicit access to temperature, a simple piecewise-linear model fits the data well. Lag features approximate this implicitly via the seasonal cycle. Not implementable in our pipeline (no weather data) but useful in the report's methodology chapter if reviewers challenge the no-weather decision.

*Cross-reference:* Q3a (FWES structural growth framing — KEY INSIGHT L24-A), F2 (structural growth ERCOT-specific challenge), L22-A (population is normally 90% proxy — L24-A complements with temperature), C1 (ERCOT LTLF growth data), "deliberately not asked" weather features entry (L24 + L24-B as defensive citation).

---

## L25 — Xu et al. (2020) — US 82,000-bus high-resolution test system, population-weighted disaggregation

**Citation:** Xu, Y., Myhrvold, N., Sivam, D., Mueller, K., Olsen, D.J., Xia, B., Livengood, D., Hunt, V., Rouille d'Orfeuil, B., Muldrew, D., Ondreicka, M., Bettilyon, M. (2020). "U.S. Test System with High Spatial and Temporal Resolution for Renewable Integration Studies." IEEE conference paper (ISBN 978-1-7281-5508-1/20), likely PMAPS 2020 — venue identification incomplete from PDF. Affiliation: Intellectual Ventures, Bellevue WA. Funded by The Global Good Fund I, LLC. Dataset: https://zenodo.org/record/3530899.

**Core idea:** Construction of an open-access continental-scale US test system (82,000 buses, 104,121 branches, 13,419 generators) by integrating the TAMU synthetic network topology with publicly available time-series data for 2016: BAA-level demand from EIA's real-time grid feed, NSRDB solar irradiance + SAM PVWatts for solar generation, Rapid Refresh (RAP) numerical weather model wind data + IEC Class 2 power curve for wind, EIA Form 923 monthly hydro totals + USACE hourly shapes for hydroelectric. Demand profile generation (Section III-A): each county is assigned to a BAA, the BAA's hourly load is decomposed to bus-level profiles proportional to the population of each ZIP code associated with each bus. ERCOT (Section III-A.2): the TAMU network's eight load zones are geographically consistent with ERCOT's eight weather zones; each zone's demand profile is disaggregated to buses proportional to ZIP-code population.

**Key empirical detail — Eastern Interconnection missing-data treatment (Section III-A.3):** Missing weekday demand is imputed by averaging the corresponding hours of neighboring weekdays; missing weekend data is duplicated from the other weekend day or averaged from neighboring weekends. Anomalous demand (|hourly ramp rate| > 5σ above average) is discarded and linearly interpolated. This is a more sophisticated treatment than our M1 plan but addresses different missingness patterns (BAA-level systematic gaps vs. our isolated single-hour DST-related gaps).

**Relevance score: 4/10. Cite as canonical reference for population-weighted top-down disaggregation; cite L25-A optionally if F3 (Winter Storm Elliott) anomaly handling becomes blocking.** Wrong scale (continental US, not ERCOT-specific despite covering ERCOT as one interconnection) and wrong problem framing (synthetic data construction for renewable integration, not forecasting). The methodology — population-weighted disaggregation from BAA to bus — is mechanically the same as L23 and is the cleanest canonical example of the static-weight disaggregation that Q1's top-down baseline implements (with historical share as our weight instead of population).

Venue identification is incomplete (DOI not visible in PDF, but Zenodo dataset confirms publication). Intellectual Ventures is a controversial entity but the Global Good Fund runs legitimate technical work. The TAMU synthetic network references are well-established and credible.

**Insight L25-A — 5σ ramp-rate filter for anomaly identification in load time series.** L25's anomaly detection: |first difference of load| > 5σ above the average ramp-rate magnitude. Standard outlier-detection method. **Useful if F3 (Winter Storm Elliott, December 2022) handling becomes blocking** — we have currently deferred F3 with "consider flagging and downweighting." L25-A documents one defensible operationalization. Low priority for now.

**Insight L25-B — Population-weighted BAA-to-bus disaggregation as canonical top-down baseline.** L25's disaggregation rule: bus_load[t] = BAA_load[t] × (ZIP_population[bus] / Σ ZIP_population[BAA]). This is the cleanest version of static-weight disaggregation. Our Q1 baseline uses historical share (mean_bus_pd / mean_zone_pd on training years) instead of population. **Refinement to Q1 method specification:** both weight choices (population and historical share) are static-weight disaggregations; L5's anchor result (direct beats top-down) is expected to generalize across either weight choice. Citing L25 alongside L5 in the Q1 methodology paragraph strengthens the top-down baseline framing.

*Cross-reference:* Q1 (top-down baseline framing — supporting citation alongside L5 anchor), L5 (primary methodological anchor for direct vs. top-down), L23 (similar top-down approach on ISO-NE), F3 in primary backlog (Winter Storm Elliott — L25-A as anomaly-detection option), M1 (missing-data treatment — L25 is more sophisticated than our plan but addresses different gap patterns).

---

## L26 — Gaikwad, Markham & Pourbeik (2016) — WECC Composite Load Model (wrong domain)

**Citation:** Gaikwad, A., Markham, P., Pourbeik, P. (2016). "Implementation of the WECC Composite Load Model for Utilities using the Component-Based Modeling Approach." IEEE conference paper (ISBN 978-1-5090-2157-4/16). Venue not explicitly stated in PDF; ISBN format suggests an IEEE PES conference (likely T&D 2016 or General Meeting 2016). Affiliations: Grid Operations & Planning, Electric Power Research Institute (EPRI), Knoxville TN and Irving TX.

**Core idea:** EPRI's implementation of the WECC Composite Load Model (CLM) — a dynamic load model used in transmission planning for time-domain simulations of fault-induced delayed voltage recovery (FIDVR) and other dynamic disturbance events. The component-based methodology aggregates distribution loads bottom-up by load class (residential, commercial, industrial) and load component (cooling, heating, lighting, etc.), with each component's electrical behavior characterized by static (constant impedance, constant current, constant power) and dynamic (induction motor models A, B, C, D) percentages. Data sources: EIA Residential Energy Consumption Survey (RECS 2005), Commercial Building Energy Consumption Survey (CBECS 2003), DOE Building America Program climate zones. Output: parameterized model record in PSS®E (CMLD) or GE PSLF (CMPLDW) format for use in dynamic simulation software.

**Relevance score: 1/10. Do not cite.** Wrong problem domain entirely — dynamic load modeling for transmission planning (voltage stability, FIDVR) versus our hourly-resolution load forecasting problem. The model parameters characterize the *electrical response* of aggregate load to voltage disturbances in millisecond-to-second timescales, not the time-varying *magnitude* of active power demand over hours and days. There is no time series content in the paper; the percentages in Tables I–IV are time-invariant (one per season).

The only conceptual connection — that L26 classifies buses by dominant load class (residential / commercial / industrial) using external survey data, while our Q6 plans to do the same via diurnal-shape clustering — is too weak to motivate a citation. L26's percentages are not actionable inputs to our forecasting problem.

Venue identification is incomplete. Authors are EPRI Fellows/Members (Pourbeik is a leading authority on power-system load modeling), which is credible — but the domain mismatch makes that credibility irrelevant for our purposes. Paper is also nine years old (2016) and the CLM has been superseded by subsequent WECC working group updates.

**No insight labels. No Q-sheet change.** Walked all 8 questions. L26 addresses none of them. Logged as wrong-domain dynamic load modeling.

*Cross-reference:* None.

---

## L27 — Li, Yeo, Bornsheuer & Overbye (2021) — ACTIVSg synthetic load time series (PRIMARY Q6 ANCHOR; supersedes L2)

**Citation:** Li, H., Yeo, J.H., Bornsheuer, A.L., Overbye, T.J. (2021). "The Creation and Validation of Load Time Series for Synthetic Electric Power Systems." *IEEE Transactions on Power Systems* 36(2), 961–969. DOI: 10.1109/TPWRS.2020.3018936. Affiliation: Department of Electrical and Computer Engineering, Texas A&M University. Funded by DOE ARPA-E GRID DATA program and NSF Grant 1916142.

**⭐ This paper supersedes L2 (Li et al. 2018, TPEC conference) as the canonical citation. Same group, same methodology, peer-reviewed extension in IEEE Trans. Power Systems (flagship power-systems venue).**

**Core idea:** Bottom-up methodology for generating bus-level hourly load time series for synthetic transmission grids, validated on two case studies: ACTIVSg2000 (1,125 load buses, ERCOT footprint, eight weather zones, 71.1 GW peak) and ACTIVSg10K (4,170 load buses, Western Interconnection footprint, 132.5 GW peak). Three nested layers:

*Layer 1 — Bus characterization:* Each load bus has a peak size (base case), geographic coordinate, and residential/commercial/industrial (RCI) composition ratio derived from EIA Annual Electric Power Industry Report sales-by-sector data, assigned to each bus by utility service territory.

*Layer 2 — Prototypical end-user time series library:* 1,020 residential building profiles (DOE/NREL TMY3-location simulations, hourly, one year), 16,320 commercial building profiles (16 DOE reference building types × 1,020 locations), and per-facility industrial profiles built from ORNL per-unit daily curves scaled by Industrial Assessment Centers Database (>14,000 US facilities).

*Layer 3 — Heuristic aggregation with diversity transformations:* Bus peak decomposed into residential/commercial/industrial peak components per RCI ratio; prototypical profiles iteratively selected (probability weighted by geographic proximity for residential/commercial, sector mix for industrial) and added until peak is reached. Three diversity transformations per selected profile: (i) time shift drawn from probability distribution calibrated against real Pecan Street residential metering data via detrended cross-correlation analysis (residential ±12 hours with measured PMF; commercial/industrial shifting probabilities heuristically 30% and 50% lower); (ii) time permutation of 100/100/50 random hour-pairs per year per class for stochastic surges/drops; (iii) Gaussian noise injection without distorting seasonality. Constant offset added to each bus to match the load factor to a randomly selected NREL taxonomy distribution feeder from the same region — enforces realistic aggregation effects.

**Validation (Section IV):** Four-metric statistical validation against a reference range built from 37 European countries (Open Power System Data) + 66 US Balancing Authorities (EIA): (1) monthly load factor (Fig. 11) — synthetic values lie within reference band, summer values slightly higher (base cooling load); (2) load distribution curves (Fig. 12) — denser concentration between 0.8 and 1.2 per-unit of yearly mean; (3) autocorrelation of log-differenced load (Fig. 13) — canonical 24-hour periodic pattern with slight per-cycle magnitude decrease, local maximum at 12-hour lag (residential overnight/daytime anti-correlation); (4) power spectral density (Fig. 14) — sharp peaks at 24h, 12h, weekly (168h), three-month, six-month, and annual periods.

**Key empirical finding — RCI composition on ERCOT-footprint synthetic grid (Fig. 7, ACTIVSg2000):** 67.4% of buses dominated by residential load, 18.8% commercial, 13.8% industrial. This is the closest published reference for the expected sector composition of an ERCOT bus-level grid.

**Relevance score: 8/10. PRIMARY METHODOLOGICAL ANCHOR for Q6 clustering. Cite prominently as the methodological reference for ERCOT bus-level load composition expectations and for synthetic load validation methodology.** Correct domain (transmission bus-level load), correct footprint (ACTIVSg2000 = ERCOT eight weather zones), peer-reviewed in IEEE TPS (flagship power-systems venue), credible TAMU group.

The methodology is the canonical *inverse* of what we want to do: L27 *synthesizes* bus-level profiles from sector composition; we *forecast* bus-level profiles and might *infer* sector composition via Q6 clustering. L27's expected RCI composition (~67/19/14 for ERCOT footprint) becomes the sanity check on our Q6 cluster proportions.

**Insight L27-A — Expected RCI composition on ERCOT bus-level grid: ~67% residential, ~19% commercial, ~14% industrial.** From ACTIVSg2000 Fig. 7. This is the single most direct quantitative reference we have for what to expect when we cluster our 15,000 ERCOT buses by diurnal shape in Q6. If our clusters come out roughly 60–70% residential-shape, 15–25% commercial-shape, 10–20% industrial-shape, this validates the clustering against the L27 baseline. Significantly: if our 2022–2025 clustering produces noticeably *more* industrial-shape buses than 2016's 14%, that is evidence of ERCOT's recent industrial expansion (FWES data centers, crypto, Permian; NOTH industrial growth) — a reportable finding that ties Q6 to Q3a.

**Insight L27-B — Diurnal shapes are sector-specific and visually distinguishable in normalized form.** Fig. 8 of L27 shows: residential-dominated buses have strong seasonal signature with two daily peaks in winter and one in summer; commercial-dominated buses have constant baseline with sharp weekday daytime peak and weekend dropoff; industrial-dominated buses have flat profiles with highest load factor and least daily variation. **Direct methodological support for L4-A (normalize before clustering) and our Q6 plan.** L27 replaces L2 as the primary citation for Q6's clustering rationale.

**Insight L27-C — Four-metric statistical validation framework (load factor + load distribution + autocorrelation + PSD).** L27's validation methodology (Section IV) is publishable as a sanity-check framework for our forecast evaluation. **Optional enhancement for notebook 06 evaluation:** in addition to MAE/RMSE/WMAPE, compute the autocorrelation pattern and PSD of the predicted bus-level load and check that they match the actual bus load's statistical structure. Confirms the model has learned the temporal structure, not just the magnitude. Low priority but methodologically interesting if the report needs depth.

**Insight L27-D — Bus-level diurnal shapes are aggregates over diverse customer-level shapes (±12 hour shifts).** L27 calibrates residential customer-level time shifts against real Pecan Street smart-meter data — up to ±12 hours of shifting. At the bus level, this is averaged out, but it implies that bus-level diurnal shape reflects the *dominant aggregate* behavior, not individual customer composition. **Framing consideration for Q6 results section:** cluster labels reflect aggregate behavior; we should report cluster shapes as aggregates of underlying diversity, not as homogeneous customer types.

*Cross-reference:* Q6 (primary anchor — supersedes L2), L2 (precursor conference version), L4-A (normalize before clustering — L27 provides direct empirical validation), L18-A, L19-A, L20-A (similarity metric refinements), Q5 (L27 demonstrates bus-summed synthetic loads aggregate coherently to system level — framing support), L22 (population-load correlation; L27 documents the bus-level composition that underlies that correlation), L24 (temperature-load correlation; L27's industrial-dominant bus fraction is consistent with L24's Far West weather-insensitivity finding).

---

## L28 — Panapakidis, Skiadopoulos & Christoforidis (2020) — Cluster + FFNN for bus load forecasting (KEY Q6 EMPIRICAL EVIDENCE)

**Citation:** Panapakidis, I.P., Skiadopoulos, N., Christoforidis, G.C. (2020). "Combined forecasting system for short-term bus load forecasting based on clustering and neural networks." *IET Generation, Transmission & Distribution* 14(18), 3652–3664. DOI: 10.1049/iet-gtd.2019.1057. Free Access. Published 8 April 2020. Affiliation: Technological Educational Institute of Western Macedonia, Kozani, Greece.

**Core idea:** Hybrid clustering + feed-forward neural network (FFNN) architecture for short-term bus load forecasting, tested on four medium-voltage transformer buses on the Greek island of Corfu (Kerkira 1, Kerkira 2, Agios Vasilios, Mesogi; peak loads ~25–50 MW per bus). Data: 2013–2015 training, 2016 test (full year, hourly). Architecture: (1) cluster the training matrix (e.g. 1461 days × 48 features for Case 1) into k=3 clusters using one of three algorithms (Fuzzy C-Means, K-means, Ward's); (2) for each cluster, train six parallel FFNN configurations differing in training algorithm (Levenberg–Marquardt, Resilient Backprop, Scaled Conjugate Gradient Backprop) and activation pair (Linear/Tanh, Tanh/Tanh in hidden/output); (3) select the FFNN with lowest training MAPE per cluster; (4) at test time, assign each test pattern to its closest cluster centroid (Euclidean distance, with the unknown current-day load removed) and use that cluster's selected FFNN for the forecast. Three input feature sets tested: Case 1 (loads at d−2 and d−7 only, 48 inputs); Case 2 (+ mean daily temperatures of d−2, d−7, and forecasted d, 51 inputs); Case 3 (+ day-of-week and month-of-year codes, 54 inputs). Compared against single-FFNN baseline and against RBFNN, GRNN, Elman NN, SVR.

**Key empirical findings:**
1. **Cluster + FFNN beats single FFNN on every comparison** (Table 15). Example (Agios Vasilios, Case 1): hybrid MAPE 5.88–6.44% vs. single FFNN 7.01–7.98%.
2. **Case 1 (load only) is the best input feature set**, MAPE range 3.99–9.45%. Case 2 (+ temperature) is 4.16–9.23% — slightly worse. Case 3 (+ calendar codes) is 4.24–9.78% — worst.
3. **K-means is the best clustering algorithm**, followed by FCM; Ward's is last.
4. **Clustering algorithm choice matters more** than NN training algorithm or activation function (Section 4 conclusion).
5. **Elman NN is second-best comparison model**, followed by RBFNN; SVR is worst — supports neural over kernel methods on this small-bus problem.

**Bus scale:** medium-voltage *distribution* transformer buses (Greek island grid), not transmission-level ISO buses. Order-of-magnitude smaller than our ERCOT problem (4 buses vs. 15,000; ~30 MW peak per bus vs. our wider range).

**Relevance score: 7/10. Cite prominently for Q6 as direct peer-reviewed empirical evidence that cluster-then-train improves bus load forecasting; cite L28-C as third independent piece of negative evidence on weather features (for the "deliberately not asked" defensive entry).** Most direct methodological match to Q6 we have found.

**Architecture difference from our Q6 plan (important caveat):** L28 clusters *daily patterns* within each bus's history (cluster-then-separate-models architecture, conceptually similar to L5's Grouped-Global-Bus framework applied per-pattern). Our Q6 plan clusters *across buses* on aggregated diurnal shape (cluster-as-feature in a single global model). These are different clustering targets producing different methodological gains. The L28 architecture is computationally infeasible at our 15,000-bus scale (would require 45,000 separate FFNNs). The **finding** (that clustering improves bus-level forecasting) transfers; the **architecture** does not.

Venue (IET GTD) is Q2 power systems journal, peer-reviewed, 2024 IF ~2.3. Authors have a solid load forecasting publication record. Paper has been cited 100+ times since 2020.

**Insight L28-A — Cluster-then-train empirically beats single-model baseline for bus load forecasting (peer-reviewed evidence).** Across 4 Greek island buses, 3 clustering algorithms, and 3 input feature sets, the cluster + FFNN hybrid produces lower MAPE than the same FFNN with no clustering. Improvement on Agios Vasilios Case 1 is ~1–2 percentage points in MAPE. **The strongest single peer-reviewed citation supporting Q6's central hypothesis that cluster-based architecture improves bus-level forecasting.** L4 (Salgado) and L27 (Li et al.) are foundational but did not run the with-vs-without-clustering ablation; L28 did.

**Insight L28-B — Clustering algorithm choice matters more than NN training algorithm or activation function; K-means is empirically the best default.** Authors' explicit conclusion in Section 4. K-means consistently outperforms FCM and Ward's. Supports our Q6 baseline choice of K-means on normalized diurnal-shape vectors. Cite as defense of the K-means default; positions PCC clustering (L18-A, L19-A) and mask-aware Euclidean (L20-A) as principled robustness upgrades to test.

**Insight L28-C — Adding temperature features made forecasts worse, not better, on this dataset.** Case 1 (load only) MAPE 3.99–9.45%; Case 2 (load + temperature) MAPE 4.16–9.23%; Case 3 (load + temperature + calendar codes) MAPE 4.24–9.78%. Authors attribute to overfitting on small dataset (~1461 days). **Third independent piece of negative evidence on weather features** after L21 (Habib et al. 2026 survey: 22% of STLF studies omit weather) and L24-A (Far West ERCOT zone has near-zero temperature-load correlation). Strengthens the "deliberately not asked" weather-features defensive entry: weather absence is not always a methodological disadvantage; adding weather features does not automatically improve bus-level forecasts even when available.

**Insight L28-D — Adding hand-engineered calendar features (day-type, month-type) made forecasts worse on small-FFNN-per-cluster architecture.** Case 3 has highest MAPE of three cases. Possibly small-sample / dimensionality-growth artifact specific to FFNN-per-cluster training with ~500 days per cluster. Not directly transferable to our global LightGBM with 15,000 buses × 4 years (~22M rows) where dimensionality concerns are minimal. **Low-priority insight; do not over-extend** — calendar features should remain in our feature set.

**Honest weakness disclosure noted by authors (Section 4):** "A large number of clusters can lead to training sets with a low number of members and thus a possible poor training efficiency for the FFNNs; on the contrary, a low number of clusters lead to clusters with many dissimilar patterns." This is the cluster-count tradeoff our Q6 plan also faces. At our scale (15,000 buses / 3-5 clusters = 3,000-5,000 buses per cluster) the small-sample-per-cluster problem does not bind — a methodological advantage of working at transmission-bus scale that L28 explicitly does not have.

*Cross-reference:* Q6 (primary new empirical anchor for cluster-then-train hypothesis — alongside L4 and L27 foundational anchors), L5 (L28's per-pattern clustering is conceptually similar to Grouped-Global-Bus but applied differently), L18-A, L19-A, L20-A (similarity metric refinements — L28-B confirms K-means as baseline), L21 (survey-level negative evidence on weather), L24-A (regime-specific negative evidence on weather), "deliberately not asked" weather features entry (L28-C as third independent piece of negative evidence).

---

## L29 — Sevlian & Rajagopal (2018) — A scaling law for short-term load forecasting on varying levels of aggregation (Q4 ANCHOR — DIRECT ASSESSMENT)

**Citation:** Sevlian, R., Rajagopal, R. (2018). "A scaling law for short term load forecasting on varying levels of aggregation." *International Journal of Electrical Power & Energy Systems* 98, 350–361. DOI: 10.1016/j.ijepes.2017.10.032. Affiliation: Department of Electrical Engineering and Stanford Sustainable Systems Lab, Department of Civil and Environmental Engineering, Stanford University. Funding: NSF CAREER Award (ECCS1554178), NSC CPS Award #1545043, DOE SunShot Office Solar Program Award #31003 Fellowship.

**Note on entry status:** Q4's anchor citation in BIG_QUESTIONS.md has been resting on a single line in L5 (Triebe et al. 2025): "accuracy degrades steeply below 1 MW." Direct assessment from the source PDF in this session. L29 supersedes the citation-through-L5 of Q4's anchor and shifts Q4 framing materially (see Q-sheet pass below). Reference paper for the Q4 watch item flagged in BACKLOG_REFERENCE_TRACKER.md.

**Core idea:** The paper develops a two-parameter empirical scaling law that describes how the relative forecast error (Coefficient of Variation, CV) of any short-term load forecasting method varies with the mean aggregate load *W* of the forecasted set. The form is:

> CV(W) = α₀ / Wᵖ + α₁

where α₀ is the "reducible error" (decreases with aggregation), α₁ is the "irreducible error" (the asymptotic floor), and p ≈ 1 (with empirically observed range 0.88–1.07 across forecast methods). The same form fits MAPE with parameters β₀, β₁.

Two distinct regimes emerge from this form, separated by a **critical load** W★ where α₀/Wᵖ = α₁:
1. **Scaling regime** (W ≪ W★): forecasting accuracy improves roughly as 1/W with aggregation — bigger aggregates are dramatically easier to forecast.
2. **Saturation regime** (W ≫ W★): no further improvement — CV stabilizes at the irreducible floor α₁.

The critical load is method-dependent — better forecasters (lower α₁) have higher W★ (the saturation regime starts later, meaning aggregation continues to help over a wider range).

**Dataset:** Pacific Gas & Electric (PG&E) hourly metering data, August 2010 to July 2011. Residential (RES): >180,000 customers across 408 California ZIP codes, mean consumption 1.05 kWh, max ~4 kWh. Small/Medium Business (SMB): ~150,000 customer profiles, mean consumption 8.94 kWh. **This is the most comprehensive empirical evaluation of aggregation effects on load forecasting in the published literature** — by an order of magnitude over prior work that capped at 1,000–2,000 customers.

**Forecasting models tested:**
| Model | Description |
|-------|-------------|
| M1 | SARMA(1,0)(1,0)×24 |
| M2 | SARMA(2,0)(1,0)×24 |
| M3 | SARMA(3,0)(1,0)×24 |
| M4 | Support Vector Regression — Radial Basis Function kernel |
| M5 | Feed-Forward Neural Network — Logistic activation |
| M6 | Daily-total SARMAX + shape forecast (day-ahead only) |

Models M1–M5 are used for 1-hour-ahead and multi-hour-ahead forecasting; M6 is the day-ahead model.

**The headline Table 2 results (1-hour-ahead, CV-based scaling law):**

| Model | p | α₀ | α₁ | W★ (kWh) |
|-------|------|------|------|----------|
| M1 (SARMA-1) | 0.89 | 53.8 | 2.13 | 2,179 |
| M2 (SARMA-2) | 0.92 | 53.5 | 1.25 | 8,925 |
| M3 (SARMA-3) | 0.91 | 54.4 | 1.19 | 11,615 |
| M4 (SVR) | 0.92 | 52.9 | 1.96 | 6,089 |
| M5 (FFNN) | 0.92 | 55.8 | 1.33 | 72,218 |

Critical loads range from **~2.2 MWh (worst model, M1) to ~72 MWh (best model M5)**. The variation in α₀ across models is small (52.9 to 55.8) — only ~5% range — which the authors note is consistent with α₀ representing a forecaster-independent property of the underlying consumption signal (noise floor). The α₁ values vary by a factor of ~2× across models (1.19 to 2.13), and α₁ — the irreducible error — is the principal performance differentiator between forecasters.

**Theoretical underpinning:** Appendix D provides a derivation from a load-shape stochastic process model. Each individual customer's daily profile is decomposed as x_n(d) = p_n(d) + ε_n(d), where p_n(d) is drawn from a population of "typical" daily shapes and ε_n is zero-mean additive noise with finite within-customer correlation γ. The CV decomposition yields:

> CV(W) = √[(δ(M)² + γ) / μ² + (κ + σ²)/(μN)]

where δ(M)² is a **forecasting-method-dependent population bias term** (how well the model captures the average daily shape), γ is the within-customer error correlation, κ is the shape-variation-around-the-mean term, σ² is the additive noise variance, and μ is mean per-customer consumption (so μN = W). For large N this asymptotes to α₁ = √[(δ(M)² + γ)/μ²], confirming that the irreducible error has two components: the forecaster's intrinsic bias (δ) and a population-level error correlation (γ) that no amount of aggregation can smooth away.

**Forecast horizon scaling (Table 3, model M3):** Multi-hour-ahead extension reveals that irreducible error α₁ grows monotonically with forecast horizon: α₁ = 1.28 (1-hour), 5.30 (2-hour), 8.92 (3-hour), 10.94 (4-hour). The reducible error α₀ remains roughly stable (50–85 across horizons). Critical interpretation: **longer forecast horizons hit the saturation floor sooner** — aggregation provides less benefit when forecasting further ahead.

**Day-ahead result (Section 5.3.2):** Model M6 (full day-ahead) yields CV(W) = 3562/W + 41.9, with p ≈ 1.01. The implied irreducible error is √41.9 ≈ 6.47%, and the critical load is only W★ = 85 kWh (≈80 homes). The authors honestly note that **published day-ahead forecasters dramatically outperform this 6.47% floor** because they are fine-tuned to small specific datasets; this "static" baseline forecaster degrades when applied across many randomly generated aggregates.

**Coastal vs. Inland sub-population analysis (Section 5.5, Table 4):** When the scaling law is fit separately to PG&E coastal (climate zones 1–4) vs. inland (climate zones 11–14) populations, M3 yields critical loads of **W★ = 6.2 MWh (coastal) and W★ = 4.8 MWh (inland)**. Inland has lower irreducible error (α₁ = 1.31 vs. 1.39), even though coastal has higher maximum aggregate load. **The scaling law is robust across climate subpopulations** — the same two-regime structure holds with quantitatively similar parameters.

**SMB consistency check (Section 5.4):** When the same M3 model is applied to the SMB dataset (commercial buildings), the scaling law parameters are α₀ = 46.67, α₁ = 0.92, p = 0.82. The AECs for residential and SMB are visually nearly identical when plotted against mean load in kWh, **despite SMB customers being roughly 9× the per-customer consumption of residential**. The authors interpret this as validation that **kWh is the correct scaling axis, not customer count** — a building is conceptually "an aggregate of tasks" so larger buildings sit further along the same aggregation curve.

**What the paper actually says about the "1 MW threshold":** The 1 MW figure that L5 paraphrases as the threshold for steep degradation is *not* a single number in L29. The critical load W★ varies from ~2.2 MWh to ~72 MWh depending on the model, with day-ahead forecasting hitting saturation at only ~85 kWh. **The "below ~1 MW you're in the scaling regime" intuition is correct but loose** — the actual transition is model-specific and forecast-horizon-specific, and it sits in the range 0.1–10 MWh for hour-ahead forecasting and even lower for day-ahead.

**Methodological positioning:** This is published in IJEPES (Elsevier, Q1 power systems journal, 2024 IF ~5.0), peer-reviewed, with extensive theoretical backing (Appendix D bias-variance derivation), 180,000-customer empirical validation, and clean replication across sub-populations and customer types. It is the canonical reference for aggregation-effect scaling in load forecasting and has been cited extensively (including by L5, where it serves as the empirical foundation for the architectural choice to forecast at zonal level then disaggregate).

**Relevance score: 9/10. Cite prominently as the Q4 ANCHOR. This is the empirical and theoretical foundation for understanding why bus-level forecasting at our scale is intrinsically harder than zone-level forecasting, and for predicting roughly where (in MWh) bus-level forecast accuracy will plateau.** Same tier as L12 (LightGBM defense, 9/10) and one step below L5 (architectural anchor, 10/10).

**Why this scores 9/10 and not 10/10:**
- Wrong dataset domain (PG&E *distribution-level* residential and SMB customers, not transmission-level ERCOT buses). Bus-level loads in our problem are not built up from individual residential meters — they are already aggregations at the transmission-network node level. The scaling law's mechanism (smoothing of individual customer noise via central limit theorem) operates at a scale we don't have direct access to. However, the *empirical regularity* — that relative error follows α₀/Wᵖ + α₁ — is likely to hold for any aggregation level because the bias-variance decomposition in Appendix D doesn't depend on the specific aggregation scale.
- The paper does not test against gradient-boosting methods (LightGBM, XGBoost). All five models are SARMA, SVR, or FFNN. The scaling law form is methodology-agnostic by construction, but we don't have direct evidence that LightGBM at 15,000-bus ERCOT scale will follow the same curve. **This is a research opportunity for our work** — if we plot per-bus WMAPE against bus mean load and find the same α₀/Wᵖ + α₁ structure, we have replicated L29 on transmission-bus data. If we don't, the deviation is reportable.

---

**Insight L29-A — The scaling law form CV(W) = α₀/Wᵖ + α₁ with p ≈ 1 is empirically robust across forecast methods, customer types (residential vs. commercial), and climate sub-populations.** The fitted exponent p ranges 0.88–1.07 (95% CI) across all tested configurations and never deviates dramatically from 1.0. The reducible error α₀ is method-independent (≈50 across all hour-ahead models). The irreducible error α₁ is method-dependent and is the principal performance metric for comparing forecasters. **This gives us a direct prediction for Q4:** plot per-bus WMAPE against bus mean load on a log scale, fit α₀/Wᵖ + α₁, and report (a) the exponent, (b) the irreducible error, (c) the critical load W★ for the chosen forecaster. If our LightGBM at ERCOT-bus-scale produces a clean fit, we have replicated L29 on a fundamentally different (transmission-bus) domain. If it doesn't, the deviation is itself a finding.

**Insight L29-B — Critical load is the diagnostic that distinguishes good forecasters from bad ones.** A forecaster with low irreducible error α₁ has high W★ (saturation sets in only at very large aggregates), meaning aggregation continues to help over a wide range. A forecaster with high α₁ saturates early. M5 (FFNN) has W★ = 72 MWh; M1 (SARMA-1) has W★ = 2.2 MWh. **For our Q4 framing, this means the question "at what spatial resolution does forecast accuracy degrade?" has no single answer — it depends on which forecaster we are using.** Our LightGBM model's critical load is the empirically meaningful quantity to report, alongside α₁.

**Insight L29-C — Day-ahead forecasting hits the saturation floor at much smaller aggregates than hour-ahead forecasting.** Day-ahead M6: W★ = 85 kWh (≈80 homes). Hour-ahead M3: W★ = 11.6 MWh (≈11,000 homes). The same paper's model framework demonstrates that **the value of aggregation is roughly two orders of magnitude smaller for day-ahead vs. hour-ahead forecasting.** For our Assignment 2 work (which forecasts at hourly resolution next-day-ahead), we should expect critical loads in the hundreds-of-kWh to low-MWh range rather than tens of MWh. This is a tighter constraint than L5's "1 MW" paraphrase suggests — we should report Q4 critical loads with day-ahead horizon clearly noted, not conflate them with hour-ahead literature numbers.

**Insight L29-D — Multi-hour-ahead forecasting irreducible error grows monotonically with horizon.** α₁ goes from 1.28 (1-hour) to 10.94 (4-hour). The implication for our work: if we ever extend the next-day-ahead forecast to multi-day-ahead, expect the irreducible error to climb substantially, and expect a smaller effective benefit from aggregation. This is methodological context for any future-horizon extensions; not directly actionable for Assignment 2 but worth flagging in the report's limitations section.

**Insight L29-E — kWh (mean load), not customer count, is the correct scaling axis.** The residential and SMB AECs in Fig. 6(a) overlap nearly perfectly when plotted against mean load in kWh, despite SMB customers averaging 9× the per-customer consumption. The authors interpret this as "every building consumes electricity as a series of tasks of similar average sizes" and larger buildings are conceptually aggregates of smaller ones along the same curve. **For Q4, this validates using bus mean pd (in MWh) — not bus count, not customer count — as the x-axis for the per-bus error analysis.** This is the right scaling choice; our backlog plan should specify mean bus pd in MWh as the abscissa.

**Insight L29-F — The "reducible error" α₀ ≈ 50 is forecaster-independent and represents a property of the underlying consumption signal.** The 95% confidence intervals for α₀ across all five hour-ahead CV models intersect in the range [50.0, 53.8]. The authors interpret this as evidence that **most forecasting work at small aggregation levels is essentially modeling random noise**, since the variation between forecasters' α₀ values is statistically indistinguishable. For our pipeline, this is mild justification for not over-engineering the forecast at the lowest-pd buses; below the critical load, no amount of model sophistication will help. A defensible methodology stance: be honest that small buses are intrinsically hard, and report stratified WMAPE by bus-size quartile (which is already E1 in our primary backlog).

---

**Honest limitations / disclosures in the paper itself:**
1. **Section 5.3.2:** Authors explicitly note that custom-tuned day-ahead forecasters in the literature outperform the static-model M6 baseline. The 6.47% irreducible error for day-ahead is a property of the static model, not a fundamental limit.
2. **Section 5.6:** The model fit's exponent p ≠ 1 is empirically required for good fit on the experimental data, but the authors admit there is **no theoretical model basis for p ≠ 1** — the simple bias-variance theory predicts p = 1. The deviation is treated pragmatically as a fitting parameter.
3. **Section 2.3:** Authors honestly acknowledge that their own prior work (their reference [25], a 2013 conference paper) was limited to 2,000 customers and could not capture the saturation regime. This paper's main contribution is extending to >100,000 customers and identifying the critical load.
4. **Section 6:** Authors flag that aggregation effects should also be tested on **probabilistic forecasts, wind/solar forecasts, and EV-availability forecasting** as future work — none of which appear in our backlog yet but may matter for the broader question of forecastability across resource types.

---

### Q-sheet pass for L29

Walking all 8 questions:

**Q1 (direct vs top-down):** L29 does not test direct-vs-top-down architecture directly. However, its result (aggregation reduces relative error monotonically up to a saturation floor) is the empirical foundation for **why top-down forecasting at the zone level should be more accurate in relative terms than direct forecasting at the bus level** — exactly the question L5 (Triebe et al.) addresses architecturally. L29 + L5 together form a tight argument: L29 establishes that aggregation reduces relative error; L5 establishes that direct bus forecasting nevertheless beats top-down zone-disaggregation in absolute terms via Grouped-Global-Bus architecture. **The two findings are not contradictory** — L29 says zone-level forecasts will be more accurate in relative terms, L5 says we can still recover bus-level information more accurately by direct forecasting *if we share information across buses* via global model + cluster grouping. No change to Q1, but the L29 + L5 pairing becomes the clearest articulation of the architectural tradeoff.

**Q2 (beat naive):** L29 does not test against naive baselines. The α₁ values for SARMA, SVR, and FFNN can be compared but they're not naive. No change to Q2.

**Q3a (FWES/NOTH structural growth):** L29 covers one year (2010–2011) with no structural-growth analysis. No change to Q3a.

**Q3b (rolling-window retraining):** L29's models are static — trained once. No retraining ablation. No change to Q3b.

**Q4 (spatial resolution / bus-size stratification):** MAJOR ANCHOR REVISION. L29 directly provides:
- The functional form for the per-bus-size error analysis: CV(W) = α₀/Wᵖ + α₁
- The expected exponent range: p ≈ 0.88–1.07
- The diagnostic interpretation: report α₁ (irreducible error) and W★ (critical load) for our LightGBM model
- The empirical regularity that residential vs. commercial subpopulations follow the same scaling — so we may want to test our bus-size analysis separately for industrial-dominated buses (Q6 cluster) vs. residential-dominated buses
- The horizon dependence: day-ahead has dramatically smaller W★ than hour-ahead — our Q4 results should clearly note the forecast horizon
- The scaling axis: mean pd in MWh, not bus count
- Cite L29 directly as the Q4 anchor (replacing the citation-through-L5)

**Q5 (zone-direct vs bus-summed):** L29 doesn't test internal consistency between aggregation levels of the *same* forecaster's output. However, the paper's framing — that forecasts at different aggregation levels have systematically different relative errors — has implications for Q5. **If summed bus-level predictions are recombined to the zone level, the relative error should follow the zone-level scaling law, not the bus-level one.** This means a coherent bus-level model whose predictions sum to a zone-level prediction should produce zone-level error comparable to a direct zone-level model. Q5's hypothesis (that direct zone forecasting beats bus-summed) tests precisely this — if bus predictions are noisy enough, their sum has high variance and the bus-summed zone prediction is worse than the direct zone prediction even though the underlying aggregation level (the zone) is the same. **L29 provides the theoretical lens for interpreting Q5 results.** No method change, but framing note added.

**Q6 (cluster label as feature):** L29 does not address sector-type clustering. However, the SMB-vs-residential consistency finding (Section 5.4) shows that the scaling law is robust to customer-type heterogeneity — but only when scaled by mean kWh. This means **the scaling law operates at the aggregate-load level, irrespective of internal sector composition**. For Q6, this implies that cluster information is a refinement *within* the aggregate-load explanation, not an alternative to it. The two should be combined as features. No change to Q6.

**Q7 (imputation):** L29 does not address missingness. No change.

**Q-sheet changes:**
- **Q4:** anchor revised — cite L29 directly (replaces "via L5"). Add specific predictions: expected scaling form, expected exponent, expected diagnostic outputs (α₁ and W★). Add the L29-C note that day-ahead has smaller W★ than hour-ahead. Add the L29-E note that mean pd in MWh is the correct scaling axis.
- **Q5:** add framing note citing L29 — relative errors at different aggregation levels follow systematically different scaling, which provides the theoretical lens for interpreting Q5 results.
- **Q1:** no change, but note in the report's methodology chapter that L29 + L5 together explain the architectural tradeoff.

*Cross-reference:* Q4 (PRIMARY ANCHOR — direct citation replaces citation-through-L5), Q5 (framing support for interpreting bus-summed vs. zone-direct error), L5 (L29 is L5's empirical foundation for the architectural choice), L27 (both papers use the same TAMU-style synthetic-data framing implicitly, though L29 uses real PG&E data not synthetic), L12 (similar tier as a peer-reviewed empirical anchor), "deliberately not asked" deep learning entry (L29's α₀ ≈ 50 finding — that forecaster differences below the critical load are statistically indistinguishable — is mild reinforcement that model-architecture optimization at small bus scales has diminishing returns).

---

## L30 — Sun, Luh, Michel, Corbo, Cheung, Guan & Chung (2013) — Efficient Approach for Short-Term Substation Load Forecasting (BLDF + Decoupled EKF Neural Network)

**Citation:** Sun, X., Luh, P.B., Michel, L.D., Corbo, S., Cheung, K.W., Guan, W., Chung, K. (2013). "An Efficient Approach for Short-Term Substation Load Forecasting." 2013 IEEE Power & Energy Society General Meeting (likely venue, based on IEEE conference paper formatting and the publication date pattern; venue not explicitly named in the PDF). IEEE Conference ID: 978-1-4799-1303-9/13/$31.00 ©2013 IEEE. Affiliations: Electrical and Computer Engineering / Computer Science, University of Connecticut, Storrs CT (Sun, Luh, Michel, Corbo); R&D at Alstom Grid Inc., Redmond WA (Cheung, Guan, Chung). Funding: Alstom Grid grant.

**Important methodological note:** This is the **same TAMU-adjacent UConn research group as L23 (Unlu & Peña 2025)** — Eversource Energy Center at UConn. Continuity in methodology between this 2013 paper and L23's 2025 paper is worth tracking.

### PhD-level defense

The paper proposes a **two-tier substation forecasting architecture** that combines a top-down disaggregation method (Bus Load Distribution Factor, BLDF) with a per-substation neural network (Decoupled Extended Kalman Filter Neural Network, DEKFNN). The architecture's key idea is that substation load patterns vary in their similarity to the parent zone's load pattern, and forecasting accuracy and computational cost can both be optimized by routing "normal" substations through cheap BLDF disaggregation while reserving expensive DEKFNN modeling for "special" substations whose patterns deviate from the zonal pattern.

**The classifier (Section III-B):** Each substation *i* is classified at each forecast point by computing a Euclidean distance between its normalized load profile and the parent zone's normalized load profile over a recent observation window (typically several days to several weeks):

> d(i, k) = √[Σₜ (L_S(t, k, i) − L_Z(t, k))²]

If d falls below a (case-specific) threshold, the substation is classified "Normal" and forecast by BLDF. Otherwise it is classified "Special" and forecast by DEKFNN. The threshold is implicitly chosen per case study by inspection — the paper does not formally derive it.

**The BLDF disaggregation (Section IV-A):** For Normal substations, the forecast is:

> L_S_p(t, k, i) = L_Z_p(t, k) × BLDF(t, k, i)

where BLDF(t, k, i) = L_S(t, k, i) / L_Z(t, k) is the historical share of substation *i* in the parent zone load, averaged over the last several weeks at the same weekday-hour. **This is mechanically identical to our Q1 top-down baseline** — disaggregate zone forecast to bus via static historical share.

**The zone forecaster (Section IV-B):** Zone load is forecasted by a multi-level wavelet neural network: similar-day input selection → discrete wavelet decomposition into H/LH/LL frequency components → separate NN per component → recombine. Each NN is trained by Decoupled Extended Kalman Filter (DEKF), which simplifies the standard EKF by ignoring weak cross-neuron weight correlations in the error covariance matrix. The paper's empirical contribution to the EKF literature is the observation that **decoupling strategy can be derived from inspection of the error covariance matrix itself**: matrix-block patterns reveal which neurons can be safely decoupled (Fig. 4).

**The DEKFNN for Special substations (Section IV-C):** The DEKFNN architecture is applied directly per-substation. Inputs are historical substation load, nearby weather, similar-day load, and day index. Three hidden-layer sizes (18, 20, 20) for H/LH/LL NNs respectively.

**Two empirical case studies:**

*Example 1 (ISO-NE zonal load, 2006–2008):* Tests the DEKFNN approach on zone-level (not substation-level) load forecasting for ISO New England, with the goal of validating the decoupling strategy choice. Four decoupling levels are compared (Fig. 5):
- Standard EKF (1 group, all weights coupled): MAPE baseline, computation time ~270 min
- Node-decoupling EKF (22 groups, one per node): MAPE marginally higher, time ~60 min
- **Error-covariance-derived decoupling (44 groups)**: MAPE only slightly worse than full EKF, time ~30 min — **best tradeoff**
- Diagonal EKF (572 groups, all weights independent): MAPE substantially worse, time ~10 min

The 44-group decoupling reduces computation by ~9× with negligible MAPE degradation. This is the methodological innovation the paper claims.

*Example 2 (23 substations on a New Zealand grid, 2009–2011, 30-min interval):* The full two-tier architecture is tested. Substations are stratified by the Euclidean distance threshold in Table I. For the 18-substation Zone A and 5-substation Zone B reported, the BLDF route handles 18 substations (those with distance < threshold) and the DEKFNN route handles 5 (A3, A14, A15, A18, B3).

**Table III headline results (Zone A and Zone B, May–Sept 2011 test period):**

| Substation | MAPE | MAE (MW) | Route |
|------------|------|----------|-------|
| A1 (BLDF) | 8.76 | 0.90 | BLDF |
| A3 (DEKFNN) | 12.60 | 5.61 | DEKFNN |
| A8 (BLDF) | 3.42 | 5.45 | BLDF |
| A15 (DEKFNN) | 14.23 | 0.23 | DEKFNN |
| A18 (DEKFNN) | NaN | 0.41 | DEKFNN |
| B3 (DEKFNN) | 2.01 | 0.80 | DEKFNN |
| B5 (BLDF) | 6.03 | 1.98 | BLDF |

MAPE values range from 2.01% to 14.23%, with several substations producing NaN MAPE (load values near zero made percentage error undefined).

**The honest finding documented by the authors (Section V):** Substations A9 and A11 are geographically adjacent, and their sum produces MAPE 3.71% — but individually they produce MAPE 6.05% and 10.34% respectively. The authors observe that **"the load sometimes switched from one substation to the other for reasons hidden from the outside"** — i.e., the network was being reconfigured by operators in ways the model couldn't observe. This is direct empirical evidence that **substation-level patterns can be dominated by unobserved switching events**, a relevant phenomenon for our 4,425 LOAD↔ISOLATED-switching buses (primary backlog D3/D4).

A second honest disclosure: Substation A15 has MAPE 14.23% because "the load seems to be dominated by [a major industry company] geographically. Hence, the load could be based on factors hidden from the outside." This is direct evidence at the substation scale that **industrial-dominated nodes are harder to forecast than residential-dominated nodes** — concordant with L24-A's zone-level finding for FWES (Permian Basin).

**Methodological limitations the paper does not address:**
1. The Euclidean distance threshold for Normal/Special classification is set by inspection, not derived. No sensitivity analysis on threshold choice.
2. The BLDF route uses **average BLDF over the last three weeks at the same weekday index** — this is structurally similar to L19's similar-day method but is not contrasted against alternatives (e.g., per-hour BLDF, exponentially weighted BLDF).
3. Only 23 substations tested; no aggregation-effect analysis across substation sizes (this is the gap L29 fills).
4. No comparison against direct-bus forecasting (Triebe et al.'s L5 framework hadn't been published yet — L30 predates L5 by 12 years).

**Venue:** IEEE conference paper, almost certainly the 2013 IEEE PES General Meeting or a related IEEE PES venue (the ID format and conference paper formatting match). Cheaper venue than IEEE Trans. Power Systems, but the UConn-Alstom partnership is credible and the authors (especially Luh) have substantial publication history in load forecasting. **Cited ~150 times since 2013.**

### Brutal relevance assessment

**Relevance score: 6/10. Cite as supporting evidence for Q1's top-down baseline framing, for D3/D4 documenting LOAD↔ISOLATED switching, and as the architectural precursor to L5's Grouped-Global-Bus framework.**

This paper is **L5's intellectual antecedent**. L30 (2013) and L5 (2025) both partition the bus/substation population by load-pattern similarity and then apply different forecasting machinery to different groups. The key architectural differences:

| Dimension | L30 (2013) | L5 (Triebe 2025) |
|-----------|-----------|------------------|
| Classifier | Euclidean distance to zone profile | Per-bus learned (Grouped-Global-Bus) |
| Forecasting machinery | BLDF + per-substation DEKFNN | Single global model with cluster grouping |
| Scale | 23 substations (NZ grid) | thousands of MISO transmission nodes |
| Architecture | Cluster-then-separate-models | Cluster-as-grouping-feature in global model |
| Per-substation cost | High (separate NN per Special substation) | Low (global model amortizes) |

L30 essentially **proves the concept** that pattern-similarity-based routing improves substation forecasting; L5 **operationalizes the concept at transmission-scale** by replacing per-substation NNs with a global model + grouping feature. The 12-year arc between them is the methodological journey our work also benefits from.

**The architecture is not transferable at 15,000-bus scale** for the same reason L28's was not — training 15,000 separate NNs (one per "Special" bus) is computationally infeasible. But the **architectural framing** (route different buses through different machinery based on pattern similarity) is exactly what Q6 + Q1 jointly explore at our scale.

**Why this is 6/10 and not 7/10:**
- Conference paper, not journal — lower citation tier than L29 (IJEPES) or L27 (IEEE TPS).
- Only 23 substations in the case study — too small a sample to establish empirical generalizations.
- The BLDF method as described is identical to our Q1 top-down baseline — no methodological novelty for our pipeline, just confirmation that the static-historical-share disaggregation has been tried before.
- Pre-dates gradient boosting; uses neural networks throughout.

**Why this is 6/10 and not 5/10:**
- The architectural framing (route by pattern similarity) is directly relevant to Q6.
- The two empirical disclosures (load switching between adjacent buses; industrial dominance making forecasting harder) are independent corroboration of findings we have from other sources.
- Same UConn group as L23 (Unlu & Peña 2025) — methodological continuity worth tracking.

---

**Insight L30-A — Adjacent buses can exchange load through network reconfiguration that is not observable from time-series alone.** Sun et al. document that substations A9 and A11 have individual MAPEs of 6.05% and 10.34% but their *summed* load has MAPE 3.71%. Sometimes the load "switched from one to the other for reasons hidden from the outside." For our pipeline this matters in two ways: (a) it provides independent corroboration for D3/D4 in our primary backlog (the 4,425 LOAD↔ISOLATED-switching buses) — at substation scale, network reconfiguration is a real and observable phenomenon, not just an artifact of our PSS/E topology files; (b) it provides a methodological motivation for evaluating zone-summed bus predictions (Q5) — if bus-level errors are partially anticorrelated due to unobservable switching, their sum can be more accurate than the individual buses, which is a separate mechanism from L29's relative-error scaling.

**Insight L30-B — Substations dominated by single large industrial loads are intrinsically harder to forecast.** Sun et al. document substation A15 with MAPE 14.23% because it is "very close to a major industry geographically, and the load seems to be dominated by that company." This is independent empirical corroboration at substation scale of the same finding we have at zone scale from L24-A (Far West / FWES = Permian Basin industrial dominance, near-zero temperature-load correlation). The pattern is: industrial dominance → low correlation with weather → high residual unexplained variance → high forecast error. **For our pipeline, this strengthens the Q3a × Q4 narrative: FWES is hard not only because of structural growth (Q3a) but because individual buses within FWES that are industrial-dominated will also be hard at the bus level (Q4). The two findings compound, not duplicate.**

**Insight L30-C — Empirical demonstration that classifier-then-route architectures work for substation-level forecasting (architectural precursor to L5).** The two-tier BLDF + DEKFNN approach reduces overall MAPE compared to applying either approach uniformly, by sending easy-pattern substations through the cheap path and saving the expensive path for hard-pattern substations. This is the same architectural idea that L5 operationalizes at transmission-scale via Grouped-Global-Bus. **For our Q6 framing, L30 is the original methodological reference for the principle that bus-population heterogeneity should be reflected in the architecture, not just in features.** Cite L30 as the architectural precursor when discussing the L5 Grouped-Global-Bus framework, with the honest caveat that L30's per-substation neural networks are not scalable to our 15,000-bus problem.

**Insight L30-D — Error-covariance-matrix-derived decoupling (44 groups) achieves 9× speedup with negligible accuracy loss in EKF-NN training.** This is a methodological observation about EKF training that does not apply to LightGBM (which has no equivalent training-state covariance structure). Logged for completeness; not actionable in our pipeline.

### Question-sheet pass for L30

Walking all 8 questions:

**Q1 (direct vs top-down):** L30's BLDF method is mechanically identical to our Q1 top-down baseline. Provides additional supporting citation (alongside L23 and L25) for the top-down disaggregation framing. No new evidence on whether direct beats top-down — L30 does not run a direct bus comparison. No change to Q1 method spec, additional citation.

**Q2 (beat naive):** L30 does not compare against naive baselines (same-hour-last-week, etc.). No change to Q2.

**Q3a (FWES/NOTH structural growth):** L30 covers May–Sept 2011 only. No structural growth analysis. However, **L30-B (industrial dominance → high MAPE) is independent corroboration of the FWES framing** at substation scale. Adds support for Q3a's narrative that FWES is hard partly because of sector composition, not just because of structural growth. Framing reinforcement only; no new question.

**Q3b (rolling-window retraining):** L30 uses static training (3 years) with 5-month test. No rolling window. No change to Q3b.

**Q4 (bus-size stratification / scaling law):** L30 has only 23 substations across two zones, too small for scaling-law fit. However, the MAPE range (2.01% to 14.23%) spans 7×, showing substantial substation-to-substation variance — consistent with L29's prediction that intrinsic forecastability varies. No formal Q4 contribution, framing consistency only.

**Q5 (zone-direct vs bus-summed):** **L30-A is directly relevant to Q5.** Sun et al. document a case where the *summed* prediction of two adjacent substations is much more accurate (MAPE 3.71%) than the individual predictions (MAPE 6.05%, 10.34%). The mechanism is unobserved switching between adjacent substations — bus-level errors are anticorrelated by physical conservation. **This is a mechanism for Q5's hypothesis that bus-summed predictions could match (or even beat) zone-direct predictions when bus-level residuals are anticorrelated.** Add framing note to Q5.

**Q6 (cluster label as feature):** L30's architectural framing (classifier-then-route) is conceptually similar to Q6's cluster-as-feature framing, but operates per-forecast-step using Euclidean distance to zone profile, not as a static bus property. The mechanism is different — L30's classification is dynamic (a substation can be Normal one week and Special the next), while our Q6 plan uses static cluster labels. **L30-C cited as the architectural precursor.** No change to Q6 method spec.

**Q7 (imputation):** L30 does not address missingness. No change.

**Q-sheet changes:**
- Q1: add L30 as supporting citation for static-share top-down disaggregation, alongside L23 and L25.
- Q3a: add L30-B as substation-scale corroboration of the industrial-dominance finding (alongside L24-A at zone scale).
- Q5: add L30-A framing note — bus-level errors can be anticorrelated due to unobserved switching, providing a mechanism for bus-summed predictions to outperform what L29's scaling law alone would predict.

*Cross-reference:* Q1 (supporting citation), Q3a (framing), Q5 (L30-A mechanism for bus-summed coherence), Q6 (L30-C architectural precursor to L5), L5 (intellectual ancestor), L23 (same UConn group), L24-A (FWES industrial dominance — zone-scale corroborated by L30-B at substation scale), D3/D4 in primary backlog (LOAD↔ISOLATED switching — L30-A is published evidence the phenomenon is real).

---

## L31 — Haben, Arora, Giasemidis, Voss & Greetham (2021) — Review of low voltage load forecasting (Applied Energy)

**Citation:** Haben, S., Arora, S., Giasemidis, G., Voss, M., Greetham, D.V. (2021). "Review of low voltage load forecasting: Methods, applications, and recommendations." *Applied Energy* 304, 117798. DOI: 10.1016/j.apenergy.2021.117798. Affiliations: Mathematical Institute, University of Oxford (Haben, Arora); CountingLab Ltd (Giasemidis); DAI-Labor, TU Berlin (Voss); Department of Computer Science, University of Reading (Greetham). Funding: BMWi (Germany) WindNODE project (Voss). **Published in Applied Energy (Elsevier, Q1 energy journal, 2024 IF ~11.4) — flagship venue.** Cited 143+ times since 2021.

**Note on PDF content:** The uploaded PDF is the **ScienceDirect landing-page version** (12 pages), containing the full abstract, introduction, scope definition, review methodology, and section snippets — but **not** the complete body of the review and not the full 243-reference bibliography. The landing page shows only the **first 10 references**. For complete reference-list extraction, the full PDF would be needed; the current assessment is based on the available content and is honest about this limitation. **The first 10 references shown on the landing page** are:
1. Wang et al. — *Appl Energy* — smart meter analytics review (mentioned in text as [1])
2. Haben et al. (2019) — *Int J Forecast* — "Short term load forecasting and the effect of temperature at the low voltage level" — **cited explicitly as the power-law-evidence source [7]**
3. Hong et al. (2016) — *Int J Forecast* — Probabilistic electric load forecasting review (= L29's Ref 1)
4. Haben et al. (2014) — *Int J Forecast* — New error measure for household-level forecasts (= L29's Ref 31)
5. Yildiz et al. (2017) — *Appl Energy* — Smart meter analytics review
6. Liu et al. (2019) — *Information* — Two-stage household demand estimation
7. Wang Y. et al. (2019) — *Appl Energy* — Probabilistic individual load forecasting with pinball-loss LSTM
8. Dong B. et al. (2016) — *Energy Build* — Hybrid model for residential forecasting
9. Kipping et al. (2016) — *Energy Build* — Norwegian dwellings disaggregation
10. Litjens et al. (2018) — *Appl Energy* — PV-battery forecasting performance

### PhD-level defense

The paper is a **comprehensive review of forecasting methodology at the low voltage (LV) level** of electrical distribution networks, defined by the authors as ranging from the **medium-voltage 10–35 kV level down to the household level** (Section 1.1, Fig. 1). The review explicitly excludes the higher-voltage transmission level (where our ERCOT work sits) and focuses on the "last mile" of the distribution system — secondary and primary substations, microgrids, and individual customers.

**Scope clarification (critical for our relevance assessment):** Haben et al. explicitly position LV-level forecasting as having "unique features compared to medium (MV) and high voltage (HV) level demand" — namely **increased volatility, increased variety of demand mix, less well-understood explanatory variables, and an increased range of applications.** Our ERCOT problem is HV (transmission-level) bus forecasting at 15,000 nodes — outside L31's stated scope. However, several of L31's claims about LV behaviour can be tested for whether they extrapolate to HV.

**Review methodology (Section 1.3):** Scopus query: `(substation OR feeder OR "low voltage" OR "smart meter") AND (load OR electricity OR consumption) AND (forecast*)`. Returned 1,487 manuscripts. After filtering (2000+ only, conference papers with 20+ citations only, journal papers with 5+ citations or 2019/2020 papers, English only, paywall-accessible): 492 papers, of which 221 were read and reviewed.

**The headline empirical claim (Section 1.1, second paragraph):** "There is often a **power law relationship between the size of the feeder and the relative error**" — cited to Haben et al. 2019 (their reference [7]). The text states: *"This means it becomes exponentially more difficult to accurately forecast smaller feeders (in terms of average demand or number of customers connected)."* **This is independent corroboration of L29's scaling law by a different author group, using independent UK LV-feeder data.** L31's Figure 2 (described, not shown in the landing-page version) explicitly visualizes this relationship.

**Three additional empirical claims from L31 worth tracking:**
1. **The double-penalty effect for smart-meter forecasts (Section 1.1):** "Traditional pointwise error measures such as RMSE and MAPE may not be appropriate (or informative) to describe the accuracy of forecasts of smart meter demand (i.e. individual households) due to the so-called double-penalty effect." This refers to the fact that a forecast that correctly predicts a spike but with a small timing offset gets penalized twice (once for the missed spike at its true time, once for the false spike at the predicted time). The reference is Haben et al. 2014.
2. **The "knowledge of household types is vital" claim (Section 1.1, citing their reference [2]):** "Knowledge of the types of households is vital, for example, households with overnight storage heaters can produce dramatically different behaviours." This is direct empirical evidence that **sector-type information matters for forecast accuracy**, at the household level. Architecturally equivalent to our Q6 motivation but at a different scale.
3. **The "lack of influence of temperature on the forecast accuracy" claim (Section 1.1, citing their reference [2]):** Haben et al. (2019) — the same paper that establishes the LV power-law — *also* finds that temperature **does not improve LV-feeder forecast accuracy** in their real-feeder study. This is a **fourth independent piece of negative evidence on weather features** for our "deliberately not asked" defense, after L24-A (FWES industrial), L28-C (Greek island residential), and L21 (survey-level).

**The 243-reference scope (Section 4 — Datasets):** Of the 221 reviewed papers, only 52 (less than 24%) use openly available datasets, of which 22 (42%) use the Irish CER Smart Metering Project, 4 use UK Low Carbon London project, 4 use Ausgrid. This is meta-information about the field that is useful for understanding the literature's reproducibility limits but not directly actionable in our pipeline.

**Five LV-level features Haben et al. identify (Section 1.1):**
- Increased volatility due to lower aggregation of demand
- Increased variety of demands (feeders with different numbers and types of consumers)
- Less well understood explanatory variables
- Increased range and variety of applications

The first feature is the scaling-law mechanism. The second is sector heterogeneity (Q6 motivation). The third is direct acknowledgment that **LV-level explanatory variables are still being figured out** — a methodological humility that applies even more strongly to transmission buses where the literature is even thinner.

**What's in the PDF that's useful:**
- Abstract and introduction (sections 1, 1.1, 1.2, 1.3) — fully readable, ~50% of the substantive content needed for assessment
- "Section snippets" — partial paragraph-level extracts of methods, trends, datasets, applications, discussion sections
- Reference list — first 10 of 243

**What's NOT in the PDF that would be useful:**
- Full Section 2 (Methods) — text categorization of all 221 reviewed approaches by method type
- Table 1 (overview of methods by aggregation level and forecast horizon) — referenced multiple times but not visible
- Full Section 3 (Trends) — covering probabilistic forecasting, forecast combination, hierarchical forecasting
- Table 2 (open LV datasets) — referenced but only described qualitatively
- Full Section 5 (Applications)
- Section 6 (Discussion and recommendations)
- The complete 243-reference bibliography

This is a **content limitation**, not a paper limitation. The assessment below reflects what can be defended on the basis of the available content.

### Brutal relevance assessment

**Relevance score: 6/10. Cite as supporting framing for the "deliberately not asked" weather defense (L31's temperature finding becomes the fourth piece of independent negative evidence), as independent corroboration of L29's scaling law on UK LV-feeder data, and as background contextualization for the LV-vs-HV distinction in the report. Do not cite as a methodological source for our specific architecture — the review's scope is distribution-level, not transmission-level.**

**Why 6/10 and not higher:**
- **Wrong scale.** The review is explicitly LV-level (10 kV down to household), not HV-transmission-level where our 15,000 ERCOT buses live. The features Haben et al. identify (high volatility, varied consumer mix, unclear explanatory variables) apply most strongly at the household and feeder level. At transmission-bus scale, several of these features attenuate (the law of large numbers smooths much of the volatility once you sum thousands of customers per bus).
- **PDF is the landing-page version, not the full paper.** The richest content (Section 2 methods overview, Table 1, full conclusions) is not in the file. Citations to specific findings are limited to what's in the abstract, introduction, and section snippets.
- **No 243-reference bibliography for the cross-examination phase.** This is a missed opportunity for the reference tracker frequency tally — the full bibliography would have been the single largest input to the cross-citation analysis.

**Why 6/10 and not lower (5/10 or below):**
- **Independent corroboration of L29's scaling law.** L31 cites a UK LV-feeder study (Haben 2019) that finds the same power-law relationship as L29's PG&E study. This is the strongest possible cross-validation: different country, different grid, different methodology, same result. Our Q4 framing is strengthened.
- **Fourth independent piece of negative weather evidence.** Combined with L24-A (FWES industrial), L28-C (Greek island residential), and L21 (survey), L31's "lack of influence of temperature on the forecast accuracy" at LV-feeder level extends the no-weather-defense to a four-piece cluster spanning four different scales (zone, island residential, survey average, UK LV feeder).
- **Flagship Applied Energy venue, 143+ citations, peer-reviewed Q1.** When citing the no-weather defense or the scaling-law cross-validation in the Assignment 2 report, L31 carries strong citation weight.
- **Defines the LV-vs-HV distinction clearly** — useful framing for the report's literature review chapter when contextualizing our HV-transmission-bus problem against the LV-distribution-bus literature.

---

**Insight L31-A — Independent corroboration of the L29 scaling law on UK LV-feeder data.** Haben et al. (2019), cited by L31 as reference [7], find the same power-law relationship between feeder size and relative forecast error that Sevlian & Rajagopal (2018) find on PG&E residential and SMB data. **The scaling law is therefore not specific to PG&E or to California climate** — it is a property of load aggregation that holds across at least two countries (US and UK), two grid types (PG&E distribution and UK LV-feeders), and two author groups. For our Q4, this is the strongest defense available against a reviewer questioning whether L29's PG&E result generalizes. Cite L31 (and through L31, Haben 2019) alongside L29 as Q4 supporting evidence.

**Insight L31-B — Fourth independent piece of negative evidence on weather features for forecast accuracy.** Haben et al. (2019), cited by L31, find "lack of influence of temperature on the forecast accuracy" at UK LV-feeder level. Combined with L24-A (ERCOT Far West industrial, near-zero temperature correlation), L28-C (Greek island residential, weather features made forecasts worse), and L21 (22% of STLF studies omit weather successfully), this is a **four-piece evidence cluster spanning very different scales and grid types** — zone-level industrial-dominated (L24), island residential (L28), survey-level (L21), and UK LV-feeder (L31). For our "deliberately not asked" weather defense, this is the strongest the citation cluster has ever been. Update the deliberately-not-asked entry.

**Insight L31-C — "Knowledge of the types of households is vital" — independent endorsement of the sector-type-matters-for-forecasting principle.** Haben et al. document that LV feeders with high proportions of overnight-storage-heater customers behave dramatically differently from feeders with other customer mixes. This is the household-feeder-level analog of our Q6 motivation. The mechanism transfers upward to transmission buses: if customer-mix matters at LV scale, sector-mix matters at HV scale. Adds independent reinforcement to the L4 + L27 + L28 Q6 anchor stack. Cite where appropriate.

**Insight L31-D — LV-level forecasting is "less well understood" in terms of explanatory variables — the literature is actively figuring out what features matter at scales below MV.** This is meta-information that justifies, in part, why our backlog has been agnostic about feature engineering beyond the established set (lag features, calendar features, zone label). The LV literature itself does not have settled answers on what matters. By extension, the HV-transmission-bus literature has even fewer settled answers. The methodological humility this implies is worth carrying into the report's limitations section.

**Insight L31-E — The double-penalty effect makes RMSE/MAPE less reliable for highly volatile (LV-level) load.** At LV scale, a forecast that correctly predicts the *shape* of a load spike but misses its timing by an hour gets penalized worse than a forecast that predicts no spike at all. This is a measurement-theoretic point. **At our HV-transmission scale, individual buses are aggregations large enough that the double-penalty effect is muted but not eliminated**. For our small buses (the lowest quartile in Q4's stratification), the double-penalty effect may matter and could explain part of any small-bus accuracy degradation. Worth flagging in Q4's discussion if small-bus errors look "worse than they should" by RMSE/MAPE — the issue may be measurement-theoretic rather than model-deficiency.

### Question-sheet pass for L31

Walking all 8 questions:

**Q1 (direct vs top-down):** L31 covers hierarchical forecasting as one of the trends but I cannot extract the specific recommendations from the landing-page text. No direct contribution to Q1.

**Q2 (beat naive):** No specific contribution visible in the landing-page text.

**Q3a (FWES/NOTH structural growth):** No specific contribution.

**Q3b (rolling-window retraining):** No specific contribution.

**Q4 (bus-size stratification / scaling law):** **L31-A is a major Q4 reinforcement.** Independent corroboration of L29's scaling law on UK LV-feeder data. Q4's "Why it matters" section should cite L31 alongside L29 as evidence that the scaling law is robust across countries and grid types. No method spec change — but the citation strength is increased.

**Q5 (zone-direct vs bus-summed):** L31 discusses hierarchical forecasting as a trend, but specific findings not visible in landing-page text. No contribution at this assessment level.

**Q6 (cluster label as feature):** **L31-C is independent endorsement of the sector-type-matters principle.** Add L31-C as supporting evidence in Q6's "Why it matters" section, alongside L4 / L27 / L28 anchors. Strengthens the Q6 citation cluster from three peer-reviewed sources (L4, L27, L28) to four (L4, L27, L28, L31).

**Q7 (imputation):** No specific contribution.

**Deliberately not asked weather features: L31-B strengthens the cluster to four pieces.** Update the deliberately-not-asked entry to include L31-B as the fourth independent piece of negative weather evidence (L24-A + L28-C + L21 + L31-B).

**Q-sheet changes:**
- Q4: add L31-A as independent cross-country, cross-grid-type corroboration of L29's scaling law.
- Q6: add L31-C as independent endorsement of sector-type informativeness.
- Deliberately-not-asked weather features: upgrade to four-piece evidence cluster.

*Cross-reference:* Q4 (L31-A reinforces L29), Q6 (L31-C reinforces L4/L27/L28), deliberately-not-asked weather (L31-B is fourth piece of evidence), L29 (cross-country cross-grid corroboration), L21 (both are review papers in the same family).

---

## L32 — Nabavi, Mohammadi, Motlagh, Tarkoma & Geyer (2024) — Deep learning modeling in electricity load forecasting: DWT-LSTM (Energy Reports)

**Citation:** Nabavi, S.A., Mohammadi, S., Motlagh, N.H., Tarkoma, S., Geyer, P. (2024). "Deep learning modeling in electricity load forecasting: Improved accuracy by combining DWT and LSTM." *Energy Reports* 12, 2873–2900. DOI: 10.1016/j.egyr.2024.08.070. Open-access (CC BY). Affiliations: Sustainable Building Systems Group, Faculty of Architecture and Landscape, Leibniz University Hannover (Nabavi, Mohammadi, Geyer); Department of Computer Science, University of Helsinki (Motlagh, Tarkoma). Funding: DFG researcher unit FOR2363 (EarlyBIM project) and DFG Heisenberg grant (EarlyBIM: GE 1652/3-2, Heisenberg: GE 1652/4-1).

**Note on venue:** *Energy Reports* is an open-access Elsevier journal (2024 IF ~5.2). Lower-tier than Applied Energy (L31), IEEE TPS (L27), or IJEPES (L29), but legitimate peer-reviewed venue.

### PhD-level defense

The paper proposes a **hybrid Discrete Wavelet Transformation + Long Short-Term Memory (DWT-LSTM)** architecture for electricity load forecasting at short-to-long-term horizons (hour-ahead, day-ahead, week-ahead, year-ahead). The architecture is benchmarked against three alternatives: pure LSTM, NARX (a recurrent neural network with exogenous inputs), and SVM regressor. Two case studies: **Iran (2013–2018, with 2018 held out for evaluation)** and **Germany (2015–2019, with 2019 held out)**. Inputs include: previous-hour, day, week, and year load lags; temperature, cloud cover, air density, solar irradiance (decomposed via DWT); day-of-week, holiday flags, religious vacation flags, national vacation flags, hour-of-day.

**The DWT-LSTM architecture (Section 3.2.4, Fig. 10):** Three-stage:
1. **DWT decomposition** of input variables (temperature, solar irradiance, air density) into five frequency bands using cascaded high-pass and low-pass filters. Each input variable becomes five frequency-band variables.
2. **LSTM** with one hidden layer of 200 units, trained with Adam optimizer, 200 epochs max, learning rate 0.01 with piecewise schedule. The DWT-decomposed inputs are concatenated with raw lag features (which are not decomposed because they are already high-frequency time series) and calendric features (which are categorical/binary and inappropriate for DWT decomposition).
3. **Output recombination** via inverse DWT to produce the final forecast.

The authors are explicit that calendric variables are **not** DWT-decomposed because (a) binary/categorical variables are not signals, (b) "having numerous input variables will have a negative impact on the performance of the LSTM model."

**Headline empirical results (Tables 3 and 4, abstract):**

| Forecast horizon | DWT-LSTM MAPE (Iran) | DWT-LSTM MAPE (Germany) |
|------------------|----------------------|--------------------------|
| Hour-ahead | 0.59% | 0.29% |
| Day-ahead | 1.70% | 1.98% |
| Week-ahead | 3.70% | 3.09% |
| Year-ahead | 4.20% | 3.02% |

**Comparison MAPE for hour-ahead (Iran / Germany):**
- DWT-LSTM: 0.59% / 0.29%
- LSTM: 0.72% / 0.72%
- NARX: 1.36% / 1.24%
- SVM: 2.63% / 1.81%

**DWT contribution:** Comparing DWT-LSTM to plain LSTM, the DWT preprocessing reduces MAPE by ~18% (Iran hour-ahead: 0.59% vs. 0.72%) to ~60% (Germany hour-ahead: 0.29% vs. 0.72%). The effect size is meaningful but not transformative — the methodological story is "DWT preprocessing modestly improves a strong LSTM baseline" rather than "DWT is a game-changer."

**Special-events handling (Section 4.2, Table 4):** Religious and national vacations are explicit inputs. For Iranian Nowruz (new year, fixed lunar-calendar date), Easter and Christmas (Germany), and Ramadan/mourning months (Iran), the model is evaluated separately. DWT-LSTM achieves MAPE 0.55%–3.07% during Iranian special events and 0.33%–6.01% during German special events.

**Feature importance analysis (Section 4.3):** Permutation importance per forecast horizon. **Findings for both countries:**
- Hour-ahead: recent consumption lags (EC Hour Lag) overwhelmingly dominate (importance score 100). Weather features (solar irradiance, temperature) and calendar features contribute <10%.
- Day-ahead: EC Day Lag dominates; Day of Week becomes second-most-important.
- Week-ahead: EC Week Lag dominates; Day of Week and solar irradiance both contribute substantially.
- Year-ahead: EC Year Lag dominates; Day of Week and solar irradiance contribute meaningfully.

**Method ranking (consistent across both case studies):** DWT-LSTM > LSTM > NARX > SVM for almost all configurations.

**National-scale aggregation:** Both case studies are **national-grid total load forecasting** (Iranian Grid Management Company data; SMARD German data). This means the data is in the saturation regime of L29's scaling law — the entire dataset is at the highest aggregation level possible for a single country. The MAPE values of 0.29%–4.2% are consistent with what L29 would predict for that regime.

**Methodological observations the paper does not explicitly address:**
1. The DWT preprocessing applied only to weather inputs (not load lags) is unusual; most DWT-in-load-forecasting literature decomposes the load signal itself. The choice is reasonable but not well-justified in the paper.
2. No comparison against gradient boosting (LightGBM, XGBoost). All comparisons are within the neural-network and SVM families.
3. The cross-country generalization claim ("our method is versatile and robust across diverse contexts") is implicit, not explicitly tested via transfer learning. The two case studies are trained and evaluated separately.
4. The 200-unit LSTM hidden layer with 200 max epochs is a substantial neural network; computation cost is mentioned (Section 3.2: "18 min to be trained on 43,824 samples") but not benchmarked against simpler alternatives.

### Brutal relevance assessment

**Relevance score: 4/10. Cite as supporting evidence for the DWT-as-preprocessing methodology and as a reference point for cross-country generalizability of deep-learning forecasting, but with explicit acknowledgment of the wrong-scale problem.**

**Why 4/10 and not higher:**
- **Wrong scale by two orders of magnitude.** Nabavi et al. forecast Iranian and German *national* total electricity load (tens of GW). We forecast individual ERCOT buses (often <10 MW). National-scale forecasting is in L29's saturation regime where the irreducible error α₁ is what matters. Bus-level forecasting is in the scaling regime where α₀/W^p dominates. These are different problems methodologically — what works at national scale may not transfer.
- **Wrong method family for our problem.** We use LightGBM. Nabavi et al. test LSTM, NARX, SVM. They do not compare against gradient boosting. The closest method tested is SVM, which they show consistently underperforms — not the same conclusion as the L12 finding (Shiblee & Koukaras 2025) that gradient boosting beats deep learning at Greek STLF scale. The two findings could co-exist: deep learning beats SVM, gradient boosting beats deep learning. But L32 does not provide direct evidence for our LightGBM choice.
- **No bus-level, substation-level, or even regional disaggregation analysis.** L32 is a single-time-series-per-country problem. The phenomena we care about (per-bus scaling, sector-type heterogeneity, structural growth in subregions) are not in scope.
- **The DWT preprocessing is for weather inputs only, not the load signal itself.** This is a niche choice that does not have an obvious analog in our pipeline. We do not use weather inputs at all (per our deliberately-not-asked decision), so the DWT-weather pre-processing is doubly irrelevant.

**Why 4/10 and not lower (3/10 or 2/10):**
- **Three pieces of independent positive evidence for our pipeline:**
  1. **Cross-country generalization at all** (Iran + Germany, in the same architecture, both achieve low MAPE) is mild reinforcement that load-forecasting methodology can transfer across grids. Our work transfers methodology *within* one grid (ERCOT) but across thousands of buses — a different transfer direction but the same general claim.
  2. **Calendric features (especially day-of-week) emerge as second-most-important in feature importance** — independent corroboration that calendar features matter for load forecasting and should be included in any pipeline. We already include them, but L32 is supportive evidence that the choice is correct.
  3. **The temperature feature contributes <10% to hour-ahead importance** (Section 4.3.1 Iran, 4.3.2 Germany) — adds mild reinforcement to the no-weather defense, although at national-scale aggregation this finding is less generalizable to bus scale.
- **Open-access publication with full reference list.** Useful for the reference tracker.
- **2024 publication** — places L32 in the recent literature, post-L29 (2018), post-L20 (2024), post-L27 (2021). Confirms the DWT methodology family is still actively published. (Although L32 is much weaker than L20, which we already have at 5/10 for AGCIN/LASTGCN.)

**Note on the relationship to L20 and L7:**
- L20 (Zhao et al. 2024): AGCIN + LASTGCN — uses DWT-like decomposition in attention-graph networks at distribution scale. Scored 5/10.
- L7 (Türkoğlu et al. 2024): TCN with missing data — also uses temporal decomposition. Scored 4/10.
- L32 (Nabavi et al. 2024): DWT + LSTM at national scale.

L20 > L32 in scale relevance (L20 is at distribution scale, closer to our bus problem). L32's DWT-on-weather-only is a more limited use of DWT than L20's full graph-wavelet approach.

---

**Insight L32-A — DWT preprocessing of weather inputs modestly improves LSTM-based load forecasting accuracy (18%–60% MAPE reduction at hour-ahead).** The improvement is meaningful but not transformative. The mechanism is presumably that DWT decomposition extracts long-term and short-term weather patterns that LSTM can correlate with load. **Not directly applicable to our pipeline** because we do not use weather features. Logged as background context for the DWT-as-preprocessing methodology family. If we later decide to add weather features (which we are not planning to do), L32 is a supporting reference. As currently scoped, L32-A is not actionable.

**Insight L32-B — Recent consumption lags (hour, day, week, year) overwhelmingly dominate feature importance across all forecast horizons.** L32 Figure 27 (Germany) and Figure 28 (Iran) both show that for hour-ahead, day-ahead, week-ahead, and year-ahead forecasts, the lag feature at the relevant horizon has importance score 100, with all other features in single digits or low double digits. **This is direct reinforcement that our pipeline's heavy reliance on lag features (F1 in primary backlog) is methodologically standard and empirically defensible.** For any reviewer questioning why we use lag features so prominently, L32's feature importance analysis at national scale is direct supporting evidence. The pattern likely holds at bus scale by extension.

**Insight L32-C — Day-of-week is the consistently second-most-important feature across all horizons.** L32's feature importance shows that day-of-week importance scores are in the 40–100 range across hour/day/week/year-ahead horizons, well above weather features. This is mild reinforcement that day-of-week is a high-value calendar feature and should be included in any feature engineering. Our pipeline includes day-of-week (F1); L32 confirms this is the right choice.

**Insight L32-D — Temperature importance is consistently low (<10% relative to lag features) at national-aggregation scale.** Adds mild fifth-piece reinforcement to the no-weather defense, but with the caveat that the scale (national) is very different from our scale (bus). At national scale, weather effects average out across many sub-regions; at bus scale, weather effects on a single industrial node could matter more. The L32 finding is consistent with L24-A but does not strengthen the defense as much as L24-A (which is at ERCOT zone scale) or L28-C (which is at bus scale on Greek islands).

### Question-sheet pass for L32

Walking all 8 questions:

**Q1 (direct vs top-down):** L32 is single-time-series national-scale, no spatial disaggregation. No contribution to Q1.

**Q2 (beat naive):** L32 does not benchmark against naive baselines (same-hour-last-week, etc.). Comparisons are only among ML methods (DWT-LSTM vs LSTM vs NARX vs SVM). No direct evidence for Q2.

**Q3a (FWES/NOTH structural growth):** L32 covers 2013–2018 (Iran) and 2015–2019 (Germany), with year-ahead forecasting reported. The MAPE of 4.2% (Iran) and 3.02% (Germany) at year-ahead is consistent with what would be expected when there is no structural break in the data. **L32 does not directly address structural growth, but its year-ahead success at relatively flat-growth national datasets is mild evidence that our pipeline's challenge in FWES/NOTH is specifically the structural-growth component, not year-ahead forecasting per se.** No new question, framing note only.

**Q3b (rolling-window retraining):** L32 uses fixed train/test split, no rolling-window analysis. No contribution.

**Q4 (bus-size stratification):** L32 is single-aggregation-level (national). No contribution to Q4.

**Q5 (zone-direct vs bus-summed):** L32 does not aggregate or disaggregate. No contribution.

**Q6 (cluster label as feature):** L32 does not cluster. No contribution.

**Q7 (imputation):** L32 mentions filling missing data with neighbor-average but does not run any imputation-method ablation. No contribution.

**Deliberately not asked weather features (L32-D adds mild reinforcement):** Update the deliberately-not-asked entry from a four-piece evidence cluster (L24-A + L28-C + L21 + L31-B) to a five-piece cluster including L32-D, with the caveat that L32-D is at national scale (less direct than the others).

**Feature engineering reinforcement (F1 in primary backlog):** L32-B and L32-C provide reinforcement that lag features (especially at the relevant horizon) and day-of-week are high-value features. This is not a new question but is direct supporting evidence for F1 design choices.

**Q-sheet changes:**
- Deliberately-not-asked weather: upgrade to five-piece evidence cluster (with L32-D as the weakest piece due to scale mismatch).
- F1 (primary backlog) framing: L32-B and L32-C are independent confirmation that lag features and day-of-week are standard high-importance features.

*Cross-reference:* L20 (closest method family, also uses DWT, scored 5/10), L7 (also decomposition-based, scored 4/10), F1 in primary backlog (feature importance reinforcement), deliberately-not-asked weather (L32-D adds national-scale evidence), L29 (L32 operates in L29's saturation regime by virtue of national-scale aggregation).

---

## L33 — Gielen, Boshell, Saygin, Bazilian, Wagner & Gorini (2019) — The role of renewable energy in the global energy transformation (Energy Strategy Reviews)

**Citation:** Gielen, D., Boshell, F., Saygin, D., Bazilian, M.D., Wagner, N., Gorini, R. (2019). "The role of renewable energy in the global energy transformation." *Energy Strategy Reviews* 24, 38–50. DOI: 10.1016/j.esr.2019.01.006. Open-access (CC BY-NC-ND). Affiliations: International Renewable Energy Agency (IRENA) Innovation and Technology Centre, Bonn, Germany (Gielen, Boshell, Wagner, Gorini); SHURA Energy Transition Centre, Istanbul, Turkey (Saygin); Payne Institute, Colorado School of Mines (Bazilian, Gielen).

**Note on inclusion:** Per user statement, this paper is included in L5's (Triebe et al. 2025) reference list. I have not verified the citation chain directly (the full L5 bibliography is not in this session's context). Assessment proceeds on good faith that the inclusion is correct.

### PhD-level defense

The paper is a **policy-economic analysis of the global energy transition to 2050**, based on the IRENA REmap (Renewable Energy Roadmap) modeling framework. It assesses the technical and economic characteristics of an accelerated transition pathway in which renewable energy supplies two-thirds of global energy demand by 2050. The paper compares the REmap pathway against a Reference Case (current policies) and against parallel scenarios from the IEA (World Energy Outlook 2°C/66%) and Shell (Sky scenario).

**Methodology (Section 2):** REmap is a techno-economic accounting framework, not an integrated assessment model. It uses bottom-up country-level data from 70 countries (collected directly from national experts and governments) representing ~90% of global energy use, combined with top-down sectoral assessments at the end-use level (residential buildings, industry, transport). Technology costs are derived from a comprehensive IRENA database of renewable energy technology costs. Macro-economic impacts (employment, GDP) are computed using the E3ME post-Keynesian macro-econometric model with 59 country/region coverage and 43 economic sectors. The REmap Case explores low-carbon technology pathways consistent with the Paris Agreement 2°C limit (66% probability).

**Headline findings:**
- Renewable energy share rises from 14% (2015) to **63% of total primary energy supply by 2050** in REmap. The fossil-fuel share drops from 86% to 37%.
- Energy efficiency and renewables together achieve **94% of needed emissions reductions** by 2050. The remaining 6% comes from fossil-fuel switching, nuclear continuity, and CCS in industry.
- Cumulative additional investment 2015–2050: **USD 27 trillion** above the Reference Case (~0.4% of cumulative global GDP).
- Net job impact: **+11.6 million** direct and indirect jobs in the energy sector by 2050 (19M gained, 7.4M lost in fossil fuels).
- Renewable power generation capacity grows 8× from ~2,000 GW (2015) to **~16,000 GW (2050)**, with 7,122 GW solar PV and 5,445 GW wind.
- **Variable renewable energy share rises from ~10% to 60% of total power generation**, requiring "a paradigm shift in the power sector."

**One specific operational claim worth noting (Section 6):** "In 2017, 50 Hz the grid operator in eastern Germany recorded an annual average of 53.4% variable renewable energy. This indicates that it is possible to operate grids with high shares of variable renewables." This is an empirical claim about grid operability that is relevant to power-system planning but **not to load forecasting methodology**.

**Sector breakdown (Fig. 3, REmap 2050):** Power 58% of total renewable deployment, with heat-and-direct-uses 29% (biomass, solar thermal, geothermal heat) and transport 13% (electrification, biofuels, hydrogen). No mention of load forecasting or any methodology applicable to our problem.

**Innovation discussion (Section 7):** Identifies sector-coupling, smart grids, EV charging integration, and storage as priority R&D areas. Box 1 (EV charging impact on Hamburg distribution grid) is the **only place the paper discusses anything resembling load patterns** — and it does so qualitatively, describing how Stromnetz Hamburg expects 9% EV penetration to cause bottlenecks in 800 out of 6,000 feeders, without any forecasting methodology or empirical load analysis.

**Venue (Energy Strategy Reviews, Elsevier, 2024 IF ~10.8):** Strong policy venue for energy transition analysis. Cited 500+ times since 2019. This is a high-impact paper *in its field*. Its field is **energy policy and transition modeling**, not load forecasting.

### Brutal relevance assessment

**Relevance score: 1/10. Do not cite. Wrong problem domain entirely.**

This is an IRENA-authored energy policy and economic transition paper focused on global decarbonization pathways to 2050. The paper has **zero connection to load forecasting methodology**:
- Zero mentions of load forecasting, forecast accuracy, prediction error, or any methodology-evaluation metrics (verified by text search: 0 matches for `load forecast`, `MAPE`, `RMSE`, `forecast accuracy`, `prediction error`, `neural network`, `machine learning`, `regression`, `XGBoost`, `LightGBM` in the paper body)
- Zero quantitative methodology for bus-level, substation-level, or feeder-level forecasting
- Zero ERCOT-relevant content (the paper covers global G20 economies; ERCOT is mentioned nowhere)
- The one mention of "load" in an operational sense (Box 1: Stromnetz Hamburg EV charging study) is qualitative scenario description, not methodology

**The only conceivable connection to our problem:**
- The paper documents that **variable renewable energy (VRE) penetration in power grids is growing rapidly** (60% of total generation in REmap 2050, 53.4% on 50Hz Germany already in 2017). This implies that **load-net-of-renewables forecasting will become increasingly important in the long run** — but our Assignment 2 is bus-level *load* forecasting (the pd column in our PSS/E data), not net-load forecasting, and the time horizon is one year (2025), not 30 years. The connection is too distant to support a citation.

**Honest acknowledgment about why this paper is in the L5 reference list:** Without seeing L5's full bibliography, I can speculate but not verify. L5 (Triebe et al. 2025) is a zonal-to-nodal load forecasting paper for MISO. Plausible reasons L5 might cite Gielen et al.:
- Background framing in L5's introduction about why high-resolution grid modeling matters (decarbonization, VRE integration)
- Citation in L5's discussion section about future-direction implications (net-load forecasting with high renewables penetration)
- General citation about the importance of grid modernization

None of these reasons translate into methodological value for our pipeline. L33 is an **introduction/background citation** in L5's bibliography, not a methodological source.

**This is the second background/policy-domain paper in our 33-paper review** — first was L26 (Gaikwad et al. 2016, WECC Composite Load Model) which is dynamic load modeling for FIDVR studies. L26 scored 1/10 for the same reason as L33: correct-domain-adjacent but no methodological transfer.

### Question-sheet pass for L33

Walking all 8 questions:

**Q1 through Q7:** L33 does not address any of the questions. Energy policy and transition modeling are orthogonal to bus-level load forecasting.

**Q-sheet change:** None.

*Cross-reference:* Tagged in the reference tracker for the cross-examination phase (its presence in L5's bibliography is a data point about the *type* of references L5 cites — background framing material alongside methodological sources). Logged for completeness.


## L34 — Schröter, Richter, Götze, Naumann, Gronau & Wolter (2020) — Substation Related Forecasts of Electrical Energy Storage Systems: TSO Requirements

**Citation:** Schröter, T., Richter, A., Götze, J., Naumann, A., Gronau, J., Wolter, M. (2020). "Substation Related Forecasts of Electrical Energy Storage Systems: Transmission System Operator Requirements." *Energies* 13(23), 6207. DOI: 10.3390/en13236207. Open access (CC-BY). Affiliations: Otto von Guericke University Magdeburg (Chair Electric Power Systems and Renewable Energy Sources); Fraunhofer Institute for Factory Operation and Automation IFF, Magdeburg; 50Hertz Transmission GmbH, Neuenhagen bei Berlin (the TSO providing the operational data). Funded by German Federal Ministry for Economic Affairs and Energy, grant 0350027.

**Methodological note:** This is an **industrial-academic collaboration with a German TSO**, not a pure academic paper. The operational constraints described (stability, comprehensibility, recalibration tolerance) reflect production-system requirements. The paper is in L5's reference list per the user, which is consistent with L5's interest in real-world transmission-scale operational forecasting (50Hertz is the German equivalent in scale and operational role to MISO and ERCOT).

### PhD-level defense

The paper proposes the **Integrated Load, generation and Storage Forecast (ILEP)** concept — a three-component system for German TSOs that produces *substation-specific* forecasts of (i) renewable energy generation, (ii) load, and (iii) the impact of distributed household battery storage on the residual load. The motivating problem is that German distributed PV (~1.4 million PV systems, 12.2 GW installed in the 50Hertz control area) and the recent surge in household battery storage (~850,000 systems with 600 MWh capacity by end of 2017) operate below the TSO's measurement horizon, so the TSO sees only their aggregate impact at substation transformers and cannot disaggregate generation/load/storage without explicit modeling. The paper's framing — "uncertainty in the estimation of power system load" growing with distributed RE penetration — generalizes directly to ERCOT, which faces a parallel growth in distributed solar and behind-the-meter generation in its residential/commercial zones.

**System scope (Section 2):** The 50Hertz control area contains **56 TSO substations and 942 DSO substations** connected by 1,133 110 kV lines. This is the closest scale-match yet in our literature review to ERCOT's transmission-bus footprint (~15,000 buses across all voltage levels; the high-voltage transmission subset is a closer numerical analog to 56). The paper's spatial allocation methodology — Voronoi catchment areas around each DSO substation, power-flow-based sensitivity weights aᵢ,ₖ for each DSO-to-TSO assignment — formalizes the "static historical share" disaggregation that L25 and L23 use as population/IEEE-base-case weights and that we use as historical-share weights. **Important distinction:** Schröter et al.'s aᵢ,ₖ are *derived from a 1 MW DC-then-Newton-Raphson power flow* (Section 2.3) and represent the fraction of injected power at DSO node *i* that physically flows to TSO substation *k*. This is a more principled weight than population share — it captures actual network topology — but requires the TSO model to do it. The fact that the authors had to do this suggests TSOs currently use cruder shortest-distance allocation, which they explicitly call out as inferior ("more precise than using the shortest distance between units and TSO substations, which is currently applied").

**The generation forecast (Section 3.1) — Combination forecasting via three optimizers in parallel:** The paper extends a prior method ([23], Schröter et al. 2018 PMAPS) by combining three different optimization algorithms — Genetic Algorithm (GA), Particle Swarm Optimization (PSO), and Weighted Least Squares (WLS) — each producing its own combination of three external provider forecasts (Pro1, Pro2, Pro3). The three resulting combination forecasts are then themselves combined by RMSE-weighting based on the previous day's forecast quality. The architecture is unusual: rather than picking the best optimizer, run all three and re-combine. The reported RMSE improvements over the best individual provider are modest: **0.45% to 2.72% (wind), 0.42% to 2.48% (PV)** across five substations (Table 5/6). These are small absolute gains but consistently positive across all five substations and both RE types.

The CoF (Combination Forecast) framing is mathematically:

> p_c = P_pro · w

where P_pro is a matrix (columns = providers, rows = time-steps) and w is the optimal weight vector. The loss function for the optimizers is RMSE normalized by installed capacity (Equation 8):

> RMSE_i = √[(1/N) Σ_t (p_pre,t · w_i − p_act,t)²] / P_installed

The provider weights w are recalibrated *daily* using historical provider time-series over the prior three months (the paper notes empirically that three months is the optimal training window — shorter is too noisy, longer is too stale).

**The load forecast (Section 3.2) — Neural Networks + Multiple Linear Regression, statically combined:**

The load forecaster uses two parallel algorithms, deliberately chosen for the production-system constraints of "criticality of subsequent processes" and "stable and simple calculation methods" (Section 3.2.2, direct quote). **This is the same operational philosophy that motivates our LightGBM choice over deep learning.** The two algorithms are:

1. **Neural network** (architecture not specified beyond "neural network"; likely shallow MLP based on context)
2. **Multiple linear regression** with substation-specific weather features and day-type categorical features

Both algorithms are wrapped in a **96-models-per-day daily-pattern architecture**: every quarter-hour (4 × 24 = 96 timestamps) is treated as a separate model with its own calibration. The motivation is to handle "transition period from winter to summer" where dark-hour boundaries shift sharply — a single model with global-radiation inputs produces unwanted peaks during sunrise/sunset, which the daily-pattern decomposition avoids by isolating each quarter-hour's behavior. **Parallel to our problem:** the daily-pattern model is conceptually equivalent to using hour-of-day as a categorical feature in a single global model with sufficient interaction capacity — which is what our LightGBM-with-hour-of-day-feature pipeline does implicitly. Schröter et al. essentially had to brute-force this because their NN and OLS architectures could not represent hour-conditional response curves cleanly.

The NN-OLS combination is **static**, weighted by historical performance over the last three months. The paper acknowledges this is suboptimal — "a dynamic combination is currently not feasible (for technical reasons) since the forecasts must be calculated in a stable and comprehensible manner." This is a transparent acknowledgment of practitioner constraints overriding methodological optimality — a parallel to our "LightGBM is appropriate for assignment scale" framing.

**Weather correlation analysis (Section 3.2.1) — the headline empirical finding for our work:** The authors ran a Pearson correlation analysis between substation-specific interpolated weather data (global radiation, wind speed, temperature) and historical total substation load, over 6 months on weekly windows, with significance threshold 5% (95% CI). The Pearson coefficients reported, across the 56 substations:

- **Temperature: r = 0.016 to 0.498** (31-fold range)
- **Global radiation: r = 0.064 to 0.609** (10-fold range)
- **Wind speed: r = 0.0152 to 0.8191** (54-fold range)

**This is the most consequential finding for our literature review.** It demonstrates empirically, at *transmission-substation scale in a real grid* (not synthetic, not LV-feeder, not residential meter), that weather-load correlations are extremely substation-specific. Some substations have temperature correlations indistinguishable from zero (r = 0.016) while others are moderate (r = 0.498). For ERCOT bus-level forecasting, this implies that even if we had weather data we would need *bus-specific* weather joins — a uniform "ERCOT-wide weather feature" would be misleading because the per-bus correlation would average across high-r and near-zero-r buses, diluting the signal. The 50Hertz dataset and the L24 ERCOT-zone analysis tell a consistent story: weather-load coupling is heterogeneous across both space (substations) and zones, and the heterogeneity is the dominant signal, not the average.

**Load forecast accuracy results (Section 4.2.1):** RMSE values are reported in *absolute MW*, not as percentages:
- **Neural network: RMSE 20–250 MW** across substations and weeks
- **Regression: RMSE 17–239 MW** across substations and weeks
- **Static combination: best on most days, regression best on some calendar events, NN best on others**

This is a serious calibration issue for cross-paper comparison: without normalization to substation maximum load (which Table 4 provides: 0.00 GW to 0.74 GW max load), we cannot compute MAPE-equivalents. Doing it manually:

- Wolmirstedt (max load 740 MW): RMSE 17–239 MW → roughly 2.3–32% normalized error
- Dresden-Sued (max load 690 MW): similar range
- Wuhlheide (max load 0 GW per Table 4): RMSE 20–250 MW is nonsensical against zero max-load — likely the table is missing data or the substation is generation-dominated

The wide range (2% to 32% normalized) suggests substantial substation-to-substation variance in forecast difficulty — directly relevant to our Q4 scaling-law diagnostic — but the paper does not stratify or fit a scaling law to it. **Missed opportunity for the authors; reportable framing for us.**

**The storage forecast (Section 3.3) — wrong problem for us, briefly noted:** The storage component models battery state-of-charge evolution under maximize-self-consumption charging strategy, using statistical assumptions (battery-to-multi-person-household ratios, SoC limits 0.2-0.98, inverter-capacity = battery-capacity = rated-power 1:1:1 ratio). Two relevant-load concepts are tested: (Concept 1) standardized load profiles times statistical household counts; (Concept 2) the substation-specific load forecast times federal-state household share. Concept 1 produces visually realistic storage behavior; Concept 2 produces a load so high that PV never exceeds it and the battery never charges, indicating the load forecast cannot be cleanly split into "household-relevant" subcomponents without measurement data the TSO does not have. **TRL self-assessment (Section 6):** the storage forecast is TRL 3 ("experimental proof of concept"), the load forecast is TRL 5, the generation forecast is the most mature. We do not have a directly analogous storage-disaggregation problem.

**System integration (Section 5):** A REST-API-mediated Python/MATLAB hybrid (MATLAB algorithms + Python orchestration + PostgreSQL/PostGIS storage + Flask/Dash visualization). Reads like a real industrial demonstrator — not directly transferable to our notebook pipeline but useful as a sanity check on the kinds of architectures TSOs actually run.

### Brutal relevance assessment: 5/10

**Reasons for 5:**
- **Closest scale-match to date in the literature review** for our problem. 56 transmission substations is the same order of magnitude as ERCOT's transmission-bus footprint; the load levels (max 740 MW = ~0.74 GW per substation) are comparable to mid-size ERCOT zones; the operational role (day-ahead congestion forecasting / DACF, intraday congestion forecasting / IDCF) is the same kind of decision-support our forecast feeds into hypothetically.
- **The substation-specific weather correlation table is empirically unique** — no other paper in our review reports per-substation Pearson coefficients spanning two orders of magnitude on a real transmission grid. Strengthens the no-weather defense in a *new direction*: not "weather doesn't matter," but "weather effects are so spatially heterogeneous that a uniform weather feature is potentially misleading."
- **Direct practitioner-philosophy parallel to our LightGBM choice:** explicit articulation that operational forecasting prioritizes stability and comprehensibility over methodological optimality, with dynamic combination explicitly rejected for stability reasons. This is what we've been claiming about LightGBM at 15,000 buses; Schröter et al. provide the same argument in the production-TSO context.
- **L34-D (network-reconfiguration mechanism):** the "Further Improvement Possibilities" sections (4.1.2 and 4.2.1) explicitly note that switching operations in subordinate networks shift generation/load between substations without TSO knowledge — same physical mechanism as L30-A (anticorrelated bus errors from unobserved topology changes). Two independent TSO-context citations now corroborate this as a real published source of forecast error at bus/substation scale.

**Reasons not higher than 5:**
- **No MAPE reported.** RMSE in absolute MW makes cross-paper comparison and scaling-law fitting nearly impossible from the published figures alone. The Q4 diagnostic we'd want to run on this data — fit α₀/Wᵖ + α₁ — would require recomputing per-substation MAPE from the raw Table 5 RMSE and Table 4 load values, and the paper does not provide enough granularity.
- **Methodology of the load forecast is barely described.** "Neural network" with no architecture, multiple linear regression with unspecified feature set, 96 daily-pattern sub-models, static combination via OLS over 3 months — none of this gives us architectural transfer to LightGBM.
- **Storage forecast component is wrong-problem.** We have no battery data, and household-battery disaggregation is not part of Assignment 2 scope.
- **German distribution context differs from ERCOT.** 50Hertz has very high penetration of distributed PV and household battery, with strong policy framing (EEG, RE-Marketing, redispatch). ERCOT has different industrial composition, less rooftop PV, no household battery program at comparable scale.
- **Combination-of-providers framing not transferable to our pipeline.** We have one model (LightGBM), not three external forecast vendors to combine.

**Anchor stack placement:** L34 sits in the same 5–6/10 tier as L20-B (DWT preprocessing for imputation), L30 (BLDF + DEKFNN substation), and L31 (Haben LV review). Tier label: "transferable diagnostic finding + practitioner-philosophy support, no direct method or architecture transfer."

### Insight labels

- **L34-A (weather correlation heterogeneity at transmission-substation scale):** Pearson r ranges 0.016–0.498 (temperature), 0.064–0.609 (global radiation), 0.0152–0.819 (wind speed) across 56 German TSO substations. Empirical evidence that weather-load coupling is highly substation-specific and that a uniform weather feature would dilute signal across high-r and near-zero-r substations. **Strengthens the deliberately-not-asked-weather defense in a new direction** (heterogeneity argument, distinct from the absence-doesn't-hurt argument of L24-A / L28-C / L31-B / L32-D).
- **L34-B (NN + OLS static combination, no overall winner):** Neither neural network nor multiple linear regression dominates; static OLS combination is best on most calendar days, with each individual algorithm winning on specific calendar events. **Mild Q1 corroboration**: even within the more-flexible-vs-less-flexible model family axis, no single architecture dominates at operational TSO scale, and combination/ensembling can match the best individual on average.
- **L34-C (practitioner-philosophy citation for simple-and-stable):** Direct quote, Section 3.2.2: "Due to the criticality of the subsequent processes, stable and simple calculation methods are primarily used for system management." Direct quote, Section 3.2.2: "A dynamic combination is currently not feasible (for technical reasons) since the forecasts must be calculated in a stable and comprehensible manner." **Citable alongside L12 (Shiblee & Koukaras LightGBM defense)** in justifying LightGBM-over-deep-learning for our pipeline, with the production-TSO framing.
- **L34-D (network-reconfiguration as forecast-error source):** Section 4.1.2: "There are switching operations in the subordinate network from time to time that are not communicated to the transmission system operator. These switching operations lead to a shift in the generation between substations and must be considered in future analyses." **Same physical mechanism as L30-A (Sun et al. 2013 anticorrelated bus errors)**. Two independent peer-reviewed sources now identify this mechanism. Useful as supporting citation for Q5's bus-summed-vs-zone-direct framing.
- **L34-E (power-flow-based sensitivity weights for spatial allocation):** Section 2.3 derives static aᵢ,ₖ weights from a 1 MW injection power flow per DSO-to-TSO node. Conceptually more principled than population or shortest-distance weights, but requires the TSO's network model. **Architecturally analogous to our historical-share approach** for bus disaggregation, with the substitution that historical kWh share (data-driven) replaces simulated power-flow share (model-driven). Worth a brief note in the report's Q1 methodology section that we are using the empirical/data-driven version of the spatial-allocation problem.
- **L34-F (RMSE-only reporting limitation):** Paper reports RMSE in absolute MW without normalization to substation maximum load, making cross-paper MAPE comparison difficult. **Reportable framing observation for our report's evaluation-metrics section** — we should normalize all error metrics to bus capacity or mean load, and explicitly note that absolute RMSE without normalization is unsuitable for cross-bus comparison given our Q4 finding (which extends L29 to ERCOT) that bus error scales with bus capacity.
- **L34-G (substation-scale forecast accuracy variance, undiagnosed by paper):** RMSE 17–250 MW across 56 substations with max-load range 0–740 MW implies normalized error in the ~2%–~32% range — comparable to or higher than the per-bus variance L29's PG&E data showed. The paper does not stratify or fit a scaling law. **Q4 framing reinforcement** that the per-substation error variance is a universal phenomenon at this scale, observable in any sufficiently-large substation dataset, and that L29's scaling-law diagnostic is generically applicable.

### Question-sheet pass

- **Q1 (direct bus-level vs zone-disaggregation):** L34 uses a top-down approach (zone/control-area forecast disaggregated to substation via power-flow-derived static weights aᵢ,ₖ) — opposite architectural choice from L5. The paper does not test direct substation forecasting against this disaggregation, so it does not directly inform Q1's empirical question, but it does add **L34-E** as a third static-share variant (alongside L25's population-weighted and L30's BLDF) in the Q1 method-specification paragraph. Light add to Q1 framing.

- **Q2 (naive baseline):** No comparison to a naive baseline. The paper compares against three external provider forecasts (which themselves are not naive baselines, so this does not inform Q2). No change.

- **Q3a (FWES/NOTH structural growth):** No direct evidence — 50Hertz is a different grid. However, **L34-A's substation-specific weather correlation heterogeneity** is consistent with our framing that FWES's near-zero temperature correlation (L24-A) is part of a broader pattern of zone/substation-specific weather coupling — Schröter et al. document the same heterogeneity at the German transmission-substation scale. Tangential support for the framing, no change to question structure.

- **Q3b (rolling vs full-history retraining):** The paper uses *three-month sliding windows* for both the generation-forecast combination weights and the load-forecast static combination weights, with the empirical observation that three months is the sweet spot ("shorter is too noisy, longer is too stale"). This is a production-system empirical data point: **at transmission-substation scale, a 3-month sliding window outperforms full-history calibration for combination weights.** Tangentially supports Q3b's rolling-window hypothesis, but the architectural setting is different (combination weights, not the model itself). No formal Q3b update — too indirect — but worth noting in the report's methodological discussion if Q3b becomes a primary result.

- **Q4 (scaling law diagnostic):** **L34-G reinforces the universality framing.** RMSE 17–250 MW across substations with max-load 0–740 MW implies per-substation normalized error variance comparable to L29's PG&E and L31-A's UK LV-feeder findings. Schröter et al. did not fit a scaling law, but their raw data is consistent with one. **L34-A adds a methodological framing note:** any per-substation error variance is potentially explained by *substation-specific weather coupling heterogeneity*, not just by aggregation level. This means the Q4 scaling-law diagnostic should be interpreted carefully — large α₁ (irreducible error) at small buses may reflect uncaptured weather-coupling heterogeneity, not pure stochasticity. Frames a robustness note for the Q4 report: "we expect some residual scatter around the fitted scaling law due to substation-specific factors not captured in our pipeline (weather coupling heterogeneity per L34-A, industrial dominance per L30-B)."

- **Q5 (zone-direct vs bus-summed):** **L34-D adds a second independent citation** (alongside L30-A) for the network-reconfiguration mechanism. Two peer-reviewed papers (Sun et al. 2013 IEEE PES; Schröter et al. 2020 *Energies*) now document that unobserved subordinate-network switching operations shift generation/load between substations, creating bus-level errors that partially cancel when summed. This is now a **two-piece anchor stack** for Q5's bus-summed-coherence mechanism, strengthening the framing that bus-summed-matches-zone-direct is plausible and physically motivated.

- **Q6 (sector type as feature):** No direct relevance. 50Hertz substations are classified by city/country (Wuhlheide is urban Berlin; Altenfeld and Klostermansfeld are rural) but this is not the same as sector-type clustering from diurnal load shape. No update.

- **Q7 (imputation method choice):** No imputation discussion in L34. The paper assumes complete provider forecast data and complete substation measurements; missing data is not a methodological concern. No update.

- **Deliberately-not-asked weather features:** **L34-A becomes the sixth piece** in the no-weather evidence cluster. Worth a separate framing because L34-A is a different *type* of evidence than the prior five — not "weather doesn't help on this dataset" (L24-A, L28-C, L31-B, L32-D), and not "weather absence is empirically normal" (L21), but rather "weather-load coupling is so substation-specific that a uniform weather feature would dilute signal across heterogeneous coupling regimes." **This argument is specific to bus/substation-level forecasting** and is the most directly applicable to our problem.

- **Deliberately-not-asked deep learning (LightGBM defense):** **L34-C strengthens the practitioner-pragmatism argument.** Now cited alongside L12 (Shiblee & Koukaras) as the second direct industrial-philosophy citation for stable-and-simple operational forecasting.

- **Primary backlog F1 (lag features, hour-of-day, day-of-week):** L34's 96-models-per-day daily-pattern architecture implicitly confirms that **hour-of-day is a dominant interaction variable for load forecasting**, since their NN and OLS architectures required this brute-force decomposition. Mild reinforcement of F1's hour-of-day-as-feature inclusion.

**Q-sheet changes summary:** Q1 (L34-E added to disaggregation-variant paragraph), Q5 (L34-D added as second network-reconfiguration mechanism citation alongside L30-A), deliberately-not-asked weather (sixth-piece L34-A heterogeneity argument), deliberately-not-asked deep learning (L34-C practitioner-philosophy reinforcement alongside L12).


## L35 — Wang, Majumdar & Rajagopal (2023) — Geospatial mapping of distribution grid with ML and publicly-accessible multi-modal data (wrong domain)

**Citation:** Wang, Z., Majumdar, A., Rajagopal, R. (2023). "Geospatial mapping of distribution grid with machine learning and publicly-accessible multi-modal data." *Nature Communications* 14:5006. DOI: 10.1038/s41467-023-39647-3. Open access (CC-BY). Affiliations: Stanford University, Departments of Civil & Environmental Engineering, Electrical Engineering, Mechanical Engineering, and Energy Science & Engineering. Funded by U.S. DOE EERE Solar Energy Technologies Office (DE-EE0009359) and Stanford Precourt Pioneering Project. **Critical observation:** R. Rajagopal is senior author here AND on L29 (Sevlian & Rajagopal 2018, our Q4 anchor) AND on L5 (Triebe, Laptev & Rajagopal 2025, our architectural anchor). Rajagopal's Stanford lab is now established as a major citation hub in our review.

### PhD-level defense (compressed)

The paper builds a **computer-vision pipeline** for inferring the geospatial location of distribution-grid utility poles and power lines from publicly-available data — Google Street View upward-facing images, OpenStreetMap road networks, Microsoft US Building Footprints. The motivation is wildfire risk assessment (Dixie Fire 2021 caused by PG&E distribution lines contacting trees) and electrification expansion in Sub-Saharan Africa, where distribution grid maps are not publicly available or do not exist. There is no time-series forecasting content, no MAPE or RMSE on load, no electrical load data at all.

**Methodology:** Two CNNs (Inception-v3 backbone) are trained weakly-supervised on 10,000 hand-labeled upward Street View images — one classifier detects whether the image contains utility poles, the other whether it contains power lines. Class Activation Maps + Hough transform extract pole orientations and line directions. Pole geolocations are recovered by intersecting orientation rays from multiple street view vantage points. A gradient-boosting link prediction model decides which poles are connected (features: image-based line detections, road co-location, modified Dijkstra path-output). Underground grids are predicted by dilating the overhead-grid map by 70m, identifying uncovered buildings (under the 100%-electrification assumption), and running modified Dijkstra path-finding along roads to greedily connect them.

**Results:** Pole localization F1 = 0.83 (California) / 0.75 (SSA, with the detection threshold lowered to 0.2 due to shorter African poles). Link prediction F1 = 0.77 (California) / 0.72 (SSA). Overall grid mapping F1 = 0.84 (California) / 0.85 (SSA, overhead-only). Census-block-group undergrounding-rate prediction R² = 0.627. The model transfers from California to Uganda/Kenya/Nigeria without re-training.

### Brutal relevance assessment: 1/10

This is a **computer-vision-on-imagery paper for distribution-grid topology inference**. We are doing **time-series forecasting of transmission-bus load with LightGBM**. The intersection between the two problems is zero on every axis:

- **No load data.** No time-series. No forecast horizon. No MAPE/RMSE/WMAPE.
- **Distribution grid only** (12 kV class, residential streets) — three voltage levels below ERCOT's 138/345 kV transmission buses.
- **Static geospatial outputs**, not temporal predictions. The "graph" output is a topology, not a forecast.
- **Wrong method family.** CNN computer vision + Hough transform + Dijkstra path-finding vs. our gradient boosting on lag features.
- **The word "buses" appears once in the paper, dismissively** — in the literature review, characterizing prior graph-based topology-inference work that requires smart-meter time-series at nodes. The paper explicitly positions itself *away from* time-series-based methods.

No transferable insight to any of Q1–Q7. Logged for completeness because the paper appears in L5's reference list (per user statement) — most likely as a Rajagopal-lab self-citation or as a "why distribution-grid information matters for renewable integration" framing reference in L5's introduction, **not as a methodological source**. Third background-domain paper in the 35-paper review after L26 (WECC CLM) and L33 (Gielen IRENA).

### Insight labels

- **L35-A (Rajagopal-lab citation-hub observation, not a methodological insight):** Ram Rajagopal is senior author of three of our most consequential entries — L5 (10/10 architectural anchor, Triebe-Laptev-Rajagopal 2025), L29 (9/10 Q4 scaling-law anchor, Sevlian-Rajagopal 2018), and now L35 (1/10 geospatial mapping, Wang-Majumdar-Rajagopal 2023). Three Rajagopal-lab papers across three different problem domains (forecasting architecture, scaling laws, geospatial CV) now appear in our review. This contextualizes L5's intellectual lineage — Triebe et al. cite their own lab's foundational scaling-law work (L29) and their own lab's broader infrastructure-mapping portfolio (L35). Not a citable observation for the report, but useful for understanding why L5's reference list looks the way it does.
- **L35-B (wildfire-risk framing for distribution-grid mapping):** The 2021 Dixie Fire — caused by PG&E distribution lines contacting a tree — and the Edison Electric Institute undergrounding cost-benefit analysis (cited as Ref 32) provide the policy motivation. If our literature review ever needs to discuss why high-resolution distribution-grid information matters beyond load forecasting, this is the citation. Tangential to our problem; possibly relevant to the "broader impact" section of a thesis-level write-up but not to Assignment 2.

### Question-sheet pass (compressed)

Walked all 8 questions: **no Q-sheet change.** The paper does not address load forecasting at any spatial scale, any temporal horizon, or any method family relevant to our pipeline. Q1, Q2, Q3a, Q3b, Q4, Q5, Q6, Q7: all unaffected. Deliberately-not-asked weather: unaffected (paper does not use weather features). Deliberately-not-asked deep learning: unaffected (CNN for image classification is not the same problem as DL for load forecasting, and the paper does not benchmark against LightGBM-equivalent methods).

**Q-sheet change:** None.

*Cross-reference:* Tagged in the reference tracker for L5-bibliography accounting. Its presence in L5's reference list — together with L33 (Gielen IRENA) — confirms that L5 cites both methodological and motivational/framing references, which is normal for a Nature-tier or top-venue paper. Logged for completeness.


## L36 — Chen, Li, Chen & Bai (2025) — Day-ahead bus load forecasting via fully connected spatial-temporal graph attention network (FC-STGAT)

**Citation:** Chen, Y., Li, B., Chen, B., Bai, X. (2025). "Day-ahead bus load forecasting method based on fully connected spatial-temporal graph attention network." *Electric Power Systems Research* 241, 111294. DOI: 10.1016/j.epsr.2024.111294. Affiliations: School of Electrical Engineering and Guangxi Key Laboratory of Power System Optimization and Energy Technology, Guangxi University, Nanning, China. Same venue as L19 (Wei et al.) and L24 (Safdarian et al.).

### PhD-level defense (compressed)

The paper proposes **FC-STGAT**, a novel GNN architecture for day-ahead bus load forecasting that jointly models spatial and temporal correlations rather than capturing them separately. The methodological innovation is the **Fully Connected Spatial-Temporal Graph (FC-STG)** — a graph where each bus-timestamp pair is a node, edges within a timestamp follow the supply area's single-line diagram (the actual electrical topology, not a learned adjacency), and edges across adjacent timestamps connect the same bus at *t* and *t−1*. This allows GNN message-passing to flow simultaneously in space and time, addressing the criticism (raised against L18, L20, and similar) that separating spatial GCN from temporal RNN/TCN modules fails to capture mutually-coupled ST dynamics.

The architecture (FC-STGAT) consists of stacked Spatial-Temporal Attention (STA) blocks, each containing a Local Graph Attention (LGA) module — gated GAM layers operating on the L-hop neighborhood — and a Global Graph Attention (GGA) module using ProbSparse Self-Attention from Informer (Zhou et al. 2021) to alleviate the O(N²H²) memory cost of full self-attention across all bus-timestamp pairs. Outputs are concatenated and fed to an MLP for the prediction window. Look-back window H=144 timestamps (3 days at 30-min resolution), prediction window T=48 (1 day ahead).

**Dataset:** Essential Energy (Australia), Terranora supply area (7 buses) and Temora supply area (9 buses), 730 days of 30-min interval data from 2021-09-30 to 2023-09-30. Total 35,040 samples per bus. 8:1:1 train/val/test split. Missing-data handling via KNNImputer (for buses with <200 missing values) or bus deletion (for buses with ≥200 missing values).

**Critical empirical finding (the headline for our Q-sheet):** Pearson correlation analysis between bus loads and 6 meteorological features (U10, V10, T2m, SSR, SP, SSRD). Mean absolute PCC across 7 buses in Terranora supply area: T2m = 0.29 (highest), SSR = 0.12, SSRD = 0.12, U10 = 0.08, SP = 0.06, V10 = 0.04. **Maximum individual PCC across all bus-weather pairs: r = 0.46 (T2m with bus 2).** In contrast, PCC between loads on buses *within the same supply area*: mean ≥ 0.6 on most buses, exceeding 0.8 between many bus pairs in Terranora. Their Table 4 reports per-bus mean absolute PCC vs. other buses: 0.70, 0.35, 0.72, 0.72, 0.69, 0.69, 0.68 (Terranora), 0.70, 0.71, 0.71, 0.73, 0.72, 0.67, 0.73, 0.48, 0.58 (Temora). Direct quote: *"the correlation between a bus and the remaining buses within the same supply area is far more significant than its correlation with meteorological features."*

**Forecasting results:** FC-STGAT achieves the best result on 12 of 14 metrics in Terranora and 16 of 18 in Temora. Mean MAE 1.145 MW (Terranora), 0.499 MW (Temora). Mean RMSE 1.828 MW (Terranora), 0.771 MW (Temora). **TCN is consistently 2nd-place** — 6.5% worse mean MAE than FC-STGAT on Terranora, 1.8% worse on Temora. Transformer and Informer are significantly worse: Transformer's mean RMSE is 54.7% worse than FC-STGAT (Terranora). FEDformer (frequency-decomposed transformer) is middle-of-pack. FC-STGNN (also fully-connected ST graph but different mechanism) is the 3rd-best overall, validating the simultaneous-ST-modeling framing as the architectural lever. Ablation study confirms LGA contributes 5.4% MAE improvement and GGA contributes 3.3%.

### Brutal relevance assessment: 6/10

**Reasons for 6:**
- **L36-A (Pearson correlation table — most consequential finding for us):** Mean(|PCC|) of bus-to-weather peaks at 0.29 (T2m) while bus-to-bus PCC within the same supply area routinely exceeds 0.6-0.8. **This is independent corroboration of L34-A's heterogeneity finding from a different country (Australia vs Germany), different grid type (sub-transmission vs transmission), different supply-area scale (7-9 buses vs 56)**, AND adds a new explicit claim: bus-to-bus load correlation dominates bus-to-weather correlation. This is the strongest single piece of evidence in our entire no-weather defense because it directly states the comparison.
- **L36-B (TCN beats Transformer/Informer, 2nd-place to GNN):** TCN's mean RMSE is competitive with FC-STGAT (only 4.6% worse on Terranora, 6.4% worse on Temora) while Transformer/Informer are 50%+ worse. Reinforces the "simple architectures hold up against attention" narrative shared with L12 (LightGBM defense).
- **L36-C (real bus-level data with actual electrical topology, day-ahead horizon, 30-min resolution):** Closest match to our problem in the entire batch for *operational setting* — they use the supply area's single-line diagram as the spatial adjacency, day-ahead horizon (T=48 timestamps = 24h), and 30-min resolution comparable to our hourly. Their look-back window H=144 (3 days) is a methodological data point for our lag-feature design (F1).
- **L36-D (FC-STGNN as 3rd-place validates simultaneous-ST principle):** Independent architecture (Wang et al. 2024 AAAI) also using fully-connected ST graphs achieves 3rd-best mean MAE, separately confirming that the architectural choice matters more than the specific attention variant.

**Reasons not higher than 6:**
- **Wrong method family.** GNN+attention vs our LightGBM. The architectural insight (FC-STG with same-supply-area spatial edges + same-bus temporal edges) is **not directly transferable to LightGBM** — gradient boosting doesn't operate on graph structures. We could potentially feature-engineer neighboring-bus loads as inputs (which is what FC-STG implicitly does), but that's a generic feature-engineering decision, not an architectural transfer.
- **Wrong scale.** 7-9 buses vs our 15,000 buses. FC-STGAT trains separately per supply area; scaling to 15,000 buses would require either (a) supply-area decomposition or (b) global-model training, neither tested in the paper.
- **No comparison to gradient boosting.** Their 10 baselines include SVR, TCN, LSTM, Transformer, Informer, FEDformer, T-GCN, FC-STGNN, GraphWaveNet, PMLP — but no LightGBM, XGBoost, or CatBoost. We cannot infer from this paper how FC-STGAT would compare to our chosen method.
- **No weather sensitivity analysis.** They include T2m, SSR, SSRD as inputs (selected by PCC) but do not run a with-vs-without-weather ablation. The "weather is dominated by bus-to-bus correlation" observation is descriptive, not causal — they don't actually test whether removing weather features changes their forecasts.

### Insight labels

- **L36-A (bus-to-bus correlation dominates bus-to-weather correlation):** Mean |PCC| of bus loads with weather features peaks at 0.29 (T2m), maximum individual r = 0.46. Mean |PCC| of bus loads with other buses in the same supply area routinely exceeds 0.6-0.8. **Direct independent corroboration of L34-A from a different country and grid scale**, with the additional explicit claim that intra-supply-area correlation dominates weather correlation.
- **L36-B (TCN beats Transformer/Informer, near-ties GNN at supply-area scale):** TCN ranks 2nd of 10 baselines, only 4.6%-6.4% worse RMSE than FC-STGAT. Transformer/Informer are 50%+ worse. Reinforces L12 + L34-C + L5's anti-DL framing.
- **L36-C (supply-area-bounded spatial graph as the architectural lever):** The FC-STG architecture's improvement over separate spatial-then-temporal GNN comes from joint ST message-passing within a supply area, not from any specific attention variant. FC-STGNN (different paper, different attention) achieves 3rd-best, confirming the principle is the architecture-family lever.
- **L36-D (independent confirmation of the "simultaneous ST modeling" principle from L5):** Like L5 (Triebe et al.) and L20 (Zhao et al.), L36 argues that separate spatial and temporal modules are inferior to joint ST modeling. Three independent papers (L5, L20, L36) now make the same architectural argument.

### Question-sheet pass (compressed)

- **Q1:** No direct evidence on top-down disaggregation vs direct bus forecasting (L36 uses neither — it uses joint ST graph modeling). However, **L36's FC-STG framework is structurally analogous to L5's Grouped-Global-Bus** in the sense that both share information across buses within a defined group (L5: ERCOT zones via grouped global model; L36: supply area via FC-STG message-passing). Mild framing reinforcement of Q1's architectural narrative but no Q-sheet change.
- **Q2:** No naive baseline tested. No change.
- **Q3a:** Not relevant (Australian supply-area data, not ERCOT zone-level). No change.
- **Q3b:** Not relevant (no rolling-window experiment). No change.
- **Q4:** L36 does not stratify error by bus capacity or fit a scaling law. The reported MAE values (range 0.022 MW to 3.39 MW across 16 buses) span 2 orders of magnitude, consistent with L29's scaling law expectation, but the paper does not analyze this. No formal Q-sheet change, but **the wide MAE range across buses in their Tables 5-6 is consistent with L29 + L31's scaling-law predictions** and provides another data point that the scaling-law variance is universal at bus scale.
- **Q5:** No bus-summed vs zone-direct comparison. No change.
- **Q6:** No cluster-as-feature experiment. Buses are not clustered by sector type. No change.
- **Q7:** No imputation-method ablation. They use KNNImputer for buses with <200 missing values, drop buses with ≥200 missing values. The 200-value threshold (out of 35,040 = 0.57%) is a defensible practitioner choice but not formally evaluated. No change.
- **Deliberately-not-asked weather:** **L36-A added as the 7th piece in the evidence cluster — and arguably the strongest single piece because it directly states the dominance comparison.** L36-A is methodologically stronger than L34-A because Chen et al. compute the PCC for both axes (bus-to-weather AND bus-to-bus) on the same dataset and directly compare them, whereas L34 reports only bus-to-weather. Will update BIG_QUESTIONS.md accordingly.
- **Deliberately-not-asked deep learning:** **L36-B added as additional reinforcement** — TCN being near-tied with the GNN winner and substantially beating Transformer/Informer is consistent with the simple-method-defense narrative. Three citations now (L12, L34-C, L36-B) plus the Grinsztajn et al. tabular-DL paper cited via L5. Will update BIG_QUESTIONS.md accordingly.

**Q-sheet changes summary:** Deliberately-not-asked weather upgraded from 6-piece to 7-piece evidence cluster (L36-A added as the most direct independent corroboration of L34-A). Deliberately-not-asked deep learning expanded to include L36-B (TCN near-ties GNN, beats attention).


## L37 — Ferreira, Leite & Salvadeo (2025) — Power substation load forecasting using interpretable Temporal Fusion Transformer (TFT)

**Citation:** Ferreira, A.B.A., Leite, J.B., Salvadeo, D.H.P. (2025). "Power substation load forecasting using interpretable transformer-based temporal fusion neural networks." *Electric Power Systems Research* 238, 111169. DOI: 10.1016/j.epsr.2024.111169. Affiliations: Department of Electrical Engineering, UNESP (São Paulo State University), Ilha Solteira, Brazil; Institute of Geosciences and Exact Sciences, UNESP Rio Claro. Funded by CAPES (Brazilian higher-education funding agency). Same venue as L19 (Wei et al.), L24 (Safdarian et al.), and L36 (Chen et al.) — *Electric Power Systems Research* is clearly a hub journal for this literature.

### PhD-level defense (compressed)

The paper applies the **Temporal Fusion Transformer (TFT)** architecture, proposed by Lim, Arık, Loeff & Pfister (2021) at Google DeepMind, to substation load forecasting in the Waikato region of New Zealand. The TFT is one of the most architecturally sophisticated time-series forecasting models in the modern attention-based family: it combines (a) variable selection networks that learn per-timestep importance weights for each input feature, (b) gating mechanisms (GLU + GRN) that skip non-linear processing when not needed, (c) static covariate encoders for time-invariant metadata, (d) an LSTM encoder-decoder Seq2Seq block for local temporal patterns, and (e) interpretable multi-head attention for long-range dependencies. The architecture's claimed advantages over plain Transformer/Informer are **interpretability** (variable selection weights and attention weights can be inspected post-hoc) and **multi-horizon point + quantile forecasting** capability — although Ferreira et al. only validate point forecasts in this work.

**Dataset:** Aggregated electrical load of New Zealand Electrical Company subsystem in Waikato region — 2 thermoelectric stations + 7 substations (including 1 hybrid hydro+thermo substation), half-hourly resolution, January 2007 to March 2009 (=39,408 samples). This is a small operational subsystem dataset, **not bus-level data**: the target variable is the aggregated load of the entire subsystem, not individual substation or bus loads. Comparable historically to other published methods on the same dataset (Euclidean ARTMAP, Fuzzy ARTMAP, FAM-ANN — all from prior UNESP group work).

**Forecasting setup:** Two horizons tested — 24-hour (predictive window T=48, look-back H=336) and 48-hour (T=96, H=672). Exogenous covariates include hour, day-of-year, day-of-week, month, year, holidays, atypical days, daylight saving time, min temperature, max temperature, plus engineered avg/min/max load features over half-hour windows.

**Results:** MAPE = 0.94% (24h forecast), 1.47% (48h forecast). RMSE = 3,222 MW (24h), 4,533 MW (48h) on a system with peak load ~310,000 MW (so relative RMSE ~1.0% and ~1.5%). Comparison to prior literature on the same dataset (Table 5): TFT 0.94% MAPE vs. Euclidean ARTMAP 2.12%, Fuzzy ARTMAP 2.89%, FAM-ANN 2.91% — substantial improvement over the prior generation of ANN-based methods. **No comparison to LightGBM, gradient boosting, or any modern tabular ML method.** **No comparison to other transformer variants (Informer, FEDformer, etc.) directly tested on this dataset within this paper** — only an indirect reference to other studies (Nazir 2023; Huy 2022; Lopez Santos 2022) showing TFT beats LSTM, TCN, XGBoost, ARIMA, MLP on different datasets.

**The interpretability findings (variable importance via Variable Selection Networks):**
- **Encoder (past inputs):** Top features by importance are **hour (17.5%)**, **energy_demand (~16%)**, **tMax (max temperature, ~11%)**, **day_week (~10%)**, **relative_time_idx (~8%)**, **idx (~8%)**, **tMin (min temperature, ~7%)**, **cMin (min load, ~7%)**, **holiday (~6%)**, **atypical_days (~6%)**, **daylight_saving (~2%)**, **cMed (avg load, ~1%)**.
- **Decoder (known future):** **day_week (~24%)** > **hour (~22%)** > **holiday (~18%)** > **idx (~13%)** > **daylight_saving (~10%)** > **atypical_days (~7%)** > **relative_time_idx (~6%)**.
- Attention weight analysis on the 168-hour (7-day) look-back window shows daily peaks (24h-spaced minor peaks) and a strong weekly peak at ~48-hour-back position (Fig. 8). Interpretation: weekly seasonality is the dominant long-range temporal pattern.

**Notable observation: tMax substantially more important than tMin** (~11% vs ~7% in the encoder importance ranking) — direct quote: *"This indicates that, in the context of the main variables observed in the past, periods of greater climatization play a far greater role than periods of milder weather."* The TFT identifies an asymmetric weather effect on load.

### Brutal relevance assessment: 4/10

**Reasons for 4:**
- **L37-A (variable importance ranking confirms F1 lag features + day-of-week + hour-of-day dominate):** Lag features (energy_demand + cMin) and temporal features (hour + day_week) are the top contributors in both encoder and decoder. Consistent with L32-B + L32-C from our prior batch, but on a different dataset (NZ vs Iran/Germany) and with a different method (TFT vs LSTM). 3rd independent confirmation that hour-of-day and day-of-week are the dominant time features, with lag features near-universal. F1 reinforcement.
- **L37-B (tMax > tMin asymmetry in feature importance):** Empirical observation from a third-party dataset that hot extremes matter more than cold extremes for load forecasting. This is at the aggregated-subsystem scale in temperate New Zealand, not bus scale in Texas, but **it is suggestive framing support for our FWES heat-driven industrial load story (Q3a) and for the general asymmetric-temperature-effect literature** (which appears in Hong's load forecasting tutorials). Worth a one-line mention in the report if Q3a becomes a primary result.
- **L37-C (TFT achieves MAPE 0.94% on aggregated NZ subsystem):** Operational benchmark — a sophisticated DL model on a small-system aggregated load achieves <1% MAPE at 24h horizon. This is the **best-published aggregated-system MAPE in our entire literature review**, but on a much smaller, much more aggregated, and much less industrially-heterogeneous system than ERCOT. Not directly comparable to our expected bus-level WMAPE but useful as an upper-bound reference point for what "very good" looks like on a clean, well-aggregated system.
- **L37-D (transformer-with-variable-selection beats plain ANN by 2-3×):** TFT MAPE 0.94% vs. ARTMAP-family MAPE 2.12%-2.91%. Reinforces the L12 framing that *some* modern methods do improve over older ANN baselines — but L12 also shows LightGBM matches or beats deep learning. We do not know from L37 alone whether LightGBM would also reach <1% MAPE on this dataset.

**Reasons not higher than 4:**
- **Wrong scale entirely.** Aggregated subsystem load (peak ~310 GW), not bus-level forecasting. This is the *most* aggregated scale we've seen in any STLF paper in our review — even more aggregated than L32's national load. The L29 scaling law predicts that high-aggregation forecasts should achieve much lower relative error simply because they sit deep in the saturation regime; the 0.94% MAPE is consistent with this expectation but tells us nothing about bus-level forecastability.
- **Wrong method family.** TFT is a complex DL architecture with ~2.5M trainable parameters, requiring GPU training. Not applicable to our 15,000-bus LightGBM pipeline at assignment scale.
- **No comparison to gradient boosting.** Same gap as L36. We cannot infer from this paper whether TFT actually beats LightGBM at this scale and aggregation.
- **NZ data, small subsystem.** 7 substations in a temperate climate, residential/commercial mix without the industrial-dominance challenges of ERCOT FWES.
- **No no-weather ablation.** They include tMax and tMin as inputs and report their importance, but do not test whether the model degrades without them. The interpretability finding (tMax > tMin) is descriptive of feature attribution, not causal evidence that weather adds accuracy.

### Insight labels

- **L37-A (lag features + hour + day-of-week dominate feature importance):** Variable Selection Network attribution ranks energy_demand + hour + day_week + cMin in the top tier on a third-party dataset using a third method family. 3rd independent confirmation of the F1 feature design (after L32-B/L32-C and standard practice).
- **L37-B (tMax > tMin asymmetric weather importance):** Aggregated-system finding that hot temperature extremes contribute more to forecast accuracy than cold extremes. Suggestive framing support for Q3a's FWES industrial-heat narrative; not a direct empirical comparison but consistent with the broader asymmetric-temperature literature.
- **L37-C (TFT achieves <1% MAPE on aggregated NZ subsystem):** Best-published aggregated-system MAPE in our review. Useful as a reference upper bound for what "very good" forecast accuracy looks like at high aggregation, but not a target for bus-level work (where L29 predicts substantially higher relative errors).
- **L37-D (TFT 2-3× improvement over ARTMAP-family ANN on same dataset):** Confirms modern DL improves substantially over older ANN baselines, but does not address LightGBM-vs-TFT comparison which is the actually-relevant question for our pipeline.

### Question-sheet pass (compressed)

- **Q1:** Not relevant. TFT predicts aggregated subsystem load, not individual bus or substation loads. No top-down vs direct comparison. No change.
- **Q2:** No naive baseline tested. No change.
- **Q3a:** **Tangential support via L37-B.** The tMax > tMin asymmetric weather importance is suggestive of asymmetric temperature effects, broadly consistent with FWES's heat-driven structural-growth narrative. Not strong enough for a Q-sheet entry but worth a one-sentence framing note if Q3a becomes a primary result.
- **Q3b:** Not relevant (no rolling-window experiment). No change.
- **Q4:** Not relevant (single aggregated forecast target, not per-bus stratified). L37's 0.94% MAPE at very high aggregation is *consistent with* L29's scaling law prediction (saturation regime for large W), but does not test it. No change.
- **Q5:** Not relevant (no zone-direct vs bus-summed comparison; aggregated subsystem only). No change.
- **Q6:** Not relevant (no cluster-as-feature experiment). No change.
- **Q7:** Not relevant (no imputation method ablation). No change.
- **Deliberately-not-asked weather:** L37 includes weather (tMax, tMin) and reports their importance, but does not run a no-weather ablation. **Not added to the no-weather defense cluster** because L37 does not demonstrate that the model maintains performance without weather. However, L37-B (tMax > tMin) is a separate observation worth tracking if asymmetric weather effects become methodologically relevant.
- **Deliberately-not-asked deep learning:** **L37-D added as a counterpoint observation in the report** — TFT does achieve 2-3× improvement over older ANN methods on a small aggregated system, so the "DL is better than older methods" claim has empirical support. But this is orthogonal to the LightGBM-vs-DL question that L12 + L36-B + L34-C address. Worth honest acknowledgment in the methodology section that modern DL beats older ANN methods, while LightGBM remains our defensible choice for scale, simplicity, and interpretability reasons.
- **Primary backlog F1 (lag features, hour-of-day, day-of-week):** **L37-A reinforces F1** with a 3rd independent confirmation (after L32-B, L32-C, and standard practice). Lag features (energy_demand + cMin), hour-of-day, day-of-week are universally in the top tier of feature importance regardless of method or dataset.

**Q-sheet changes summary:** No major Q-sheet revisions. F1 (primary backlog) reinforced with L37-A as 3rd independent feature-importance ranking. Q3a tangentially supported by L37-B (tMax > tMin asymmetry) as a framing note. Deliberately-not-asked deep learning expanded to acknowledge L37-D's TFT-over-old-ANN improvement while maintaining LightGBM defense via L12 + L34-C + L36-B.


## L38 — Wu, Zhao, Wang & Hao (2022) — Mini-batch SGD regression for STLF in big-data Map-Reduce framework

**Citation:** Wu, L., Zhao, Y., Wang, G., Hao, X. (2022). "A novel short-term load forecasting method based on mini-batch stochastic gradient descent regression model." *Electric Power Systems Research* 211, 108226. DOI: 10.1016/j.epsr.2022.108226. Affiliations: College of Electrical and Information Engineering, Lanzhou University of Technology, Lanzhou, China. Funded by National Natural Science Foundation of China (62063016), Gansu Province Science and Technology Plan (20JR10RA177), and State Grid Corporation of China. Same venue as L19 (Wei et al.), L24 (Safdarian et al.), L36 (Chen et al.), L37 (Ferreira et al.) — *Electric Power Systems Research* now contributes 5 papers in our review.

### PhD-level defense (compressed)

The paper proposes a **mini-batch SGD-trained multiple linear regression** model for STLF under a big-data Map-Reduce parallel-computing framework (Hadoop/HDFS). The core methodological argument is that traditional OLS regression via normal equations (X^T X)^{-1} X^T y becomes computationally intractable at "big data" scale, while mini-batch SGD with Map-Reduce parallelization can handle larger datasets faster. The architecture has three components: (1) data cleaning via Adaptive Sorting Neighbor Method (ASNM) for duplicates + K-means clustering for abnormal/incomplete data separation, (2) multiple linear regression model trained by mini-batch SGD, (3) F-test and T-test for feature significance (p < 0.05 threshold).

**The model is shockingly simple architecturally:** a 9-feature linear regression where features are system phase voltage (x1), phase voltage distortion rate (x2), system phase current (x3), phase current distortion rate (x4), load phase current (x5), compensation current (x6), power factor (x7), lowest temperature (x8), dew point (x9). After T-test significance screening at α=0.05, **x2 (voltage distortion rate) and x7 (power factor) are removed** because their p-values exceed the threshold. The final fitted Belgium model is:

> y = -1.6361 + 0.0072x1 + 0.2411x3 - 0.0032x4 + 0.0177x5 + 0.2037x6 + 0.1398x8 + 0.1911x9

Note that this is a **linear function of electrical measurements (currents, voltages) plus temperature**, not lag features. There is no lag-of-load feature, no day-of-week feature, no hour-of-day feature, no seasonality term.

**Datasets:** Two operational datasets. (1) Belgian national distribution network, 35,040 records at 15-minute granularity from 2021-01-17 to 2022-01-17. (2) Baiyin city transformer station, Gansu Province, China, 525,600 records at 1-minute granularity from 2018-08-01 to 2019-08-01. 80/20 train/test split.

**Results:** Belgium dataset MAPE 1.902%, max APE 3.61%, RMSE 2.18 (units appear to be kW based on Fig. 7 scale of ~10 MW). Baiyin dataset MAPE 2.058%, max APE 4.703%, RMSE 3.25 (kW). The paper also compares to a Back-Propagation neural network on both datasets (Figs. 8 and 11) and claims the proposed method beats BPNN, though the exact BPNN numbers are not reported in tables.

**The big-data performance argument:** Fig. 6 reports that for data sizes >60 MB, parallelized mini-batch SGD outperforms traditional gradient descent. For <60 MB, traditional gradient descent is faster because the inter-machine communication overhead in parallel computing dominates. So the Map-Reduce framework is only useful above a threshold dataset size.

### Brutal relevance assessment: 3/10

**Reasons for 3 (limited but not zero relevance):**
- **L38-A (electrical measurements as direct regression inputs at substation scale):** The model uses system voltage, current, distortion rate, and power factor — *not* lag features — as primary predictors. This is structurally different from every other STLF paper in our review and reflects a transformer/substation engineering perspective where the measurements *are* the system state, not derived features. **Tangential observation:** at our ERCOT bus level we have only load time-series (pd) and possibly some metadata, not direct voltage/current/power-factor measurements per bus. So we cannot replicate L38's feature design. But it is a useful conceptual data point that *if* such direct measurements were available, they would have predictive value (x1 system voltage retains a significant t-statistic of 26.4 after feature selection on Belgium data).
- **L38-B (T-test/F-test feature significance methodology at α=0.05):** Uses standard statistical-significance hypothesis testing (T-test on individual coefficients, F-test on the joint model) to prune non-contributing features. Voltage distortion rate (x2) and power factor (x7) are removed for failing T-test at α=0.05. **Methodologically straightforward but not novel.** Worth mentioning as an explicit example of statistical-significance-driven feature selection in the STLF literature (alongside our planned LightGBM feature importance approach).
- **L38-C (Map-Reduce parallelization for >60 MB datasets):** The empirical threshold finding — that parallelization only helps above ~60 MB — is a practical data point worth knowing, but **our 15,000-bus ERCOT dataset is in the GB range** so we are firmly in the regime where parallelization (or LightGBM's native parallel histogram construction) helps. Not transferable as a technique (we use LightGBM's built-in parallelism, not Hadoop) but useful as a published comparison.
- **L38-D (data cleaning via ASNM + K-means clustering):** Two-stage pipeline: ASNM detects duplicate records via time-based key attributes, K-means clustering separates valid/abnormal/incomplete data into 3 clusters. **Conceptually similar to our M2 imputation/cleaning step (Q7)** but operating at substantially different granularity (record-level deduplication, not bus-level missing-value imputation). Useful as a data-cleaning architecture reference.

**Reasons not higher than 3:**
- **Wrong method family for our problem.** Multiple linear regression has been superseded for STLF — virtually every paper in our review uses something more sophisticated (LightGBM, GNN, RNN, Transformer, hybrid).
- **No lag features.** This is methodologically very unusual for STLF and explains why we cannot directly adopt their feature design. Both L32-B/L32-C and L37-A confirm that lag features dominate feature importance across the modern STLF literature. L38 simply does not use them.
- **No temporal stratification.** No hour-of-day, day-of-week, or seasonality features. The model implicitly assumes that voltage/current/power-factor measurements capture the temporal pattern; this works on aggregated substation data (where the relationship is fairly stable) but would not work at bus level.
- **Two datasets are aggregated/substation-level**, not bus-level — Belgian national distribution network is far more aggregated than ERCOT buses.
- **No comparison to LightGBM or gradient boosting.** Comparison baseline is BPNN only, not modern tabular ML.
- **Map-Reduce / Hadoop infrastructure is not transferable** to our LightGBM pipeline. LightGBM has built-in parallel training with histogram-based splits that achieve similar parallelization benefits without needing a Hadoop cluster.
- **Modest MAPE numbers (1.9%, 2.06%) on aggregated systems are not directly comparable** to our expected bus-level WMAPE. L29's scaling law predicts that aggregated systems achieve substantially lower relative errors than bus-level forecasts, so MAPE <2% at substation scale is expected, not impressive.

### Insight labels

- **L38-A (electrical-measurement-based regression, no lag features):** Direct electrical state variables (voltage, current, power factor, distortion rate) as primary regression inputs. Not transferable to our bus-level pipeline (we don't have these measurements per bus) but a conceptual reference point for "what if the data were richer per spatial unit."
- **L38-B (T-test/F-test statistical-significance feature selection):** Explicit α=0.05 threshold-based feature pruning. Standard methodology, worth citing as an example of frequentist feature selection in STLF if our LightGBM feature importance discussion needs methodological context.
- **L38-C (60 MB parallelization threshold):** Empirical Hadoop Map-Reduce vs serial GD crossover point. Useful only as a practitioner observation that parallelization has overhead and isn't universally beneficial.
- **L38-D (ASNM + K-means data cleaning two-stage):** Data preprocessing architecture, comparable in spirit to our M2 step but at different granularity.

### Question-sheet pass (compressed)

- **Q1, Q2, Q3a, Q3b, Q5, Q6:** No relevance. L38 does not address top-down vs direct, naive baselines, FWES/zone-specific behavior, rolling-window retraining, bus-summed vs zone-direct, or sector-type clustering. No change.
- **Q4:** L38's MAPE of 1.902% (Belgium) and 2.058% (Baiyin) are at substantially higher aggregation than our bus problem. Consistent with L29's scaling law (saturation regime for large W) but does not test it. No change.
- **Q7 (imputation methods):** **Tangential support via L38-D.** ASNM + K-means is a data-cleaning architecture, not directly an imputation method, but conceptually adjacent to Q7's M2 preprocessing question. The paper does not report ablation on the cleaning choices, so it does not strengthen Q7 empirically. No change.
- **Deliberately-not-asked weather:** L38 includes temperature (x8 lowest temperature) and dew point (x9) as inputs that survive significance testing. The T-statistics (14.0 for x8, 21.6 for x9 on Belgium data) indicate weather features *do* contribute when using a linear model without lag features. **However, this is at substantially aggregated scale and without lag features, so it does not contradict our no-weather defense** — L38 is a different methodological regime than ours. No change.
- **Deliberately-not-asked deep learning:** No relevance (L38 uses linear regression, not DL).
- **Primary backlog F1 (lag features, hour-of-day, day-of-week):** L38 *does not use any of these features*, which is methodologically anomalous in the modern STLF literature. **L38 is the one paper in our review where lag features are absent**, which is interesting as a counterpoint but the paper does not report ablation against lag-feature-based methods. No formal F1 change.

**Q-sheet changes summary:** None. L38 is a methodologically simple and somewhat anomalous paper for the STLF literature (linear regression on direct electrical measurements without lag features), and does not contribute new evidence to any of Q1–Q7 or the deliberately-not-asked entries. Logged in the standard format because it is on-topic (STLF, peer-reviewed, *Electric Power Systems Research*) even though the relevance is limited.


## L39 — He, Zhao, Gao, Zhang, Zhang & Li (2025) — GRU + DDPG reinforcement learning for adaptive hyperparameter optimization in STLF

**Citation:** He, X., Zhao, W., Gao, Z., Zhang, L., Zhang, Q., Li, X. (2025). "Short-term load forecasting by GRU neural network and DDPG algorithm for adaptive optimization of hyperparameters." *Electric Power Systems Research* 238, 111119. DOI: 10.1016/j.epsr.2024.111119. Affiliations: Shenyang Normal University (School of Mathematics and Systems Science), Shenyang Jianzhu University (Electrical and Control Engineering / Municipal and Environmental Engineering), Shenyang Action Automation Control Co. Funded by National Natural Science Foundation of China (62273243), central government local-science-and-technology guidance funds, Liaoning Province Department of Education key foundation, Shenyang City Science and Technology Program.

### PhD-level defense (compressed)

The paper proposes **DDPG-GRU**, a hybrid model that combines Gated Recurrent Unit (GRU) neural networks with Deep Deterministic Policy Gradient (DDPG) reinforcement learning for **adaptive hyperparameter optimization** during STLF. The core methodological argument is that traditional hyperparameter tuning (manual search, grid search, random search, sparrow search, gray wolf optimization) is inefficient in high-dimensional continuous hyperparameter spaces, while DDPG — which is designed for continuous action spaces — can dynamically tune hyperparameters as the model trains. The hyperparameters being optimized are GRU **number of units**, **dropout rate**, and **learning rate** — three continuous variables.

**Architecture (Fig. 1, Fig. 3):** The framework has three modules — (1) data preprocessing (max-min normalization + polar coordinate encoding for date features + Pearson correlation feature selection), (2) DDPG-GRU prediction with five neural networks (online actor, online critic, target actor, target critic, plus the GRU itself) and an experience replay buffer, (3) evaluation via MAPE/MAE/RMSE/R². The DDPG agent observes the GRU prediction error as the *state*, recommends hyperparameter values as the *action*, and receives a reward = -|state| (negative absolute prediction error). The reward function explicitly penalizes higher prediction errors, encouraging the agent to find hyperparameter configurations that minimize MAPE.

**The Markov Decision Process formulation:** S = {GRU prediction error ξ_t^GRU}, A = {ψ_t^G-Unit, ψ_t^G-Drop, ψ_t^G-Learn} (number of units, dropout rate, learning rate), R = -|ξ_t^GRU(S,A,R)| or +|ξ_t^GRU(S,A,R)| depending on whether the error is below or above the best-recorded error. The DDPG critic network estimates Q-values for state-action pairs; the actor network outputs continuous hyperparameter recommendations; target networks provide stable learning objectives via soft updates (τ = 0.001).

**Data feature engineering — the most useful piece for our pipeline:** The paper introduces a **polar coordinate encoding for cyclical categorical features** (hour-of-day, day-of-week, day-of-month). Instead of natural encoding (which fails to capture cyclicity — e.g., 23:00 and 01:00 are 22 hours apart in natural encoding but only 2 hours apart in cyclical reality) or one-hot encoding (which loses ordinal information), the paper encodes each cyclical feature using sin/cos pairs:
- p_h^sin = sin(2πh/24), p_h^cos = cos(2πh/24) for hour-of-day
- p_w^sin = sin(2πw/7), p_w^cos = cos(2πw/7) for day-of-week
- p_d^sin = sin(2πd/T_d), p_d^cos = cos(2πd/T_d) for day-of-month

**This is a standard sinusoidal encoding technique used widely in time-series ML** (Fourier features, time2vec), but the paper applies it to STLF feature engineering and confirms empirically that it works better than natural or one-hot encoding for capturing cyclical load patterns.

**Datasets:** Two real Chinese power datasets from the Ninth National Student Electrical Mathematical Modeling Competition. Area 1: 211,296 records, 2009-01-01 to 2015-01-10, 15-min sampling. Area 2: 3,175,200 records, 2009-01-01 to 2015-01-31, 15-min sampling. Inputs include historical load, daily max/min/average temperature, average humidity, average precipitation, hour/day-of-week/day-of-year. After Pearson correlation feature selection, humidity and precipitation are dropped. Six experimental cases varying train/test splits and single-step vs 96-step-ahead forecasting.

**Results — most relevant findings:**

| Case | Setup | DDPG-GRU MAPE | GRU baseline MAPE | DDPG-GRU MAE | Improvement |
|---|---|---|---|---|---|
| Case 1 | Area 1, single-step, 2013 test | 1.336% (avg over 12 months) | 1.474% | 92.92 MW | -9.4% MAPE, -11.3% MAE |
| Case 2 | Area 1, single-step, 2014 test | 1.357% | 1.493% | 92.48 MW | -9.1% MAPE |
| Case 3 | Area 1, 96-step, 2013 test | (scatter plot only) | — | — | — |
| Case 4 | Area 1, 96-step, 2014 test | 5.582% | 6.852% | 82.74 MW | **-22.75% MAPE, -14.4% MAE, R² +13.2%** |
| Case 5 | Area 2, single-step, Dec 2014 | 2.364% | 2.779% | 84.37 MW | -14.9% MAPE |
| Case 6 | Area 2, 96-step, Aug 2013 | 5.638% | 6.962% | 88.996 MW | -19.0% MAPE (but RMSE *worse* 352.5 vs 215.3) |

**Case 4 (96-step multi-step forecasting) shows the largest improvement** — 22.75% MAPE reduction, 14.4% MAE reduction, 13.2% R² improvement. This suggests that the adaptive hyperparameter tuning is most beneficial for the harder multi-step prediction task where the model needs to balance multiple competing objectives. **Case 6 is an interesting failure mode** — MAPE improves (5.638% vs 6.962%) but RMSE *worsens* substantially (352.5 vs 215.3), suggesting DDPG-GRU produces lower average error but with larger tail errors. This is a methodological caveat worth noting.

### Brutal relevance assessment: 5/10

**Reasons for 5:**
- **L39-A (polar coordinate / sinusoidal cyclical encoding for hour-of-day and day-of-week):** This is the most directly applicable insight to our pipeline. Hour-of-day and day-of-week are confirmed as F1-tier features (L32-B/C, L37-A), and the standard approach in modern time-series ML is to use sin/cos encoding rather than natural integer encoding. **For our LightGBM pipeline, this is the recommended feature engineering practice for the hour-of-day and day-of-week features** — encode each cyclical variable as a (sin, cos) pair on its natural period. LightGBM can handle this natively as two continuous features. This is a small but concrete methodological recommendation for our notebook 03 feature engineering step.
- **L39-B (Pearson correlation feature selection):** Confirms the same feature-selection methodology used by L34, L36, and standard practice. Humidity and precipitation removed because they have weak Pearson correlation with load. Temperature retained (max/min/average). This is consistent with our no-weather defense framing — even at scale where weather data is available, Pearson-based feature selection regularly identifies temperature (max especially) as the only meaningful weather variable.
- **L39-C (DDPG outperforms baseline GRU on real Chinese power data):** Demonstrates that *adaptive hyperparameter tuning via RL can substantially improve DL model performance for STLF*, with improvements of 9–22% MAPE depending on the task. **Not directly transferable to LightGBM** (LightGBM has different hyperparameters with different tuning landscapes, and we are not implementing RL-based tuning at assignment scope), but a useful counterpoint observation for the LightGBM defense — modern DL models can be made substantially better with sophisticated tuning, which raises the bar for what counts as "DL beats LightGBM."
- **L39-D (TFT/Transformer-family architectures are increasingly the default for STLF research):** The paper's literature review table (Table 1) lists 11 GRU-family STLF papers from 2022-2023, all using attention/CNN/decomposition/clustering variations of GRU. This confirms the architectural trend observed in L36-L37 and L32 — *the STLF literature has converged on attention-augmented RNN/GRU/LSTM architectures as the modern default*, with LightGBM and tree-based methods receiving less attention in this venue. Our LightGBM choice is defensible per L12 + L36-B but increasingly contrarian relative to this literature.
- **L39-E (Case 6 RMSE worsening — important methodological caveat):** When MAPE improves but RMSE worsens, the model is producing lower average error but larger tail errors. This is a useful caution for our pipeline — **MAPE alone is not sufficient; we should report both MAPE and RMSE for our Q1, Q2, Q3a results** to detect cases where the model is making large tail errors that MAPE is hiding.

**Reasons not higher than 5:**
- **Wrong method family.** GRU + DDPG vs our LightGBM. The hyperparameter-tuning-via-RL machinery is not transferable; LightGBM has a different (and more constrained) hyperparameter space, typically tuned via Optuna/random search at our scale.
- **Wrong scale.** Area 1 and Area 2 are large regional aggregations of Chinese power data with peak loads in the high-GW range (Fig. 7 shows peak loads around 12 GW). Substantially more aggregated than our bus-level forecasting problem. L29's scaling law predicts that MAPE <2% at this aggregation is expected; the 1.3%–1.5% numbers in Cases 1–2 are consistent with saturation-regime performance.
- **Hyperparameter optimization is not our problem.** At assignment scope we will use default LightGBM hyperparameters or modest Optuna tuning; we are not deploying continuous hyperparameter adjustment during training.
- **No comparison to gradient boosting.** Same gap as L36 and L37 — they compare DL-against-DL but not DL-against-LightGBM. We cannot infer from this paper whether DDPG-GRU would beat LightGBM at the same scale.
- **Case 6 RMSE worsening is a real methodological concern** that the paper acknowledges only implicitly. If the proposed method's MAPE improvements come at the cost of higher tail errors, it is not strictly better.

### Insight labels

- **L39-A (sinusoidal/polar coordinate encoding for hour-of-day, day-of-week, day-of-month):** Standard cyclical feature encoding. **Direct recommendation for our pipeline:** in notebook 03 feature engineering, encode hour-of-day as sin/cos pair on period 24, day-of-week as sin/cos pair on period 7, day-of-month as sin/cos pair on its natural period. This is a small concrete change with empirical support.
- **L39-B (Pearson correlation feature selection identifies temperature, drops humidity/precipitation):** Confirms standard practice from L34, L36. If we ever revisit the no-weather decision, the recommended weather feature is temperature (max especially per L37-B), not humidity or precipitation.
- **L39-C (adaptive hyperparameter tuning yields 9–22% MAPE improvement for DL models):** DDPG-RL outperforms manual/random/sparrow/gray-wolf tuning on continuous hyperparameter spaces. Not transferable to LightGBM directly, but a counterpoint to consider in the LightGBM-vs-DL discussion.
- **L39-D (STLF literature has converged on attention-augmented RNN/GRU/LSTM architectures):** Table 1's literature review confirms the architectural-trend observation from L36/L37 — modern STLF research predominantly uses attention/CNN/decomposition variants of GRU/LSTM.
- **L39-E (MAPE-improves-but-RMSE-worsens failure mode in Case 6):** Useful methodological caveat. **Direct recommendation for our pipeline:** always report MAPE AND RMSE for all model comparisons to detect this failure mode where average error decreases but tail errors grow.

### Question-sheet pass (compressed)

- **Q1, Q2, Q3a, Q3b, Q4, Q5, Q6:** No direct relevance. L39 does not address top-down disaggregation, naive baselines, FWES/zone-specific structural growth, rolling-window retraining, scaling laws, bus-summed vs zone-direct comparison, or sector-type clustering. L39's data is aggregated regional, not bus-level. No change to any of Q1–Q6.
- **Q7 (imputation method choice):** No imputation discussion in L39. The paper uses complete data with feature selection but does not handle missing values explicitly. No change.
- **Deliberately-not-asked weather:** L39 uses temperature (max, min, avg) as inputs after Pearson correlation feature selection that drops humidity and precipitation. **Does not directly inform our no-weather defense** because L39 operates at large aggregation where weather effects are present. However, the **feature-selection finding (temperature retained, humidity/precipitation dropped) reinforces the broader pattern that even when weather is included, only a small subset of weather variables actually matter** — consistent with L32-D's <10% feature importance for temperature at national scale. No formal Q-sheet change but a small note.
- **Deliberately-not-asked deep learning:** **L39 strengthens the broader observation that modern STLF has converged on DL methods** — Table 1's literature review confirms 11 recent GRU-family papers all using DL approaches. Our LightGBM choice remains defensible per L12 + L36-B + L34-C but is increasingly contrarian. **No change to the defense** — we are not abandoning LightGBM — but worth honest acknowledgment in the methodology section that the broader STLF literature trend is toward attention-augmented RNN architectures.
- **Primary backlog F1 (lag features, hour-of-day, day-of-week):** **L39-A recommendation for feature engineering:** encode hour-of-day, day-of-week, and day-of-month as sin/cos pairs using polar coordinate encoding (period 24, 7, and month-length respectively). This is a concrete change to our notebook 03 implementation that should improve LightGBM's ability to learn cyclical patterns without requiring tree depth to be artificially high. **F1 reinforcement** with a methodological refinement.
- **Reportable methodology observation (L39-E):** Report both MAPE and RMSE for all model comparisons in our pipeline, to detect cases where one improves at the cost of the other.

**Q-sheet changes summary:** No major Q-sheet revisions. F1 (primary backlog) refined with L39-A sinusoidal encoding recommendation. Methodology note added: report MAPE + RMSE together per L39-E to detect mean-improves-but-tail-worsens failure mode.


## L40 — Pinheiro, Madeira & Francisco (2023) — Systematic STLF from system level to all 96,989 secondary substations of Portugal's distribution grid (GAM/GLM/XGBoost comparison + WMC ensemble + PREDIS production deployment)

**Citation:** Pinheiro, M. G., Madeira, S. C., & Francisco, A. P. (2023). Short-term electricity load forecasting—A systematic approach from system level to secondary substations. *Applied Energy*, 332, 120493. https://doi.org/10.1016/j.apenergy.2022.120493. Instituto Superior Técnico Lisboa / EDP / LASIGE / INESC-ID Lisboa.

**Core idea:** Build a systematic STLF methodology with four co-equal evaluation criteria — applicability, interpretability, reproducibility, accuracy — and scale it from Portugal's national power load (Dataset I, half-hourly, 12 weather stations) down to **every secondary substation in the Portuguese mainland distribution grid (Dataset II, 96,989 substations: 26,479 client-owned PTC + 70,510 DSO-owned PTD)**. The methodology starts with a Tao-Hong-style benchmark GLM (GLMLF-B), evolves through three GAM variants (GAMLF-SL-M1/M2/M3 with progressively richer features: spline-based smooth functions, lagged-load autoregressive terms, expanded day-type calendar), benchmarks against XGBoost (GBMLF-SL), and culminates in a Weighted Majority Continuous (WMC) ensemble (GAMLF-SLE) that combines a general-purpose GAM with eight regime-specific GAMs (Christmas/Easter/Carnival/public-holidays/weekends/August-summer/spring-summer-other/autumn-winter). At secondary substation level, the same GAM-M3 structure is replicated 96,989 times with per-asset feature data and NWP-derived temperature from the closest grid point. The system is deployed in production as PREDIS (a 22-server Hadoop cluster) running daily forecasts. Headline performance: at national scale, GAMLF-SL-M3 achieves MAPE 2.18% / RMSE 148 MW (1-day cycle), reducing error 42–47% vs the GLMLF-B benchmark and beating XGBoost (RMSE 199 MW); at substation scale, 82.8% of PTD models beat naive-day-ahead, vs 66.0% of PTC models.

**Relevance score: 6/10.** Same tier as L24 (FWES framing), L30 (BLDF + Q5 mechanism), L31 (LV review), L34 (no-weather heterogeneity), L36 (no-weather strongest piece). Worth using for: (i) the **PTD-vs-PTC aggregation effect** as direct empirical support for bus-summed coherence (Q5/Q4), (ii) the **GAM-beats-XGBoost-at-system-level finding** as a complicating data point in our LightGBM-over-DL defense, (iii) the **ACF/PACF-on-residuals methodology** as a concrete refinement for our lag-feature selection in notebook 03, (iv) the **interpretability-as-deployment-criterion framing** as published precedent for the practitioner-philosophy thread. Wrong scale (secondary substations are LV/MV interface, several levels below ERCOT transmission buses) and wrong method family (GAM with thin-plate splines, not gradient-boosted trees), but the methodological lessons partially transfer and the operational deployment context (PREDIS at Portuguese DSO) provides a concrete picture of what "100,000 individual forecasts in useful time" looks like.

### Brutal relevance defense

**What this paper actually does and where it scores:**

- **Genuine methodological contribution at scale.** The novelty is not the GAM technique itself (well established) but the disciplined application of it to *every* secondary substation in a national grid, with attention to deployment constraints. The PREDIS system runs 100,000 individual GAM model fits and 24-hour forecasts daily on a 22-server Hadoop/YARN cluster, completing the inference in ~5h42' with 100 vcores, parallelizable to ~550 vcores. This is the **largest operational STLF deployment** we've seen in our review (L23 ISO-NE top-down was zone-level; L25 ACTIVSg was synthetic-grid simulation; L34 50Hertz was 56 transmission substations; this is 96,989 secondary substations).
- **GAM > XGBoost at national scale on the same data.** Table 7 reports GAMLF-SL-M3 at 191 MW RMSE (1-year cycle), 148 MW (1-day cycle). Section 3.4 reports GBMLF-SL at 199 MW RMSE with hyperparameter exhaustive grid search. The authors note: "Given the same data modeling, even with some tweaks to accommodate the characteristics of the gradient booster machine (GBM), it achieves the same accuracy, but with two disadvantages [hyperparameter search cost + interpretability loss]." This is a **direct counterpoint to L12's LightGBM-over-DL framing**: at Portuguese national scale with carefully engineered features, GBM does *not* clearly win. The general principle this surfaces is that **method-family choice matters less than feature engineering and domain adaptation** when domain-knowledge features are well-designed.
- **ACF/PACF on residuals, not on raw signal.** Section 2.3.1 makes an important methodological point: when selecting which lagged-load values to include as autoregressive features, computing ACF/PACF on the raw signal misleads because daily and weekly seasonality already dominate. Instead, compute ACF/PACF on the *residuals* of a baseline model that already includes calendar features — the remaining autocorrelation tells you which lags add information beyond calendar features. Their result: include 24h lag (`y_{t-48}`) and 1-week lag (`y_{t-336}`) as autoregressive covariates because residual-PACF shows distinct peaks at exactly those lags. **Directly applicable to our notebook 03/04 lag-feature selection.**
- **PTD-vs-PTC aggregation effect.** Section 3.6, Figures 13–16. PTD substations (DSO-owned, aggregating a neighborhood of dozens to hundreds of low-voltage clients) forecast substantially better than PTC substations (client-owned, single large industrial or commercial customer). Median MAPE: PTD 0.126 vs PTC ~0.4+. MASE < 1 (i.e., beats naive day-ahead): PTD 82.8% vs PTC 66.0%. Same model structure, same training pipeline — the only difference is *aggregation level*. This is **direct empirical evidence that aggregating across heterogeneous consumers improves forecastability** — the same mechanism that underlies bus-summed coherence (Q5) and the Sevlian-Rajagopal scaling law (L29 / Q4). The PTD-PTC comparison is more controlled than most scaling-law studies because the methodology is held constant.
- **Interpretability as deployment criterion.** Section 3.1 articulates a four-criterion framework (applicability + interpretability + reproducibility + accuracy) and explicitly argues: "a model containing that variable is not applicable, no matter how well that variable would improve the predictions" if the utility cannot operationally obtain that variable, and "model agnostic interpretation techniques for machine learning models such as partial dependence plots (PDP), permutation feature importance (PFI) and Shapley values provide insightful model interpretations if well used" — but they prefer intrinsic interpretability over post-hoc. **Published precedent for the practitioner-philosophy thread we've built from L12 + L34-C + L36-B**, but with a twist: this paper argues for GAM rather than gradient boosting on interpretability grounds. Our LightGBM choice would need to lean on SHAP or PFI to match this paper's interpretability bar.
- **Peak-sensitive evaluation via Haben's adjusted p-norm.** Section 3.1 cites Haben 2014 directly and uses Haben's adjusted p-norm error (APN) with p=4 and w=3 (i.e., forecasts can be displaced up to 3 half-hours either side of original time). The rationale: "At the distribution level, it is the peak that matters for many use cases. So, for secondary substation models, we used Haben's adjusted error and a normalized version that prefer models that predict peaks even within a restricted displacement." Not directly applicable to our pipeline (we plan MAE/RMSE/WMAPE per E1), but a **flag for the evaluation phase**: if the bus-level forecasts have peak-prediction failures that don't show up in MAE/RMSE/WMAPE, peak-displacement-tolerant metrics like APN could surface them.
- **Weighted Majority Continuous (WMC) ensemble with regime-specific predictors.** Algorithm 1 + Section 2.5 + Tables 8–9. They define 8 disjoint temporal regimes (Christmas/New Year, Carnival, Easter, Other public holidays, Weekends, August, Spring/Summer-other, Autumn/Winter) and fit a general-purpose model plus a regime-specific model for each. The WMC ensemble combines them with learned weights that adapt as labels arrive (with 24h delay). Final RMSE: 154 MW, vs 191 MW for GAMLF-SL-M3 alone — an additional 20% improvement on top of the already-improved GAM. **The principle (regime-specific models for difficult periods) is useful** but the implementation overhead is heavy. For our pipeline, this would translate to "build a separate model for Winter Storm Elliott / extreme cold events" (F3 in primary backlog) — which is a less ambitious version of the same idea.

**What it's weak on (for our specific problem):**

- **Wrong scale.** Secondary substations in Portugal (LV/MV interface, ~hundreds of clients per PTD, often ~10 kV class) are several voltage levels below our ERCOT transmission buses (138/345 kV). The aggregation level analogy is partial — PTD substations are more like distribution feeders or LV pockets than transmission buses.
- **Wrong method family.** GAM with thin-plate regression splines and mgcv R package is not in our LightGBM/gradient-boosted-tree family. We could in principle build GAM-based baselines for comparison, but the primary pipeline is committed to LightGBM (notebook 03/04). The interpretability case for GAM via spline visualization doesn't directly transfer to LightGBM (which is less intrinsically interpretable but post-hoc-interpretable via SHAP).
- **Includes temperature as a major feature.** Counter to our no-weather defense. They use temperature with very pronounced effects (Figure 11(a) shows the U-shaped temperature-load relationship; bivariate temperature × time-of-day and temperature × day-of-year interactions also pronounced in 11(b)–(c)). However, *they use temperature at national-aggregated and substation-aggregated scales, where weather effects are strongest*; our bus-level transmission scale is closer to L34/L36 evidence that weather correlations become highly heterogeneous and bus-to-bus correlation dominates bus-to-weather correlation. So L40's use of temperature is consistent with the *scale-dependent* picture our no-weather defense already accommodates.
- **Update cycle methodology unusual.** Uses 1-year, 2-week, 1-week, 1-day cycles with 3-year fixed training window. The 1-day cycle is closest to our rolling-window retraining (F1) but the 3-year fixed window is unusual — we'd want to use an expanding or sliding window depending on whether structural growth dominates.

### Insight labels

- **L40-A (PREDIS operational deployment at 96,989 secondary substations):** Largest operational STLF deployment in our review. 22-server Hadoop/YARN cluster, ~5h42' total inference time for 100,000 individual model forecasts (100 vcores), parallelizable to ~550 vcores. **Reportable context for our pipeline:** if our bus-level pipeline ever needs to scale beyond ERCOT's ~5,000 active load buses, the PREDIS architecture (per-asset models, distributed scheduling, big-data storage for time series) is a published precedent.
- **L40-B (GAM-beats-XGBoost-at-system-level on Portuguese national load):** GAMLF-SL-M3 191 MW RMSE vs GBMLF-SL 199 MW RMSE with exhaustive hyperparameter grid search. **Methodological caveat for our LightGBM-over-DL defense:** gradient boosting is not always the strongest choice when domain-knowledge features are carefully engineered. This *complicates* the L12 LightGBM-over-DL framing without overturning it — at our bus-level scale with the kind of features we plan (lag features, hour-of-day, day-of-week, calendar), LightGBM's tree splits should handle non-linear interactions that GAM splines handle natively, but the comparison isn't as one-sided as the L12 framing implies.
- **L40-C (ACF/PACF on residuals, not raw signal, for lag-feature selection):** When selecting autoregressive lag features, compute ACF/PACF on residuals of a baseline calendar-only model rather than on the raw load signal. Raw-signal ACF is dominated by already-known seasonality; residual ACF surfaces lags that add information *beyond* calendar features. **Direct recommendation for notebook 03/04 feature engineering:** fit a calendar-only baseline, compute residual PACF, select lag features whose residual PACF coefficients exceed the confidence interval.
- **L40-D (PTD-vs-PTC aggregation effect on forecast accuracy):** Same model structure, same training pipeline, only difference is aggregation level: PTD (DSO-owned, neighborhood-aggregated) achieves median MAPE 12.6% with 82.8% beating naive; PTC (client-owned, single-customer) achieves median MAPE ~40%+ with 66.0% beating naive. **Direct empirical support for bus-summed coherence (Q5) and Sevlian-Rajagopal scaling (Q4/L29).** The PTC case is also direct empirical evidence for why **single-bus single-industrial-customer cases** (which we may have in the ERCOT bus dataset, especially in FWES where Permian Basin industrial sites or data centers may dominate single buses) **may forecast much worse than buses with diverse customer mix** — relevant for the per-bus-size-quartile or per-customer-type stratification in E1.
- **L40-E (interpretability as deployment criterion, with WMC ensemble preserving interpretability):** Four co-equal criteria (applicability + interpretability + reproducibility + accuracy). The WMC ensemble layer is itself interpretable because the learned weights directly indicate which sub-model dominates in which regime. **Published precedent for our practitioner-philosophy thread** (L12 + L34-C + L36-B), but with a GAM-not-gradient-boosting framing that we'd need to counter with SHAP-based interpretation arguments if we leaned on this paper.
- **L40-F (Haben adjusted p-norm error with p=4, w=3 for peak-sensitive evaluation):** Peak-displacement-tolerant error metric that penalizes large errors (missed peaks) much more than small errors. **Flag for evaluation phase (E1):** if our MAE/RMSE/WMAPE results show good aggregate performance but possible peak-prediction failures, consider adding APN-style metric to surface them.
- **L40-G (WMC ensemble with regime-specific predictors yields additional 20% RMSE improvement on top of GAM):** Weighted Majority Continuous ensemble combining general-purpose GAM with 8 regime-specific GAMs (Christmas, Easter, Carnival, holidays, weekends, August, spring/summer-other, autumn/winter) reduces RMSE from 191 MW (M3 alone) to 154 MW. **Possible methodological note for our pipeline:** F3 in primary backlog flags Winter Storm Elliott as a tail-risk event; the WMC framework is a published method for handling regime-specific tail events without disrupting the general-purpose model.
- **L40-H (Tao Hong PhD thesis as foundational benchmark, 6th cross-appearance in our review):** GLMLF-B is built directly on Hong's 2010 NC State PhD thesis. With Hong 2010 + Hong & Fan 2016 (L29/L31) + Hong-Wang fuzzy interaction regression ([48] here) + Nowotarski-Liu-Weron-Hong 2016 ([24] in L34) + Hong-Wang naive MLR benchmark ([32] here), **Tao Hong's group is now the most-cited author cluster in our review across 6 distinct works**. The reference tracker should note this concentration as a hub observation for the consolidation phase.

### Question-sheet pass

- **Q1 (top-down vs direct architecture):** L40 implements a per-asset direct approach (one GAM per substation, 96,989 models total) rather than a grouped-global or top-down approach. **Useful contrast for Q1:** L40 is what "direct" looks like at the upper-extreme end of model count, while L5 grouped-global is the methodological anchor for sharing parameters across similar assets. L40 does *not* test grouped-global or top-down on the same data, so it doesn't directly inform the Q1 architectural comparison. **No formal Q1 change** but worth a single-sentence note that L40 demonstrates the operational feasibility of pure-direct at very large scale.
- **Q2, Q3a, Q3b:** No direct relevance. L40 does not address zonal naive baselines, FWES structural growth, or zone-specific load characteristics. The Portuguese national grid has different seasonality structure than ERCOT (no Texas-summer industrial heat surge).
- **Q4 (scaling law):** **Indirect support via L40-D PTD-vs-PTC aggregation effect.** PTC single-customer cases forecast much worse than PTD aggregated cases, consistent with the Sevlian-Rajagopal scaling law (L29) that finds forecast error increases as aggregation level decreases. L40 does not fit a scaling law (i.e., MAPE vs log(load)) but the qualitative finding is consistent. **Minor strengthening of Q4** as a 3rd independent empirical confirmation (after L29 direct, L31 cross-country LV review).
- **Q5 (bus-summed vs zone-direct coherence):** **L40-D PTD-vs-PTC aggregation effect is direct empirical support for the bus-summed coherence framing.** Aggregation across heterogeneous consumers (PTD case) substantially improves forecastability vs single-customer aggregation (PTC case). Same model, same pipeline, only aggregation level differs. **Add as third independent citation to Q5's mechanism stack** (alongside L30-A switching operations and L34-D substation-reconfiguration). This is a *different* mechanism — averaging across heterogeneous consumers — but it operates in the same direction (aggregation improves predictability).
- **Q6 (sector-type clustering):** No direct relevance. L40 distinguishes PTD vs PTC but does not cluster on consumer sector type. The PTD-vs-PTC split is a proxy for "aggregated mixed-use" vs "single-large-customer" which is a coarser distinction than sector clustering.
- **Q7 (imputation):** No direct relevance. L40 uses linear interpolation for short gaps and removes assets with insufficient data. The reference [78] Haben 2014 is for the APN metric, not for imputation.
- **Deliberately-not-asked weather:** L40 uses temperature with substantial effect at both national and substation level. **Does not contradict our no-weather defense** because (i) L40 operates at aggregation levels where weather effects are strongest, and (ii) L36-A's bus-to-bus PCC > bus-to-weather PCC finding remains the directly-applicable bus-scale evidence. **No change to the no-weather defense.** A small note may be needed that at higher aggregation levels (national, regional, large-distribution-substation), temperature is genuinely useful — the no-weather argument is scale-specific to bus-level.
- **Deliberately-not-asked deep learning:** L40-B GAM-beats-XGBoost is a **methodological caveat** to our LightGBM-over-DL defense. Not a contradiction (LightGBM is gradient boosting, same family as XGBoost; the question is GBM-vs-GAM-vs-DL, with L40 finding GAM > GBM > [implicitly] DL on Portuguese national load with carefully engineered features). **The takeaway is that feature engineering matters more than method family**, which is consistent with our F1/F2 emphasis on lag features and structural growth modeling, but worth honest acknowledgment in the methodology section that LightGBM is not unconditionally the strongest choice — it's the strongest choice given (i) our scale, (ii) our feature set, and (iii) the L12 + L34-C + L36-B + L40-E practitioner-philosophy stack.
- **Primary backlog F1 (lag features, hour-of-day, day-of-week):** **L40-C ACF/PACF on residuals refinement is a concrete addition to F1.** Specifically: fit a calendar-only baseline, compute residual PACF, select lag features whose coefficients exceed confidence interval. This is more principled than picking lags by intuition (24h, 168h) and may identify lags we wouldn't otherwise include (e.g., 48h, 336h if 7-day-shifted weekly patterns show up).
- **Primary backlog F3 (Winter Storm Elliott as tail-risk event):** **L40-G WMC ensemble framework is a possible enhancement.** If we want to handle Elliott as a regime-specific case without disrupting the general model, the WMC approach is a published method. Probably overkill for Assignment 2 but worth noting.
- **Primary backlog E1 (evaluation metrics):** **L40-F APN peak-sensitive metric is a possible addition** if MAE/RMSE/WMAPE show good aggregate performance but possible peak failures. **L40-D PTD-vs-PTC effect strengthens the case for per-bus-size-quartile stratification** in E1, since single-customer-dominated buses may forecast substantially worse than mixed-use buses.

**Q-sheet changes summary:** Q4 gets a third independent confirmation (minor strengthening). Q5 gets a third independent citation in the mechanism stack (L30-A, L34-D, L40-D). Deliberately-not-asked deep learning thread gets a methodological caveat (L40-B: GAM > GBM at carefully-engineered national scale, complicating but not contradicting the LightGBM-over-DL defense). Primary backlog F1 refined with L40-C ACF-on-residuals lag selection. Primary backlog E1 reinforced with L40-D as motivation for per-bus-size-quartile stratification. Practitioner-philosophy thread gets a 4th citation (L40-E) but with a GAM-not-gradient-boosting framing.


## L41 — Mathew, Chikte, Sadanandan, Abdelaziz, Ijaz & Ghaoud (2024) — Medium-term (1-year-ahead) feeder load forecasting at Dubai DEWA using PWP-XGBoost (custom prominence-guided peak-weighted loss) with XGBoost-vs-RF-vs-SGD-vs-LSTM benchmark on 5 feeders

**Citation:** Mathew, A., Chikte, R., Sadanandan, S. K., Abdelaziz, S., Ijaz, S., & Ghaoud, T. (2024). Medium-term feeder load forecasting and boosting peak accuracy prediction using the PWP-XGBoost model. *Electric Power Systems Research*, 237, 111051. https://doi.org/10.1016/j.epsr.2024.111051. Dubai Electricity and Water Authority (DEWA), UAE.

**Core idea:** Forecast load at 5 feeders within the Dubai distribution network 1 year ahead at 30-minute resolution. Compare four ML methods — XGBoost, Random Forest, SGD Regressor, LSTM — and propose a custom XGBoost loss function (PWP-XGBoost: Prominence-guided Weighted Peaks) that multiplies MSE by (1 + w·p) where p=1 in pre-identified peak months/hours and w is a tuned hyperparameter. Peak months/hours identified data-driven via SciPy-style prominence peak detection on the historical load profile plus a bivariate histogram across (month, hour). Features used: 1-year lag, temperature, humidity, KVA, number of customers, number of substations, year, month, day, hour, day-of-year, week, quarter, holidays. Hyperparameter tuning via Optuna with 5-fold time-series cross-validation. Headline results: XGBoost achieves best R² on 4 of 5 feeders (0.92–0.95); LSTM has marginally better RMSE/MAPE on most feeders but requires per-feeder hyperparameter tuning, fails when data is limited, and is ~78× slower (366.74 s vs 4.68 s on a normal CPU, no GPU needed for XGBoost). PWP-XGBoost adds 0.5–5% to peak prediction accuracy over baseline XGBoost.

**Relevance score: 6/10.** Same tier as L24, L30, L31, L34, L36, L40. **The single closest method-family analog to our LightGBM pipeline in our entire review** — XGBoost (gradient-boosted trees), feeder/substation level, calendar + weather + customer-count features, 30-min resolution. Wrong horizon (medium-term not short-term) and wrong scale (Dubai feeder not ERCOT transmission bus), but the modeling pattern transfers and the XGB-vs-LSTM empirical comparison is the most directly applicable evidence we have for our LightGBM-over-DL defense.

### Brutal relevance defense

**Where it scores positively:**

- **L41-A XGBoost-vs-LSTM head-to-head with deployment-feasibility lens.** Table 2 compares R², RMSE, MAPE across RF, SGD, XGB, LSTM on 5 Dubai feeders. R²: XGB wins 4 of 5 feeders (0.92–0.95), LSTM trails by 0.01–0.05. RMSE/MAPE: LSTM marginally wins on most feeders, but Section 4 is explicit about why XGBoost is preferred: *"LSTM model is very data-specific, requiring hyperparameter tuning for each feeder, and if the data is less, the LSTM model fails... the training and inference time taken by the XGBoost model is comparatively much less than that of LSTM and other models. Also, we can execute the XGBoost model on a normal CPU, whereas we need GPUs to run the LSTM model. XGBoost model is more explainable than the deep learning models like LSTM."* Computational cost: 4.68 s (XGB) vs 366.74 s (LSTM) per feeder — a **78× speedup**. At our 15,000-bus scale this matters enormously: per-bus LSTM training would take ~140 hours vs ~20 minutes with XGBoost, before considering that LSTM "fails when data is less" which our 269 mixed-null buses qualify as.

- **L41-B PWP custom peak-weighted loss function design pattern.** Equation 4: `PWP-XGBoost loss(y, ŷ) = (1/n)·Σ(y - ŷ)² · (1 + w·p)` where p∈{0,1} indicates peak month/hour and w is tuned. Peak months/hours identified via two-stage data-driven workflow: prominence peak detection on load signal → bivariate (month, hour) histogram of detected peaks → assign p=1 to high-frequency peak bins. Result: 0.5–5% improvement in peak accuracy. **LightGBM supports custom objective functions natively via Python callables returning grad and hess vectors.** If our pipeline shows systematic peak-underprediction (likely for Winter Storm Elliott per F3, possibly for FWES summer peaks), this is a published transferable design pattern. Connects to L40-F (Haben APN peak-sensitive evaluation) as the training-side counterpart.

- **L41-C one-year-lag dominance at medium-term horizon.** Figure 12: across 5 feeders, "One year lag" is the top-importance feature for Feeders 1, 2, 3 (top 1) and second for Feeder 5. **Direct empirical evidence that very-long lag features dominate at long horizons.** For our pipeline:
  - **Next-day forecast:** short lags (24h, 168h) — already standard per F1, reinforced by L40-C.
  - **Next-month forecast:** lag features at 30d and 365d — **F1 in primary backlog should be extended** to include `lag_30d` and `lag_365d` where data availability supports them. Our 2022–2024 training window means lag_365 is only available for 2023–2024 rows; document this caveat. **L41-C is the citation.**

- **L41-D feeder-specific feature importance heterogeneity.** Figure 12 shows dominant features differ by feeder: Feeders 1–3 (one-year-lag + month/week), Feeder 4 (KVA ~0.45 dominates with temperature second, one-year-lag mid-rank — striking outlier), Feeder 5 (number of customers + one-year-lag + temperature). **Reinforces L34-A / L36-A heterogeneity argument** at feeder scale. Pragmatic implication: global LightGBM with bus-id as feature (per Q1 architecture) learns per-bus differences automatically.

- **L41-E temperature-load Pearson r=0.75 at Dubai DEWA.** Section 2.1.3, Figure 7: across all 5 feeders, "high correlation between load and temperature." **Highest weather-load correlation we've seen** (vs L36-A max r=0.46 Australia, L34-A range 0.016–0.498 Germany). **Counter-evidence to no-weather defense at face value**, but with critical caveats: (i) Dubai summer-AC-dominated extreme climate (40°C+); (ii) no winter heating (never below 16°C); (iii) AC load makes temperature near-physical predictor; (iv) feeder-level not transmission-bus-level. Closest ERCOT analog (FWES summer industrial cooling) was documented by L24-A to have *near-zero* temperature correlation because industrial process load dominates. **L41-E reinforces the heterogeneity argument** rather than contradicting it: weather correlation spans r=0.016 to r=0.75 across reviewed papers, depending on climate and load mix.

- **L41-G DEWA practitioner-philosophy citation.** Same passage as L41-A but with explainability/deployability front-and-center: XGBoost handles missing data, robust to outliers, CPU-deployable, more explainable than LSTM. **5th practitioner-philosophy citation in the LightGBM-over-DL stack** (L12 + L34-C + L36-B + L40-E + L41-G). The DEWA citation is methodologically the *most directly transferable* of the five because it explicitly endorses gradient boosting over LSTM in industrial utility deployment context with constraints matching ERCOT pipelines.

**Where it's weak:**

- **Horizon mismatch.** Medium-term (1 year). L41-C's one-year-lag dominance is partly horizon-driven. For next-day, short lags + hour-of-day should dominate per L32-B/L32-C/L37-A; for next-month, L41-C is more directly applicable.
- **Small sample (5 feeders).** No scaling-law analysis, no per-feeder-size breakdown.
- **Custom-loss math light.** Equation 4 presented without XGBoost grad/hess derivation.
- **Confidential DEWA data.** Not externally replicable.
- **Temperature correlation as honest counter-evidence to no-weather defense** (mitigated by climate/load-mix caveat).

### Insight labels

- **L41-A (XGBoost-vs-LSTM head-to-head with computational-cost lens):** XGB wins R² on 4 of 5 feeders; LSTM marginally wins RMSE/MAPE on most but requires per-feeder tuning, fails on small data, takes 78× longer (366.74s vs 4.68s), needs GPU. **Strongest single methodological-comparison citation in our review for the LightGBM-over-LSTM choice from industrial utility context.** Use alongside L12 in methodology.
- **L41-B (PWP custom peak-weighted loss design pattern):** Two-stage workflow (prominence peaks → bivariate histogram → weighted loss). **Transferable design pattern** for our pipeline if E1 evaluation shows systematic peak-underprediction. LightGBM supports custom objectives natively. Connects to L40-F (Haben APN) as evaluation/training pair.
- **L41-C (one-year-lag dominance at medium-term horizon):** Top feature for 3 of 5 Dubai feeders, top-3 for all 5. **F1 extension for next-month forecast:** include `lag_30d` and `lag_365d` where data availability permits (2023–2024 rows only for lag_365). Document caveat.
- **L41-D (feeder-specific feature importance heterogeneity):** Feature ranking varies markedly across feeders; reinforces L34-A / L36-A heterogeneity argument at feeder scale. Pragmatic implication: global model with bus-id feature learns per-bus differences automatically.
- **L41-E (temperature-load r=0.75 at Dubai DEWA):** Highest weather correlation in our review. **Counter-evidence to no-weather defense face-value but consistent with heterogeneity framing**: r=0.016 to r=0.75 across reviewed papers depending on climate and load mix. Update no-weather defense to acknowledge spectrum.
- **L41-F (data-driven peak identification → weighted training workflow):** Prominence peaks → bivariate histogram → loss weighting. **Transferable design pattern** documented for evaluation-phase consideration.
- **L41-G (DEWA practitioner-philosophy — 5th in our stack):** "XGBoost can also deal with missing data... robust to outliers... we can execute on a normal CPU, whereas we need GPUs to run the LSTM... XGBoost model is more explainable than the deep learning models." Strongest single-paper industrial utility articulation for XGBoost-over-LSTM. Use alongside L12 + L34-C + L36-B + L40-E.

### Question-sheet pass (compressed)

- **Q1 (architecture):** No direct relevance. L41 fits one XGBoost per feeder (5 models for 5 feeders), not grouped-global or top-down comparison. Per-asset modeling at small N is feasible; per-bus modeling at 15,000-bus scale would be impractical and is why L5's grouped-global is the architectural anchor. No formal Q1 change. Worth a sentence: L41 demonstrates per-asset XGBoost is feasible at small scale; for 15,000 buses the global-model-with-bus-id approach (per L5) is the scalable analog.
- **Q2, Q3a, Q3b, Q4, Q5:** No direct relevance. L41 does not address zone naive baselines, structural growth, rolling retraining, scaling laws, or zone/bus aggregation comparison.
- **Q6:** Tangentially via L41-D feeder-specific feature heterogeneity, but L41 does not cluster feeders by sector. No formal Q6 change.
- **Q7 (imputation):** L41 uses padding (forward-fill) for feature-resolution conversion (Section 2.1.2). This is a simple choice that does not engage with our M2 imputation decision. No change.
- **Deliberately-not-asked weather:** **L41-E provides honest counter-evidence to no-weather defense at face value** (r=0.75 at Dubai feeders) but climate/load-mix caveat preserves the heterogeneity argument. **Update no-weather defense to acknowledge L41-E as the high-end of the weather-correlation heterogeneity spectrum**: weather correlations range from near-zero (50Hertz substations, FWES industrial) to very high (Dubai AC-dominated feeders); ERCOT's diversified 8-zone footprint with substantial FWES industrial load sits closer to the low-correlation end, and a uniform weather feature across the footprint would be more dilutive than additive.
- **Deliberately-not-asked deep learning:** **L41-A and L41-G provide direct empirical and industrial-philosophy support for the LightGBM-over-LSTM choice.** This is the strongest single-paper evidence in our review for XGBoost-over-LSTM at industrial utility scale. **5th piece in practitioner-philosophy stack** (L12 + L34-C + L36-B + L40-E + L41-G) and **5th piece in methodological-benchmark stack** for tree-based-beats-DL or ties-DL: L5 (Triebe), L12 (Shiblee), L36-B (TCN-near-ties-FC-STGAT), L40-B (GAM beats XGBoost — counter-evidence at national scale), L41-A (XGBoost ties/beats LSTM on R² at feeder scale). Cumulative picture: gradient boosting wins on practical grounds (computational cost, deployment, explainability) and ties on accuracy in most peer-reviewed comparisons, while sophisticated feature-engineered statistical methods (GAM) can occasionally beat gradient boosting at national-aggregated scale (L40-B). For our bus-level scale per L29's scaling law, LightGBM is the appropriate choice.
- **Primary backlog F1 (lag features, hour-of-day, day-of-week):** **L41-C extension for next-month forecast**: include `lag_30d` and `lag_365d` where data permits.
- **Primary backlog E1 (evaluation metrics) and F3 (Winter Storm Elliott tail event):** **L41-B PWP custom loss is a methodological option** if E1 evaluation reveals systematic peak-underprediction. Not committed to implementation in Assignment 2 but documented for evaluation-phase consideration.

**Q-sheet changes summary:** No formal Q1–Q7 changes. Deliberately-not-asked weather updated to acknowledge L41-E as high-end of heterogeneity spectrum (r=0.016 to r=0.75 across reviewed papers). Deliberately-not-asked deep learning strengthened with L41-A and L41-G as 5th-piece methodological-benchmark and 5th-piece practitioner-philosophy citations. Primary F1 extended with L41-C lag_30d + lag_365d recommendation for next-month forecast. Primary E1/F3 documented L41-B PWP custom loss as methodological option for evaluation-phase consideration.
