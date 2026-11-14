# Task 04 — Designing a Database

<badge>Forage task guide</badge>

## Task objective

The Forage simulation describes this task as **Design the structure of information in a database**. The exact task prompt and required deliverable should remain the source of truth. This guide is a learning scaffold; it does not assume an unstated business scenario, entity list, or submission format.

**Forage task:** [Commonwealth Bank — Introduction to Data Science](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t)

## Before designing

1. Open Task 04 in the Forage simulation and read the full scenario, instructions, attached files, and required response format.
2. Copy the business requirements into a private working note. Separate explicit requirements from assumptions.
3. Identify the business events or things the database must represent.
4. Record the questions the database should answer and the expected users or processes.
5. Do not invent missing business rules. Write clarification questions or label assumptions.

## Database-design workflow

### 1. Extract entities

Look for distinct business objects that need their own identity and lifecycle. For each candidate, ask:
- Does it have attributes of its own?
- Can multiple records exist independently?
- Does it have a stable identifier?
- Does it participate in relationships with other objects?

Avoid creating an entity for every noun. Some nouns are attributes, categories, or events rather than independent entities.

### 2. Define attributes and data types

For each entity, document:
- Attribute name and business meaning.
- Data type and sensible length/precision.
- Whether NULL is allowed.
- Default value, if justified.
- Candidate key or uniqueness rule.
- Validation or domain constraints.
- Whether the field contains personal or sensitive information.

Use clear, consistent names. Distinguish identifiers from descriptive attributes.

### 3. Choose keys

- **Primary key:** uniquely identifies each row and is stable.
- **Foreign key:** references a candidate/primary key in a related table.
- **Alternate key:** another candidate key with a uniqueness requirement.
- **Composite key:** use only when the business rule genuinely requires multiple columns to identify a row.

Do not use names, email addresses, or other mutable business attributes as primary keys without a specific reason.

### 4. Define relationships and cardinality

For every relationship, state:
- The entities involved.
- One-to-one, one-to-many, or many-to-many cardinality.
- Optionality (whether participation is mandatory or optional).
- The foreign-key placement.
- Whether a junction/associative table is needed.

Resolve many-to-many relationships with an associative entity when the relational design requires it. Include attributes of the relationship on that associative entity when they describe the association itself.

### 5. Review normalisation and integrity

Use normalisation as a diagnostic, not a mechanical target:
- **1NF:** atomic values and no repeating groups.
- **2NF:** non-key attributes depend on the whole candidate key.
- **3NF:** non-key attributes do not depend transitively on a key through another non-key attribute.

Document any deliberate denormalisation and why it is needed. Define referential integrity, uniqueness, check constraints, and deletion/update behaviour where requirements justify them.

### 6. Draw and validate the ERD

Your ERD should show:
- Entity/table names.
- Primary keys and foreign keys.
- Important attributes and data types, if required.
- Relationship lines and cardinality/optionality notation.
- Junction tables for many-to-many relationships.

Validate the model against every requirement and representative business scenario. Walk through sample records mentally: can the model represent the required event without duplicating facts or losing information?

## Suggested design specification

Use this table as a private working template and publish only the final design that the task permits.

| Entity / table | Attribute | Data type | Key / constraint | Nullable? | Business meaning |
|---|---|---|---|---|---|
| To be derived from prompt | To be identified | To be selected | PK / FK / UNIQUE / CHECK | Yes / No | Requirement-backed definition |

Relationship register:

| Parent entity | Child entity | Cardinality | Optionality | FK / junction table | Business rule |
|---|---|---|---|---|---|
| To be derived from prompt | To be derived from prompt | 1:1 / 1:M / M:N | Mandatory / optional | To be defined | Cite the requirement |

## Quality checklist

- [ ] All entities are supported by the scenario.
- [ ] Each table has a primary key.
- [ ] Foreign keys reference valid parent keys.
- [ ] Relationship cardinalities and optionality are explicit.
- [ ] Many-to-many relationships are resolved appropriately.
- [ ] Attributes have suitable data types and nullability.
- [ ] Repeating groups and avoidable duplication are addressed.
- [ ] Business rules are expressed as constraints where possible.
- [ ] Personal data is minimised and access/retention needs are considered.
- [ ] The design can represent every required scenario.
- [ ] Assumptions and unresolved questions are documented.
- [ ] The final deliverable matches the exact Forage submission instructions.

## Evidence and progress

Record the actual Task 04 prompt, the entities and relationships you derived, design decisions, validation examples, and submission status in [LEARNING_LOG.md](../LEARNING_LOG.md). Do not mark Task 04 complete until the required work is submitted and the platform confirms the task response.

## References

- [Forage — Commonwealth Bank Introduction to Data Science](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t)
