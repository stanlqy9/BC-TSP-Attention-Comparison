# BC-TSP Comparison Deliverable Summary

Generated: 2026-06-05 13:54:14

## Scope

This deliverable compares Stanley's Attention-based routing workflow against Chris Gonzalez's implemented Greedy 1, Greedy 2, and P-MARL baselines on the same 48-city BC-TSP capital-city instances.

Ant-Q and ILP are intentionally excluded because Dr. Tang's direct request named Greedy 1, Greedy 2, and P-MARL only.

## Experiment Setup

- Dataset: `Capital_Cities.txt` from Chris's `BCPCTSP-handoff-code/src` folder.
- Instances: first 20 depot cities used by Chris's `TableData.generateRandomCities` convention.
- Budgets: 4000, 6000, 8000, and 10000 miles.
- Metric used for comparison: collected prize excluding depot prize.
- Distance convention: Java `CityNode.java` Haversine miles, using `6371 km * 0.62 miles/km`.
- Attention model: pretrained `op_dist_50`, decoded greedily unless the run script is changed.
- Attention feasibility note: 17 of 80 raw Attention routes were trimmed after decoding so the final reported route satisfies the same Java-mile budget; final infeasible rows: 0.
- Reproducibility note: Chris's P-MARL implementation uses unseeded Java randomness during learning, so rerunning the Java wrapper can produce slightly different P-MARL rows.

## Summary Table

| Budget | Algorithm | n | Mean prize | Std prize | Mean distance | Mean runtime ms |
|---:|---|---:|---:|---:|---:|---:|
| 4000 | Attention | 20 | 711.7 | 149.8 | 3883.1 | 3.3 |
| 4000 | P-MARL | 20 | 966.9 | 166.5 | 3875.0 | 2282.6 |
| 4000 | Greedy 2 | 20 | 947.2 | 134.1 | 3913.1 | 0.2 |
| 4000 | Greedy 1 | 20 | 475.9 | 82.6 | 3879.0 | 0.2 |
| 6000 | Attention | 20 | 1029.3 | 202.6 | 5853.5 | 3.6 |
| 6000 | P-MARL | 20 | 1365.0 | 163.2 | 5860.7 | 3356.7 |
| 6000 | Greedy 2 | 20 | 1384.7 | 98.8 | 5955.1 | 0.1 |
| 6000 | Greedy 1 | 20 | 729.3 | 115.3 | 5911.4 | 0.0 |
| 8000 | Attention | 20 | 1381.2 | 175.4 | 7857.3 | 4.4 |
| 8000 | P-MARL | 20 | 1729.6 | 152.0 | 7851.2 | 4103.7 |
| 8000 | Greedy 2 | 20 | 1724.0 | 83.7 | 7874.2 | 0.1 |
| 8000 | Greedy 1 | 20 | 746.5 | 66.6 | 7928.7 | 0.0 |
| 10000 | Attention | 20 | 1683.7 | 136.0 | 9871.9 | 5.3 |
| 10000 | P-MARL | 20 | 2038.3 | 152.2 | 9816.4 | 4692.9 |
| 10000 | Greedy 2 | 20 | 2046.5 | 78.9 | 9882.1 | 0.1 |
| 10000 | Greedy 1 | 20 | 970.2 | 125.5 | 9912.6 | 0.0 |

## Best Mean Prize By Budget

- 4000 miles: P-MARL with mean prize 966.9.
- 6000 miles: Greedy 2 with mean prize 1384.7.
- 8000 miles: P-MARL with mean prize 1729.6.
- 10000 miles: Greedy 2 with mean prize 2046.5.

## Reviewer-Scale Note

The reviewer mentioned large-scale evidence around 10,000 nodes. The attached code/data supports the 48-city and 10-city capital datasets, but no 10,000-node BC-TSP instance generator or benchmark dataset was attached. This should be raised as a next-step experiment rather than claimed as completed.

## Files

- `summary/comparison_raw_combined.csv`: all per-instance raw results.
- `summary/comparison_summary_by_budget.csv`: mean and standard deviation table.
- `summary/comparison_results.xlsx`: Excel workbook with raw results, summary, and manifest.
- `figures/prize_vs_budget.svg`, `figures/distance_vs_budget.svg`, `figures/runtime_vs_budget.svg`: plots.