# Task 03 — Propose Data Analysis Approaches

> **Status:** In progress — dataset profiling and engagement comparisons by topic, format, post type, and author type are complete; all-posts and Customer-only sentiment PivotTables are validated; qualitative text review, support, time analysis, API review, and proposal work remain.  
> **Required Forage deliverable:** A written proposal submitted in the Task 03 text field.  
> **Dataset:** Forage-provided fictional Commonwealth Bank social-media sample. Keep the workbook private; it is marked for simulation use only.

## 1. Task objective

Propose how InsightSpark could collect, structure, analyse, and use social-media data related to CommBank to support customer experience, marketing, brand monitoring, customer support, fraud awareness, and product decisions.

This task is a proposal exercise. It does not require building a live X API integration, obtaining API credentials, training a sentiment model, or scraping social media.

## 2. Source workbook profile

The supplied workbook contains five worksheets:

| Worksheet | Purpose |
|---|---|
| README | Dataset purpose, synthetic-data warning, and suggested analysis |
| Social_Media_Posts | Main sample table |
| Data_Dictionary | Definitions and API / analytics mappings |
| Analysis_Prompts | Suggested business questions |
| Resources | Links to API and sentiment resources |

The main table is an existing Excel Table named `SocialMediaPosts`, covering `A1:X61`: one header row and **60 simulated post records** across **24 columns**.

### Important data limitation

The workbook explicitly states that all posts and metrics are fictional and intended for educational use. Findings in this project therefore describe only this curated 60-row sample. They are not actual CommBank posts, real customer opinions, real campaign results, or a representative survey of CommBank customers.

### Field inventory

| Field group | Columns | Analytical use |
|---|---|---|
| Post identity and time | post_id, post_datetime | Deduplication, post tracking, time trends |
| Post / author context | post_type, author_type, author_follower_bucket | Separate brand publishing from customer conversation and compare author groups |
| Content | post_text, topic, content_format, hashtags, mentions_commbank | Topic classification, content strategy, mentions and campaign analysis |
| Sentiment | sentiment_label, sentiment_score | Positive / negative / neutral / mixed classification and score comparisons |
| Engagement | likes, replies, reposts, quote_posts, views, link_clicks, total_engagements, engagement_rate | Reach, interaction volume, amplification, clicks and rate-based performance |
| Customer service / intent | day_of_week, customer_intent, response_needed | Timing, intent categories, and service prioritisation |
| API mapping | api_field_examples | Relate sample columns to X API concepts |

The workbook includes formula-derived columns:
- `total_engagements`: `SUM(likes, replies, reposts, quote_posts) + link_clicks`
- `engagement_rate`: `total_engagements / views`
- `day_of_week`: derived from `post_datetime`

In the supplied sheet, these formulas occupy `S2:U61`. Their cached values were checked against the underlying metrics and dates.

## 3. Initial profiling results

These are baseline observations from the supplied 60-row workbook. Reproduce them in Excel during the guided workflow before using them in the final proposal.

### Record structure and data quality

| Check | Observed result |
|---|---:|
| Post records | 60 |
| Fields | 24 |
| Date range | 15 April 2026 – 12 June 2026 |
| Unique post IDs | 60 |
| Blank post IDs | 0 |
| Blank post text | 0 |
| Blank hashtags | 29 |
| Other fields with blanks | 0 |
| Formula-derived fields | 3 per record (S:U) |

Blank hashtags are not automatically errors: many replies or customer posts simply do not use hashtags. Preserve blank values rather than inventing hashtag content.

### Post type and author type

| Post type | Records |
|---|---:|
| Original post | 24 |
| Mention | 17 |
| Reply | 16 |
| Quote post | 3 |
| **Total** | **60** |

| Author type | Records |
|---|---:|
| Customer | 28 |
| CommBank | 26 |
| Business customer | 2 |
| Community org | 2 |
| Scam awareness org | 1 |
| News/media | 1 |
| **Total** | **60** |

Do not treat all 60 records as customer opinions. More than one-third are original CommBank posts, and the dataset also includes other account types. Analyse customer-authored and bank-authored content separately where appropriate.

### Topic frequency

| Topic | Posts |
|---|---:|
| Digital banking | 13 |
| Fraud & scams | 11 |
| Financial wellbeing | 9 |
| Outage/service issue | 5 |
| Security education | 5 |
| Community | 4 |
| Card services | 4 |
| Business banking | 3 |
| Branch/ATM | 3 |
| Sustainability | 2 |
| Customer support | 1 |
| **Total** | **60** |

Digital banking, fraud & scams, and financial wellbeing account for 33 of 60 records (55%) in this curated sample.

### Sentiment distribution

| Sentiment label | All posts | Share |
|---|---:|---:|
| Positive | 26 | 43.3% |
| Neutral | 15 | 25.0% |
| Negative | 11 | 18.3% |
| Mixed | 8 | 13.3% |
| **Total** | **60** | **100%** |

For the **28 Customer-authored records only**, the labels are: 11 negative, 8 positive, 7 mixed, and 2 neutral. This split is more relevant to customer-conversation analysis than an overall percentage that also includes bank-authored posts.

The sentiment labels and numeric scores are supplied curated fields. Do not describe them as model predictions or as a sentiment model that was trained and evaluated in this exercise. Use labels as categories, inspect the post text for context, and treat the score as an additional indicator rather than assuming the label is simply the sign of the score.

### Engagement baseline

| Metric | Total |
|---|---:|
| Likes | 16,367 |
| Replies | 1,849 |
| Reposts | 4,460 |
| Quote posts | 473 |
| Views | 2,906,190 |
| Link clicks | 102,840 |
| Total engagements (including link clicks) | 125,989 |

The weighted overall engagement rate is total engagements divided by total views: approximately **4.34%**. The unweighted average of the 60 row-level engagement rates is approximately **2.76%**. State which calculation is being used; do not mix them.

The highest total-engagement record is `SM048` (Fraud & scams), with **12,165** total engagements and **210,000** views. The highest row-level engagement rate is `SM004` (Financial wellbeing), at approximately **7.01%** (5,902 engagements / 84,200 views).

### Excel checkpoint: topic frequency and engagement by topic

The topic PivotTable was reproduced in Excel and reconciled to the baseline QA totals:

- **11 distinct topics**
- **60 posts**
- **Digital banking** is the most frequent topic, with 13 posts (21.7%).
- The three most frequent topics (Digital banking, Fraud & scams, and Financial wellbeing) account for 33 posts (55%).

A second PivotTable grouped the engagement measures by topic. Weighted engagement rate is calculated as topic total engagements divided by topic total views. In this supplied workbook, `total_engagements` includes likes, replies, reposts, quote posts, and link clicks.

| Topic | Posts | Total engagements | Total views | Weighted engagement rate | Avg. engagements / post |
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

The derived average engagements per post is total topic engagements divided by topic post count. It is a volume-per-post measure, not a rate adjusted for views.

#### Interpretation

1. **Most frequent does not mean most engaging:** Digital banking has the highest post count (13), but its 3.92% weighted engagement rate is below the 4.34% overall rate.
2. **Fraud & scams leads total engagement:** It contributes 36,283 engagements and has the highest topic-level weighted rate (5.33%). This is a strong candidate for closer content and customer-conversation review within the sample.
3. **Security education has the highest average engagements per post:** 22,360 engagements across five posts, or 4,472 per post, with a 5.08% weighted rate. The small post count means the result should be treated as directional.
4. **Financial wellbeing also performs above the overall rate:** 4.54% across nine posts.
5. **Outage/service issue has a low observed rate (1.30%):** Do not interpret this as low business importance. Service-issue posts may be operationally important even when they attract fewer interactions.
6. The smallest topic groups (one customer-support post, two sustainability posts, and three business-banking / branch posts) are too small for strong comparative conclusions.

The topic-level weighted rates reconcile to the supplied totals. The sum of topic engagements is 125,989 and the sum of topic views is 2,906,190, matching `QA_Overview`. The overall 4.34% is a **weighted** rate (`125,989 / 2,906,190`), not the unweighted mean of the 60 row-level rates.

These are results from a small, fictional, curated learning dataset. They do not establish actual CommBank campaign performance or real customer preferences.

### Customer support signals

There are **11 records** with `response_needed = Yes`. These are customer-authored interactions and include complaints, questions, and a scam report. The sample highlights issues around:
- Digital banking access, app timeouts, statement downloads, and NetBank recovery.
- PayID transfer delays and outage information.
- Branch / ATM availability and inaccurate location details.
- Card-blocking questions and scam reporting.

These records can support a proposed service-triage workflow, but the synthetic flags should be treated as labelled examples, not as proof that an operational classifier would be accurate.

## 4. Guided analysis workflow

Complete the steps in order and record your own Excel outputs. Keep the original workbook unchanged and work only in a separate copy. The companion workbook is a structured analysis template; it does not contain the Forage source dataset.

### Step 1 — Protect and profile the workbook

- Confirm the five worksheets and the `SocialMediaPosts` table.
- Confirm 60 records, 24 columns, unique post IDs, date range, and blanks.
- Inspect the `Data_Dictionary` and `Analysis_Prompts` sheets.
- Check the formulas in columns S, T, and U.
- Preserve the 29 blank hashtag values as blanks; do not fabricate data.

### Step 2 — Topic analysis

Use a PivotTable with:
- Rows: `topic`
- Values: Count of `post_id`
- Sort: Largest to smallest

The completed PivotTable has 11 topics and 60 posts. Digital banking is most frequent (13), followed by Fraud & scams (11) and Financial wellbeing (9). The top three topics account for 33 posts (55%). The dataset supplies the topic labels, so describe this as analysis of curated topic categories, not a topic model developed in this exercise.

### Step 3 — Engagement analysis by topic

Use a PivotTable:
- Rows: `topic`
- Values: Count of `post_id`, Sum of `total_engagements`, and Sum of `views`
- Calculate weighted engagement rate = Sum of total engagements / Sum of views.

The completed PivotTable reconciles to 60 posts, 125,989 engagements, and 2,906,190 views. Fraud & scams leads total engagements (36,283) and has the highest topic weighted rate (5.33%). Digital banking is most frequent but its weighted rate is 3.92%, below the overall rate of 4.34%. See the complete topic table in Section 3.

### Step 4 — Engagement analysis by content format

Create a PivotTable:
- Rows: `content_format`
- Values: Count of `post_id`, Sum of `total_engagements`, and Sum of `views`
- Weighted rate = Sum of total engagements / Sum of views.

| Content format | Posts | Total engagements | Total views | Weighted rate | Average engagements / post |
|---|---:|---:|---:|---:|---:|
| Link | 6 | 21,738 | 526,900 | 4.13% | 3,623.0 |
| Text | 36 | 3,809 | 264,990 | 1.44% | 105.8 |
| Text + image | 14 | 73,358 | 1,590,300 | 4.61% | 5,239.9 |
| Text + video | 4 | 27,084 | 524,000 | 5.17% | 6,771.0 |
| **Overall** | **60** | **125,989** | **2,906,190** | **4.34%** | **2,099.8** |

Text + video has the highest weighted rate, but there are only four records. Text + image has the highest total engagements. Text is the most common format (60% of records) but has the lowest weighted rate. These comparisons are descriptive, not causal; content purpose, topic, author, reach, and timing may differ across formats.

### Step 5 — Engagement analysis by post type

Create a PivotTable:
- Rows: `post_type`
- Values: Count of `post_id`, Sum of `total_engagements`, and Sum of `views`
- Weighted rate = Sum of total engagements / Sum of views.

| Post type | Posts | Total engagements | Total views | Weighted rate | Average engagements / post |
|---|---:|---:|---:|---:|---:|
| Mention | 17 | 1,112 | 73,530 | 1.51% | 65.4 |
| Original post | 24 | 123,337 | 2,730,800 | 4.52% | 5,139.0 |
| Quote post | 3 | 621 | 44,600 | 1.39% | 207.0 |
| Reply | 16 | 919 | 57,260 | 1.60% | 57.4 |
| **Overall** | **60** | **125,989** | **2,906,190** | **4.34%** | **2,099.8** |

Original posts are 40% of records but contribute about 97.9% of observed engagements. Mentions and replies serve different conversational purposes, so do not treat their lower interaction totals as weaker customer interest or poorer content quality. Quote posts have only three observations. Post type does not tell us who authored a post; use `author_type` separately.

### Step 6 — Engagement analysis by author type

Create a PivotTable:
- Rows: `author_type`
- Values: Count of `post_id`, Sum of `total_engagements`, and Sum of `views`
- Optional report filter: `post_type`, left at All.

Weighted rate = Sum of total engagements / Sum of views.

**Validated Excel results:**

| Author type | Posts | Total engagements | Total views | Weighted rate |
|---|---:|---:|---:|---:|
| Customer | 28 | 1,597 | 96,990 | 1.65% |
| CommBank | 26 | 123,593 | 2,754,800 | 4.49% |
| Business customer | 2 | 97 | 5,600 | 1.73% |
| Community org | 2 | 237 | 11,100 | 2.14% |
| News/media | 1 | 328 | 31,000 | 1.06% |
| Scam awareness org | 1 | 137 | 6,700 | 2.04% |
| **Grand Total** | **60** | **125,989** | **2,906,190** | **4.34%** |

The PivotTable reconciles to QA_Overview. CommBank-authored records contribute about 98.1% of total engagements and have a 4.49% weighted rate. Customer-authored records have a 1.65% weighted rate. This is not a customer-satisfaction comparison: author groups differ in purpose, reach, and content type. The five non-core organisational categories contain only one or two posts each and should be interpreted cautiously.

### Step 7 — Sentiment analysis

Create two PivotTable views using the supplied `sentiment_label` field. Use `post_id` as a Count in Values.

**View 1 — all posts**
- Rows: `sentiment_label`
- Columns: `author_type`
- Values: Count of `post_id`

**View 2 — Customer-authored posts**
- Rows: `sentiment_label`
- Values: Count of `post_id`
- Filter: `author_type = Customer`

**Validated Excel results**

The all-posts PivotTable reconciles to 60 records and all six author-type totals from Step 6:

| Sentiment | Business customer | CommBank | Community org | Customer | News/media | Scam awareness org | All posts |
|---|---:|---:|---:|---:|---:|---:|---:|
| Mixed | 1 | 0 | 0 | 7 | 0 | 0 | 8 |
| Negative | 0 | 0 | 0 | 11 | 0 | 0 | 11 |
| Neutral | 0 | 13 | 0 | 2 | 0 | 0 | 15 |
| Positive | 1 | 13 | 2 | 8 | 1 | 1 | 26 |
| **All posts** | **2** | **26** | **2** | **28** | **1** | **1** | **60** |

The Customer-only PivotTable reconciles to 28 records:

| Sentiment | Customer posts | Share |
|---|---:|---:|
| Mixed | 7 | 25.00% |
| Negative | 11 | 39.29% |
| Neutral | 2 | 7.14% |
| Positive | 8 | 28.57% |
| **Grand Total** | **28** | **100.00%** |

**Interpretation**
- All 11 Negative labels in the sample belong to Customer-authored posts.
- Seven of the eight Mixed posts are Customer-authored; the remaining one is from a Business customer.
- All 13 Neutral CommBank posts are bank-authored; two Customer-authored posts are Neutral.
- The Customer-only view is the relevant population for describing customer-conversation sentiment. It is still a small, curated, fictional sample and is not a representative measure of actual customer satisfaction.

**Separate qualitative review still required**

The PivotTables validate the quantitative sentiment breakdown. Before closing the full sentiment review, filter the source table to `author_type = Customer` and `sentiment_label = Negative` or `Mixed`. Review `post_text`, `customer_intent`, and `response_needed`; classify recurring themes such as complaints, service incidents, questions, scam reports, or feature feedback. Keep a private review sheet with `post_id`, `sentiment_label`, `customer_intent`, `response_needed`, `issue_theme`, and `analyst_note`. Do not infer urgency from sentiment alone.

The workbook's sentiment labels and scores are supplied curated fields, not predictions from a model trained or evaluated in this exercise. Treat them as triage indicators and use human review for ambiguity, sarcasm, mixed emotion, or urgent financial-service concerns.

### Step 8 — Time and customer-support analysis

- Group post counts by month and day of week.
- Compare topic and sentiment across time.
- Review all rows with `response_needed = Yes`.
- Group response-needed posts by topic and customer intent.
- Identify operational themes that could be routed to a customer-support queue.

The sample covers only part of April, May, and part of June 2026. Do not make strong seasonal or long-term trend claims from this short window.

### Step 9 — Review the X API and operational feasibility

Use the current official documentation, not only the older Forage PDF:
- Search Posts: https://docs.x.com/x-api/posts/search/introduction
- Data Dictionary: https://docs.x.com/x-api/fundamentals/data-dictionary
- Metrics: https://docs.x.com/x-api/fundamentals/metrics
- Getting access / Developer Console: https://docs.x.com/x-api/getting-started/getting-access

Map actual fields carefully:
- Post ID and text: `id`, `text`
- Publication time: `created_at`
- Author: `author_id` plus optional user expansions
- Conversation: `conversation_id`, `in_reply_to_user_id`, referenced posts
- Hashtags / mentions / URLs: `entities`
- Topics / entities: `context_annotations`
- Public engagement: likes, replies, reposts, quotes, impressions, bookmarks
- URL clicks and some other analytics: non-public metrics requiring user-context access to the account's own content

Important API limitation: recent search covers the last seven days. Full-archive search reaches back to March 2006 but requires pay-per-use or Enterprise access. Recent Search supports up to 100 posts per request and Full-Archive Search up to 500, with pagination for larger result sets. Confirm account access, pricing, quotas, permitted use, and data-retention rules before proposing a production integration.

The sample's `link_clicks` field should not be assumed to be universally available for all public posts. X documents URL clicks as non-public metrics and requires user-context authentication for the account's own content. Impressions, likes, reposts, and replies are listed as public metrics.

### Step 10 — Draft the InsightSpark proposal

The proposal should cover:
1. Business objective and use cases.
2. Collection method and fields.
3. Data ingestion, validation, and storage.
4. Topic, sentiment, engagement, trend, and customer-intent analysis.
5. Operational actions and recommended reporting.
6. Privacy, platform-policy, bias, and representativeness limitations.
7. Pilot scope, KPIs, and evaluation.

The proposal should distinguish what the curated sample demonstrates from what would need to be validated with real, authorised data.

## 5. Recommended analytical definitions

| Measure | Definition |
|---|---|
| Post count | Count of distinct post IDs |
| Total engagements (sample) | Likes + replies + reposts + quote posts + link clicks |
| Weighted engagement rate (sample) | Sum of total engagements / sum of views |
| Average sentiment score | Average of supplied sentiment_score for the selected group |
| Response-needed count | Count of records flagged Yes |
| Topic share | Topic post count / all posts in the defined analysis population |

For production X API use, calculate metrics from fields actually returned and authorised. Do not assume that the simulated workbook's combined engagement formula is the same as X's public `public_metrics` object.

## 6. Risks and interpretation limits

- **Synthetic sample:** all records and metrics are fictional; no real customer or business conclusion can be claimed.
- **Small sample:** 60 rows are sufficient for a learning exercise, not for reliable population estimates or statistical significance.
- **Selection bias:** curated topics, sentiment, and response-needed fields are pre-labelled.
- **Authorship mix:** bank-authored content and customer-authored content must be separated before interpreting sentiment.
- **Sentiment ambiguity:** sarcasm, context, slang, and short replies can confuse automated classification. Human review is needed.
- **Engagement confounding:** post type, reach, content format, topic, author size, and campaign timing can all affect engagement.
- **API access:** some metrics require user-context authentication and are limited to the authenticated account's own content.
- **Privacy:** minimise collection of personal data, restrict author-level access, aggregate reporting where possible, and define retention and deletion controls.
- **Platform compliance:** use official API access and follow current X Developer Agreement, policy, and technical limits.

## 7. Task completion checklist

- [x] Inspect the supplied workbook and its data dictionary.
- [x] Confirm the 60-row sample structure and baseline profile.
- [x] Reproduce topic frequencies in Excel (11 topics; 60 posts; totals reconciled).
- [x] Compare engagement by topic (topic-level engagement, views, and weighted rates reconciled to QA_Overview).
- [x] Compare engagement by content format (weighted rates reconcile to QA_Overview).
- [x] Compare engagement by post type (counts and weighted rates reconcile to QA_Overview).
- [x] Reproduce and validate the author_type PivotTable (six categories; 60 posts; totals reconcile to QA_Overview).
- [x] Reproduce all-posts and Customer-only sentiment PivotTables; both views reconcile to their expected record totals.
- [ ] Review negative and mixed Customer-authored post text; document issue themes, intent, and response-needed flags.
- [ ] Review support flags and recurring pain points.
- [ ] Document the current X API fields, access and metric restrictions.
- [ ] Write the InsightSpark proposal.
- [ ] Submit the proposal in the Forage Task 03 response field.
- [ ] Update README and learning log to mark the task complete only after submission.

## References

- [Forage — Commonwealth Bank Introduction to Data Science](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t)
- [X API — Search Posts](https://docs.x.com/x-api/posts/search/introduction)
- [X API — Data Dictionary](https://docs.x.com/x-api/fundamentals/data-dictionary)
- [X API — Metrics](https://docs.x.com/x-api/fundamentals/metrics)
- [X API — Getting Access](https://docs.x.com/x-api/getting-started/getting-access)
- [Forage — Twitter Sentiment Analysis overview](https://cdn.theforage.com/vinternships/companyassets/2sNmYuurxgpFYawco/y2NzZJASdgERdYR4S/1784127137044/Twitter_Sentiment_Analysis_A_Brief_Overview_With_Real_Example.pdf)
- [Forage — X/Twitter API Getting Started PDF](https://cdn.theforage.com/vinternships/companyassets/2sNmYuurxgpFYawco/y2NzZJASdgERdYR4S/1675682056660/getting_started.pdf)
