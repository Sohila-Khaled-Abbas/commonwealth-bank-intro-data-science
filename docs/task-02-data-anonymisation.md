# Task 02 — Data Anonymisation

> **Task status:** Preparation and research guide published; source schema not yet documented.  
> **Required deliverable:** One anonymised / appropriately de-identified CSV file uploaded to the Forage Task 02 submission area.  
> **Source handling:** The supplied file is marked for Forage simulation use only. Do not publish the source file, working copy, mapping table, or customer-level output in this repository.

## 1. Business objective

The team lead needs a customer dataset from the CBA mobile app to be used alongside supermarket transaction data by InsightSpark. The objective is to reduce the risk of identifying individuals while retaining the fields and level of detail needed for the approved analysis.

The job is not simply to delete names. The combination of demographic, geographic, temporal, behavioural, and identifier fields may enable linkage to other information. The release must be assessed as a whole.

## 2. Obtain and protect the source file

The Task 02 page describes the input as **mobile_customers.csv**, but the supplied download URL points to an Excel workbook named **mobile_customers.xlsx**. Keep its real file extension.

1. Download the [Task 02 source workbook — mobile_customers.xlsx](https://cdn.theforage.com/vinternships/companyassets/2sNmYuurxgpFYawco/y2NzZJASdgERdYR4S/1675678382479/mobile_customers.xlsx) from the Forage resources. The [simulation page](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t) contains the task instructions.
2. Save the original workbook locally as **data/raw/mobile_customers.xlsx**.
3. Keep the original unchanged.
4. Create a working copy named **data/working/mobile_customers_working.xlsx**.
5. Work only in the copy. Do not upload either workbook to GitHub.

The repository ignores files inside **data/raw/** and **data/working/**. Confirm this with **git status --ignored** before committing documentation.

## 3. Inspect the workbook before transforming anything

Open the working copy in Microsoft Excel Desktop.

Record the following without sharing individual customer values:

- worksheet names and header names;
- number of rows and columns;
- what one row represents (one customer, one account, one event, or something else);
- whether there is a unique customer key;
- data types and example categories for each field;
- missing-value counts;
- duplicate-row and duplicate-ID counts;
- numeric ranges, date ranges, category frequencies, and rare values.

Do not guess the schema. The exact field names and the row grain determine which transformations are appropriate.

### Suggested workbook work areas

Keep these as working sheets inside the private workbook if helpful:

| Sheet | Purpose |
|---|---|
| Raw_Customers | Untouched copy of source data inside the private working workbook |
| Column_Inventory | Field classification, business utility and proposed transformation |
| Transformations | Formula-based or Power Query transformations |
| QA_Checks | Row counts, missing values, uniqueness and disclosure-risk checks |
| Release_Ready | Only the fields approved for the CSV export |

The final CSV should contain only the approved release table, not helper columns, formulas, mapping data, or a raw-data sheet.

## 4. Build a column inventory and risk plan

Create a table with these columns:

| Column | Data type | Role | Identification risk | Analytical value | Proposed action | Reason / QA check |
|---|---|---|---|---|---|---|
| Enter actual header | Text / number / date | Direct ID / quasi-ID / sensitive / linkage key / analytical | High / medium / low | High / medium / low | Remove / generalise / band / tokenise / retain / suppress | Explain why and how it will be checked |

Use the definitions and examples in [Data anonymisation and privacy techniques](data-privacy-techniques.md).

### Classification rules of thumb

| If a column contains… | Initial classification | Possible treatment |
|---|---|---|
| Name, personal email, phone number, passport, card or account number, full street address | Direct identifier | Remove from the released file |
| Date of birth, exact age, full postcode, suburb, exact location, precise sign-up date | Potential quasi-identifier | Generalise or suppress after assessing combinations |
| Exact income or detailed financial information | Sensitive / high-risk attribute | Convert to an approved band or remove if not required |
| Health, highly personal preference or other sensitive characteristic | Sensitive attribute | Retain only if essential and explicitly justified; assess inference risk |
| Unique customer ID that must connect to transaction data | Linkage key | Use a consistent, protected token only if linkage is authorised and required |
| Broad customer segment or other business variable | Analytical attribute | Retain only if it does not create high re-identification risk in combination |

These are examples, not assumptions about the supplied file. Do not create transformations for columns that are not present.

## 5. Decide how the recipient will use the data

The background specifically mentions combining customer data with supermarket transactions. Resolve the linkage requirement before removing or changing a customer key.

### Option A — Record-level linkage is required

If there is a shared customer identifier in both datasets and InsightSpark must join records:

1. Confirm that the join is authorised and that both datasets use the same source identifier.
2. Have the data custodian create one random surrogate token for each source customer ID.
3. Apply the **same mapping** to the customer file and the transaction file.
4. Keep the original-ID-to-token mapping in a separate, access-restricted location controlled by the data custodian.
5. Do not include the original ID or the mapping table in the recipient package.

This preserves a join without revealing the original identifier to the analyst. It is **pseudonymisation**, not proof of irreversible anonymisation: the custodian still has the mapping and records remain linkable.

Do not create unrelated replacement IDs in each file. The join will fail. Do not rely on a partially masked ID or a plain, unsalted hash as a privacy guarantee.

### Option B — No person-level linkage is required

If InsightSpark only needs aggregate insights, remove the customer-level linkage key from the release file and consider whether an aggregated output is sufficient. Do not retain a stable pseudonymous identifier just because it is convenient.

Document which option was selected and why. If the task brief does not make the required linkage clear, record that as an assumption to confirm rather than silently making a high-impact decision.

## 6. Apply transformations while preserving useful information

Use the actual distribution and analytical purpose to select boundaries. The examples below are illustrative only.

| Source field pattern | Example transformation | Utility retained |
|---|---|---|
| Full date of birth | Age band (for example, 18–24, 25–34, 35–44, 45–54, 55+) | Broad age segmentation |
| Exact annual income | Approved income bands | Relative customer segment |
| Full address / street / house number | Remove; retain a broad region only if necessary | Broad geographic analysis |
| Detailed postcode | State or larger region | Regional comparison |
| Exact sign-up timestamp | Month or quarter, if day-level precision is not needed | Acquisition trends |
| Rare occupation or customer category | Broader occupation / segment group or suppression | Broad segmentation |
| Phone, personal email, name, passport or card number | Remove | Usually not required for general customer analysis |
| Shared customer ID | Consistent random token only where authorised linkage is necessary | Cross-dataset joins without exposing the original ID |

### Excel implementation principles

- Prefer deleting unnecessary direct-identifier columns over hiding them.
- Replace exact numeric fields with reviewed category bands when granular values are not needed.
- Replace precise dates and locations with broader values only to the granularity required.
- Review rare values and combinations after transformation; one-off categories can remain identifying.
- If formulas are used, create a release table, copy it, and **Paste Special → Values** before export.
- Never leave the source ID, helper lookup table, raw fields, or hidden sensitive columns in the release sheet.

For an optional age-band formula, adapt the boundaries to the observed age range and approved analysis:

~~~excel
=IFS([@Age]<25,"Under 25",[@Age]<35,"25–34",[@Age]<45,"35–44",[@Age]<55,"45–54",TRUE,"55+")
~~~

This example assumes an **Age** column and is not a recommended fixed policy. Check for missing, invalid, or out-of-range ages before using it.

## 7. Assess combinations and re-identification risk

Do not validate fields one at a time only. A person may be distinguishable by a combination of fields.

1. Choose the fields retained as quasi-identifiers (for example, the transformed age band, broad region and a demographic category—only if those fields exist).
2. Count the number of rows sharing each combination.
3. Review unique and small groups.
4. Generalise or suppress rare combinations if the analytical purpose still permits it.
5. Review whether the values of a sensitive attribute are effectively revealed within a group.
6. Consider whether a recipient could link the rows to supermarket transactions or other reasonably available information.

For a training-only k-anonymity check, an Excel Table could use a calculated column such as:

~~~excel
=COUNTIFS(ReleaseReady[Age_Band],[@Age_Band],ReleaseReady[Region],[@Region],ReleaseReady[Gender],[@Gender])
~~~

Replace the example fields with the actual quasi-identifiers. The result is the size of the equivalence group for that row. Then review the minimum group size and count records below the chosen threshold, for example:

~~~excel
=MIN(ReleaseReady[Equivalence_Class_Size])
=COUNTIF(ReleaseReady[Equivalence_Class_Size],"<5")
~~~

A value of five is an illustrative learning threshold, not a universal standard or guarantee. K-anonymity does not alone prevent homogeneity, background-knowledge or other inference attacks. See the ICO discussion in the privacy techniques guide.

Use a motivated-intruder mindset: could a reasonably capable recipient use public or other available data to match a distinctive record to a person?

## 8. Validate the release table

Complete these checks before exporting:

- [ ] The release table contains only approved columns.
- [ ] Direct identifiers have been removed.
- [ ] The original customer ID is absent unless an approved, consistent token is being used.
- [ ] No mapping table, raw sheet, helper column, hidden column, comment, or formula is included in the release.
- [ ] Quasi-identifiers have been generalised or justified.
- [ ] Rare combinations and small equivalence groups have been reviewed.
- [ ] Sensitive attributes do not become trivially inferable within groups.
- [ ] The release row count matches the intended scope; any removed rows are explained.
- [ ] Required fields have appropriate completeness and data types.
- [ ] A retained surrogate key is unique and consistent across both datasets if record linkage is required.
- [ ] The file can be opened and read correctly after export.
- [ ] No raw customer-level values or source workbook have been committed to GitHub.

A clean result from a spreadsheet check is not a formal privacy certification. Record the transformations and remaining assumptions accurately.

## 9. Export the required CSV

The supplied resource is an **.xlsx** workbook, but the task requires a **CSV** deliverable.

1. Select the **Release_Ready** sheet and ensure it contains only the final approved table.
2. Convert formulas to values and remove helper columns.
3. Check that the first row is the intended header row.
4. Select **File → Save As → Browse**.
5. Choose **CSV UTF-8 (Comma delimited) (*.csv)**.
6. Save the file locally as:

~~~text
data/working/mobile_customers_anonymised.csv
~~~

7. Confirm Excel's warning that only the active sheet is saved. This is expected for CSV; verify that the active sheet is the release table.
8. Close the export, reopen the CSV in a fresh Excel workbook, and verify the headers, row count, delimiters, special characters, date formats, and numeric fields.
9. Upload only the CSV through the Task 02 submission area in Forage.

## 10. Record the decision and handover

In the private working workbook or your learning notes, document:

| Item | Record |
|---|---|
| Source file and version | |
| Row grain and source row count | |
| Direct identifiers removed | |
| Quasi-identifiers generalised / suppressed | |
| Sensitive attributes retained and justification | |
| Linkage requirement and chosen approach | |
| Join-key handling (if applicable) | |
| Risk checks performed | |
| Utility checks performed | |
| Output CSV row and column counts | |
| Assumptions / limitations | |
| Forage submission status | |

Do not put customer-level data or the token mapping in this public record.

## 11. Completion criteria

Task 02 is ready to mark complete only when:

- the actual workbook schema has been inspected;
- the column inventory and transformation decisions are documented;
- the release table has passed data-utility and disclosure-risk checks;
- the CSV has been reopened and validated;
- the required CSV has been submitted through Forage.

## References

- [Forage — Commonwealth Bank Introduction to Data Science](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t)
- [Data anonymisation and privacy techniques](data-privacy-techniques.md)
- [OAIC — De-identification and the Privacy Act](https://www.oaic.gov.au/privacy/privacy-guidance-for-organisations-and-government-agencies/handling-personal-information/de-identification-and-the-privacy-act)
- [NIST SP 800-188](https://doi.org/10.6028/NIST.SP.800-188)
- [ICO — How do we ensure anonymisation is effective?](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/data-sharing/anonymisation/how-do-we-ensure-anonymisation-is-effective/)
- [Schneier — Anonymity and the Netflix Dataset](https://www.schneier.com/blog/archives/2007/12/anonymity_and_t_2.html)
- [Testim — An In-Depth Guide to Data Masking](https://www.testim.io/blog/data-masking/)
- [AutomateExcel — How to Mask Data in Microsoft Excel and Google Sheets](https://www.automateexcel.com/how-to/mask-data-input/)
- [Microsoft Support — Overview of formulas in Excel](https://support.microsoft.com/en-us/office/overview-of-formulas-in-excel-ecfdc708-9162-49e8-b993-c311f47ca173)
- [Microsoft Support — Top ten ways to clean your data](https://support.microsoft.com/en-us/office/top-ten-ways-to-clean-your-data-2844b620-677c-47a7-ac3e-c2e157d1db19)

