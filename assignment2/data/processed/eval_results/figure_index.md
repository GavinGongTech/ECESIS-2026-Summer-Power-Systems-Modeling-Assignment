# Notebook 06 — Figure Index

All figures are at `data/processed/eval_figures/`.

## `architectural_comparison_nextday.png`

**THE HEADLINE FIGURE** — 4-panel comparison of zone-direct + weather vs global-bus + weather for nextday: aggregate, per-zone, per-hour, cold-start. Single best figure for the report. (Cell 7)

## `architectural_comparison_nextmonth.png`

**THE HEADLINE FIGURE** — same 4-panel structure for nextmonth. Shows zone-direct dominates zone-level metrics. (Cell 7)

## `cold_start_wmape_nextday.png`

Cold-start vs non-cold-start WMAPE per model. Shows zone-direct's catastrophic 8.8× penalty and global-bus's 1.19× near-no-penalty. (Cell 6)

## `cold_start_wmape_nextmonth.png`

Same structure for nextmonth. (Cell 6)

## `per_hour_wmape_nextday.png`

WMAPE by hour-of-day (HE 1-24) for all 7 models, nextday. Two-panel: top shows WMAPE lines; bottom shows mean actual load for diurnal context. Reveals flat diurnal pattern for global-bus + weather. (Cell 5)

## `per_hour_wmape_nextmonth.png`

Same structure as above for nextmonth. Reveals counterintuitive early-morning weakness. (Cell 5)

## `per_zone_wmape_bus_nextday.png`

Per-zone bus-level WMAPE for nextday — grouped bars per model. Shows zone-by-zone variation in bus-level forecast quality. (Cell 4)

## `per_zone_wmape_bus_nextmonth.png`

Per-zone bus-level WMAPE for nextmonth — grouped bars per model. (Cell 4)

## `per_zone_wmape_zone_nextday.png`

Per-zone zone-level WMAPE for nextday — grouped bars per model. Shows that zone-direct + weather wins decisively in metropolitan zones. (Cell 4)

## `per_zone_wmape_zone_nextmonth.png`

Per-zone zone-level WMAPE for nextmonth — zone-direct + weather wins all 8 zones. (Cell 4)

## `scaling_laws_nextday.png`

Per-bus RMSE vs per-bus mean load on log-log axes, all 7 models with power-law fits. Replicates Sevlian & Rajagopal (2018) α ≈ 0.5-0.6 in the ERCOT context. (Cell 9)

## `scaling_laws_nextmonth.png`

Same structure for nextmonth. Reveals global-bus nextmonth anomalous near-flat scaling. (Cell 9)

## `weather_lift_per_zone_nextday.png`

Per-zone weather lift for nextday — bus and zone level. Shows FWES as weather-resistant outlier. (Cell 8)

## `weather_lift_per_zone_nextmonth.png`

Per-zone weather lift for nextmonth — shows NCEN reaches +62.5% zone-direct zone-level lift. (Cell 8)

## `weather_lift_summary.png`

Aggregate weather lift across the 2×2 ablation matrix. Single chart showing how much weather features helped each (architecture, granularity, task). Headline: zone-direct nextmonth zone-level +50.2%. (Cell 8)
