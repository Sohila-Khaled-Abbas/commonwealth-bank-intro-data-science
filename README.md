<div align="center">

# Commonwealth Bank
## Introduction to Data Science — Forage Virtual Experience

<p>
  <a href="https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t">
    <img src="https://img.shields.io/badge/FORAGE-Virtual%20Experience-176B58?style=for-the-badge" alt="Forage virtual experience" />
  </a>
  <img src="https://img.shields.io/badge/COMMONWEALTH%20BANK-Data%20Science-E4002B?style=for-the-badge" alt="Commonwealth Bank data science" />
  <img src="https://img.shields.io/badge/STATUS-IN%20PROGRESS-D97706?style=for-the-badge" alt="Project status: in progress" />
</p>
<p>
  <img src="https://img.shields.io/badge/Excel-Microsoft%20Excel-217346?style=flat-square&logo=microsoftexcel&logoColor=white" alt="Microsoft Excel" />
  <img src="https://img.shields.io/badge/Analysis-Data%20Aggregation-2563EB?style=flat-square" alt="Data aggregation" />
  <img src="https://img.shields.io/badge/Methods-Formulas%20%7C%20PivotTables-7C3AED?style=flat-square" alt="Excel formulas and PivotTables" />
  <img src="https://img.shields.io/badge/Platform-Windows%2011-0078D4?style=flat-square&logo=windows&logoColor=white" alt="Windows 11" />
</p>
<p>
  <a href="https://github.com/Sohila-Khaled-Abbas/commonwealth-bank-intro-data-science/commits/main"><img src="https://img.shields.io/github/last-commit/Sohila-Khaled-Abbas/commonwealth-bank-intro-data-science?style=flat-square&label=last%20updated" alt="Last commit" /></a>
  <a href="https://github.com/Sohila-Khaled-Abbas/commonwealth-bank-intro-data-science"><img src="https://img.shields.io/github/repo-size/Sohila-Khaled-Abbas/commonwealth-bank-intro-data-science?style=flat-square&label=repository%20size" alt="Repository size" /></a>
  <a href="https://github.com/Sohila-Khaled-Abbas/commonwealth-bank-intro-data-science/stargazers"><img src="https://img.shields.io/github/stars/Sohila-Khaled-Abbas/commonwealth-bank-intro-data-science?style=flat-square&label=stars" alt="GitHub stars" /></a>
</p>

**A task-by-task learning and portfolio project focused on practical data analysis, data handling, business reasoning, and reproducible documentation.**

[Simulation Brief](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t) · [Task 01 Guide](docs/task-01-data-aggregation-analysis.md) · [Task 02 Guide](docs/task-02-data-anonymisation.md) · [Task 03 Guide](docs/task-03-propose-data-analysis-approaches.md) · [Privacy Techniques](docs/data-privacy-techniques.md) · [CV / LinkedIn Positioning](docs/project-experience-positioning.md) · [Local Setup](docs/local-setup-windows.md) · [Learning Log](LEARNING_LOG.md)

</div>

---

> [!IMPORTANT]
> This is an independent learning repository documenting participation in a Forage virtual job simulation. It is **not** an official Commonwealth Bank or Forage repository and does not represent employment, a paid client engagement, or endorsement by either organisation.

> [!CAUTION]
> The Forage source workbook is labelled for simulation use only. Keep the downloaded source file, working copies containing the full dataset, and submitted analysis workbook out of this public repository. The `.gitignore` is configured to help prevent accidental commits.

## Project overview

The simulation uses practical business scenarios to develop data-science and data-management skills. This repository keeps each exercise reviewable through task guides, privacy references, working notes, documented methods, and validation decisions. Source workbooks, customer-level data, and private deliverables remain local.

| Project detail | Description |
|---|---|
| **Program** | Commonwealth Bank — Introduction to Data Science |
| **Provider** | Forage |
| **Format** | Self-paced virtual job simulation |
| **Primary practice tool** | Microsoft Excel Desktop for Tasks 01 and 02 |
| **Analytical methods** | Excel Tables, SUMIFS, COUNTIFS, PivotTables, data minimisation, generalisation, banding, disclosure-risk checks, CSV validation |
| **Completed analyses** | Task 01 — Data Aggregation and Analysis; Task 02 — Data Anonymisation workbook analysis and CSV preparation |
| **Current focus** | Task 03 — Social Media Data and Business Insights |

## Learning roadmap

~~~mermaid
flowchart TD
    A([Forage simulation]) --> B[Task 01 · Data aggregation and analysis]
    B --> B1[Inspect transaction data]
    B1 --> B2[Excel Table and data types]
    B2 --> B3[COUNTIFS and SUMIFS]
    B3 --> B4[PivotTable cross-checks]
    B4 --> B5[Document findings]
    B5 --> C[Task 02 · Data anonymisation and privacy review]
    C --> D[Task 03 · Social media insights]
    D --> E[Task 04 · Database design]
    E --> F([Consolidated learning portfolio])
~~~

| Task | Topic | Skills / methods | Status |
|:---:|---|---|:---:|
| **01** | Data Aggregation and Analysis | Excel Tables, `COUNTIFS`, `SUMIFS`, PivotTables, validation | <img src="https://img.shields.io/badge/ANALYSIS%20COMPLETE-15803D?style=flat-square" alt="Analysis complete" /> |
| **02** | Data Anonymisation | Direct and quasi-identifiers, generalisation, age/salary banding, k-anonymity-style and salary-diversity checks, CSV validation | <img src="https://img.shields.io/badge/CSV%20READY-2563EB?style=flat-square" alt="CSV ready; Forage upload not verified" /> |
| **03** | Propose Data Analysis Approaches | Social-media topic, sentiment, engagement, trend, customer-support analysis, X API mapping, proposal writing | <img src="https://img.shields.io/badge/IN%20PROGRESS-D97706?style=flat-square" alt="In progress" /> |
| **04** | Database Design | Entities, attributes, keys, relationships, analytical data design | <img src="https://img.shields.io/badge/PLANNED-64748B?style=flat-square" alt="Planned" /> |

---

## Task 01 — Data Aggregation and Analysis

<details open>
<summary><strong>Business scenario · Supermarket transaction analysis</strong></summary>

The team lead supplied a transaction dataset containing three years of supermarket activity across Australia. The requested analysis must answer three business questions before the sample is shared with InsightSpark.

### Questions to answer

| # | Business question | Aggregation |
|:---:|---|---|
| **01** | Across locations, how many apples were purchased in cash? | Count matching purchase records, or sum quantity if the row grain requires it |
| **02** | How much total cash was spent on these apples? | Sum transaction amount for apples paid in cash |
| **03** | Across all payment methods, how much money was spent at Bakershire by non-member customers? | Sum transaction amount for Bakershire non-members |

### Formula reference

After importing the source workbook and converting the dataset into an Excel Table named `Transactions`, use these as **field-name examples**. Verify the exact headers in the source file before entering them.

| Question | Excel formula pattern |
|---|---|
| Cash apple purchase records | `=COUNTIFS(Transactions[product_name],"apple",Transactions[payment_method],"cash")` |
| Cash spent on apples | `=SUMIFS(Transactions[total_amount],Transactions[product_name],"apple",Transactions[payment_method],"cash")` |
| Bakershire non-member spend | `=SUMIFS(Transactions[total_amount],Transactions[store],"Bakershire",Transactions[customer_type],"non-member")` |

> [!TIP]
> Counting matching rows is not necessarily the same as counting units sold. Inspect the dataset grain and any quantity column first. If one row can contain multiple apples, use `SUMIFS` over the quantity field for the unit count.

### Analysis and validation workflow

1. Inspect sheet names, headers, row grain, data types, and missing values.
2. Preserve the downloaded source file and create a separate working copy.
3. Format the data as an Excel Table named `Transactions`.
4. Calculate the requested metrics with conditional aggregation formulas.
5. Build PivotTables with the field layouts in the [Task 01 guide](docs/task-01-data-aggregation-analysis.md).
6. Reconcile formula outputs with the PivotTable results.
7. Document formulas, values, validation evidence, and business interpretation.

### Validated Task 01 results

The completed Excel analysis used `SUMIFS` and `COUNTIFS` and was independently reconciled with two PivotTables.

| Business metric | Validated result |
|---|---:|
| Apple units purchased in cash | **117** |
| Cash-paid apple transaction records | **20** |
| Total cash spent on apples | **$537.03** |
| Bakershire non-member spending (all payment methods) | **$2,857.51** |
| Bakershire non-member transaction records | **179** |

The source table contained **50,783** records. The formula outputs matched the corresponding PivotTable values. See the [Task 01 analysis guide](docs/task-01-data-aggregation-analysis.md) for data-quality checks, exact formulas, full PivotTable breakdowns, and limitations.

**Practice workbook:** [Task 01 Excel Practice Starter](workbooks/Task_01_Excel_Practice_Starter.xlsx)  
**Step-by-step instructions:** [Task 01 — Data Aggregation and Analysis](docs/task-01-data-aggregation-analysis.md)

</details>

## Task 02 — Data Anonymisation

The workbook analysis, transformation workflow, privacy checks, and values-only CSV export are complete. The 10,000-record source was reduced to four fields: `Registration_Year`, `Gender`, `Age_Band`, and `Salary_Band`. Direct identifiers, exact dates of birth, precise location data, the customer linkage key, high-cardinality employer/job fields, and unnecessary card details were removed.

**Verified aggregate results:** 10,000 release rows; four columns; zero blanks or `REVIEW` values; 40 quasi-identifier profiles; minimum profile size 111; zero records in groups smaller than five; all 40 profiles contain all five salary bands, with a minimum profile-by-salary count of 12. These are screening results, not a guarantee that re-identification is impossible.

This task was completed independently: no Task 01 transaction data was joined or used. The CSV has been generated and structurally validated; Forage upload/acceptance has not yet been verified.

**Resources:**
- [Task 02 completed workflow, field decisions, and QA results](docs/task-02-data-anonymisation.md)
- [Anonymisation techniques, linkage attacks, and k-anonymity limitations](docs/data-privacy-techniques.md)

Keep the source workbook, working workbook, customer-level CSV, and any identifier mappings outside this public repository.

## Task 03 — Propose Data Analysis Approaches

Task 03 is now in progress. The supplied workbook contains 60 fictional social-media records across 24 fields and five worksheets. The initial profile and field inventory are documented in the [Task 03 guide](docs/task-03-propose-data-analysis-approaches.md).

Initial analysis confirms that digital banking (13 posts), fraud & scams (11), and financial wellbeing (9) are the most frequent curated topics. The workbook includes sentiment labels, engagement metrics, author/post types, customer intent, and response-needed flags. Because the sample is explicitly fictional and only 60 records, results are for learning and method demonstration, not evidence of real CommBank customer opinion or campaign performance.

The next work is to reproduce the profile in Excel, build topic / engagement / sentiment / customer-support analyses, review current X API field and access limitations, and prepare the InsightSpark proposal. Do not mark Task 03 complete until the proposal is submitted through Forage.

## Repository structure

~~~text
commonwealth-bank-intro-data-science/
├── README.md
├── LEARNING_LOG.md
├── .gitignore
├── docs/
│   ├── data-privacy-techniques.md
│   ├── local-setup-windows.md
│   ├── project-experience-positioning.md
│   ├── task-01-data-aggregation-analysis.md
│   └── task-02-data-anonymisation.md
├── workbooks/
│   └── Task_01_Excel_Practice_Starter.xlsx
└── data/                 # Local-only; ignored by Git
    ├── raw/
    └── working/
~~~

## Work locally

Clone the repository into its own project subfolder under the Data Analysis course directory:

~~~powershell
$Parent = "D:\courses\Data Analysis 26-27"
$RepoPath = Join-Path $Parent "commonwealth-bank-intro-data-science"

New-Item -ItemType Directory -Force -Path $Parent | Out-Null
git clone https://github.com/Sohila-Khaled-Abbas/commonwealth-bank-intro-data-science.git $RepoPath
Set-Location $RepoPath
git status
~~~

See [Local Setup — Windows and Microsoft Excel](docs/local-setup-windows.md) for the full workflow, including where to place the private Forage workbook.

## Documentation and learning resources

| Resource | Description |
|---|---|
| [Task 01 guide](docs/task-01-data-aggregation-analysis.md) | Completed Excel analysis, formulas, PivotTables, data-quality checks, and reconciliation |
| [Task 02 guide](docs/task-02-data-anonymisation.md) | Data inventory, transformation decisions, linkage, privacy checks, and CSV delivery |
| [Privacy techniques](docs/data-privacy-techniques.md) | Direct identifiers, quasi-identifiers, pseudonymisation, linkage attacks, and k-anonymity |
| [Project positioning](docs/project-experience-positioning.md) | Accurate CV, LinkedIn, portfolio, and interview wording |
| [Excel starter workbook](workbooks/Task_01_Excel_Practice_Starter.xlsx) | Practice sheets, formula reference, PivotTable guide, and blank analysis area |
| [Learning log](LEARNING_LOG.md) | Task progress, verified results, decisions, and reflection prompts |
| [Windows setup](docs/local-setup-windows.md) | Clone, private data folders, Excel workflow, and safe Git practices |

## Project-based experience

This repository is an independent, task-by-task project based on the Commonwealth Bank Introduction to Data Science virtual experience on Forage. Task 01 is complete and reconciled; Task 02 is being prepared. It is not employment at Commonwealth Bank or a paid freelance engagement.

**CV-ready title:** Data Analytics & Data Science Virtual Experience | Forage — Commonwealth Bank Simulation

The current verified scope includes analysis of 50,783 supermarket records in Excel, data-quality checks, SUMIFS / COUNTIFS calculations, and reconciliation of five metrics using two PivotTables. See [CV and LinkedIn positioning guidance](docs/project-experience-positioning.md). Do not claim later tasks as completed until they have been performed and validated.

## Quality and portfolio standards

- **Reproducibility:** Keep the source, working copy, formulas, and validation steps distinct.
- **Traceability:** Document exact formulas, criteria, data grain, and assumptions.
- **Accuracy:** Cross-check formula results independently with PivotTables.
- **Privacy and licensing:** Do not publish Forage-provided materials marked for simulation-only use.
- **Honest representation:** Describe completed work as a virtual job simulation and independent learning project, not paid freelance experience or employment.
- **Evidence-led completion:** Change task badges and completion dates only after work has been completed and reviewed.

## References

- [Forage — Commonwealth Bank Introduction to Data Science](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t)
- [Microsoft Support — Overview of formulas in Excel](https://support.microsoft.com/en-us/office/overview-of-formulas-in-excel-ecfdc708-9162-49e8-b993-c311f47ca173)
- [Microsoft Support — Sort data in a worksheet](https://support.microsoft.com/en-us/office/quick-start-sort-data-in-an-excel-worksheet-60153f94-d782-47e2-96a8-15cbb7712539)
- [Microsoft Support — Filter data in a range or table](https://support.microsoft.com/en-us/office/filter-data-in-a-range-or-table-01832226-31b5-4568-8806-38c37dcc180e)

---

<div align="center">

**Built as a practical learning record — from raw data inspection to validated business answers.**

<sub>Maintained as part of an independent data analytics learning portfolio.</sub>

</div>
