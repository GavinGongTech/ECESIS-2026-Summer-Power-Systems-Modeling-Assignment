# Literature Review Synthesis — Insights and Open Questions

*Consolidation phase after L1–L41 (41 papers reviewed). This document synthesizes the actionable insights from the literature review and surfaces critical questions that emerged from the body of work but are not yet captured in our Q-sheet (Q1–Q7 + deliberately-not-asked).*

---

## Part 1 — Insights organized by where they apply in the pipeline

The literature insights cluster naturally into seven implementation areas. For each I note the concrete change, the citation(s) supporting it, and whether we should treat it as committed (will implement in Assignment 2), conditional (implement if a diagnostic check fails), or future-iteration (out of scope now, worth noting for the report's future-work section).

### 1.1 Feature engineering (notebook 03/04)

**Committed: Cyclical encoding of calendar features.** Encode hour-of-day as (sin(2πh/24), cos(2πh/24)); day-of-week as (sin(2πw/7), cos(2πw/7)); day-of-month or day-of-year similarly. This avoids the natural-integer artifact where hour 23 and hour 0 look maximally distant when they are adjacent. **Citation: L39-A (He et al. 2025).** Cost: trivial. Risk: none. This should be in every version of the feature matrix.

**Committed: Multi-horizon lag features per forecast horizon.**
- Next-day forecast: lag_24h, lag_48h, lag_168h (1-week). Standard practice; reinforced by L32-B/L32-C/L37-A all finding lag features dominate.
- Next-month forecast: lag_30d minimum; lag_365d where data availability permits. With our 2022–2025 training window, lag_365 is available for 2023–2025 rows only — document the caveat. **Citation: L41-C (Mathew et al. 2024) — one-year-lag was top-importance feature for 3 of 5 Dubai feeders at 1-year horizon, top-3 for all 5.**

**Conditional: ACF/PACF-on-residuals lag selection.** If our initial lag-feature set (lag_24h, lag_168h) underperforms or shows large residual autocorrelation at unexpected lags, refit a calendar-only baseline and inspect residual PACF. Add lag features where residual PACF coefficients exceed the confidence interval. **Citation: L40-C (Pinheiro et al. 2023).** Rationale: raw-signal ACF is dominated by seasonality already captured by calendar features; residual ACF surfaces lags that add information *beyond* calendar features.

**Committed: Structural growth feature.** Include a monotonic trend feature (year as integer, or days-since-epoch) to capture the +60% FWES and +51% NOTH structural growth 2022→2025. This is non-negotiable per F2 in the primary backlog. **Citations: L24 (Safdarian et al. 2023, ERCOT zone framing), L40 (Pinheiro uses trend index `x_t^(Trend)`), C1 (ERCOT LTLF Report).** Without this, the same-hour-last-year baseline (B1 baseline #2) will outperform any ML model in FWES and NOTH.

**Committed: Bus-id as a categorical feature in the global model.** Per Q1's grouped-global architecture (L5 anchor), the model sees bus-id as a learned embedding/encoding, which lets it capture per-bus baseline-level and per-bus seasonal-pattern differences without us hand-engineering per-bus features. **Citation: L5 (Triebe et al. 2025); reinforced by L34-A, L36-A, L41-D heterogeneity findings.** The L41-D feeder-specific feature-importance pattern (Feeder 4 dominated by KVA + temperature while Feeders 1–3 dominated by one-year-lag + month) is exactly the kind of heterogeneity a bus-id feature lets a tree-based model handle automatically through interaction splits.

**Future-iteration: Static bus-level features.** Per L5's architecture, augment the time-series features with static per-bus features: zone (8 weather zones from C3), base_kv (transmission voltage class from C5), latitude/longitude or zone centroid if available, customer count and KVA if available (per L41 feature set). For Assignment 2, zone is the only easily available static feature; the others would require external data joins outside scope. **Citations: L5, L41, L40.**

**Future-iteration: Weather features (deliberately omitted in Assignment 2 per 8-piece evidence cluster).** If a future iteration acquires NOAA ASOS data and joins it to ERCOT zones at the zone level (not bus level), this would be the lightest-touch weather integration. Per L34-A and L41-E, the heterogeneity-spectrum framing means a single zone-level temperature feature is acceptable but a bus-specific weather feature would require substantially more data engineering. **Citations: all 8 pieces of the no-weather cluster.**

### 1.2 Target/loss function (notebook 04)

**Committed: Standard MSE loss with LightGBM's default objective.** No exotic loss function for the baseline run.

**Conditional: Custom peak-weighted loss (PWP-style).** If E1 evaluation shows systematic underprediction at peak hours or during extreme events (Winter Storm Elliott, FWES summer peaks), implement a LightGBM custom objective that weights loss by (1 + w·p) where p=1 on pre-identified peak (month, hour) bins. The two-stage workflow is: (a) prominence peak detection on the load time series, (b) bivariate histogram of (month, hour) for detected peaks, (c) assign p=1 to top-N peak bins, (d) tune w via cross-validation. **Citation: L41-B (Mathew et al. 2024).** Cost: moderate (custom objective requires returning grad and hess vectors). LightGBM supports this natively.

**Future-iteration: Probabilistic forecasting / quantile regression.** L40 cites this as a methodological extension (their L40 [Refs 51, 52, 53]); we are not committed to it in Assignment 2 but it's worth noting as a future direction if operational use requires uncertainty quantification.

### 1.3 Architecture (notebook 04)

**Committed: Single global LightGBM model with bus-id as a feature.** The "grouped-global" approach per L5. This is the architectural anchor decision. The model is trained on all buses jointly, with bus-id as one of the input features, so the model can learn both shared patterns across buses (via the global gradient-boosted structure) and per-bus-specific behaviour (via interaction splits on bus-id). **Citations: L5 (anchor); L40 demonstrates per-asset modeling at small scale is feasible but doesn't scale, while L5's grouped-global is the scalable analog.**

**Committed: Compare against zone-direct + top-down disaggregation baseline.** Per Q1. Zone-level model trained on zone aggregates, then disaggregate to buses via historical share. This is the Q1 comparison. **Citations: L5 (motivates the comparison); L25 (population-weighted disaggregation); L23 (top-down example).**

**Conditional: Per-cluster models if sector-mix matters (Q6).** If Q6 finds that sector-type clustering (residential / commercial / industrial inferred from diurnal load shape per L4 + L27 + L28) substantially improves forecasts, consider per-cluster sub-models or sector-type as a categorical feature. The L4 + L27 + L28 anchor stack supports the principle. **Caveat: L40-D PTD-vs-PTC effect is empirical evidence that aggregation level matters more than sector mix per se** — single-customer cases forecast worse regardless of customer type. Worth investigating both in Q6.

**Deferred / non-implementation: Deep learning (LSTM, TFT, GNN, Transformer-family).** Defensible on five grounds per the 5-piece practitioner-philosophy stack (L12 + L34-C + L36-B + L40-E + L41-G) and 5-piece methodological-benchmark stack (L5 + L12 + L36-B + L40-B + L41-A). The strongest single argument: L41-A's XGBoost-vs-LSTM head-to-head shows 78× training speedup with R² wins on 4 of 5 feeders. At our 15,000-bus scale this matters enormously.

### 1.4 Imputation and missing-data handling (notebook 02)

**Committed (per M1 + M2 in primary backlog):** DST HE24 gaps not imputed; isolated single-hour gaps linear-interpolated; 2025-12-04 flagged as missing in test; always-null LOAD buses dropped; mixed-null contiguous blocks linear-interpolated. **No literature citation strictly required** — these are EDA-driven decisions consistent with industry practice.

**Conditional: AGCIN-style preprocessing (Q7).** If standard linear interpolation shows residual issues for the 269 mixed-null buses, consider the L20-B AGCIN-derived imputation insight (use neighboring buses' time series to inform imputation rather than purely linear interpolation within a single bus). **Citation: L20-B (Zhao et al. 2024).** This is Q7's empirical test.

**Honest acknowledgment:** Forward-fill / padding (L41 uses this for feature-resolution conversion) is the lightest-touch imputation method and is empirically common but rarely optimal. We should justify our linear-interpolation choice against the simpler forward-fill alternative.

### 1.5 Evaluation (notebook 06)

**Committed: Both MAPE and RMSE for every model comparison, never one alone.** L39-E documented a real failure mode where MAPE improves but RMSE worsens dramatically — average error down, tail error up. Always report both to detect mean-improves-but-tail-worsens regressions. **Citation: L39-E (He et al. 2025).** Per E1 in primary backlog.

**Committed: Stratified error analysis.** Per E1: zone, hour-of-day, month, bus-size quartile. **Citation: L40-D PTD-vs-PTC effect strengthens the case for bus-size-quartile stratification** because L40-D is direct empirical evidence that aggregation level / customer-mix dominates over methodological details. **L29 scaling law (Q4) anchors the bus-size-quartile interpretation:** if our results match the α₀ + α₁·W^(-1) functional form, we are adding a 4th independent confirmation (after L29 PG&E, L31 UK LV-feeders, L40 Portugal PTC/PTD).

**Committed: Zone-aggregation check (E3).** Sum predicted bus pd → compare with actual zone pd. Internal consistency check.

**Conditional: Peak-sensitive evaluation (Haben APN).** If MAE/RMSE/WMAPE show good aggregate but possible peak failures, consider adding APN (adjusted p-norm with p=4, w=3) which penalizes large errors much more than small errors and tolerates small timing displacements. **Citation: L40-F (Pinheiro et al. cites Haben 2014 directly).** Pairs with L41-B PWP custom loss as the evaluation/training pair for peak handling.

**Conditional: MASE comparison against naive day-ahead.** L40 uses MASE with m=48 (1-day seasonal naive) and reports the fraction of assets that beat naive. This is a clean way to communicate "how many buses does our model actually help?" Useful framing for the report. **Citation: L40 (Pinheiro et al. 2023).**

**Future-iteration: Probabilistic / quantile metrics.** If probabilistic forecasting is implemented (1.2), pinball loss / continuous ranked probability score are the standard metrics.

### 1.6 Training/validation protocol (notebook 04)

**Committed: Sequential time-series cross-validation, not random splits.** Per F1 in primary backlog. **Citations: L40 (Pinheiro uses fixed 3-year window + next 1-day/1-week/2-week/1-year as test, rolling); L41 (Mathew uses time-ordered 5-fold CV).**

**Conditional: Rolling-window retraining vs full-history training (Q3b).** This is one of our existing questions. The L40 result that update-cycle granularity (1-day, 1-week, 2-week, 1-year) substantially affects accuracy — finer-grained updates help — supports the rolling-retraining hypothesis. **Citation: L40 Tables 6, 7.**

**Conditional: Hyperparameter tuning via Optuna.** Per L41 (Mathew uses Optuna for XGBoost). If the LightGBM default hyperparameters underperform, Optuna's bayesian-optimization-over-tree-structured-Parzen-estimator is the standard tool. **Citation: L41.** Cost: moderate (search budget); benefit: typically 2–10% accuracy improvement.

### 1.7 Reporting and operational deployment (notebook 06 + report)

**Committed: Interpretability emphasis.** Per L40-E's four-criterion framework (applicability + interpretability + reproducibility + accuracy as co-equal). For LightGBM, this means using SHAP values for feature importance and per-prediction explanations. **Citations: L40-E + L41-G articulate the practitioner-philosophy case; Molnar et al. 2022 (L40 [83]) flags pitfalls of model-agnostic interpretation methods.**

**Committed: Reproducibility.** Random seeds fixed, environment pinned, code versioned. Per L40-E.

**Future-iteration: Production deployment architecture.** If this project ever moves to production, PREDIS (L40, Hadoop/YARN cluster, ~5h42' for 100,000 individual forecasts on 100 vcores) is the published precedent. Not for Assignment 2.

---

## Part 2 — Cross-walk between literature insights and primary backlog (BACKLOG_PRIMARY.md)

Showing how the literature insights connect to the EDA-derived primary-backlog items:

| Primary item | Literature reinforcement / refinement |
|---|---|
| **M1 missing pairs / DST handling** | None from literature; EDA-driven. |
| **M2 null pd preprocessing** | L20-B AGCIN preprocessing (Q7 test); L40/L41 padding-as-alternative |
| **D1 placeholder buses** | None from literature; EDA-driven. |
| **D2 ISOLATED zone exclusion** | None from literature; EDA-driven. |
| **D3 load_bus_count as feature** | Indirect: L30-A + L34-D network-reconfiguration mechanism makes load_bus_count a partial proxy for topology changes |
| **D4 SWING bus treatment** | None from literature. |
| **B1 baselines (last week, last year, hist hourly avg)** | L29 scaling law motivates per-bus-size stratification of baseline comparison; L40 MASE-vs-naive framing |
| **F1 feature engineering** | L39-A sin/cos cyclical encoding; L41-C lag_30d + lag_365d for next-month; L40-C ACF-on-residuals if needed; L5 bus-id feature; structural-growth trend |
| **F2 structural growth modeling** | L24 ERCOT zones framing; L40 trend covariate precedent; C1 ERCOT LTLF directly |
| **F3 Winter Storm Elliott handling** | L40-G WMC regime-specific ensemble framework (future-iteration); L41-B PWP peak-weighted loss (conditional) |
| **E1 evaluation metrics** | L39-E MAPE+RMSE-together; L40-D PTC-vs-PTD motivates bus-size-quartile stratification; L40-F Haben APN (conditional); L29 scaling-law fit (Q4) |
| **E2 2025-12-04 missing** | None from literature; EDA-driven. |
| **E3 zone-aggregation check** | None from literature; basic consistency. |

The cross-walk shows that **the primary backlog is largely independent of the literature** — most M/D/E items are EDA-driven and don't need citation backing — while **F1/F2/F3/E1 are where the literature is most useful**. Q1–Q7 in BIG_QUESTIONS.md are where the literature is most necessary.

---

## Part 3 — Anchor stack summary by score tier

| Score | Entries | Primary role in our work |
|---|---|---|
| 10/10 | L5 | Q1 architectural anchor; grouped-global-bus framework |
| 9/10 | L29 | Q4 scaling-law anchor (direct); error vs aggregation |
| 9/10 | L12 | LightGBM-over-DL methodological-benchmark anchor |
| 8/10 | L27 | Q6 ACTIVSg anchor; synthetic-load-by-sector |
| 8/10 | L4 | Q6 methodological foundation; cluster-then-forecast |
| 7/10 | L28 | Q6 empirical ablation; weather-hurts case |
| 6/10 | L36 | No-weather strongest single piece (L36-A); LightGBM defense 4th piece (L36-B) |
| 6/10 | L41 | Closest method-family analog; XGB-vs-LSTM head-to-head; one-year-lag dominance |
| 6/10 | L40 | Largest operational deployment; PTC-vs-PTD aggregation effect (Q5 + Q4 support); ACF-on-residuals |
| 6/10 | L34 | No-weather heterogeneity (L34-A); Q5 second-citation mechanism stack |
| 6/10 | L31 | LV review; Q4 cross-country support; no-weather 4th piece |
| 6/10 | L30 | Substation BLDF; Q5 mechanism citation |
| 6/10 | L24 | FWES framing; no-weather 1st piece (zero-correlation industrial) |
| 5/10 | L20-B | Q7 imputation candidate |
| 5/10 | L39 | sin/cos cyclical encoding (F1); MAPE+RMSE together (E1) |
| 4/10 | L7, L32, L37 | Background / specific methodological touchpoints |
| 3/10 | L38 | Logged for completeness |
| 1/10 | L33, L35 | Wrong domain |

---

## Part 4 — Big questions that have arisen but are NOT yet captured

This is the section where I'm actively producing new questions from gaps the literature surfaced. Some of these may warrant adding to the Q-sheet for Assignment 2; others belong in the report's future-work section or as honest acknowledgments of methodological limitations.

### Q8 (candidate for Q-sheet) — Does the L29 scaling law hold on our ERCOT bus data, and if so, what are α₀, α₁, and W★?

**Source of the question:** L29 (Sevlian-Rajagopal 2018) anchored Q4 as the foundational scaling-law evidence, and L31 + L40 provide cross-country confirmation. But our existing Q4 is framed as a *diagnostic* question ("does accuracy degrade at small buses?"), not as a *quantitative-fit* question ("what are the scaling-law parameters?"). We have a unique opportunity here: with ~5,000 active LOAD buses spanning multiple orders of magnitude in average load size, we can fit the L29 functional form `MAPE(W) = α₀ + α₁·W^(-1)` (or its Sevlian-Rajagopal generalization with a saturation point W★) and report the parameters. **If our parameters match the order-of-magnitude of L29's PG&E parameters, it is a 4th independent confirmation of the scaling law.** This is reportable as a top-line result even if the rest of the model is mediocre. **Recommendation: promote to a formal Q8 in the Q-sheet.** Cost: a single regression after Q4's per-bus-size-quartile analysis is done.

### Q9 (candidate for Q-sheet) — Does the model show systematic peak-hour underprediction, and if so, would a peak-weighted custom loss improve it?

**Source of the question:** L40-F (Haben APN) and L41-B (PWP custom loss) both surfaced peak-handling as a methodologically distinct concern from aggregate accuracy. Our current E1 stratifies by hour-of-day but does not specifically diagnose peak-prediction failure as a structural issue. **Recommendation: add as a conditional Q9 — diagnostic first (does peak prediction systematically underperform aggregate?), then conditional treatment (PWP-style custom loss).** Cost: diagnostic is essentially free (it's a slice of E1); treatment is moderate (LightGBM custom objective).

### Q10 (candidate for Q-sheet or future-work) — Is there a mechanism-of-error decomposition: how much of bus-level error is attributable to network reconfiguration (L30-A, L34-D), how much to consumer-heterogeneity (L40-D), how much to scaling-law floor (L29), how much to weather (deliberately omitted), and how much to model misspecification?

**Source of the question:** This is the deepest methodological gap our review surfaced. We have **multiple independent mechanisms** documented in the literature for bus-level forecast error:
- L29: scaling-law floor — error has an irreducible component that scales with 1/W
- L30-A + L34-D: network reconfiguration — switching operations not reflected in time series cause anticorrelated errors at adjacent buses
- L40-D: consumer-heterogeneity averaging — single-customer buses are inherently harder to forecast
- L36-A: weather absence — at least for our scale, weather captures less signal than bus-to-bus correlation
- L7, L32, L20: imputation / missing-data effects

**These are not mutually exclusive.** A given bus's error budget is some convex combination of these. **The diagnostic question is whether we can quantify the relative shares.** This is hard but reportable: for instance, regress per-bus MAPE against (i) average load size (scaling-law signature), (ii) ISOLATED-switch frequency from D3 (network-reconfig signature), (iii) load-coefficient-of-variation (consumer-heterogeneity signature). The residual is unexplained by these mechanisms — methodological refinement potential. **Recommendation: belongs in future-work section of the report unless we want to make it a formal Q10.** Cost: moderate but high-value-per-cost.

### Q11 (future-work) — Does temperature add value when joined at the zone level (one of the 8 weather zones), even though we deliberately omit it at the bus level?

**Source of the question:** Our 8-piece no-weather evidence cluster operates with the framing "weather correlation is heterogeneous at bus scale." But L24 + L40 + L41 all use temperature at higher aggregation levels (zone, national, distribution-feeder) successfully. **There is a middle option we have not tested: zone-level temperature joined to bus rows by zone membership.** This would test whether weather adds value at all in our pipeline even if not at bus-specific granularity. **Recommendation: future-work; out of scope for Assignment 2 unless time permits.** Cost: requires NOAA ASOS data acquisition and zone-mapping.

### Q12 (future-work) — Does the Sevlian-Rajagopal "bias-variance tradeoff for forecast aggregation" hold: is there an optimal aggregation level W★ above which forecasting is "easy" and below which it is fundamentally hard?

**Source of the question:** L29 found a "saturation" point W★ in the scaling law where MAPE becomes approximately constant for W > W★. This is a deeper structural claim than the simple power law: it says there is a *characteristic scale* above which all systems forecast similarly well, and below which forecast difficulty grows rapidly. **For our ERCOT data, if we estimate W★, it tells us where the "hard" buses are.** It also speaks to the architectural question (Q1): if our buses are mostly W < W★, grouped-global is more important than per-bus modeling because the per-bus signal is intrinsically weak. **Recommendation: belongs naturally inside Q8 if we promote that.**

### Q13 (future-work) — How does forecast quality interact with the LOAD↔ISOLATED switching dynamic in ERCOT's 4,425 zone-switching buses?

**Source of the question:** Our D3 in the primary backlog documents that 4,425 buses switch LOAD↔ISOLATED across the dataset. Our planned treatment is "when ISOLATED, treat pd as zero." But the L30-A + L34-D mechanism is exactly that network reconfiguration creates anticorrelated errors: when bus A becomes ISOLATED, bus B picks up its load. **Our current pipeline does not exploit this. A zero-load prediction at bus A and a normal prediction at bus B will sum correctly at the zone level — but at the bus level we lose accuracy at bus B.** If we identified "anticorrelated-pair" buses from historical data, we could either (a) use bus-pair features (bus A's recent load as a feature for bus B), or (b) jointly predict bus-pairs. **Recommendation: future-work; novel and likely beyond Assignment 2 scope.** Cost: high but potentially high reward — and it directly leverages L30-A + L34-D + L40-D as published evidence that the mechanism is real.

### Q14 (future-work) — Does the choice of training-window length affect FWES/NOTH forecast accuracy differently than other zones, given their +60% / +51% structural growth?

**Source of the question:** Our existing Q3b asks whether rolling-window retraining beats full-history training. But this conflates two effects: (a) the *frequency* of retraining (rolling vs static), and (b) the *length* of the training window (recent 6 months vs full 3 years). For FWES and NOTH, where structural growth is steep, a shorter recent window may capture the current load level better; for stable zones (Coast, North, South), a longer window provides more pattern diversity. **Q3b doesn't decompose these two effects.** **Recommendation: future-work or refinement of Q3b.**

### Q15 (future-work / honest limitation) — Are the buses with no historical signal (2,137 always-null LOAD buses, dropped per M2) systematically distributed in a way that biases our per-zone aggregated metrics?

**Source of the question:** Per M2, we drop 2,137 always-null LOAD buses entirely. If these are not uniformly distributed across zones (e.g., concentrated in FWES because Permian Basin industrial buses lack metering), our per-zone aggregate metrics may be biased — we're reporting performance only on the subset of buses that *do* have data, which may not be representative of operational reality. **Recommendation: include in honest-limitations section of the report.** Cost: a single value_counts check on zone for the dropped buses.

### Q16 (out of scope but reportable acknowledgment) — Does our model's forecast quality interact with day-ahead vs hour-ahead ERCOT market price patterns in operationally meaningful ways?

**Source of the question:** L34 (Schröter et al.) is explicit that TSO-level forecasts feed into market clearing and storage dispatch. Our forecast is purely numeric and has no operational deployment context. **Recommendation: out of scope; mention in conclusions as the natural next operational step.**

### Q17 (cluster-level future-work) — Is there a methodologically defensible way to construct a per-bus prior using L4 / L27 / L28 sector clustering combined with L5 grouped-global modeling?

**Source of the question:** Our Q6 asks whether sector clustering helps; if it does, we currently treat it as a *feature* (sector-type categorical). But the L4 + L27 + L28 + L40-D evidence suggests sector-mix is more than a feature — it's a *latent structure* that defines coherent sub-populations of buses. **An alternative architecture: cluster buses into K sector-types, fit K grouped-global models (one per cluster), each with its own learned parameters.** This is a hybrid of L4 (cluster-then-forecast) and L5 (grouped-global). **Recommendation: future-work; possibly a thesis-worthy direction.** Out of scope for Assignment 2.

### Q18 (honest acknowledgment) — Are we underutilizing the static topology information available in the ERCOT data?

**Source of the question:** L22 (Cheng & Chen 2025, Alberta topology recovery) and L34-E (Schröter power-flow-derived sensitivity weights) and L35 (Wang-Majumdar-Rajagopal geospatial mapping) all use topology information as a forecast aid. Our pipeline ignores topology entirely — buses are treated as independent observations modulo bus-id. **The L30-A + L34-D + L40-D Q5 mechanism stack documents that network topology drives bus-summed coherence and anticorrelated errors.** Our base_kv field carries some topology signal (transmission voltage class), and zone membership carries spatial-proximity signal. But we have no adjacency matrix and no inter-bus connectivity features. **Recommendation: honest limitation in the report; future-work to consider adjacency-aware models.** This is also related to the L17 / L18 / L20 GNN literature we surveyed but did not adopt.

### Q19 (out of scope) — Would TFT or another deep learning model genuinely outperform LightGBM on our specific ERCOT data, holding feature engineering constant?

**Source of the question:** Our 5-piece methodological-benchmark stack defends LightGBM, but the strongest single counter-evidence is L37 (TFT 0.94% MAPE on NZ subsystem). Our defense rests on: (i) L37's scale is far from ours, (ii) L37 doesn't benchmark against LightGBM, (iii) L41-A and L12 show XGBoost/LightGBM ties or beats LSTM at scale. **But none of these is a direct test on our specific data.** This is an honest open question. **Recommendation: acknowledge in honest-limitations; if compute permits, run a TFT baseline for the report as a sanity check.** Cost: high; benefit: closes a real methodological loophole.

### Q20 (cross-cutting methodological) — Do our forecast errors exhibit residual autocorrelation that suggests we are still missing structure?

**Source of the question:** L40-C (Pinheiro's ACF-on-residuals methodology) is not just a lag-feature-selection tool — it is a *model-diagnostic* tool. After our LightGBM forecasts, the residuals should ideally be white noise. If they show autocorrelation at specific lags, we have not captured all the structure. **Recommendation: add as a routine diagnostic in notebook 06.** Cost: trivial; benefit: high — it surfaces whether further feature engineering would help.

---

## Part 5 — Honest assessment of the literature review's coverage gaps

A self-critical look at what *kinds* of papers our 41-paper review covered well versus poorly:

**Well-covered areas:**
- STLF methodology comparisons (XGBoost / LSTM / GAM / Transformer at various scales)
- LV / feeder / substation-level forecasting (L28, L30, L31, L34, L40, L41)
- Synthetic-grid / ACTIVSg literature for Q6 (L2, L25, L27)
- Scaling laws (L29, L31, L40)
- Cluster-then-forecast (L4, L27, L28)
- Industrial-utility production deployment (L40 PREDIS, L41 DEWA)

**Under-covered areas:**
- **Bus-level (transmission-bus) STLF.** Despite our search, only L18 (EV bus charging) and L19 (dissertation) directly address bus-level forecasting in our methodological sense. L36 and L37 are the closest, but at supply-area aggregation and substation aggregation respectively. **The literature simply has less bus-level work than we hoped.** Amjady 2007 (Bucket 2 watch item, title-direct match) remains unread.
- **Probabilistic / quantile forecasting.** Multiple papers (L40, L22) reference it but we have no anchor paper specifically for it.
- **Concept drift / non-stationary load.** FWES +60% growth is a concept-drift problem; no single paper in our review treats it as such methodologically.
- **Imputation methodology for time series specifically.** L20-B touches it, L7 mentions missing-data-tolerant TCN, but we have no anchor for imputation-and-forecast jointly modeled.
- **Cost-aware / asymmetric loss functions.** Under-prediction at peak vs over-prediction in off-peak may have very different operational costs. L41-B touches it via PWP but only the binary peak/non-peak case.
- **Holiday and special-event handling.** L40 has the most extensive treatment (8 regime types via WMC ensemble); no other paper engages systematically. Our F3 Winter Storm Elliott concern is under-cited.
- **Long-term scaling-law theory.** L29 is the empirical anchor; the theoretical paper that originally formulated the bias-variance tradeoff for forecast aggregation is not in our review.

**Geographic coverage:** US (L24, L25, L27, L29), UK (L31), Germany (L34), Portugal (L40), Greece (L12, L28), Australia (L36), New Zealand (L37), Iran/Germany (L32), Dubai (L41), China (L18, L38, L39). This is broad. **Conspicuously underrepresented:** Canada, Brazil, India, Japan — all major grid operators with published STLF literature.

**Architectural coverage:** Regression / GLM / GAM (L40), tree-based ensembles / XGBoost / LightGBM (L12, L40, L41), LSTM / GRU (L32, L39, L41), CNN+LSTM (L37 background, L7), TCN (L7, L36), Transformer / TFT (L37), GNN (L9, L11, L17, L18, L20, L36), PINN (L8, L14, L15). This is also broad. **Conspicuously underrepresented:** N-BEATS, Prophet (Facebook), classical state-space models (ARMA / SARIMA only mentioned in L40 background).

---

## Part 6 — Recommended next steps

In priority order:

1. **Add Q8 (scaling-law parameter fit) and Q9 (peak-prediction diagnostic) to the Q-sheet as formal questions.** Both are low-cost, high-value, and directly leverage our anchor stack.

2. **Treat Q10 (mechanism-of-error decomposition) as the primary "novel" reportable result** if time permits, since no single paper in our review attempts this synthesis.

3. **Implement the committed insights from Part 1 in order:** F1 feature engineering (sin/cos encoding, lag features per horizon, structural growth trend, bus-id), then F2 evaluation protocol (sequential CV, MAPE+RMSE always together, bus-size-quartile stratification), then F3 LightGBM grouped-global architecture.

4. **Defer conditional insights to diagnostic-triggered implementation:** ACF-on-residuals (only if residual structure suggests missed lags), PWP custom loss (only if peak underprediction is diagnosed), AGCIN-style imputation (only if Q7 baseline shows imputation matters).

5. **Document the future-work questions (Q11–Q20) explicitly in the report's discussion section** as honest acknowledgments and natural extensions. This shows methodological maturity.

6. **Decide on Q19 (TFT baseline) based on compute budget.** Running a single TFT comparison on one zone or a bus-size-stratified subsample would close a real methodological loophole.

7. **Decide whether the two Bucket-1 watch-list candidates (Haben 2014, Hong & Fan 2016) need direct reading** to strengthen the report's citation rigor. Both are now at 3-citation status. Haben 2014 in particular is method-in-use via L40's APN evaluation. **My recommendation: skip both unless the report's reviewer specifically asks for them** — our anchor stack is strong enough without primary-source citation of these two.

---

*End of consolidation document.*
