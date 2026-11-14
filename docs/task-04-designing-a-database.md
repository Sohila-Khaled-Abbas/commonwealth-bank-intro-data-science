# Task 04 — Designing a Database

> **Status:** In progress — task requirements reviewed; field inventory, relational schema, and Forage text submission remain.
>
> **Required Forage deliverable:** A text proposal listing tables, each field and its description, primary keys, and relationships.
>
> **Task scope:** Store posts published by the CommBank X/Twitter account, user replies, user quote retweets, and direct mentions.

## 1. Task objective and requirements

The Forage brief asks for a proposed database structure for four connected kinds of social-media information:

1. Tweets/posts published by the CommBank account.
2. User replies to CommBank posts.
3. User quote retweets / quote posts referring to CommBank posts.
4. Direct mentions of CommBank in user posts.

The submission must contain:
- The list of tables.
- Each table's fields, variable names, and brief descriptions.
- The primary key for every table.
- Relationships between tables.

This is a **database-design proposal**, not an instruction to build or deploy a live database. No specific database engine or SQL dialect is required by the task prompt. Use a relational design and explain assumptions.

**Forage task:** [Commonwealth Bank — Introduction to Data Science](https://www.theforage.com/simulations/commonwealth-bank/intro-data-science-sd7t)

## 2. Design principles from the required resources

Microsoft's *Database Design Basics* recommends:
- Separating information into subject-based tables to reduce duplication.
- Defining keys and relationships so related information can be joined.
- Supporting accuracy and integrity.
- Designing for actual processing and reporting requirements.
- Refining a preliminary design with sample records and normalisation checks.

The key modelling implications for this exercise are:
- Store each fact once; do not repeat author profile information on every post.
- Keep one value per field (for example, one post ID and one creation timestamp per row).
- Use stable, unique primary keys and foreign keys for relationships.
- Avoid storing values that can be reliably calculated from other stored values unless there is a justified reporting/performance need.
- Use a junction table to represent a many-to-many relationship such as posts and mentioned accounts.

Datanamic's database-modelling tutorial complements this by emphasising the identification of entities, attributes, relationship cardinality, optionality, and associative entities for many-to-many relationships.

## 3. Understand the data grain before naming tables

A useful starting rule is:

> **One row in the core Posts table represents one social-media post, regardless of whether it is an original CommBank post, a customer reply, a quote post, or a post that mentions CommBank.**

A reply or quote post is itself a post authored by an account. Its connection to another post is stored as a relationship, not by copying the referenced post's text into a second table. A direct mention is a relationship between a post and the account it mentions; it should not create a duplicate copy of the post.

| Required item | What must be represented | Modelling consideration |
|---|---|---|
| CommBank posts | Original content authored by the CommBank account | A post linked to the CommBank account as its author |
| User replies | User-authored posts that reply to a CommBank post | A post plus a `replied_to` relationship to the parent post |
| User quote retweets | User-authored quote posts referring to a CommBank post | A post plus a `quoted` relationship to the referenced post |
| Direct mentions | Posts whose entities mention the CommBank account | A post-to-account association; a post may mention more than one account |

A post may satisfy more than one condition. For example, a reply may also directly mention CommBank. The design should preserve both facts without storing duplicate post records.

## 4. Initial subject / entity inventory

Treat these as **candidate subjects to validate**, not a final schema. The next steps will determine the exact fields and keys.

| Candidate subject | Why it may deserve a table | Example information to investigate |
|---|---|---|
| Accounts | CommBank and users can author posts or be mentioned by posts | Platform account ID, handle, display name, account category |
| Posts | Original posts, replies, and quote posts are all post records | Platform post ID, author, text, creation time, language, conversation context |
| Post relationships | Replies and quote posts refer to another post | Source post, target post, relationship type (reply / quote) |
| Post mentions | One post can mention multiple accounts; an account can be mentioned in many posts | Post ID, mentioned account ID, mention position if needed |
| Post metric snapshots (optional) | Engagement counts can change over time and support Task 03-style reporting | Post ID, collection timestamp, likes, replies, reposts, quotes, impressions |

The metrics table is an optional extension for analytics. The core Forage requirement can be met without it if the proposal's scope is limited to storing the required post and interaction relationships.

## 5. Database-design workflow

### Step 1 — Confirm purpose and scope

Write a one-paragraph purpose statement. List the four required interaction types and define the grain of a post record. Separate explicit requirements from optional extensions.

### Step 2 — Find and organise information

Build a field inventory from the Forage requirements and relevant X API concepts. Likely source concepts include:
- Post identity and content: `id`, `text`, `created_at`.
- Author identity: `author_id`.
- Conversation and reply context: `conversation_id`, `in_reply_to_user_id`, `referenced_tweets`.
- Mentions: `entities.mentions`.
- Language and media, where useful: `lang`, `attachments`.

Do not assume every field is always returned by an API call. Record only fields needed for the required use cases and label optional data.

### Step 3 — Divide information into tables

Group facts by subject (accounts, posts, post-to-post relationships, post-to-account mentions). Avoid one wide table with repeated author names or numbered mention columns such as `mention_1`, `mention_2`.

### Step 4 — Define columns and data types

For each field, specify:
- Variable name and plain-language description.
- Proposed data type and sensible length.
- Whether it may be NULL.
- Any uniqueness or check constraint.
- Whether the field is an API source field or a derived value.

Keep attributes atomic. For platform IDs, storing the identifier as a character value is a portable option when the API presents it as an opaque string; do not use a mutable handle as a primary key.

### Step 5 — Choose primary and foreign keys

Use stable platform IDs as candidate keys for source Posts and Accounts when they are unique and always present. If using surrogate internal keys, retain a UNIQUE constraint on the corresponding platform ID. A foreign key must reference a valid parent key.

### Step 6 — Define relationships and cardinality

For each relationship, state 1:1, 1:M, or M:N and identify optionality:
- One account can author many posts; each post has one author.
- A post can have zero or more reply/quote relationship records; each relationship connects one source post to one target post.
- Posts and mentioned accounts form a many-to-many relationship; resolve it with a junction table.
- A post can have multiple metric snapshots over time if the optional analytics extension is included.

### Step 7 — Check normalisation and data integrity

Use 1NF to avoid lists/repeating groups in a field, 2NF to check full dependence on a composite key, and 3NF to avoid non-key attributes depending on other non-key attributes. Define foreign-key integrity and uniqueness rules. Do not add cascading deletion rules without a clear retention requirement.

### Step 8 — Validate with examples

Test whether the design can represent:
1. An original CommBank post.
2. A user reply to that post.
3. A user quote post referring to the original post.
4. A user post that directly mentions CommBank.
5. A reply that also mentions CommBank.

The fifth case is important: the same post should be stored once, with both a post relationship and a mention association where applicable.

## 6. Private design specification template

Use this locally while drafting. The Forage submission itself is a text response; an ERD or SQL script is optional unless you decide it helps explain the proposal.

| Table | Field / variable | Data type | PK / FK / constraint | Description |
|---|---|---|---|---|
| To be determined | To be determined | To be determined | To be determined | What this field stores |

Relationship register:

| Parent table | Child table | Cardinality | Foreign key / junction | Business rule |
|---|---|---|---|---|
| To be determined | To be determined | 1:1 / 1:M / M:N | To be determined | To be determined |

## 7. Quality checklist

- [ ] The design includes CommBank-authored posts.
- [ ] The design represents user replies without duplicating the parent post.
- [ ] The design represents user quote retweets / quote posts.
- [ ] The design represents direct mentions as post-to-account relationships.
- [ ] Each table has a primary key.
- [ ] Foreign keys and relationship cardinalities are explicit.
- [ ] Many-to-many mentions are resolved with a junction table.
- [ ] Fields have clear names and descriptions.
- [ ] Repeating groups and avoidable duplication are removed.
- [ ] Optional analytics fields are distinguished from required fields.
- [ ] The design is checked against example records and normalisation principles.
- [ ] The final response includes tables, fields/descriptions, primary keys, and relationships.
- [ ] The proposal is submitted in Forage and the response status is confirmed.

## 8. References

- [Forage — Commonwealth Bank Introduction to Data Science, Task 04](https://www.theforage.com/virtual-experience/smwfytX3mcLboA9bf/commonwealth-bank/intro-data-science-sd7t/designing-a-database)
- [Microsoft — Database Design Basics](https://support.microsoft.com/en-us/office/database-design-basics-eb2159cf-1e30-401a-8084-bd4f9c9ca1f5)
- [Microsoft — Guide to Table Relationships](https://support.microsoft.com/en-us/office/guide-to-table-relationships-1eeb5a3b-9a0c-4c2d-8f8b-6d47d7a0b3e7)
- [Datanamic — Basics of Database Modeling](https://www.datanamic.com/support/lt-dez005-introduction-db-modeling.html)
