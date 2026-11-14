# Commonwealth Bank — Introduction to Data Science

## Completion record

- **Provider:** Forage
- **Simulation:** Commonwealth Bank — Introduction to Data Science
- **Overall status:** Completed
- **Completion date:** 14 November 2026
- **Final assessment:** 10/10 correct answers (100%)
- **Completion certificate:** https://www.theforage.com/completion-certificates/2sNmYuurxgpFYawco/smwfytX3mcLboA9bf_2sNmYuurxgpFYawco_6ae9f5fbcfb5bb576b61ccb2_1794678594628_completion_certificate.pdf

This repository documents an independent, task-by-task learning project based on the Forage virtual job simulation. It is not an official Commonwealth Bank or Forage repository and does not represent employment, a paid client engagement, or endorsement.

## Four-task summary

| Task | Work completed | Evidence / result |
|---|---|---|
| 01 — Data Aggregation and Analysis | Profiled transaction data; applied Excel Tables, SUMIFS, COUNTIFS, and PivotTables; reconciled business metrics. | 50,783 transaction rows; 117 cash-paid apple units; $537.03 spent on cash-paid apples; $2,857.51 spent by Bakershire non-members. |
| 02 — Data Anonymisation | Classified identifiers, removed direct identifiers and unnecessary fields, generalised dates, created age/salary bands, and checked grouped disclosure risk. | 10,000 records transformed into a four-column values-only release; 40 quasi-identifier profiles; minimum group size 111; each profile contained all five salary bands; smallest profile-by-salary count 12. |
| 03 — Propose Data Analysis Approaches | Analysed a fictional social-media sample, validated formula-derived metrics and PivotTables, reviewed all 11 response-needed records, assessed X API feasibility, and completed the InsightSpark proposal. | 60 fictional posts; 11 topics; 125,989 simulated engagements across 2,906,190 views (4.34% weighted engagement rate); all 11 flagged records reviewed against their actual text in the private workbook. |
| 04 — Designing a Database | Mapped simulated and API-related fields; designed a normalised relational schema; defined columns, keys, cardinalities, ERD, and scenario checks. | Six core tables: Accounts, Posts, Post_References, Post_Mentions, Hashtags, and Post_Hashtags; optional media, metric-snapshot, and analysis extensions. |

## How Microsoft Excel was applied

Excel was used as an analytical environment, not only as a final reporting tool:

- **Structured data handling:** Excel Tables and structured references kept calculations tied to named fields.
- **Conditional aggregation:** SUMIFS and COUNTIFS converted business questions into traceable calculations.
- **Independent validation:** PivotTables were used to cross-check formula outputs and compare categories.
- **Privacy-aware transformation:** Excel formulas and grouped profiles supported date generalisation, age/salary banding, and disclosure-risk checks.
- **Descriptive analysis:** Formula-derived fields and PivotTables supported topic, engagement, author-type, sentiment, and time analysis.
- **Documentation:** Field inventories, transformation notes, validation tables, and task logs made the decisions reviewable.

## Important limitations

- Task 03's 60 social-media records and engagement values are fictional simulation data, not actual CommBank posts, customer opinions, or business performance.
- Sentiment and topic labels in the supplied workbook were curated source fields; no predictive sentiment model was trained.
- The X API review was a feasibility and documentation exercise. No developer app, credentials, API spend, production collection, or live integration was created.
- Task 04 is a relational schema proposal. No database was deployed and no SQL ingestion application was built.
- The Task 02 disclosure-risk checks are dataset-level screening, not a guarantee of anonymity or zero re-identification risk.
- The Forage completion certificate confirms overall program completion. Separate per-task workbook or response-field receipts are not retained in this public repository.

## Public evidence

- [GitHub repository](https://github.com/Sohila-Khaled-Abbas/commonwealth-bank-intro-data-science)
- [Forage completion certificate](https://www.theforage.com/completion-certificates/2sNmYuurxgpFYawco/smwfytX3mcLboA9bf_2sNmYuurxgpFYawco_6ae9f5fbcfb5bb576b61ccb2_1794678594628_completion_certificate.pdf)
- [Task 01 guide](task-01-data-aggregation-analysis.md)
- [Task 02 guide](task-02-data-anonymisation.md)
- [Task 03 guide](task-03-propose-data-analysis-approaches.md)
- [Task 04 guide](task-04-designing-a-database.md)
- [Learning log](../LEARNING_LOG.md)
- [CV and LinkedIn positioning](project-experience-positioning.md)

## Professional positioning

**Data Science Job Simulation | Commonwealth Bank (via Forage)** is suitable for a CV's Experience section when clearly labelled as a virtual job simulation. It must not be described as employment or a paid engagement with Commonwealth Bank.