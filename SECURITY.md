# Security and privacy policy

This repository is a public learning project. It is not a production service and does not accept security-sensitive uploads.

## Sensitive material — do not publish

Never commit:
- Forage source datasets or simulation-only downloads.
- Customer-level or author-level records and post text.
- Names, emails, phone numbers, account identifiers, tokens, passwords, or API keys.
- Private working spreadsheets, exported row-level CSV files, or identifier mappings.
- Credentials, browser profiles, environment files, or local configuration containing secrets.

Use local-only folders ignored by Git for private source and working data. Review every staged file before committing.

## Reporting a concern

If a sensitive file or credential is accidentally committed:
1. Stop further pushes.
2. Revoke or rotate any exposed credential immediately.
3. Remove the exposed content from the repository history as appropriate; deleting it in a later commit alone does not erase history.
4. Review repository access and any downstream copies.
5. For a security or privacy concern, contact the repository owner privately rather than publishing sensitive details in an issue.

This policy does not replace Forage's terms, privacy requirements, or the relevant platform policies.
