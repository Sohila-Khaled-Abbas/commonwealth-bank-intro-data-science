# Companion Excel workbooks

This folder contains the original Task 01 practice starter. Two additional companion workbook templates have been prepared for Tasks 02 and 03:

- `task-02-data-anonymisation-workbook.xlsx`
- `task-03-social-media-analysis-workbook.xlsx`

The templates support reproducible practice and documentation. They contain aggregate results already validated in the learning log, plus worksheets for QA, field decisions, author-type analysis, sentiment, API mapping, and proposal planning.

## Important data-handling rules

- The Task 02 template does not contain the 10,000 customer records or the row-level anonymised CSV. Never commit the Forage customer source workbook, working workbook, or released customer-level CSV.
- The Task 03 template does not contain the Forage social-media source workbook. The simulation resource is marked for Forage use only; retain the original dataset locally.
- The Task 03 source-data worksheet includes only confirmed field names. If the source workbook contains additional fields, preserve them in the private working copy and extend your local analysis without silently dropping source columns.
- Do not treat prefilled aggregate summaries as proof that the raw data is included or that every remaining analysis has been completed.
- Keep author-level or customer-level extracts private. Publish aggregate findings and methodology only.

## Workbook status

| Workbook | Purpose | Current state |
|---|---|---|
| Task 02 | Field inventory, anonymisation decisions, QA reference, release schema | Template aligned to the documented Registration_Year, Gender, Age_Band, and Salary_Band release |
| Task 03 | Source-data template, QA baseline, topic/format/post-type summaries, author analysis, sentiment, time, support flags, API mapping, proposal outline | Topic, engagement, author, sentiment, month, weekday, and 11-row support-flag aggregates validated; current X API fields/access/metrics review documented; private row-level support review and proposal remain |

All three generic companion workbooks are present in this folder. They are practice templates, not copies of Forage source files. Before updating or committing any workbook, inspect its sheets and hidden content to confirm it contains no source dataset, post text, customer-level records, or private exports.
