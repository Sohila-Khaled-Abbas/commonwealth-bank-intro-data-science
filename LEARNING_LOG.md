# Learning Log — Commonwealth Bank Introduction to Data Science

Use this log to document the analytical process, decisions, validation, and reflections. Update a task's status only after the work is completed and reviewed.

## Task 01 — Data Aggregation and Analysis

**Work date:** 2026-11-13  
**Analysis status:** Complete  
**Formula/PivotTable reconciliation:** Passed  
**Forage submission:** Not yet recorded

| Date | Activity | Techniques practiced | Validation / evidence | Status |
|---|---|---|---|---|
| 2026-11-13 | Completed Task 01 supermarket transaction analysis in Excel | Excel Tables, structured references, data-quality checks, `SUMIFS`, `COUNTIFS`, PivotTables, result interpretation | 50,783 rows audited; five formula metrics matched two PivotTables; findings documented in the Task 01 guide | Analysis complete |

### Work log

- [x] Set up the public project documentation and local folder structure.
- [x] Create an Excel practice companion with formula and PivotTable guidance.
- [x] Download and inspect the original Forage workbook locally.
- [x] Confirm source headers, data types, row grain, and quantity field.
- [x] Convert the source data into an Excel Table named `Transactions`.
- [x] Review completeness, duplicate IDs, numeric values, arithmetic consistency, numeric anomalies, and selected text hygiene checks.
- [x] Calculate the three requested business answers with Excel formulas.
- [x] Validate formula outputs with PivotTables.
- [x] Document the formulas, criteria, results, reconciliation, and limitations in [the Task 01 guide](docs/task-01-data-aggregation-analysis.md).
- [ ] Save and submit the completed analysis workbook through Forage.
- [ ] Add any permitted, non-restricted visual evidence after reviewing the simulation's sharing rules.

### Task 01 result snapshot

| Business metric | Validated result |
|---|---:|
| Apple units purchased in cash | 117 |
| Cash-paid apple transaction records | 20 |
| Total cash spent on apples | $537.03 |
| Bakershire non-member spending | $2,857.51 |
| Bakershire non-member transaction records | 179 |

### Reflection

1. **Why did the first question need `SUMIFS` rather than only `COUNTIFS`?**  
   The table contains a `quantity` field, so counting matching rows would count purchase records rather than individual apple units. `SUMIFS` over `quantity` returned the requested unit total; `COUNTIFS` was retained as a supporting record count.

2. **How were the results validated?**  
   Two PivotTables independently grouped the same transaction table. The apple PivotTable filtered to the exact product `apple` and broke results down by payment method. The Bakershire PivotTable filtered to Bakershire and grouped by customer type. All five formula-based metrics matched the corresponding PivotTable values.

3. **Why was `id` used for the transaction count?**  
   `id` identifies transaction records. `customer_id` identifies customers and counting it does not produce a distinct-customer count; it is not the correct field for counting transaction rows.

4. **What is the most important scope detail in the third question?**  
   The calculation includes every payment method. Only the store and customer-type criteria were applied.

5. **What does the arithmetic check establish—and what does it not establish?**  
   It establishes that `quantity × unit_price`, rounded to two decimal places, agrees with `total_amount` for each checked record. It does not prove the original quantity or price was correct.

## Next task

Proceed to **Task 02 — Data Anonymisation** after saving and submitting the Task 01 workbook through Forage. Keep the overall repository status as in progress until the remaining simulation tasks are completed.
