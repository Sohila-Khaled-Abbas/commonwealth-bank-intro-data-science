# Task 01 — Data Aggregation and Analysis

## Objective

Analyse the supermarket transaction sample provided in the Commonwealth Bank Introduction to Data Science simulation and document the calculations requested by the team lead.

## 1. Acquire and inspect the source workbook

1. Open the [official Forage simulation](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t).
2. Open the Resources section for Task 01 and download `supermarket_transactions.xlsx`.
3. Save the downloaded file to `data/raw/supermarket_transactions.xlsx`.
4. Open it in Microsoft Excel Desktop. The supplied resource link is an `.xlsx` workbook even though the task text refers to a “CSV Data Set”. Do not rename the file extension to `.csv`.
5. Inspect sheet names, the header row, first and last records, and the data types of the product, payment, store, customer-type, and amount fields.

**Do not change the raw source file.** Make a separate working copy in `data/working/` before analysis. Do not upload the source workbook to this public repository; the task identifies it as Forage simulation-use-only material.

## 2. Structure the transaction data as an Excel Table

1. In your working copy, select any populated cell in the transaction dataset.
2. Press `Ctrl + T` (or use **Insert → Table**).
3. Confirm **My table has headers** and select **OK**.
4. With the table selected, open **Table Design** and set the Table Name to `Transactions`.
5. Keep the original column names unless you have a documented reason to standardise them. Structured-reference formulas depend on the exact headers.

An independent write-up of this same exercise refers to the fields `product_name`, `payment_method`, `total_amount`, `store`, and `customer_type`. Use them only if they match the actual workbook headers; otherwise adapt the formulas to the real names. This is a learning reference, not an official answer key.

## 3. Question 1 — Count apples purchased in cash

**Question:** Across locations, how many apples were purchased in cash?

### Formula method

If one row represents one product purchase record:

~~~excel
=COUNTIFS(Transactions[product_name],"apple",Transactions[payment_method],"cash")
~~~

If the workbook contains a `quantity` column and a row can represent more than one unit, calculate units rather than records:

~~~excel
=SUMIFS(Transactions[quantity],Transactions[product_name],"apple",Transactions[payment_method],"cash")
~~~

Do not assume counting rows equals counting units. Check the data grain and quantity field first.

### PivotTable cross-check

- **Rows:** `product_name`
- **Columns:** `payment_method`
- **Values:** Count of `product_name` (or Count of a nonblank transaction ID)
- Read the intersection of the Apple row and Cash column.

If the question is interpreted as units and a quantity field exists, use **Sum of quantity** in Values instead of Count.

## 4. Question 2 — Sum the cash spent on apples

**Question:** How much total cash was spent on these apples?

### Formula method

~~~excel
=SUMIFS(Transactions[total_amount],Transactions[product_name],"apple",Transactions[payment_method],"cash")
~~~

This adds the transaction amount only for rows that meet both conditions. Confirm that `total_amount` contains numeric values rather than numbers stored as text.

### PivotTable cross-check

- **Rows:** `product_name`
- **Columns:** `payment_method`
- **Values:** Sum of `total_amount`
- Read the intersection of Apple and Cash.

Open **Value Field Settings** and confirm the calculation is **Sum**, not Count.

## 5. Question 3 — Sum Bakershire spending by non-members

**Question:** Across all payment methods, how much money was spent at Bakershire by non-member customers?

### Formula method

~~~excel
=SUMIFS(Transactions[total_amount],Transactions[store],"Bakershire",Transactions[customer_type],"non-member")
~~~

Do not add a payment-method condition: the question explicitly includes all payment methods.

### PivotTable cross-check

- **Filters:** `store` — select `Bakershire`
- **Rows:** `customer_type`
- **Values:** Sum of `total_amount`
- Read the `non-member` row.

## 6. Validate the results

- [ ] Record the source data row count and ensure no unintended rows were excluded.
- [ ] Check consistent values in key fields (for example `apple`, `cash`, `Bakershire`, and `non-member`).
- [ ] Confirm the amount field is numeric and formatted consistently as currency.
- [ ] State whether the first result counts purchase records or individual units.
- [ ] Reconcile both spending formulas with their PivotTable totals.
- [ ] Confirm the Bakershire calculation includes every payment method.
- [ ] Document formulas and criteria in the workbook.
- [ ] Keep the original downloaded file unchanged.

## 7. Document the output

For each question, record:

| Field | What to document |
|---|---|
| Business question | Exact question from the team lead |
| Method | Formula and/or PivotTable configuration |
| Formula | Exact Excel formula used |
| Result | Independently calculated answer |
| Validation | Matching PivotTable result and data-quality checks |
| Interpretation | One sentence explaining what the result means |

Save the private working deliverable as `outputs/task-01/Task_01_Analysis_Completed.xlsx`. Submit the appropriate workbook through the Forage task upload area. Do not commit a workbook containing the restricted source dataset to this public GitHub repository.

## 8. Excel functions practiced

| Function / feature | Why it is used |
|---|---|
| `COUNTIFS` | Counts records matching multiple criteria |
| `SUMIFS` | Sums values matching multiple criteria |
| Excel Tables | Provide named columns and automatically expanding ranges |
| Structured references | Make formulas readable and less dependent on fixed cell addresses |
| PivotTables | Independently aggregate and cross-check results |
| Filters | Focus analysis on specified categories and locations |

## References

- [Commonwealth Bank — Introduction to Data Science (Forage)](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t)
- [Microsoft Support — Overview of formulas in Excel](https://support.microsoft.com/en-us/office/overview-of-formulas-in-excel-ecfdc708-9162-49e8-b993-c311f47ca173)
- [Microsoft Support — Sort data in a worksheet](https://support.microsoft.com/en-us/office/quick-start-sort-data-in-an-excel-worksheet-60153f94-d782-47e2-96a8-15cbb7712539)
- [Microsoft Support — Filter data in a range or table](https://support.microsoft.com/en-us/office/filter-data-in-a-range-or-table-01832226-31b5-4568-8806-38c37dcc180e)
