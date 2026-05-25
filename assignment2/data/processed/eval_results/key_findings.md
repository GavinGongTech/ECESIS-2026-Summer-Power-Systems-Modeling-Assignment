# Notebook 06 — Key Findings (Report Scaffolding)

This document distills the substantive findings from notebook 06's evaluation, structured to
mirror the report's findings sections. Numbers are direct quotes from the analysis cells;
narrative interpretation is condensed.

## 1. Architectural recommendation (the assignment's headline question)

**Direct bus forecast (global-bus LightGBM) vs Zone forecast + bus share (zone-direct LightGBM + disaggregation):**

The architectural answer is regime-dependent:

| Scenario | Best architecture | Best WMAPE |
|---|---|---|
| Nextday + bus-level accuracy | global_bus_lgbm_weather | 7.51% |
| Nextday + zone-level accuracy | zone_direct_lgbm_weather | 4.10% (essentially tied with global-bus 4.73%) |
| Nextmonth + bus-level accuracy | global_bus_lgbm_weather | 22.35% |
| Nextmonth + zone-level accuracy | zone_direct_lgbm_weather | 4.75% (decisive — global-bus 8.20%) |
| Grid with entity churn (new buses) | global_bus_lgbm_weather (1.19× cold-start penalty vs 8.82× for zone-direct) |

**Why this happens:**

- Top-down disaggregation (zone-direct) optimizes the zone-level target directly, then spreads
  via fixed hour-of-day shares. Excellent zone aggregates; loses bus-specific signal.
- Direct bus modeling (global-bus) optimizes bus-level target with bus identity as a feature.
  Excellent at bus level; zone aggregates accumulate per-bus noise.
- Top-down's share-fallback fails on cold-start buses because cold-start buses are systematically
  smaller than the average bus the fallback assumes. Global-bus degrades gracefully because
  non-identity features still carry signal.

## 2. Weather feature impact

Adding weather features (temperature observations and trailing means; notebooks 02w/04b/05b)
improved every model on every aggregate metric, with magnitudes:

| Configuration | Lift |
|---|---|
| Zone-direct nextmonth zone-level | **+50.2%** (single largest improvement) |
| Zone-direct nextday zone-level | +31.6% |
| Global-bus nextday zone-level | +24.9% |
| Global-bus nextmonth zone-level | +19.0% |
| Global-bus nextday bus-level | +15.0% |
| Zone-direct nextmonth bus-level | +7.1% |
| Global-bus nextmonth bus-level | +4.1% |
| Zone-direct nextday bus-level | +2.5% |

**Key sub-findings:**

- Each architecture exploits weather best at its native optimization target. Zone-direct gets
  31-50% zone-level lift but only 2-7% bus-level lift. Global-bus gets 15% bus-level nextday
  lift; zone-level lift comes through aggregation rather than direct optimization.
- FWES (Far West Texas, industrial growth zone) is weather-resistant. In 6 of 8 (architecture,
  granularity, task) cells, weather features make FWES *worse* (lifts of -1% to -8%).
- Metropolitan zones (COAS, NCEN, SCEN) show the largest lift; NCEN nextmonth zone-level
  hits +62.5%.
- Weather provides essentially no lift for zone-direct cold-start predictions (-0.03% to
  -0.95%) and only modest lift for global-bus cold-start (0.14-3.75%).

## 3. Where models work well and where they fail

**Models work well:**

- Metropolitan zones (COAS, NCEN, SCEN, SOUT): bus-level WMAPE 6.5-7% for global-bus +
  weather; zone-level WMAPE 3.4-4.7% for zone-direct + weather.
- Predictions at hour 13-19 (mid-afternoon): all LightGBM models achieve slight WMAPE
  improvements vs early-morning hours.
- Non-cold-start buses (3,911 of 3,953 = 99% of universe): all sophisticated models
  perform competently.

**Models fail (or struggle):**

- NOTH (Lubbock/Panhandle): bus-level WMAPE up to 60% for zone-direct nextday, 24% for
  global-bus. Smallest zone with sparse buses. sNaïve `prev_recent` actually beats LightGBM
  at NOTH bus-level (12.4% vs 23.3%).
- WEST (San Angelo): bus-level WMAPE 35% nextmonth — second-worst zone. Sparse industrial
  load.
- FWES (Far West Texas): weather features hurt rather than help. Industrial oil & gas load
  weakly correlated with temperature.
- Cold-start buses (42 of 3,953): zone-direct catastrophically fails (WMAPE 220%+) due to
  share-fallback assumption failure. Global-bus degrades gracefully (8.97% nextday).

## 4. Scaling-law findings (Sevlian & Rajagopal 2018 replication)

Per-bus RMSE vs per-bus mean load follows a power law `log(rmse) = α × log(load) + β`.

**Nextday: α ≈ 0.5-0.6, consistent with Sevlian & Rajagopal's empirical 0.5.**

| Model | α | R² |
|---|---|---|
| Global-bus nextday | 0.62 | **0.74** (cleanest fit) |
| Global-bus nextday + weather | 0.58 | 0.69 |
| Zone-direct nextday + weather | 0.57 | 0.54 |
| sNaïve previous year | 0.50 | 0.53 |
| sNaïve prev_recent | **0.44** | 0.53 (lowest slope; uniform relative accuracy) |

**Adding weather flattens the slope** (zone-direct: 0.59 → 0.57; global-bus: 0.62 → 0.58),
meaning weather provides relatively more benefit for larger buses than smaller buses.
Consistent with weather signal scaling with metropolitan AC load.

**Nextmonth: global-bus shows anomalously shallow scaling (α=0.28, R²=0.31).** The global-bus
model regresses toward baseline predictions for small buses at long horizons, producing a
near-flat scaling stripe in the scatter plot. The scaling-law framework starts breaking down
for long-horizon forecasts.

## 5. Negative-prediction diagnostics (architectural property)

Zone-direct models produce 0% negative predictions (structural guarantee: zone forecast ×
non-negative share = non-negative).

Global-bus + weather nextday produces 2.11% negative predictions (683,157 of 32M rows),
clipped at 0 before metrics. The global-bus architecture must enforce physical feasibility
via post-hoc clipping; zone-direct provides this for free.

## 6. What would I improve with more time

1. **Replace zone-direct's mean-share cold-start fallback with a meta-learned cold-start
   predictor.** Could close most of the 8.8× cold-start penalty.
2. **Ensemble or hierarchically reconcile (MinT)** the two architectures. Zone-direct's
   zone-level accuracy + global-bus's bus-level accuracy + cold-start robustness in
   one system. Notebook 07's planned MinT reconciliation addresses this.
3. **Add quantile regression** for uncertainty intervals (notebook 08 planned).
4. **Add a deep learning baseline** (PatchTST or NHITS) to test whether tree models are
   genuinely SOTA on this problem (Stage 2 future work).
5. **Switch from observed weather to forecast weather** for fair production simulation. We
   used observed temperatures; deployed systems would use forecast temperatures whose error
   compounds with load forecast error.
6. **Refine the FWES handling.** Either drop weather features for industrial zones, use
   industrial-specific features (production schedules), or train a separate FWES model.
