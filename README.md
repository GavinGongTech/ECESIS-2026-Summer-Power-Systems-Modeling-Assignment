# ECESIS 2026 Summer Power Systems Modeling Internship

Gavin Gong — Assignment submissions for the Ecesis Investments 2026 Summer Power Systems Modeling Internship.

## Assignment 1: Constraint Mapping Across Data Sources

Maps PJM Market, Dayzer (CES/PowerGEM), and Panorama (Enverus) transmission constraint datasets into a unified output with honest confidence scoring.

- **Notebook:** `assignment1/notebooks/constraint_mapping.ipynb`
- **Output:** `assignment1/output/matched_constraints.csv`
- **Report:** `assignment1/report/Assignment_One_Report.pdf`
- **Coverage:** 73.3% of Market constraints matched (5,300 output rows)
- **Validation:** 82.5% manual spot-check accuracy, 99.9% contingency type agreement

### Methodology
Three-layer matching pipeline: exact string matching → fuzzy matching (RapidFuzz) → ML semantic matching (sentence-transformers). Domain heuristics grounded in PJM Manual 03/03A including voltage-level hard filters, base case normalization, directional symmetry, and Transfer Interface canonical lookup.

## Assignment 2
Coming soon.

## Assignment 3
Coming soon.

## Requirements

```bash
pip install -r requirements.txt
```
