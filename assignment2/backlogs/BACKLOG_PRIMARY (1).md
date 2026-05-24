# Assignment 2 — Primary Implementation Backlog
*Items established from EDA and data investigation, before literature review.*
*Address these in notebooks 01–06 before considering literature-inspired enhancements.*

---

## 🔴 CRITICAL — Must resolve before any model training

### M1 — Missing (date, he) pairs in zone and bus datasets
A naive sequential model will misalign all observations following a gap.
Reindex both datasets to a complete hourly time grid before feature construction.

**Confirmed missing (date, he) pairs — zone dataset:**

| Year | Missing pairs | Notes |
|------|--------------|-------|
| 2022 | (2022-03-13, 24), (2022-05-18, 16), (2022-07-13, 1), (2022-07-20, 24), (2022-12-01, 9), (2022-12-12, 18) | DST spring-forward expected. Remaining 5 unexplained. |
| 2023 | (2023-03-12, 24), (2023-01-09, 19), (2023-01-12, 9), (2023-01-26, 9), (2023-04-13, 17), (2023-10-04, 1), (2023-12-05, 22) | DST expected. Remaining 6 unexplained. |
| 2024 | (2024-03-10, 24), (2024-01-05, 7), (2024-04-15, 23), (2024-04-27, 24), (2024-06-04, 19), (2024-10-01, 3) | DST expected. Remaining 5 unexplained. |
| 2025 | (2025-03-09, 24), (2025-05-01, 1), (2025-08-18, 9), (2025-09-22, 10), all 24 hours of 2025-12-04 | DST expected. 3 unexplained isolated hours. 2025-12-04 entirely missing — no known cause. |

Total missing: 47 (date, he) pairs across 4 years.

**Required treatment:**
- DST HE24 gaps: do not impute — these hours do not exist in Central Time.
- Unexplained isolated single-hour gaps: linear interpolation between adjacent hours.
- 2025-12-04: flag entirely missing in test set. Do not impute. Report in evaluation that ground truth is unavailable for this date.

---

### M2 — Null pd preprocessing decision
Three structurally distinct categories confirmed empirically for LOAD-only buses (never switch to ISOLATED):

| Category | Count (2022) | Null pattern | Required treatment |
|----------|-------------|-------------|-------------------|
| Always null | 2,137 buses | 100% null, flat across all 24 hours | Exclude from forecasting entirely. No historical signal exists. Document as explicit modeling decision. |
| Never null | 2,729 buses | 0% null, always report pd | Primary forecasting targets. No imputation needed. |
| Mixed null | 269 buses | Null in contiguous blocks (10–552 hours). Consistent with planned/unplanned outages. | Impute null periods before training. Use linear interpolation or forward-fill from adjacent hours. |

Additional confirmed behaviors:
- ISOLATED-type buses: pd is null (91.8%) or exactly 0.0 (8.2%). Never positive. Treat all as zero load.
- 4,425 buses switch LOAD↔ISOLATED: when ISOLATED, treat pd as zero.
- LOAD buses with pd=0.0: valid measured state, distinct from null. Do not impute.
- GEN-type buses with pg=0.0: generator connected but not dispatched. Valid state, not missing.

Key distinction: pd=null and pd=0.0 are NOT equivalent. Null means absent measurement. Zero means measured zero consumption. This distinction must be preserved in the feature matrix.

---

## 🟡 HIGH PRIORITY — Address in notebook 02 (preprocessing)

### D1 — Drop placeholder buses before any analysis or modeling
The 6 placeholder buses (00000000_0KV_1 through _6) must be dropped.

Confirmed characteristics:
- Always ISOLATED type
- Always pd=null or pd=0.0
- Only buses in the dataset that switch zones — appear across all 9 zones including ISOLATED
- base_kv=0.0 (physically meaningless)
- Violate the bus naming convention documented in C2 Attachment C

No citation explicitly labels them as placeholders but the evidence is unambiguous. Document the drop decision with these observations.

---

### D2 — Confirm ISOLATED zone rows are excluded from forecasting
Zone dataset: ISOLATED zone has pd=0.0, pg=0.0, load_bus_count=0, gen_bus_count=0 for every row across all 4 years. Zero nulls. No forecastable signal. Drop from zone-level model targets.

---

### D3 — load_bus_count and gen_bus_count confirmed definitions
- load_bus_count = count of ALL bus types where pd is NOT NULL at that (date, he)
- gen_bus_count = count of ALL bus types where pg is NOT NULL — in practice GEN-type + SWING bus (CPSES_22KV_2)

Generators with pg=0.0 ARE counted. Buses with pd=0.0 ARE counted.
These columns can be used as features — they proxy for how many buses are active per zone per hour.

---

### D4 — SWING bus treatment
One SWING bus: CPSES_22KV_2 (Comanche Peak nuclear, NCEN zone).
- pd=0.0 always (PSS/E reference bus convention, not a forecasting target)
- pg>0 always (~1,226 MW nuclear baseload)
- Contributes to NCEN gen_bus_count
- pg can be used as a zone-level generation feature for NCEN

---

## 🟡 HIGH PRIORITY — Address in notebook 03 (baselines)

### B1 — Three required baselines
1. Same hour last week: prediction for (date D, hour H) = actual pd at (D-7, H)
2. Same hour last year: prediction for (date D, hour H) = actual pd at (D-365, H)
3. Historical hourly average: mean pd at hour H across same-season same-weekday rows in training years

Baseline 2 is the most dangerous for our problem. Structural load growth in FWES (+60%) and NOTH (+51%) means last year's values will systematically underpredict 2025 in those zones. If an ML model does not substantially outperform this baseline in FWES and NOTH, it has not learned the structural growth component.

---

## 🟡 HIGH PRIORITY — Address in notebook 04 (zone models)

### F1 — Leakage prevention
- Next-day forecast: features must be lagged ≥1 day. No same-day features.
- Next-month forecast: features must be lagged ≥1 month.
Verify all features against this constraint before training.

---

### F2 — Structural load growth must be modeled explicitly
FWES: +60.3% load growth 2022→2025 (data centers, crypto, Permian Basin industrial expansion)
NOTH: +51.1% load growth 2022→2025 (Lubbock Power & Light integration)
ERCOT system AAGR 2025–2031: 13.6%

A model trained on 2022–2024 averages will systematically underpredict 2025 in FWES and NOTH unless trend is explicitly captured. Include year or a monotonic trend feature. Evaluate per-zone WMAPE to confirm whether FWES/NOTH error exceeds other zones.

Source: C1 (ERCOT 2025 Long-Term Load Forecast Report, April 8, 2025).

---

### F3 — Winter Storm Elliott (December 2022) is in training data
Extreme cold event with anomalous demand spikes. Consider flagging and downweighting or including as representative of tail risk. Document the choice.

---

## 🟡 HIGH PRIORITY — Address in notebook 06 (evaluation)

### E1 — Evaluation metrics
Report at both bus level and zone level:

| Metric | Bus level | Zone level |
|--------|-----------|-----------|
| MAE | ✓ | ✓ |
| RMSE | ✓ | ✓ |
| WMAPE | ✓ (exclude pd=0 rows) | ✓ (exclude zero rows) |

Stratify error analysis by: zone, hour of day, month, bus size quartile.

---

### E2 — 2025-12-04 missing from test set
All 24 hours of 2025-12-04 are absent from both bus and zone datasets. Ground truth does not exist for this date. Evaluation metrics over the full 2025 test year must note this gap explicitly.

---

### E3 — Zone-level aggregation check
Sum predicted bus pd values to zone total and compare against actual zone pd from zone_load_2025.parquet. Confirms internal consistency of bus-level predictions at zone level.

---

## 📌 Citation and empirical findings inventory

### Confirmed citations
| ID | Source | What it confirms |
|----|--------|-----------------|
| C1 | ERCOT 2025 Long-Term Load Forecast Report (Apr 8, 2025) | Zone forecasting features, structural growth drivers (FWES +60%, NOTH +51%), ERCOT AAGR |
| C2 | ERCOT Planning Guide Section 6 | Bus naming convention (Attachment C), ISOLATED definition, SWING bus, base_kv, zone assignment |
| C3 | Figure 1 from C1 | Geographic mapping of 8 weather zones |
| C5 | ANSI C84.1-20XX Table 1 | Standard nominal transmission voltages: 69, 115, 138, 161, 230, 345, 500, 765 kV |

### Empirical findings (no external citation — derived from data analysis)
- E-NULL-1: Always-null LOAD buses (2,137) have flat null rate across all 24 hours — structural, not temporal
- E-NULL-2: Mixed-null LOAD buses (269) have contiguous null blocks consistent with outages
- E-NULL-3: pd=0.0 and pd=null are distinct states with different physical meanings
- E1: Zone pd = sum of bus pd (null treated as 0) to within 0.045 MW rounding — confirmed across multiple sample dates and years
- E2: load_bus_count = count of all bus types with pd NOT NULL at that (date, he)
- E3: gen_bus_count = count of all bus types with pg NOT NULL — GEN type + SWING bus only
- E4: 00000000_0KV buses are the only zone-switching buses in the dataset
- E5: ISOLATED zone has pd=0, pg=0, load_bus_count=0, gen_bus_count=0 for all rows, all years
