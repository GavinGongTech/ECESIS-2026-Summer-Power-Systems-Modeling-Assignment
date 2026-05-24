# Primary Backlog Refinements from Literature Review
*Companion to BACKLOG_PRIMARY.md. Documents specific lit-review-driven refinements to existing primary-backlog items + new items emerging from the literature review. The primary backlog structure (M1, M2, D1-D4, B1, F1-F3, E1-E3) is unchanged. These are refinements within items, plus new G-items and S-items, not a restructuring.*

*Last updated to reflect: assignment instructions in context; honest reframing of the deep-learning defense as a time-budget choice rather than a methodological-superiority claim; GPU availability enabling a staged Stage-1/Stage-2 strategy; depth-1 LightGBM added as the simple-ML baseline; assignment-feature-by-feature literature verdict; final scoping decision on physics, geospatial, weather, and sector features.*

---

## Stage 1 vs Stage 2 framing (NEW — drives the whole pipeline structure)

The literature review and the assignment together support a two-stage submission strategy:

**Stage 1 (primary submission, mandatory, 5-6 days of work):**
- Notebooks 01-06 as specified
- sNaïve baselines + depth-1 LightGBM + full LightGBM
- Direct global-bus model + zone+top-down baseline (Q1 architecture comparison)
- All stratified evaluation per E1 (refined)
- Q4/Q8 scaling-law fit, Q5 zone-vs-bus-sum, Q9 peak diagnostic, optionally Q6 cluster ablation and Q7 imputation ablation

**Stage 1.5 (optional enhancement, decide AFTER notebooks 01-03 complete, choose AT MOST ONE):**
- **Weather features** (recommended if pursued): Acquire zone-level hourly temperature + humidity from NOAA ASOS (8 stations mapped to ERCOT zones: COAS→IAH/HOU, NCEN→DFW, SCEN→AUS/SAT, NRTH→LBB/AMA, FWES→MAF, EAST→TYR, SOUTH→CRP/BRO, WEST→sparser). Alternatively use ERA5-Land Copernicus reanalysis (the L34/L36 standard). 3-4 days of work. Framed as research question Q10 (see Q-sheet). **Critical leakage caveat:** for 2025 evaluation, must use NWS forecast archive or *lagged* actual temperature — using same-day temperature actuals would be leakage. L5 handles this correctly: lagged-regressor neural network treats temperature as forecast input available at compute time, not actual.
- **Physics-informed soft coherence loss** (alternative if pursued): Modify LightGBM loss to penalize `sum(bus_predictions) - zone_prediction` deviations. Cheap version of L8 (Zou et al. 2025) physics-informed disaggregation. 1-2 days. Less reportable than weather because we already test coherence post-hoc via Q5.

**Stage 2 (extension, stretch, 2-3 days of additional work if Stage 1 finishes early, GPU required):**
- Global Temporal Fusion Transformer (TFT, Lim et al. 2021; L37 anchor) with bus-id as a static covariate, or Global PatchTST (2023)
- Trained on the same feature matrix as the LightGBM model
- Same evaluation framework as Stage 1
- Reported as a "Stage 2 results" section in the report or as a "future iterations" note

**Stage 3 (cited only, not run):**
- JEPA-family architectures, foundation models (Chronos, TimeGPT)
- Bus-level weather via ERA5-Land grid interpolation (8-12 days, future work per L34/L36)
- Bus-level geospatial via forensic geocoding (3-5 days, future work per L35/L5 future-work)
- L8-style full physics-informed disaggregation NN with embedded power-flow constraints
- Real physics-aware models with calibrated network topology (L34-E power-flow sensitivities require network model we don't have)
- Classical PINNs with electromagnetic/PDE physics (wrong domain — load forecasting at hourly resolution is not constrained by Maxwell/Navier-Stokes/etc.; the physics-relevant constraint at our scale is hierarchical coherence, which we already test in Q5)
- Limitations + future-work section in the report

**Why this staging:** Stage 1 produces a complete, defensible submission with the strongest results we can produce in the available time. Stage 2 is the "above and beyond" component. The staging framing also lets the report honestly state "LightGBM is the appropriate primary architecture given [constraints]; we extend to TFT in Stage 2 for comparison" rather than overclaiming "LightGBM dominates DL." The literature review supports this honest framing because the five-pronged LightGBM defense is genuinely a constraint argument (scale, deployability, time budget, CPU-feasibility, practitioner-philosophy consensus), not a methodological-superiority claim.

---

## B1 — Naive baselines (refined after L5 anchor-tier rewrite + depth-1 LightGBM added)

Original B1 specified three baselines. Refined to five.

**Final B1 list:**
1. **sNaïve t-168h** (same hour last week) — strong baseline standard
2. **sNaïve t-48h** (per L5-D, operationally grounded — t-24h not available at compute time for day-ahead forecasts in TSO context) — new
3. **sNaïve t-365d** (same hour last year) — warning baseline; L29/F2 predict this will systematically underpredict FWES/NOTH in 2025 due to +60%/+51% structural growth; if our ML model does not substantially beat this in those two zones, it has not learned the trend
4. **Historical hourly average** per (season, weekday, hour) — calendar-only baseline
5. **Depth-1 LightGBM** with same features as full LightGBM but max_depth=1, num_leaves=2 (NEW — the "simple ML" floor) — tests whether tree splits and interactions add value beyond the lag-feature signal; operationalizes the L40-B caveat that "method-family choice matters less than feature engineering"

**Why depth-1 LightGBM matters:** if it ties full LightGBM, the features are doing all the work and we have a reportable finding. If full LightGBM beats it substantially, the interactions matter. Either result is informative.

---

## F1 — Feature engineering (refined after L5, L12, L37, L39, L40, L41 reviews; verified against assignment instructions)

The assignment proposes: hour of day, day of week, month, weekend/holiday flag, lagged load values, rolling averages, historical bus share within each zone. Each verified against literature.

**Required time/calendar features (assignment-mandated, lit-review-confirmed):**
- `hour` — top importance in L12-C, L37-A — cyclic-encoded as `(sin(2πh/24), cos(2πh/24))` per L39-A
- `day_of_week` — top-3 in L12-C, L37-A — cyclic-encoded as `(sin(2πd/7), cos(2πd/7))`
- `month` — mid-rank in L12-C — cyclic-encoded as `(sin(2πm/12), cos(2πm/12))`
- `day_of_month` — cyclic-encoded as `(sin(2πd/31), cos(2πd/31))`
- `weekend_flag` — binary, free
- `holiday_flag` — binary, near-bottom importance in L12-C but free; use the Python `holidays` library (federal US holidays) per L12's approach

**Why cyclic encoding:** integer encoding breaks the wraparound — hour 23 is adjacent to hour 0 in reality but encoded as 23 vs 0 looks far apart to a tree. Empirically supported across L39-A and standard modern practice. Free improvement.

**Required lag features (assignment-mandated; literature specifies which lags):**
- `lag_1h` — top-2 importance in L12 Greek STLF
- `lag_24h` — top-3 importance, captures daily seasonality residual
- `lag_168h` — top-6 importance, captures weekly seasonality
- `lag_336h` (= 2 weeks) — optional, sometimes useful for stability
- For **next-month forecast specifically**: `lag_30d` and `lag_365d` per L41-C. Note the data availability caveat: lag_365d only has valid training rows from 2023-2025 against a 2022-start training window. Document this in the report.

**Required rolling-mean features (assignment-mandated; literature specifies which windows):**
- `rolling_mean_3h` — top-4 importance in L12
- `rolling_mean_24h` — captures recent daily-shape trend
- All rolling means must be lagged ≥1 day for next-day, ≥1 month for next-month — F1 leakage constraint

**Bus-share (assignment-mandated, METHODOLOGICAL CLARIFICATION):**
- `historical_bus_share` is the *disaggregation operator* for the top-down baseline (zone forecast × share = bus forecast)
- It is **NOT** a feature in the direct global-bus LightGBM model — adding it as a feature would be redundant with the bus-id feature and would conflate two distinct methodological roles
- Share computed on training years only (2022-2024) — leakage check per F1
- Share = mean(bus_pd) / mean(zone_pd) over non-null hours in training window

**Optional structural features from the provided data:**
- `bus_id` — categorical, mandatory for global model identity per L5-B
- `zone_name` — categorical, will be split on naturally by LightGBM
- `base_kv` — proxy for voltage level (sub-transmission 69kV vs transmission 138/161/230/345/500/765kV) — test as feature
- `bus_type` — LOAD/GEN/SWING; partially redundant with bus-id; test
- `load_bus_count`, `gen_bus_count` (zone-level features per D3) — zone-activity proxies; test

**Conditional summer/winter daily seasonality (per L5-G):**
- L5 fits separate daily seasonality components for April-September vs October-March
- LightGBM analog: add `season_indicator` (summer=1 for Apr-Sep, winter=0 for Oct-Mar) as a categorical feature
- LightGBM splits will model `season × hour` interaction implicitly via tree depth
- Test as ablation: with vs without season indicator

**ACF/PACF lag selection on residuals (per L40-C):**
- Compute autocorrelation on residuals of a calendar-only baseline, not on raw signal
- Raw-signal ACF is dominated by already-captured seasonality and is misleading
- Use to confirm which additional lags carry predictive signal beyond calendar features
- Optional refinement; useful if standard lags underperform

---

## F2 — Structural load growth (refined after L5-K, L29, L31, L40 reviews)

Original F2 said "include year or a monotonic trend feature."

**Refinement:** The scaling-law literature (L29 cited as L5-K; L31 Haben; L40 Pinheiro) supports more careful handling.

**Options ranked by complexity:**
1. **Simplest (recommended):** `year` as integer feature + per-zone interaction. LightGBM splits will pick up the +60%/+51% FWES/NOTH growth automatically if `year` is allowed to interact with `zone`.
2. **More principled:** Per-zone monthly mean residualized against zone-and-year — use as a trend feature.
3. **L5-style:** Piecewise-linear trend with explicit changepoints — overengineered for our timeframe; L5 uses `n_changepoints=0` for the same reason (only 4 years training data).

**Recommendation:** Start with option 1 (year + year×zone interaction). Validate by checking per-zone WMAPE — if FWES/NOTH WMAPE is comparable to other zones, the trend is captured. If FWES/NOTH WMAPE is substantially higher, escalate to option 2.

---

## F3 — Winter Storm Elliott handling (refined after L40-G, L41-B reviews)

Original F3 said "consider flagging and downweighting or including as representative of tail risk."

**Refinement:** Two literature-supported methodologies for tail-event handling are available, but only one is mandatory for Stage 1.

**Mandatory (Stage 1):**
- Flag Winter Storm Elliott (Dec 22-25, 2022) in training data as a categorical event feature. LightGBM can learn an event-specific offset.
- Report per-event-bin WMAPE alongside aggregate metrics as a diagnostic (E1 stratification).

**Conditional treatment (Stage 1 stretch or Stage 2):**
- If the diagnostic shows systematic underprediction at peak hours, implement L41-B's PWP custom MSE loss as a LightGBM custom objective. Approximately 10 lines of code; gradient and Hessian both scale by (1 + w·p) where p is a peak-indicator. Sweep w ∈ {0.5, 1, 2, 5, 10}.
- Compare to baseline MSE on aggregate WMAPE + peak-bin MAE + Haben adjusted p-norm error (APN with p=4, w=3 per L40-F).

**Not for Stage 1 (cited only):**
- L40-G WMC ensemble with regime-specific predictors — overengineered for assignment scope.

---

## E1 — Stratified evaluation (refined after L5-H, L29, L31, L40 reviews)

Original E1 said "stratify error analysis by: zone, hour of day, month, bus size quartile."

**Refinement:** Add four lit-review-driven stratifications. Standard E1 still applies; these add depth.

**Existing four stratifications (mandatory):**
- Zone (8 zones; per F2 we expect FWES/NOTH to be hardest)
- Hour of day (0-23; expect peak hours 14-19 in summer to be hardest)
- Month (1-12; expect Aug and Jan to have most peak load)
- Bus size quartile (Q1 small, Q2, Q3, Q4 large by mean training-period pd)

**Added stratifications (lit-review-driven):**

**Bus-size quartile + scaling-law fit (per L29 anchor + L31/L40 cross-country confirmation):**
- Stratify per-bus WMAPE by bus mean-load quartile (this is the standard E1 stratification refined)
- Fit the L29 scaling law `WMAPE(W) = α₀/Wᵖ + α₁` on the (W, WMAPE) pairs across all forecasted buses using `scipy.optimize.curve_fit`
- Report α₀, α₁, p, W★ (break-point where two regimes intersect) with bootstrap CIs
- This becomes the Q8 fit result; a clean fit yields a fourth independent confirmation of the scaling law in a new domain (US transmission-bus), which is itself reportable

**Per-bus error attribution at high-error zone-level timestamps (per L5-H, Figures 7-8):**
- For each top-decile zone-level error timestamp, decompose into per-bus residual contributions
- Classify as co-directional (all buses err in same direction) vs canceling (some buses err in opposite direction, partially offsetting)
- Identify the buses with highest residual contribution to zone-level errors
- Diagnostic; useful for the report's "where does error come from" narrative

**Peak-hour vs off-peak signed-error bias (per L40-F, L41-B, Q9):**
- Compute `E[ŷ - y | peak hours]` vs `E[ŷ - y | non-peak hours]`
- Stratify separately for summer afternoons (Aug 1 - Sep 15, hours 14-19), Winter Storm Elliott (Dec 22-25, 2022), and FWES vs other zones
- Compute Haben adjusted p-norm error (APN with p=4, w=3) alongside MAE/RMSE/WMAPE for peak-sensitive evaluation
- Use as diagnostic to decide whether the F3 PWP custom-loss treatment is warranted

**Q5 zone-vs-bus-sum coherence (per L5 Table 2, L30-A, L34-D, L40-D 4-citation mechanism stack):**
- Compute zone-level WMAPE for: (a) zone-direct LightGBM, (b) sum-of-bus-direct-predictions
- Per L5 Table 2, expect (b) to match or beat (a) on scaled metrics
- Reportable result regardless of direction

---

## M2 — Imputation (refined after L5-F, L12-G reviews)

Original M2 specified linear interpolation for 269 mixed-null buses.

**Refinement:** Adopt L5-F hybrid as M2's primary imputation strategy.

**Updated M2 primary plan:**
- Linear interpolation between adjacent observed hours for null blocks ≤20 consecutive hours
- Rolling-mean imputation for null blocks >20 consecutive hours (L5-F precedent from MISO TSO)

**Q7 imputation comparison (stretch, conditional on time):**
On a sample of 50 mixed-null buses stratified by null block length (10-50h, 50-200h, 200-552h):
- (a) Pure linear interpolation
- (b) Forward-fill from last observed value
- (c) Zone-mean fill (replace null with zone's mean pd at that hour)
- (d) L5-F hybrid (linear interp ≤20h + rolling-mean >20h) — M2 primary plan
- (e) L12-G KNN imputation (Bucket 2 watch item if compute permits)

Compare WMAPE on the 2025 test period. Report whichever wins; if all within ~5%, M2 is robust.

---

## G1 — Report MAE + RMSE + WMAPE trio for every model comparison (per L39-E)

L39-E documents a real failure mode: MAPE improves but RMSE worsens substantially (5.638% vs 6.962% MAPE, but 352.5 vs 215.3 RMSE). This represents *mean improves but tail worsens* — a model that on average predicts better but produces larger extreme errors.

**Mandatory:** Report MAE, RMSE, WMAPE for every model in every comparison. Never report only one absolute and one relative metric. The trio of MAE + RMSE + WMAPE detects this failure mode (MAE and WMAPE move in lockstep; RMSE diverges when tail errors grow).

For the per-bus comparison across magnitudes spanning multiple orders of magnitude, also report MASE/MSSE per L5-D (with t-48h sNaïve as the denominator).

---

## S1 — Sectorization via diurnal-shape clustering (NEW, lit-review-driven; the only "enhancement" axis well-supported AND implementable)

Of the four enhancement axes you asked about (physics, geospatial, weather, sectorization), only sectorization is both implementable with the provided data and well-supported by literature.

**Methodology (per L4 + L27 + L28 + L31-C four-piece anchor stack):**
- Compute normalized average diurnal shape (24-element vector) per bus from 2022-2024 training data
- Run k-means (k=3-5) to assign each bus a cluster label
- Validate cluster proportions against L27-A baseline (67.4% residential / 18.8% commercial / 13.8% industrial on the ACTIVSg2000 ERCOT footprint, 2016 EIA sector composition)
- Include cluster label as a categorical feature in the global LightGBM bus model
- Compare WMAPE with and without the cluster feature (Q6 ablation)

**Reportable findings to watch for:**
- If 2022-2025 clustering produces noticeably more industrial-shape buses than the 2016 baseline of 14%, this is evidence of ERCOT's recent industrial expansion (FWES data centers/crypto/Permian, NOTH growth). Ties S1 directly to F2's structural growth narrative.
- If cluster proportions roughly match L27's 67/19/14 ratio, this validates the clustering against the ERCOT-footprint baseline.

**Why this is included while weather/geospatial/physics are not:**
- Implementable with provided data (no external data acquisition required)
- Well-supported by literature (four-piece anchor stack)
- Low marginal cost (one preprocessing step + one feature in the model + one ablation comparison)
- High potential reportability (S1 ties to F2 structural growth narrative)

---

## Decisions NOT to include (scoping decisions with literature justification)

**No weather features for Stage 1.** Three-reason stack:
1. Not in provided dataset; acquiring NOAA ASOS or ERA5-Land would add 8-15 hours of data engineering
2. Literature is split — 9-piece evidence cluster spanning r=0.017 to r=0.75 weather-load correlation; ERCOT's diversified 8-zone footprint sits in the low-correlation end (L24-A FWES near-zero)
3. At 15,000 buses, single-station-per-zone weather is *more dilutive* than no weather (L34-A, L36-A heterogeneity argument); correct weather use would require bus-specific weather joins which is out of scope
**Limitations section:** acknowledge honestly per L41-E counter-evidence. Future-iteration cite-only.

**No geospatial features for Stage 1.** Bus latitude/longitude not in the provided dataset. The zone identifier is already a coarse geographic feature. L35 (Wang-Majumdar-Rajagopal 2023) shows that real geospatial integration requires real coordinates and substantial engineering; L5 future-work flags as open. Future-iteration cite-only.

**No physics integration beyond base_kv + bus_count features for Stage 1.** L34-E power-flow-derived sensitivity disaggregation requires a calibrated 110kV network model. We don't have one. The extent of physics-awareness available from the provided data: `base_kv`, `bus_type`, `load_bus_count`, `gen_bus_count`, and the SWING bus identification (D4). All included.

**No deep learning for Stage 1.** Honest framing per assignment-aware revision:
- LightGBM is the appropriate Stage 1 primary architecture given (i) scale (15,000 buses), (ii) feature set (lag features + calendar + structural growth trend), (iii) practitioner-philosophy stack (L12 + L34-C + L36-B + L40-E + L41-G), (iv) deployment-feasibility within one-week assignment time budget, and (v) the LightGBM-over-XGBoost memory justification per L12-D
- Modern DL (TFT per L37, PatchTST 2023, JEPA-family, foundation models like Chronos/TimeGPT) is not the LSTM/CNN/GRU of L12 that LightGBM dispatched — citing Grinsztajn et al. 2022 for "tree-based beats DL" in the time-series context is a slight stretch since Grinsztajn is specifically about tabular data
- Stage 2 (post-primary, if time permits with GPU access): global TFT or PatchTST with bus-id as static covariate, same evaluation framework as Stage 1
- Stage 3 (cited only): JEPA-family, foundation models — limitations + future-work

---

## Summary of changes

The primary backlog structure (M1, M2, D1-D4, B1, F1-F3, E1-E3) is unchanged. The refinements operationalize the literature review into pipeline decisions:

- **B1: 3 baselines → 5** (added t-48h sNaïve per L5-D + depth-1 LightGBM as simple-ML floor)
- **F1: vague → specific** (cyclic-encoded calendar, specific lag windows, rolling means, conditional seasonality, residual-based ACF/PACF; bus-share clarified as disaggregation operator not feature)
- **F2: vague → option-1 default** (year + year×zone interaction) **with escalation criteria**
- **F3: vague → mandatory event flag** + diagnostic + **conditional PWP treatment**
- **E1: 4 stratifications → 7** (added bus-size quartile + scaling-law fit + per-bus error attribution + peak-hour signed-error bias + zone-vs-bus-sum coherence per Q5)
- **M2: 1 imputation method → L5-F hybrid as primary** (linear interp ≤20h + rolling-mean fallback), with 5-method Q7 comparison available
- **G1 (NEW):** Mandatory MAE + RMSE + WMAPE trio reporting + MASE/MSSE for cross-magnitude comparison
- **S1 (NEW):** Sectorization via diurnal-shape clustering — the only enhancement axis both implementable and lit-supported
- **Stage 1/2/3 framing (NEW):** explicit two-stage submission strategy with GPU-enabled Stage 2 (TFT/PatchTST) as stretch
- **Explicit scope decisions:** no weather, no real geospatial, no deep physics integration for Stage 1; all moved to limitations + future-work with literature justification

None require new EDA or new data work; all are implementable in the planned notebook 01-06 sequence. Stage 2 (TFT) would be a separate notebook 07 if pursued.
