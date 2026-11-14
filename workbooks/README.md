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
- The user's private Task 03 working copy includes customer-level post text and an 11-row support-review sheet. Keep the full workbook and row-level content out of this public repository; publish only aggregate findings and methodology.
- Keep author-level or customer-level extracts private. Publish aggregate findings and methodology only.

## Task 04 private design workbook

The Task 04 working workbook is maintained locally at `data/working/Task04_Database_Design.xlsx`. It is a private design/requirements workbook, not a Forage source-data workbook or a public companion template. Keep it out of GitHub along with all other files under `data/raw/` and `data/working/`. The public Task 04 schema proposal and submission text are documented in [the Task 04 guide](../docs/task-04-designing-a-database.md).

## Workbook status

| Workbook | Purpose | Current state |
|---|---|---|
| Task 02 | Field inventory, anonymisation decisions, QA reference, release schema | Template aligned to the documented Registration_Year, Gender, Age_Band, and Salary_Band release |
| Task 03 | Generic practice template for source-data profiling, QA, topic/format/post-type/author analysis, sentiment, time, support review, API mapping, and proposal planning | The private working copy used for analysis has 16 sheets (five source/metadata plus 11 QA/analysis/review sheets), 60 fictional records, and 24 source fields. Quantitative analyses and the 11-row response-needed text review are validated; the X API review and proposal draft are prepared; Forage submission is not confirmed |

All three generic companion workbooks are present in this folder. They are practice templates, not copies of Forage source files. Before updating or committing any workbook, inspect its sheets and hidden content to confirm it contains no source dataset, post text, customer-level records, or private exports.
