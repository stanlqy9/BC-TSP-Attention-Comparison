# BC-TSP Comparison Deliverables

Stanley's Attention-based routing workflow compared with Chris Gonzalez's
implemented Greedy 1, Greedy 2, and P-MARL baselines.

## Scope

- Dataset: `Capital_Cities.txt`, 48 US state-capital nodes.
- Instances: first 20 depot-city instances from Chris's `TableData.generateRandomCities` convention.
- Budgets: 4000, 6000, 8000, and 10000 miles.
- Algorithms included: Attention, Greedy 1, Greedy 2, P-MARL.
- Algorithms intentionally excluded: Ant-Q and ILP, because Dr. Tang's direct request did not ask for them.
- Prize metric: collected prize excluding depot prize.
- Distance metric: Java `CityNode.java` Haversine-mile convention (`6371 km * 0.62 miles/km`).

## Main Files

- `summary/bctsp_comparison_summary.docx`: professor-facing Word summary.
- `summary/final_email_draft.txt`: short draft email to send with the deliverables.
- `summary/comparison_results.xlsx`: Excel workbook with summary, raw results, and manifest.
- `summary/comparison_summary_by_budget.csv`: summary statistics by budget and algorithm.
- `summary/comparison_raw_combined.csv`: all 320 per-instance result rows.
- `figures/prize_vs_budget.svg`: mean collected prize plot.
- `figures/distance_vs_budget.svg`: mean route distance plot.
- `figures/runtime_vs_budget.svg`: mean runtime plot.
- `raw/java_baselines.csv`: raw Greedy 1, Greedy 2, and P-MARL results.
- `raw/attention_results.csv`: raw Attention results after feasibility repair.

## Notes

The Attention model uses a normalized OP representation, so decoded routes were
checked again with the same Java-mile Haversine distance used by Chris's code.
When a raw Attention route exceeded the budget after this check, the route suffix
was trimmed until it satisfied the same budget. The final CSV records both the
raw Attention route distance and whether repair was applied.

Chris's P-MARL implementation uses unseeded Java randomness during learning, so
rerunning the Java wrapper can produce slightly different P-MARL rows. The CSV
and workbook preserve the actual run used for this deliverable.

The reviewer-scale 10,000-node request is not claimed as complete here. The
attached code/data supports the 48-city and 10-city capital datasets, but no
10,000-node BC-TSP benchmark dataset or generator was attached.

## Reproduction

Compile the Java baselines from the workspace root:

```bash
javac -d Deliverables/build/java \
  BCPCTSP-handoff-code/CityNode.java \
  BCPCTSP-handoff-code/Graph.java \
  BCPCTSP-handoff-code/Agent.java \
  BCPCTSP-handoff-code/main.java \
  BCPCTSP-handoff-code/TableData.java \
  Deliverables/scripts/java/ComparisonRunner.java
```

Run the Java wrapper from Chris's repo folder because the handoff code expects
`src/Capital_Cities.txt` relative to its current working directory:

```bash
cd BCPCTSP-handoff-code

java -cp "$OLDPWD/Deliverables/build/java" ComparisonRunner \
  "$OLDPWD/Deliverables/raw/java_baselines.csv" \
  20 \
  4000,6000,8000,10000
```

Run the Attention batch with a Python environment that has the Attention repo
requirements installed:

```bash
python Deliverables/scripts/run_attention_batch.py --python /path/to/python-with-torch
```

Regenerate summary outputs:

```bash
python Deliverables/scripts/generate_deliverables.py
```
