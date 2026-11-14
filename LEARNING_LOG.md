# Learning Log — Commonwealth Bank Introduction to Data Science

Use this log to document the analytical process, decisions, validation, and reflections. The overall Forage completion record is captured below; separate per-task workbook or response receipts are not archived in this public repository.

## Forage program completion record

- **Simulation:** Commonwealth Bank — Introduction to Data Science (Forage)
- **Overall status:** Completed
- **Completion date:** 2026-11-14
- **Final quiz:** 10/10 correct answers (100%)
- **Completion page:** https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t/completed
- **Certificate:** https://www.theforage.com/completion-certificates/2sNmYuurxgpFYawco/smwfytX3mcLboA9bf_2sNmYuurxgpFYawco_6ae9f5fbcfb5bb576b61ccb2_1794678594628_completion_certificate.pdf
- **Evidence boundary:** The provider page confirms overall simulation completion. It does not provide a separate receipt for each task's workbook or response field.

## Task 01 — Data Aggregation and Analysis

**Work date:** 2026-11-13  
**Analysis status:** Complete  
**Formula/PivotTable reconciliation:** Passed  
**Forage program:** Overall simulation completed on 2026-11-14; individual Task 01 workbook receipt is not separately archived.

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
- [x] Record the overall Forage completion page; individual Task 01 workbook receipt is not separately archived.

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
**Forage program:** Overall simulation completed on 2026-11-14; individual Task 02 CSV receipt is not separately archived.

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
| Overall Forage completion | Provider completion page confirms 2026-11-14 completion | Individual Task 02 upload receipt is not separately archived | Program complete |

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
6. The CSV content was checked for exactly four columns, 10,000 data rows, and no formulas, blanks, or REVIEW values; The overall Forage simulation is confirmed complete; a separate Task 02 upload receipt is not retained in this repository.

## Task 03 — Propose Data Analysis Approaches

**Start date:** 2026-11-14  
**Status:** Task 03 completed within the overall Forage simulation (2026-11-14); profiling, quantitative and time analysis, X API documentation review, private review of all 11 response-needed records, and the InsightSpark proposal are documented. Individual response-field receipt is not archived.  
**Required deliverable:** InsightSpark data-analysis proposal. Overall Forage program completion is confirmed; a separate Task 03 response-field receipt is not archived.

| Activity | Techniques / focus | Validation / evidence | Status |
|---|---|---|---|
| Inspect supplied workbook | Worksheet inventory, table structure, data dictionary | Five sheets; SocialMediaPosts table; 60 records; 24 fields | Complete |
| Profile the sample | Missingness, dates, post types, authors, topic/sentiment distributions | 60 unique IDs; date range 2026-04-15 to 2026-06-12; 29 blank hashtag cells; other fields populated | Complete |
| Inspect formula-derived metrics | Formula review and arithmetic reconciliation | 180 formulas across total_engagements, engagement_rate, day_of_week; cached values reconcile with source metrics and timestamps | Complete |
| Topic analysis | Frequency and theme interpretation | 11 topics; 60 posts; Digital banking 13, Fraud & scams 11, Financial wellbeing 9; matches baseline QA | Complete |
| Engagement analysis by topic | Sum engagements and views; weighted engagement rate; derive engagements per post | Topic totals reconcile to 125,989 engagements and 2,906,190 views; overall weighted rate 4.34% | Complete |
| Engagement analysis by content format | Compare post count, engagements, views, weighted rate, and average engagements per post | Four formats reconcile to QA totals; video 5.17%, text + image 4.61%, link 4.13%, text 1.44% | Complete |
| Engagement analysis by post type | Compare counts, total engagements, views, weighted rates, and average engagement volume | Four post types reconcile to QA totals; original posts 4.52%, replies 1.60%, mentions 1.51%, quote posts 1.39% | Complete |
| Author-type analysis | Count posts and compare engagement by author group; weighted rate; validate six categories | 60 posts; totals reconcile to QA_Overview; CommBank 26 posts / 123,593 engagements / 2,754,800 views / 4.49%; Customer 28 / 1,597 / 96,990 / 1.65% | Complete |
| Sentiment PivotTables | Overall sentiment by author type and Customer-only sentiment | All-posts: 26 positive, 15 neutral, 11 negative, 8 mixed (60 total); Customer-only: 11 negative (39.29%), 8 positive (28.57%), 7 mixed (25.00%), 2 neutral (7.14%) (28 total) | Quantitative PivotTables validated; manual text review scope is separately recorded for the 11 flagged posts |
| Customer support analysis | Filter response-needed records; review actual text, themes, and proportionate actions | 11 Customer-authored records; 5 Complaints, 5 Questions, 1 Scam report; 10 Negative and 1 Neutral; all 11 have reviewed themes/actions | Complete for the 11 flagged records |
| Review X API | Field mappings, search coverage, public/non-public metrics, access, and published pricing | Official X API references and operational constraints documented in Task 03 guide (reviewed 2026-11-14) | Complete |
| Complete proposal deliverable | Business value, data flow, KPIs, privacy, limitations | InsightSpark proposal documented within the completed Forage simulation; separate response-field receipt is not archived | Program complete |

### Task 03 — progress update through engagement-by-post-type analysis

Three engagement comparisons plus author-type analysis have now been reproduced and validated in Excel:
- By topic: 11 topics; Digital banking is most frequent (13 posts); Fraud & scams leads total engagements (36,283) and weighted rate (5.33%).
- By content format: Text + video has the highest weighted rate (5.17%) across four posts; Text + image has the highest total engagements (73,358) and a 4.61% weighted rate.
- By post type: Original posts contribute 123,337 engagements (about 97.9% of the total) across 24 records, with a 4.52% weighted rate. Mentions, replies, and quote posts have lower rates and different conversational purposes.

Each PivotTable reconciles to 60 posts, 125,989 total engagements, 2,906,190 views, and a 4.34% overall weighted engagement rate. The author_type PivotTable is now also validated. The all-posts and Customer-only sentiment PivotTables have since been reproduced in Excel and reconciled:

| Sentiment | Business customer | CommBank | Community org | Customer | News/media | Scam awareness org | All posts |
|---|---:|---:|---:|---:|---:|---:|---:|
| Mixed | 1 | 0 | 0 | 7 | 0 | 0 | 8 |
| Negative | 0 | 0 | 0 | 11 | 0 | 0 | 11 |
| Neutral | 0 | 13 | 0 | 2 | 0 | 0 | 15 |
| Positive | 1 | 13 | 2 | 8 | 1 | 1 | 26 |
| **All posts** | **2** | **26** | **2** | **28** | **1** | **1** | **60** |

For Customer-authored posts only: Mixed 7 (25.00%), Negative 11 (39.29%), Neutral 2 (7.14%), and Positive 8 (28.57%), totalling 28 (100%). All 11 Negative labels are Customer-authored. The quantitative PivotTable checkpoint is validated. The completed manual review covers the 11 records flagged with response_needed = Yes; it should not be described as a full manual audit of all Negative or Mixed Customer-authored posts.

### Task 03 data observations

- The dataset is explicitly fictional and intended for educational use.
- Post types: 24 original posts, 17 mentions, 16 replies, and 3 quote posts.
- Author categories: 28 Customer, 26 CommBank, 2 Business customer, 2 Community org, 1 Scam awareness org, and 1 News/media.
- The three most common curated topics account for 33 of 60 records (55%).
- The sample's weighted engagement rate is approximately 4.34% (125,989 engagements / 2,906,190 views). The unweighted mean row-level rate is approximately 2.76%; these are different calculations.
- Total engagements in the workbook include link clicks. For actual X API access, link clicks are non-public metrics requiring user-context access to the authenticated account's own content; they are not a generally available public metric.
- Do not pool brand-authored and customer-authored sentiment when drawing customer-opinion conclusions.
- The sample only spans part of April, May and part of June 2026; do not infer long-term seasonality from it.

### Task 03 — validated topic and engagement PivotTable results

The Excel topic-frequency PivotTable was validated against the 60-record baseline. A second PivotTable grouped post count, total engagements, total views, and weighted engagement rate by topic.

| Topic | Posts | Total engagements | Total views | Weighted rate | Avg. engagements / post |
|---|---:|---:|---:|---:|---:|
| Fraud & scams | 11 | 36,283 | 680,750 | 5.33% | 3,298.5 |
| Financial wellbeing | 9 | 23,309 | 513,500 | 4.54% | 2,589.9 |
| Security education | 5 | 22,360 | 439,800 | 5.08% | 4,472.0 |
| Digital banking | 13 | 21,176 | 540,300 | 3.92% | 1,628.9 |
| Card services | 4 | 8,303 | 192,450 | 4.31% | 2,075.8 |
| Community | 4 | 8,279 | 238,100 | 3.48% | 2,069.8 |
| Business banking | 3 | 2,671 | 59,900 | 4.46% | 890.3 |
| Sustainability | 2 | 1,787 | 101,400 | 1.76% | 893.5 |
| Outage/service issue | 5 | 1,714 | 132,140 | 1.30% | 342.8 |
| Branch/ATM | 3 | 90 | 5,750 | 1.57% | 30.0 |
| Customer support | 1 | 17 | 2,100 | 0.81% | 17.0 |
| **Total / overall** | **60** | **125,989** | **2,906,190** | **4.34%** | **2,099.8** |

Interpretation recorded:
- Digital banking is most frequent (13/60, 21.7%), but not the engagement leader.
- Fraud & scams has the most total engagements (36,283) and the highest topic-level weighted engagement rate (5.33%).
- Security education has the highest average engagements per post (4,472) and a 5.08% weighted rate, but is based on only five posts.
- The overall weighted rate is total engagements divided by total views (125,989 / 2,906,190 = 4.34%). It is not the unweighted average of the 60 row-level rates.
- Small topic groups require caution; the single customer-support post cannot support a stable comparative conclusion.
- All findings are descriptive of the fictional, curated Forage dataset only.

The workbook's total-engagement formula includes link clicks. This is a simulation-specific combined measure; the public X API does not expose every metric for every public post. Keep that distinction in the final proposal.

### Task 03 — validated engagement-by-format PivotTable results

The format PivotTable used `content_format` as rows and Count of `post_id`, Sum of `total_engagements`, and Sum of `views` as values. Weighted rates were calculated as total engagements divided by total views.

| Content format | Posts | Total engagements | Total views | Weighted rate | Avg. engagements / post |
|---|---:|---:|---:|---:|---:|
| Link | 6 | 21,738 | 526,900 | 4.13% | 3,623.0 |
| Text | 36 | 3,809 | 264,990 | 1.44% | 105.8 |
| Text + image | 14 | 73,358 | 1,590,300 | 4.61% | 5,239.9 |
| Text + video | 4 | 27,084 | 524,000 | 5.17% | 6,771.0 |
| **Overall** | **60** | **125,989** | **2,906,190** | **4.34%** | **2,099.8** |

Interpretation:
- Text + video has the highest weighted rate and average engagement volume per post, but only four observations.
- Text + image has the highest total engagement volume and a weighted rate above the overall average.
- Text is the most frequent format (36 posts, 60%) but has the lowest weighted rate and average engagement volume per post.
- These results do not establish that format causes engagement. Topic, purpose, author, audience size, and timing may confound the comparison.
- The total-engagement measure includes link clicks in this simulated dataset; production X API availability is not equivalent for all posts.

### Task 03 — validated engagement-by-post-type PivotTable results

The post-type PivotTable used `post_type` as rows and Count of `post_id`, Sum of `total_engagements`, and Sum of `views` as values. Weighted engagement rates were calculated as total engagements divided by total views.

| Post type | Posts | Total engagements | Total views | Weighted rate | Avg. engagements / post |
|---|---:|---:|---:|---:|---:|
| Mention | 17 | 1,112 | 73,530 | 1.51% | 65.4 |
| Original post | 24 | 123,337 | 2,730,800 | 4.52% | 5,139.0 |
| Quote post | 3 | 621 | 44,600 | 1.39% | 207.0 |
| Reply | 16 | 919 | 57,260 | 1.60% | 57.4 |
| **Overall** | **60** | **125,989** | **2,906,190** | **4.34%** | **2,099.8** |

Interpretation:
- Original posts are 40% of records but account for about 97.9% of total engagements in this curated sample.
- Mentions and replies have far lower per-record engagement volume; their function is often conversational or service-oriented, not broadcast.
- Quote posts have only three records, so their rate is particularly unstable.
- Post type must not be confused with author type; analyse `author_type` separately before assigning posts to the bank or customers.
- The simulated total-engagement measure includes link clicks, which should not be assumed to be universally available as a public X metric.

### Task 03 — validated author-type PivotTable results

The author-type PivotTable used `author_type` as rows and Count of `post_id`, Sum of `total_engagements`, and Sum of `views` as values. The `post_type` report filter was left at All.

| Author type | Posts | Total engagements | Total views | Weighted rate |
|---|---:|---:|---:|---:|
| Customer | 28 | 1,597 | 96,990 | 1.65% |
| CommBank | 26 | 123,593 | 2,754,800 | 4.49% |
| Business customer | 2 | 97 | 5,600 | 1.73% |
| Community org | 2 | 237 | 11,100 | 2.14% |
| News/media | 1 | 328 | 31,000 | 1.06% |
| Scam awareness org | 1 | 137 | 6,700 | 2.04% |
| **Grand Total** | **60** | **125,989** | **2,906,190** | **4.34%** |

Interpretation:
- CommBank-authored records contribute about 98.1% of total engagements in the sample and have a 4.49% weighted rate.
- Customer-authored records have a 1.65% weighted rate. This does not measure satisfaction or customer interest; the author groups have different post purposes, formats, and reach.
- Business customer, community organisation, news/media, and scam-awareness organisation groups are very small (one or two records each).
- Author type is essential context for the next sentiment analysis. Customer-authored sentiment should be analysed separately from bank-authored posts.

### Task 03 — validated sentiment, time, and support PivotTables

**Step 7 — Sentiment analysis**
- All-posts sentiment: Positive 26, Neutral 15, Negative 11, Mixed 8 (60 total).
- Customer-only sentiment: Negative 11 (39.29%), Positive 8 (28.57%), Mixed 7 (25.00%), Neutral 2 (7.14%) (28 total).
- All 11 Negative labels belong to Customer-authored posts; seven of eight Mixed labels are Customer-authored.
- The supplied sentiment labels are curated source fields, not model predictions developed in this exercise. The completed manual review is limited to the 11 records flagged for response; a broader reading of all Negative and Mixed Customer-authored records is a separate optional audit and is not claimed as complete.

**Step 8 — Time analysis**
- Monthly post counts: April 2026 = 25; May = 27; June = 8; total = 60.
- June is a partial month because the sample ends on 12 June 2026. Do not compare it as a complete month.
- Weekday counts: Monday 6; Tuesday 11; Wednesday 11; Thursday 10; Friday 8; Saturday 7; Sunday 7; total = 60.
- Tuesday and Wednesday are tied for the highest observed post count (11 each). This is descriptive only.

**Step 8 — Customer-support records and text review**

- `response_needed = Yes`: 11 Customer-authored records.
- Sentiment among the flagged subset: 10 Negative and 1 Neutral.
- Customer intent: Complaint 5; Question 5; Scam report 1.
- Evidence-based issue themes: Digital banking 3; Branch/ATM 3; Payment status / possible disruption 2; Fraud protection / reporting experience 2; Card services 1.
- The private `Customer_Support_Review` sheet contains 11 rows and the fields `post_id`, `author_type`, `customer_intent`, `topic`, `sentiment_label`, `response_needed`, `issue_theme`, and `recommended_action`.
- The actual text review distinguishes a potential phishing report from a complaint about the discoverability of the in-app scam-reporting feature. These require different routes and must not be conflated merely because both are labelled Fraud & scams.
- The review's recommended actions are suggestions for approved team routing and verification; no real customer response or service resolution was performed.
- The uploaded 16-sheet workbook contains the 11-row `Customer_Support_Review` table but no separate `PVT_Support_Flagged` tab. The support review is therefore described as a filtered 11-record table rather than a separately saved support PivotTable.


**Step 9 — Official X API documentation review (14 November 2026)**

Reviewed the current [Search Posts](https://docs.x.com/x-api/posts/search/introduction), [Data Dictionary](https://docs.x.com/x-api/fundamentals/data-dictionary), [Metrics](https://docs.x.com/x-api/fundamentals/metrics), [Getting Access](https://docs.x.com/x-api/getting-started/getting-access), and [pay-per-use pricing](https://docs.x.com/x-api/getting-started/pricing) documentation.

- Mapped post identity, text, creation time, author, conversation, reply/reference, entity, context, language, and media fields to the proposed analysis.
- Confirmed Recent Search covers the previous seven days (up to 100 posts per request); Full-Archive Search reaches back to March 2006 (up to 500 posts per request) and requires pay-per-use or Enterprise access. Both support pagination.
- Separated public post metrics (likes, replies, reposts, quotes, impressions, bookmarks) from account-owner metrics such as URL clicks, profile clicks, and total engagements. Owner-context metrics require user-context authentication and are subject to a 30-day availability limit.
- Noted that video views are returned through the expanded Media object and should not be silently combined with post impressions.
- Recorded the published pay-per-use price of US$0.005 per Post read and the 3-million-Post-read monthly cap. The pricing page warns that prices can change, so a live budget must be confirmed in the Developer Console.
- No API account, app, credential, budget, or production access has been created or verified as part of this learning exercise.

**Completion note:** the Forage completion page confirms the overall simulation is complete. The analysis, 11-record support-text review, and InsightSpark proposal are documented here; a separate Task 03 response-field receipt is not archived.

### Task 03 reflection prompts

1. Why should sentiment be interpreted separately for CommBank-authored posts and customer-authored posts?
2. Why does a weighted engagement rate answer a different question from the average of row-level rates?
3. What operational value could come from identifying recurring complaints and posts flagged as requiring a response?
4. Which fields are directly available from the official X API, and which metrics require account-owner authentication or separate analytics?
5. What limitations arise from using 60 fictional, curated records?

## Task 04 — Designing a Database

**Work date:** 2026-11-14  
**Design status:** Schema proposal prepared and checked against the Forage requirements  
**Forage program:** Overall simulation completed on 2026-11-14; individual Task 04 response-field receipt is not separately archived.

| Activity | Techniques / focus | Validation / evidence | Status |
|---|---|---|---|
| Review the task requirements | Identify required data subjects and interaction types | CommBank-authored posts, user replies, user quote posts, and public direct mentions identified; private Direct Messages excluded | Complete |
| Define the data grain | Distinguish posts from relationships | One row per unique post; replies and quote posts are separate Posts records; mentions are separate post-to-account associations | Complete |
| Build the field inventory | Map Task 03 fields and additional X API concepts | 24 Task 03 fields mapped; added author ID, username, conversation ID, referenced post details, mentions, hashtags, and media keys | Complete |
| Correct source-to-schema mappings | Distinguish raw API attributes from derived or curated fields | Post type derived from references/author; mention flag represented as an account association; follower bucket not treated as an exact count; calculated fields kept out of source tables | Complete |
| Define core entities | Normalisation and relational modelling | Accounts, Posts, Post_References, Post_Mentions, Hashtags, and Post_Hashtags | Complete |
| Add optional analytics extensions | Historical metrics and analysis provenance | Post_Media, Post_Metric_Snapshots, and Post_Analysis separated from the required core | Complete |
| Define columns and keys | Primary keys, foreign keys, data types, nullability | Each table has an explicit primary key; foreign keys and composite keys are documented in the Task 04 guide | Complete |
| Define relationships | 1:M, M:N, self-referencing relationships | Accounts-to-Posts 1:M; Posts-to-Accounts mentions M:N via junction; Posts-to-Hashtags M:N via junction; Posts self-reference via Post_References | Complete |
| Validate required scenarios | Conceptual model checks | Original CommBank post, reply, quote post, direct mention, overlapping reply+mention, and multi-value hashtags/media tested against the model | Passed |
| Complete Forage simulation | Written Task 04 response and overall program | Provider completion page records overall simulation complete; no database or API integration claimed | Program complete |

### Task 04 modelling decisions

- **Public content only:** "Direct mentions" is interpreted as public posts that mention the CommBank account. Private Direct Messages are outside the stated task.
- **One post, one record:** Original posts, replies, and quote posts share the Posts table; their relationships are represented separately.
- **References vs mentions:** `referenced_tweets` identifies the post being replied to or quoted. `entities.mentions` identifies account mentions. The concepts are not interchangeable.
- **Stable identifiers:** Use X platform IDs as text keys. Usernames are mutable profile attributes, not primary keys.
- **Conversation grouping:** Retain `conversation_id` on Posts as the root-conversation identifier; a separate Conversations table is not needed for this scope.
- **Repeating values:** Use Post_Mentions and Post_Hashtags rather than arrays or delimited strings in one row.
- **Source vs analysis:** Topic, sentiment, customer intent, and response-needed labels are separate derived analysis outputs.
- **Metric history:** Optional metric snapshots use a capture timestamp and scope so that changing counts are not overwritten. Public and user-context metric access must remain distinct.
- **Data dictionary mapping corrections:** `author_follower_bucket` is not an exact `followers_count`; `in_reply_to_user_id` identifies a replied-to author, not a parent post ID; total engagements, engagement rate, and weekday are calculated or derived values.

The schema is a design proposal, not an implemented database. No production X API credentials, API data collection, or live integration were created. The overall Forage simulation is complete; a separate Task 04 response-field receipt is not retained in this repository.

### Task 04 reflection prompts

1. Why should a reply or quote post be stored as a separate post rather than as a duplicate of the CommBank post?
2. Why are post references and account mentions modelled separately?
3. What problem does a junction table solve for Posts-to-Accounts and Posts-to-Hashtags?
4. Why are usernames unsuitable as primary keys?
5. Why should changing metric counts be stored with a collection timestamp?
6. Which fields in the Task 03 workbook are curated or calculated rather than raw source attributes?

## Final simulation review

- [x] Task 01 — transaction analysis and formula/PivotTable reconciliation documented.
- [x] Task 02 — privacy transformation, disclosure-risk screening, and CSV validation documented.
- [x] Task 03 — social-media analysis, 11-record support review, X API feasibility, and InsightSpark proposal documented.
- [x] Task 04 — relational schema, keys, relationships, ERD, and interaction validation documented.
- [x] Overall Forage completion page checked: completed 2026-11-14; final quiz 10/10 (100%).

The repository records overall completion evidence while keeping Forage source workbooks, customer/post-level records, and private outputs out of GitHub.
