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

**Work date:** 2026-11-13  
**Workbook analysis:** Complete  
**Values-only release:** Validated  
**CSV export:** Created and structurally validated  
**Forage submission:** Not yet verified

| Activity | Techniques / focus | Validation / evidence | Status |
|---|---|---|---|
| Inspect source workbook | Excel Table structure and row grain | MobileCustomers; 10,000 records; 19 source fields | Complete |
| Profile source data | ID uniqueness, ranges, distributions, category cardinality | 10,000 unique nonblank customer IDs; age 18–65; salary 20,014–244,926; 639 job categories; 8,645 employer categories | Complete |
| Build field inventory and transformation log | Direct identifiers, quasi-identifiers, sensitive attributes, utility | All 19 source fields classified in the private workbook | Complete |
| Create transformed release | Removal, date generalisation, age and salary banding | Registration_Year, Gender, Age_Band, Salary_Band | Complete |
| Check quasi-identifier combinations | Group-size / k-anonymity-style screening | 40 profiles; minimum group size 111; zero records below 5; zero singleton records | Passed |
| Review sensitive salary disclosure | Distinct salary bands within each profile | Every profile has all five salary bands; 200 profile-band cells; minimum cell count 12 | Passed |
| Validate values-only output | Row count, columns, blanks, review values | Final_Export: 10,000 records, four columns, zero formulas, zero blanks, zero REVIEW values | Passed |
| Generate and reopen CSV | UTF-8 export and structural checks | 10,000 data rows; four columns; no blank cells | Complete |
| Submit through Forage | Required Task 02 upload | Acceptance not confirmed | Pending |

### Task 02 final output distributions

**Registration year**

| Year | Records |
|---|---:|
| 2019 | 1,786 |
| 2020 | 3,300 |
| 2021 | 3,349 |
| 2022 | 1,565 |
| **Total** | **10,000** |

**Gender**

| Category | Records |
|---|---:|
| F | 4,951 |
| M | 5,049 |
| **Total** | **10,000** |

**Age band**

| Age band | Records |
|---|---:|
| 18–24 | 1,446 |
| 25–34 | 2,044 |
| 35–44 | 2,123 |
| 45–54 | 2,049 |
| 55–65 | 2,338 |
| **Total** | **10,000** |

**Salary band**

| Salary band | Records |
|---|---:|
| 20,000–49,999 | 1,267 |
| 50,000–99,999 | 2,200 |
| 100,000–149,999 | 2,293 |
| 150,000–199,999 | 2,209 |
| 200,000+ | 2,031 |
| **Total** | **10,000** |

### Task 02 privacy review

The equivalence groups were defined by Registration_Year + Gender + Age_Band. There were 40 profiles, with a minimum group size of 111 and no groups smaller than five. Salary was then assessed within each profile: all five salary categories appeared in every profile, and the smallest profile-by-salary count was 12. This supports a distinct 5-diversity-style check for the grouped salary attribute.

These are useful dataset-level screening results, not a guarantee against re-identification. External information, recipient context, and future data releases can change risk. The work was standalone: no Task 01 transaction data was joined and the original customer_id was removed because no person-level linkage requirement was established.

The values-only CSV was generated from Final_Export and structurally reopened. The source workbook, working workbook, and customer-level CSV remain outside the public repository. The workbook's preliminary inventory and transformation-log notes still refer to month/quarter treatment for date_registered; update those notes to registration year before archiving the workbook as a fully reconciled audit artifact.

### Task 02 reflection

1. Removing direct identifiers was necessary but not sufficient; the combined year/gender/age profile needed a group-size assessment.
2. Generalising registration dates to year was the first tested level that eliminated all small profiles in the selected quasi-identifier combination.
3. High-cardinality employer and job fields offered unnecessary identification risk for this release and were removed rather than mapped without evidence.
4. Salary was retained only after checking that every quasi-identifier profile contained all five salary bands; the smallest cell count was 12.
5. A group-size result alone is not proof of anonymity. The release still needs a recipient-context and external-linkage risk review.
6. The CSV content was checked for exactly four columns, 10,000 data rows, and no formulas, blanks, or REVIEW values; Forage submission remains to be confirmed.

## Task 03 — Propose Data Analysis Approaches

**Start date:** 2026-11-14  
**Status:** In progress — dataset profiling complete; analysis and proposal pending  
**Required deliverable:** Written proposal submitted in Forage Task 03 response field

| Activity | Techniques / focus | Validation / evidence | Status |
|---|---|---|---|
| Inspect supplied workbook | Worksheet inventory, table structure, data dictionary | Five sheets; SocialMediaPosts table; 60 records; 24 fields | Complete |
| Profile the sample | Missingness, dates, post types, authors, topic/sentiment distributions | 60 unique IDs; date range 2026-04-15 to 2026-06-12; 29 blank hashtag cells; other fields populated | Complete |
| Inspect formula-derived metrics | Formula review and arithmetic reconciliation | 180 formulas across total_engagements, engagement_rate, day_of_week; cached values reconcile with source metrics and timestamps | Complete |
| Topic analysis | Frequency and theme interpretation | Digital banking 13; Fraud & scams 11; Financial wellbeing 9 | Pending Excel reproduction |
| Engagement analysis | Volume, reach, weighted rate, compare post types/formats | Sample contains 125,989 engagements incl. link clicks and 2,906,190 views | Pending Excel analysis |
| Sentiment analysis | Overall and customer-only sentiment; text review | All-post labels: 26 positive, 15 neutral, 11 negative, 8 mixed; Customer-only subset: 11 negative, 8 positive, 7 mixed, 2 neutral | Pending Excel reproduction |
| Customer support analysis | Response-needed flags and recurring intents | 11 records flagged Yes, with complaints, questions and one scam report | Pending Excel reproduction |
| Review X API | Field mappings, recent/full archive, public/non-public metrics | Official X API references added to Task 03 guide | In progress |
| Draft and submit proposal | Business value, data flow, KPIs, privacy, limitations | Forage proposal response field | Pending |

### Task 03 data observations

- The dataset is explicitly fictional and intended for educational use.
- Post types: 24 original posts, 17 mentions, 16 replies, and 3 quote posts.
- Author categories: 28 Customer, 26 CommBank, 2 Business customer, 2 Community org, 1 Scam awareness org, and 1 News/media.
- The three most common curated topics account for 33 of 60 records (55%).
- The sample's weighted engagement rate is approximately 4.34% (125,989 engagements / 2,906,190 views). The unweighted mean row-level rate is approximately 2.76%; these are different calculations.
- Total engagements in the workbook include link clicks. For actual X API access, link clicks are non-public metrics requiring user-context access to the authenticated account's own content; they are not a generally available public metric.
- Do not pool brand-authored and customer-authored sentiment when drawing customer-opinion conclusions.
- The sample only spans part of April, May and part of June 2026; do not infer long-term seasonality from it.

### Task 03 reflection prompts

1. Why should sentiment be interpreted separately for CommBank-authored posts and customer-authored posts?
2. Why does a weighted engagement rate answer a different question from the average of row-level rates?
3. What operational value could come from identifying recurring complaints and posts flagged as requiring a response?
4. Which fields are directly available from the official X API, and which metrics require account-owner authentication or separate analytics?
5. What limitations arise from using 60 fictional, curated records?

## Remaining simulation tasks

- [ ] Task 03 — Social Media Data and Business Insights.
- [ ] Task 04 — Database Design.
- [ ] Review the full simulation and update the final portfolio summary.
