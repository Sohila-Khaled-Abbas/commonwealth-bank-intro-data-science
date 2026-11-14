# Task 02 — Data Anonymisation

> **Workbook analysis:** Complete  
> **Final values-only release:** Validated  
> **CSV export:** Created and structurally validated  
> **Overall Forage simulation:** Completed 14 November 2026 (100%, 10/10 assessment result).  
> **Evidence note:** The completion page verifies the overall simulation; a separate Task 02 CSV receipt is not archived here.

This guide records the completed Task 02 work using the private Forage workbook and the values-only Final_Export sheet. The source workbook and row-level CSV must remain private and must not be committed to this repository.

## 1. Task objective and scope

Forage asks the learner to anonymise mobile-app customer information while preserving useful information for InsightSpark's data scientists. Its examples include removing unnecessary fields such as names and credit-card numbers, masking identifying information, and grouping personal figures such as age and income into bands.

The Forage page describes the input as mobile_customers.csv, while its download resource is an Excel workbook named mobile_customers.xlsx. The required deliverable is a CSV.

**Standalone scope:** Task 02 was completed independently. No join was made to Task 01's supermarket transaction data, and no Task 01 records were used. The source customer_id was removed because a person-level linkage requirement was not established for this release.

## 2. Private workbook and source profile

The reviewed working workbook is mobile_customers_working.xlsx. It contains:

| Worksheet | Purpose |
|---|---|
| mobile_customers | Source data in the MobileCustomers table |
| Column_Inventory | Field classification, risk, utility, and proposed treatment |
| Transformation_Log | Field-level transformation decisions |
| QA_Checks | Source profiling, release checks, combination groups, and privacy summaries |
| Release_Ready | Formula-driven transformed dataset and salary-band PivotTable |
| Final_Export | Static values-only output intended for CSV delivery |

### Source facts

- Source table: MobileCustomers
- Data records: **10,000**
- Source fields: **19**
- Row grain: one mobile customer record per row
- customer_id: 10,000 unique, nonblank values
- Registration-date range: **22 June 2019 – 21 June 2022**
- Age range: **18–65**
- Salary range: **20,014–244,926**

Source field names:

#, customer_id, date_registered, username, name, gender, address, email, birthdate, current_location, residence, employer, job, age, salary, credit_card_provider, credit_card_number, credit_card_security_code, and credit_card_expire.

### Profiling observations

| Field / check | Result |
|---|---:|
| Gender F | 4,951 |
| Gender M | 5,049 |
| Distinct job categories | 639 |
| Distinct employer categories | 8,645 |
| Source customer IDs duplicated | 0 |
| Source customer IDs blank | 0 |

The high-cardinality job and employer fields were removed instead of attempting an unsupported occupation or industry mapping.

## 3. Field treatment

| Source field | Role / risk | Final treatment |
|---|---|---|
| # | Technical row index | Remove |
| customer_id | Stable linkage key | Remove; no linkage required for this standalone exercise |
| date_registered | Quasi-identifier when exact | Generalise to registration year |
| username, name, email | Direct identifiers | Remove |
| gender | Quasi-identifier; context-dependent sensitivity | Retain as F/M after combination review |
| address, residence | Direct / location identifiers or quasi-identifiers | Remove |
| birthdate | High-risk quasi-identifier | Remove |
| current_location | Precise GPS location | Remove |
| employer | High-cardinality quasi-identifier (8,645 categories) | Remove |
| job | High-cardinality quasi-identifier (639 categories) | Remove |
| age | Quasi-identifier | Generalise to five age bands |
| salary | Sensitive financial attribute | Generalise to five salary bands |
| credit_card_provider | Financial attribute, low utility for this task | Remove |
| credit_card_number | Sensitive financial identifier | Remove |
| credit_card_security_code | Highly sensitive authentication data | Remove |
| credit_card_expire | Financial detail | Remove |

## 4. Final release schema

The final values-only release contains exactly four fields:

| Output field | Transformation | Categories / scope |
|---|---|---|
| Registration_Year | Generalise exact registration date to calendar year | 2019, 2020, 2021, 2022 |
| Gender | Retain two existing categories | F, M |
| Age_Band | Generalise exact age | 18–24, 25–34, 35–44, 45–54, 55–65 |
| Salary_Band | Generalise exact salary | 20,000–49,999; 50,000–99,999; 100,000–149,999; 150,000–199,999; 200,000+ |

### Release distributions

**Registration year**

| Year | Records |
|---|---:|
| 2019 | 1,786 |
| 2020 | 3,300 |
| 2021 | 3,349 |
| 2022 | 1,565 |
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

## 5. Privacy assessment

The selected quasi-identifiers for the equivalence-class check were:

Registration_Year + Gender + Age_Band

The four years, two gender categories, and five age bands produce 40 possible profiles.

### K-anonymity-style group-size result

| Privacy check | Result |
|---|---:|
| Distinct quasi-identifier combinations | 40 |
| Smallest group size | 111 |
| Records in groups smaller than 5 | 0 |
| Records in singleton groups | 0 |

Every record shares its quasi-identifier profile with at least 110 other records in this release. The observed minimum equivalence-class size is 111, exceeding the training review threshold of five.

### Sensitive salary attribute review

Salary was assessed separately within each of the 40 quasi-identifier groups.

- Every group contained **all five** salary bands.
- There were **200 profile-by-salary-band cells** (40 profiles × 5 bands).
- No profile had a missing salary-band category.
- The smallest count in any profile/salary-band cell was **12**.

This satisfies a distinct 5-diversity-style check for the grouped salary attribute: each equivalence class contains at least five distinct salary categories. The smallest category frequency was also greater than one.

These checks reduce obvious uniqueness and salary-homogeneity risks within the released fields. They do **not** prove that re-identification is impossible. External datasets, background knowledge, recipient access, and future data releases may change the risk. Do not describe this file as formally certified or guaranteed anonymous.

## 6. Final export validation

The workbook's Final_Export sheet was checked as the export source:

| Validation | Result |
|---|---:|
| Header fields | 4 |
| Data rows | 10,000 |
| Formula cells in Final_Export | 0 |
| Blank cells | 0 |
| REVIEW values | 0 |
| Distinct quasi-identifier profiles | 40 |
| Minimum profile size | 111 |
| Minimum salary-band count within any profile | 12 |

A UTF-8 CSV was generated from the values-only sheet and reopened for structural validation:

- Header: Registration_Year, Gender, Age_Band, Salary_Band
- Data records: **10,000**
- Columns per record: **4**
- Blank values: **0**
- REVIEW values: **0**

The local filename is mobile_customers_anonymised.csv. Keep this row-level deliverable outside the public repository.

### Workbook documentation consistency note

The final release sheet uses Registration_Year, and the QA check shows four distinct registration years. The workbook's preliminary inventory and transformation-log wording for date_registered still refers to month/quarter generalisation. Update those two notes to **registration year** before treating the workbook as a fully reconciled audit artifact.

### Companion workbook template

A separate Task 02 workbook template accompanies this guide. It provides a field inventory, aggregate QA reference, and release-schema worksheet. It is a documentation/template artifact only; it does not contain the private source records or the actual submitted CSV. The verified release schema documented above is `Registration_Year`, `Gender`, `Age_Band`, and `Salary_Band`.

## 7. Reproduction notes

The following Excel formulas were used in the formula-driven Release_Ready sheet.

**Registration year**

~~~excel
=TEXT(INDEX(MobileCustomers[date_registered],ROWS($A$2:A2)),"yyyy")
~~~

**Gender validation**

~~~excel
=LET(g,INDEX(MobileCustomers[gender],ROWS($B$2:B2)),IF(g="","",IF(OR(g="F",g="M"),g,"REVIEW")))
~~~

**Age bands**

~~~excel
=LET(a,INDEX(MobileCustomers[age],ROWS($C$2:C2)),IF(a="","",IF(NOT(ISNUMBER(a)),"REVIEW",IFS(a<=24,"18–24",a<=34,"25–34",a<=44,"35–44",a<=54,"45–54",a<=65,"55–65",TRUE,"REVIEW"))))
~~~

**Salary bands**

~~~excel
=LET(s,INDEX(MobileCustomers[salary],ROWS($D$2:D2)),IF(s="","",IF(NOT(ISNUMBER(s)),"REVIEW",IFS(AND(s>=20000,s<50000),"20,000–49,999",AND(s>=50000,s<100000),"50,000–99,999",AND(s>=100000,s<150000),"100,000–149,999",AND(s>=150000,s<200000),"150,000–199,999",s>=200000,"200,000+",TRUE,"REVIEW"))))
~~~

For export, the Release_Ready range was copied into Final_Export with **Paste Special → Values**. Only the four approved fields were exported.

## 8. Submission and privacy handling

- [x] Inspect the actual source schema and row grain.
- [x] Complete the column inventory and transformation decisions.
- [x] Remove direct identifiers, location details, linkage key, and unnecessary financial fields.
- [x] Generalise dates, age, and salary.
- [x] Validate the 10,000-row, four-column values-only release.
- [x] Review quasi-identifier group sizes.
- [x] Review salary diversity within each profile.
- [x] Generate and structurally validate the UTF-8 CSV.
- [x] Complete the Task 02 workflow within the Forage simulation; overall completion confirmed 14 November 2026. A separate CSV upload receipt is not archived in this repository.

Do not commit the source workbook, working workbook, CSV, customer-level output, or identifier mappings. The repository should contain methodology and aggregate validation results only.

## References

- [Forage — Commonwealth Bank Introduction to Data Science](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t)
- [Data anonymisation and privacy techniques](data-privacy-techniques.md)
- [OAIC — De-identification and the Privacy Act](https://www.oaic.gov.au/privacy/privacy-guidance-for-organisations-and-government-agencies/handling-personal-information/de-identification-and-the-privacy-act)
- [NIST SP 800-188](https://doi.org/10.6028/NIST.SP.800-188)
- [ICO — How do we ensure anonymisation is effective?](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/data-sharing/anonymisation/how-do-we-ensure-anonymisation-is-effective/)
- [Schneier — Anonymity and the Netflix Dataset](https://www.schneier.com/blog/archives/2007/12/anonymity_and_t_2.html)
- [Microsoft Support — Overview of formulas in Excel](https://support.microsoft.com/en-us/office/overview-of-formulas-in-excel-ecfdc708-9162-49e8-b993-c311f47ca173)
