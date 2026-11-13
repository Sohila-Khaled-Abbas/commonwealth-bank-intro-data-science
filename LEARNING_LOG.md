# Learning Log — Commonwealth Bank Introduction to Data Science

Use this log to document the analytical process, decisions, validation, and reflections. Update a task's status only after the work is completed and reviewed.

## Task 01 — Data Aggregation and Analysis

**Work date:** 2026-11-13  
**Analysis status:** Complete  
**Formula/PivotTable reconciliation:** Passed  
**Forage submission:** Not yet recorded

| Date | Activity | Techniques practiced | Validation / evidence | Status |
|---|---|---|---|---|
| 2026-11-13 | Completed Task 01 supermarket transaction analysis in Excel | Excel Tables, structured references, data-quality checks, SUMIFS, COUNTIFS, PivotTables, result interpretation | 50,783 rows audited; five formula metrics matched two PivotTables; findings documented in the Task 01 guide | Analysis complete |

### Task 01 work log

- [x] Inspect the source workbook and confirm the data structure.
- [x] Convert the data into an Excel Table named Transactions.
- [x] Review completeness, duplicate IDs, numeric values, arithmetic consistency, numeric anomalies and selected text hygiene checks.
- [x] Calculate the business answers using Excel formulas.
- [x] Validate the formula outputs with two PivotTables.
- [x] Document formulas, criteria, results, reconciliation and limitations.
- [ ] Record Forage submission of the completed workbook.

### Task 01 validated results

| Business metric | Result |
|---|---:|
| Apple units purchased in cash | 117 |
| Cash-paid apple transaction records | 20 |
| Total cash spent on apples | $537.03 |
| Bakershire non-member spending | $2,857.51 |
| Bakershire non-member transaction records | 179 |

### Task 01 reflection

1. The quantity total and record count answer different questions: SUMIFS over quantity calculates units; COUNTIFS counts matching rows.
2. Two PivotTables independently reconciled all five formula metrics.
3. id was used for transaction counts; counting customer_id would count nonblank customer identifiers, not distinct transactions.
4. Bakershire spending included all payment methods because no payment-method condition was applied.
5. The arithmetic check confirmed internal consistency between quantity, unit price and total amount, not the commercial correctness of the source values.

## Task 02 — Data Anonymisation

**Preparation date:** 2026-11-13  
**Status:** Research and workflow documented; source workbook schema still needs inspection  
**Required output:** CSV file submitted through Forage

| Activity | Techniques / focus | Status |
|---|---|---|
| Review the Task 02 business purpose and output format | Data minimisation, data utility, CSV delivery | Complete |
| Research de-identification concepts and linkage risk | Direct identifiers, quasi-identifiers, pseudonymisation, generalisation, suppression, k-anonymity limitations | Complete |
| Document a field-inventory and transformation workflow | Privacy-by-design, purpose limitation, risk assessment | Complete |
| Download and inspect mobile_customers.xlsx locally | Row grain, schema, missingness, uniqueness, distributions | Pending |
| Classify each actual source column | Direct ID / quasi-ID / sensitive / linkage key / analytical attribute | Pending |
| Decide whether a consistent cross-dataset token is required | Linkage utility versus residual re-identification risk | Pending |
| Apply justified transformations | Removal, generalisation, banding, suppression or controlled tokenisation | Pending |
| Validate disclosure risk and analytical utility | Rare combinations, group sizes, row counts, retained fields | Pending |
| Export and reopen the CSV | UTF-8 CSV, structure and data checks | Pending |
| Submit the CSV through Forage | Required Task 02 deliverable | Pending |

### Task 02 decision notes

- The Forage page describes the input as mobile_customers.csv, but the provided download is an .xlsx workbook. Keep the source extension unchanged; the required output is CSV.
- Do not assume the source column names before inspecting the workbook.
- Because the background mentions joining customer data and supermarket transaction data, confirm whether a record-level join is required before removing or changing a customer identifier.
- A stable surrogate token can preserve a join only when the same mapping is applied to both datasets. If a mapping exists, the data is pseudonymised rather than proven irreversibly anonymous.
- Keep raw source files, working copies, mapping tables and customer-level outputs outside the public repository.

### Task 02 reflection prompts

1. Which fields are direct identifiers, and which combinations of quasi-identifiers could still identify a person?
2. Which columns are essential for InsightSpark's analysis, and which can be removed without losing the required utility?
3. Is record-level linkage to supermarket transactions required? What evidence confirms the join key?
4. How did the transformations change row uniqueness and group sizes?
5. What risks remain after transformation, including linkage and sensitive-attribute inference?
6. How did the exported CSV get validated after reopening it?

## Remaining simulation tasks

- [ ] Task 03 — Social Media Data and Business Insights.
- [ ] Task 04 — Database Design.
- [ ] Review the full simulation and update the final portfolio summary.
