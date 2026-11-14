# Contributing and working standards

This is an individual learning repository. These standards keep the work reproducible, reviewable, and safe to publish.

## Before changing files

1. Pull or fetch the latest `main` before starting.
2. Keep Forage source workbooks and all row-level/customer-level working data outside Git.
3. Check `.gitignore` and inspect staged changes before committing.
4. Update the task guide and learning log when a result has been validated.
5. Distinguish completed analysis from a submitted/accepted Forage response.

## Evidence standard

- Record the source field names, filters, formulas, PivotTable layout, and validation totals.
- Reconcile row counts and aggregate totals before writing conclusions.
- Label synthetic/simulated data clearly.
- Distinguish descriptive findings from causal claims.
- Document missing data, sample-size limitations, and assumptions.
- Never fabricate results, task completion, client work, or platform acceptance.

## Privacy and licensing

- Do not commit Forage-provided source datasets or simulation-only resources.
- Do not publish personal data, post text, customer-level extracts, identifiers, or linkage keys.
- Publish only aggregate results and methodology when permitted.
- Do not copy proprietary or confidential material into public documentation.

## Documentation conventions

- Use descriptive file names and stable relative links.
- Keep the README concise enough to navigate; put detailed methods in task-specific guides.
- Use Markdown tables for compact results and Mermaid for maintainable diagrams.
- SVG assets must be original, accessible, have a `viewBox`, and include a `title` and `desc` where practical.
- Keep badges factual. A task is not complete merely because its analysis is complete; distinguish analysis, local deliverable, and Forage submission states.

## Commit messages

Use short imperative messages with a clear scope, for example:
- `docs: record validated Task 03 support counts`
- `workbooks: add generic analysis template`
- `fix: correct Task 03 workflow navigation`

## Local verification

Before pushing:
```powershell
git status --short
git diff --check
git diff --cached --check
```

Review staged file names individually. Never use `git add .` without checking for private files first.
