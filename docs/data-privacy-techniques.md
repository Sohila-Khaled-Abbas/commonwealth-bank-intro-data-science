# Data anonymisation and privacy techniques

This guide provides the privacy concepts used to plan Task 02. It is a practical learning resource, not a legal determination or a guarantee that a transformed file is anonymous.

## 1. The objective: useful data with a lower identification risk

Anonymisation is not simply deleting names. A dataset may still identify someone through a combination of otherwise ordinary fields or by matching the records against another dataset. A sound release decision considers:

- the intended analytical purpose and the fields needed for it;
- the people who will receive the data and the environment in which they will use it;
- the fields that identify a person directly or indirectly;
- other information that a recipient or a motivated intruder could reasonably obtain;
- the potential harm if a person is identified or a sensitive attribute is inferred.

The Australian Office of the Information Commissioner describes de-identification as removing direct identifiers and then removing or altering other information that could enable identification, and/or using safeguards. It also stresses that risk depends on the context and release environment. See [OAIC — De-identification and the Privacy Act](https://www.oaic.gov.au/privacy/privacy-guidance-for-organisations-and-government-agencies/handling-personal-information/de-identification-and-the-privacy-act).

## 2. Classify fields before changing them

| Field role | Meaning | Illustrative examples | Typical decision |
|---|---|---|---|
| Direct identifier | Identifies a person on its own or with little effort | Full name, email, phone, passport number, full street address, bank/card number | Remove from the recipient dataset unless a tightly controlled identifier is essential |
| Quasi-identifier | May identify a person when combined with other fields or external data | Date of birth, exact age, postcode, suburb, gender, employer, precise dates, rare occupation | Generalise, suppress, or assess combinations |
| Sensitive attribute | Could harm or expose a person if disclosed or inferred | Health, financial details, precise location, account behaviour, other sensitive personal attributes | Retain only at the level required for the approved purpose; assess inference risk |
| Linkage key | Connects the same person across datasets | Customer number, account key, stable app ID | Remove for a standalone anonymous release; use controlled tokenisation only when cross-dataset linkage is authorised and required |
| Analytical attribute | Helps answer the business question but is not necessarily identifying alone | Broad customer segment, age band, broad region, product preference category | Retain only after considering combinations and rare values |

A field can play more than one role. For example, a unique customer ID may not reveal a name by itself, but it is still a stable linkage key. Exact age and postcode may be quasi-identifiers even when neither is a name or contact detail.

## 3. Techniques and when to use them

| Technique | What it does | Example | Trade-off / caution |
|---|---|---|---|
| Removal / suppression | Deletes a field or blanks selected values | Remove name, phone, passport number; suppress a rare suburb | Strongly reduces exposure but may reduce analytical utility |
| Generalisation | Replaces precise data with a broader category | Date of birth to age band; postcode to state/region; exact income to income band | Preserves group-level patterns but can leave small groups |
| Top/bottom coding | Combines extreme values into a single boundary category | Very high ages grouped as 90+; unusually high income grouped as Above threshold | Reduces outlier uniqueness; choose boundaries from the actual distribution and analytical need |
| Aggregation | Releases group totals rather than person-level rows | Customer counts by region and month | Can reduce row-level utility; small cells may still disclose information |
| Perturbation / randomisation | Carefully changes values or adds controlled noise | Small date shifts or statistical noise | Must be designed and assessed; casual random edits can distort analysis and may not stop linkage |
| Pseudonymisation / tokenisation | Replaces an identifier with a stable token | Original customer ID becomes a random customer token | Supports joining when the same token is applied consistently, but is not automatically anonymisation; a protected mapping or matching capability creates residual risk |
| Synthetic data | Generates artificial records that preserve selected statistical patterns | Simulated customer rows with no copied person-level records | Requires testing; synthetic data is not automatically risk-free if it memorises or reproduces source records |
| Access and release controls | Restricts who can access data and what they can do | Secure workspace, limited recipient access, contractual no-reidentification controls | Important additional safeguards; do not treat them as a substitute for necessary data transformations |

**Do not rely on simple masking, encryption, or a plain hash as proof of anonymisation.** NIST explains that some identifier-protection approaches can be reversed or attacked, and recommends a broader risk-based de-identification process. See [NIST SP 800-188 — De-Identifying Government Datasets](https://doi.org/10.6028/NIST.SP.800-188) and [NISTIR 8053 — De-identification of Personal Information](https://doi.org/10.6028/NIST.IR.8053).

## 4. Pseudonymisation is different from anonymisation

A pseudonym replaces a direct identifier with another value so that records can still be linked. If a separate mapping table exists, or the original value can reasonably be recovered or matched, the dataset remains linkable.

This distinction matters in the simulation: the task background says the customer data may be combined with supermarket transactions.

Before changing a customer ID, answer:

1. Is there a shared customer key in both source datasets?
2. Is record-level joining actually required for the analytical purpose?
3. Can the data custodian create the same random token for the same customer in both datasets?
4. Can the mapping table be retained separately by the data custodian and excluded from the recipient package?

If cross-dataset joining is required, a consistent random token can preserve the join while withholding the original ID and mapping table from the recipient. Record the result accurately as **pseudonymised / de-identified for controlled linkage**, not as proven anonymous data. If joining is not required, remove the linkage key from the release file.

Never generate unrelated new IDs independently in each dataset: that destroys the join. Never send the token-to-original-ID mapping to the recipient alongside the tokenised file.

## 5. Linkage attacks: why combinations matter

A linkage attack compares records in a dataset with external information. An individual row might not contain a name, yet a distinctive pattern of dates, locations, demographics, or behaviour could match a person elsewhere.

The classic Netflix example described by Bruce Schneier recounts how a subset of supposedly anonymised movie-rating records could be matched to public ratings associated with named users. The lesson is that removing names and replacing them with random IDs does not necessarily prevent re-identification when detailed behavioural data can be linked to another source. Read [Schneier — Anonymity and the Netflix Dataset](https://www.schneier.com/blog/archives/2007/12/anonymity_and_t_2.html).

For customer data, assess combinations such as:

- age band + region + gender;
- occupation + age band + income band;
- sign-up month + region + rare customer segment;
- any stable identifier + transaction or behavioural attributes.

These are examples only. Use the actual fields in the supplied workbook.

## 6. K-anonymity and its limitations

K-anonymity examines whether each combination of selected quasi-identifiers appears in at least **k records**. If a combination appears once, it is unique in that dataset; if it appears five times, the record shares that combination with four other records.

For a training exercise, **k = 5** can be used as an illustrative review threshold, if appropriate to the dataset and purpose. It is not a universal legal standard or proof of anonymity.

K-anonymity does not by itself prevent every inference attack. If everyone in a group shares the same sensitive attribute, an observer may infer that attribute even without identifying one row. The ICO describes these limitations, including homogeneity and background-knowledge attacks. See [ICO — How do we ensure anonymisation is effective?](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/data-sharing/anonymisation/how-do-we-ensure-anonymisation-is-effective/).

## 7. A practical risk-review mindset

Use a motivated-intruder question:

> Could a reasonably capable person, using information they could reasonably obtain, match a row or infer a sensitive fact about someone?

Review:

- unique and rare combinations;
- extreme or outlier values;
- exact dates and granular geographic fields;
- stable identifiers and repeated behavioural patterns;
- whether separate releases could be combined to narrow identities;
- whether the dataset recipient has other customer or transaction information;
- whether the intended release is public or restricted to a named recipient.

The ICO's motivated-intruder guidance is a useful practical checklist, while the OAIC guidance is especially relevant to the Australian context. Neither source replaces the data owner's privacy review.

## 8. Sources and further reading

1. [Forage — Commonwealth Bank Introduction to Data Science](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t) — Task 02 brief and supplied learning resources.
2. [OAIC — De-identification and the Privacy Act](https://www.oaic.gov.au/privacy/privacy-guidance-for-organisations-and-government-agencies/handling-personal-information/de-identification-and-the-privacy-act) — context, direct identifiers, quasi-identifiers, risk and utility.
3. [NIST SP 800-188 (2023) — De-Identifying Government Datasets: Techniques and Governance](https://doi.org/10.6028/NIST.SP.800-188) — de-identification methods, governance, release models and risk assessment.
4. [NISTIR 8053 (2015) — De-identification of Personal Information](https://doi.org/10.6028/NIST.IR.8053) — direct identifiers, quasi-identifiers, suppression, generalisation and perturbation.
5. [ICO — How do we ensure anonymisation is effective?](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/data-sharing/anonymisation/how-do-we-ensure-anonymisation-is-effective/) — identifiability, motivated-intruder test and k-anonymity limitations. The ICO notes this guidance is under review.
6. [Bruce Schneier — Anonymity and the Netflix Dataset](https://www.schneier.com/blog/archives/2007/12/anonymity_and_t_2.html) — accessible explanation of linkage risk.
7. [Testim — An In-Depth Guide to Data Masking](https://www.testim.io/blog/data-masking/) — the resource linked in the simulation.
8. [AutomateExcel — How to Mask Data in Microsoft Excel and Google Sheets](https://www.automateexcel.com/how-to/mask-data-input/) — Excel-oriented masking examples.

