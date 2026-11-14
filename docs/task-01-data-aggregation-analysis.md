# Task 01 — Data Aggregation and Analysis

> **Analysis status:** Complete  
> **Validation status:** Formula results reconciled against two PivotTables  
> **Overall Forage simulation:** Completed 14 November 2026 (100%, 10/10 assessment result).  
> **Evidence note:** The Forage completion page confirms the overall simulation; a separate Task 01 workbook receipt is not archived here.

## 1. Business context

The Commonwealth Bank Introduction to Data Science virtual experience asks the participant to analyse supermarket transaction data and answer three questions for a team lead.

1. Across all locations, how many apples were purchased in cash?
2. How much cash was spent on those apples?
3. Across all payment methods, how much money was spent at the Bakershire store by non-member customers?

This repository documents an independent learning exercise completed in Microsoft Excel Desktop. It is not an official Commonwealth Bank or Forage publication.

## 2. Dataset and analysis setup

| Item | Observed value |
|---|---|
| Source workbook | `supermarket_transactions.xlsx` |
| Worksheet | `supermarket_transactions` |
| Excel Table | `Transactions` |
| Data rows | **50,783** |
| Transaction identifier | `id` |
| Relevant fields | `quantity`, `product_name`, `unit_price`, `total_amount`, `store`, `payment_method`, `customer_type` |
| Analysis tool | Microsoft Excel Desktop |

The source worksheet contains transaction-level records. Because each record includes a `quantity` field, the number of physical apple units must be calculated by summing quantity—not by counting rows. A separate record count is useful as a supporting metric.

The analysis used a working copy of the source workbook and converted the dataset into an Excel Table named `Transactions`. The raw source file should remain unchanged.

## 3. Data-quality review

The following checks were completed before calculating the business metrics.

| Check | Result | Assessment |
|---|---:|---|
| Transaction rows | 50,783 | Recorded |
| Nonblank transaction IDs | 50,783 | No missing IDs detected |
| Duplicate transaction IDs | 0 | No duplicate IDs detected |
| Missing timestamps | 0 | No missing values detected |
| Missing quantities | 0 | No missing values detected |
| Missing product names | 0 | No missing values detected |
| Missing unit prices | 0 | No missing values detected |
| Missing total amounts | 0 | No missing values detected |
| Missing store values | 0 | No missing values detected |
| Missing payment methods | 0 | No missing values detected |
| Missing customer types | 0 | No missing values detected |
| Non-numeric values in quantity, unit price, and total amount | 0 detected | Numeric fields usable for aggregation |
| Zero or negative quantities | 0 | No flagged values |
| Zero or negative unit prices | 0 | No flagged values |
| Zero or negative total amounts | 0 | No flagged values |

### Arithmetic consistency

A calculated column named `Amount_Check` was added to the working table:

```excel
=IF(ROUND([@quantity]*[@unit_price],2)=ROUND([@total_amount],2),"OK","REVIEW")
```

**Result:** all **50,783** records returned `OK`; no rows were flagged for review by this check.

This test verifies that the recorded total amount agrees with quantity multiplied by unit price to two decimal places. It does not independently prove that the source quantity or unit price is commercially correct.

### Text hygiene

Whitespace and non-printing-character checks were run on `product_name`, `payment_method`, `store`, and `customer_type` using Excel text-cleaning functions such as `TRIM` and `CLEAN`. No issues were flagged by those checks.

The checks are targeted rather than exhaustive: they do not establish that every possible Unicode whitespace or capitalization variant has been eliminated.

### Category-profile checks

The category profiles provided additional control totals:

- Payment methods: cash **12,730**, contactless **12,781**, credit card **12,614**, and debit card **12,658**. Total: **50,783** records.
- Customer types: corporate **8,536**, gold **8,378**, member **8,395**, non-member **8,650**, premium **8,427**, and staff **8,397**. Total: **50,783** records.
- Exact product label `apple`: **80** records.

The product criterion is an exact match to `apple`; products such as `apple juice`, `apple pie`, and `apple sauce` are not included.

## 4. Formula-based analysis

All formulas below use the structured references from the `Transactions` Excel Table.

### Question 1 — Apple units purchased in cash

**Formula — units:**

```excel
=SUMIFS(Transactions[quantity],Transactions[product_name],"apple",Transactions[payment_method],"cash")
```

**Supporting formula — matching transaction records:**

```excel
=COUNTIFS(Transactions[product_name],"apple",Transactions[payment_method],"cash")
```

| Metric | Result |
|---|---:|
| Apple units purchased in cash | **117 units** |
| Cash-paid apple transaction records | **20 records** |

The distinction matters: `COUNTIFS` counts matching records, while `SUMIFS` over `quantity` calculates the number of apple units represented by those records.

### Question 2 — Total cash spent on apples

**Formula:**

```excel
=SUMIFS(Transactions[total_amount],Transactions[product_name],"apple",Transactions[payment_method],"cash")
```

**Result:** **$537.03**

The formula sums `total_amount` only when the product name is exactly `apple` and the payment method is `cash`.

### Question 3 — Bakershire non-member spending

**Formula:**

```excel
=SUMIFS(Transactions[total_amount],Transactions[store],"Bakershire",Transactions[customer_type],"non-member")
```

**Supporting formula — matching transaction records:**

```excel
=COUNTIFS(Transactions[store],"Bakershire",Transactions[customer_type],"non-member")
```

| Metric | Result |
|---|---:|
| Bakershire non-member spending | **$2,857.51** |
| Bakershire non-member transaction records | **179 records** |

No payment-method criterion was applied because the business question explicitly asks for spending across all payment methods.

## 5. Independent PivotTable reconciliation

The formula results were checked with two PivotTables built from the same `Transactions` Table. The PivotTables provide a second aggregation route and make the criteria and subtotals visible.

### PivotTable 1 — Cash purchases of apples

**Field layout**

| PivotTable area | Field / setting |
|---|---|
| Filters | `product_name` = `apple` |
| Rows | `payment_method` |
| Values | Sum of `quantity` |
| Values | Count of `id` |
| Values | Sum of `total_amount` |

**Observed PivotTable**

| Payment method | Sum of quantity | Count of id | Sum of total amount |
|---|---:|---:|---:|
| Cash | 117 | 20 | $537.03 |
| Contactless | 104 | 16 | $477.36 |
| Credit card | 138 | 22 | $633.42 |
| Debit card | 88 | 22 | $403.92 |
| **Grand total** | **447** | **80** | **$2,051.73** |

The cash row confirms all three cash-apple metrics. The grand total also reconciles to the product profile: 80 exact-`apple` records across all payment methods.

### PivotTable 2 — Bakershire non-member spending

**Field layout**

| PivotTable area | Field / setting |
|---|---|
| Filters | `store` = `Bakershire` |
| Rows | `customer_type` |
| Values | Sum of `total_amount` |
| Values | Count of `id` |

**Observed PivotTable**

| Customer type | Sum of total amount | Count of id |
|---|---:|---:|
| Corporate | $2,772.85 | 185 |
| Gold | $2,388.92 | 166 |
| Member | $2,696.72 | 188 |
| Non-member | $2,857.51 | 179 |
| Premium | $2,604.98 | 186 |
| Staff | $2,517.04 | 182 |
| **Grand total** | **$15,838.02** | **1,086** |

The non-member row confirms the spending and transaction-count results. The six customer-type counts sum to 1,086, matching the total number of records for Bakershire.

**Counting note:** `Count of id` was used to count transaction records. Counting `customer_id` would count nonblank customer-ID cells, not necessarily distinct customers, and is not the appropriate transaction-count measure.

## 6. Final business answers

| # | Business question | Validated answer |
|---:|---|---:|
| 1 | How many apples were purchased in cash across all locations? | **117 apples (units)** |
| 2 | How much cash was spent on those apples? | **$537.03** |
| 3 | How much did non-member customers spend at Bakershire across all payment methods? | **$2,857.51** |

### Reconciliation summary

| Metric | Formula result | PivotTable result | Outcome |
|---|---:|---:|---|
| Cash apple units | 117 | 117 | Match |
| Cash-paid apple records | 20 | 20 | Match |
| Cash spent on apples | $537.03 | $537.03 | Match |
| Bakershire non-member spending | $2,857.51 | $2,857.51 | Match |
| Bakershire non-member records | 179 | 179 | Match |

**Conclusion:** all five reported metrics matched between the formula-based calculations and the PivotTable reconciliation.

## 7. Method notes and limitations

- The apple analysis uses an exact `product_name = "apple"` criterion. It intentionally excludes other apple-related product names.
- The first answer reports **units**, using `SUMIFS` over `quantity`; the number of supporting transaction records is reported separately.
- The Bakershire analysis includes every payment method. Adding a payment-method filter would change the scope of the question.
- The transaction count uses `id`, not `customer_id`, because the requested supporting count is a count of records rather than distinct customers.
- The arithmetic check confirms internal consistency between quantity, unit price, and total amount. It does not validate the original source-system business rules or prices.
- The analysis is descriptive. It does not establish causal explanations or performance trends.

## 8. Reproducibility and deliverable handling

Recommended local workbook layout:

1. `supermarket_transactions` — source data in the working copy, structured as `Transactions`.
2. `Data_Quality` — completeness, uniqueness, numeric checks, text checks, and category profiles.
3. `Analysis` — business questions, formulas, and calculated answers.
4. `Pivot_Reconciliation` — the two PivotTables and their supporting totals.

Recommended private output path:

```text
data/working/Task_01_Analysis_Completed.xlsx
```

Keep the downloaded source workbook and any workbook containing the full Forage dataset out of the public repository. The overall Forage completion page records the simulation as completed on 14 November 2026; a separate Task 01 workbook receipt is not archived here. This repository contains the methodology and aggregate findings, not the restricted source data.

## 9. Excel skills practiced

| Skill / feature | Application |
|---|---|
| Excel Tables and structured references | Reusable, readable formulas over 50,783 records |
| `COUNTIFS` | Counting records that meet multiple criteria |
| `SUMIFS` | Conditional aggregation of quantities and transaction amounts |
| `IF` and `ROUND` | Row-level arithmetic consistency check |
| `TRIM` and `CLEAN` | Targeted text hygiene checks |
| PivotTables | Independent aggregation and reconciliation |
| Filters and value-field settings | Applying exact scope and selecting Sum versus Count |
| Data profiling | Checking category distributions and control totals |
| Documentation | Recording assumptions, formulas, results, and limitations |

## References

- [Commonwealth Bank — Introduction to Data Science (Forage)](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t)
- [Microsoft Support — Overview of formulas in Excel](https://support.microsoft.com/en-us/office/overview-of-formulas-in-excel-ecfdc708-9162-49e8-b993-c311f47ca173)
- [Microsoft Support — Filter data in a range or table](https://support.microsoft.com/en-us/office/filter-data-in-a-range-or-table-01832226-31b5-4568-8806-38c37dcc180e)
