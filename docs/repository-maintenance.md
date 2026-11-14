# Repository maintenance

## Purpose

Keep this repository useful as a learning record without turning it into a dump of raw data or unverified claims.

## Before every commit

- [ ] Fetch the latest remote branch.
- [ ] Confirm the current task status from the actual workbook and Forage page.
- [ ] Check that no source workbook, raw export, row-level extract, or secret is staged.
- [ ] Run `git diff --check`.
- [ ] Review the full staged diff and filenames.
- [ ] Update the task guide and learning log with validated evidence.
- [ ] Keep README summary and project-positioning language consistent.

## Task status vocabulary

| Status | Meaning |
|---|---|
| Planned | Not started |
| In progress | Work is underway |
| Analysis validated | Calculations/review reconciled, but the Forage response may still be pending |
| Prepared locally | Deliverable exists locally; upload/acceptance not verified |
| Submitted | User confirms submission in Forage |
| Complete | Required task work and submission are confirmed |

## Suggested release structure

Use Git tags only for meaningful, reviewable milestones, for example after a task is submitted and its documentation is final. Do not tag merely because a template or partial analysis has been added.

## Git sync recovery

If a push is rejected as non-fast-forward:
1. Preserve the local commits.
2. Run `git fetch origin`.
3. Inspect `git status -sb` and `git log --oneline --graph --decorate --left-right HEAD...origin/main`.
4. Rebase only after confirming the working tree is clean and understanding the remote-only commits.
5. Resolve conflicts deliberately; stop if binary workbooks conflict.
6. Push normally. Never force-push to resolve a routine sync issue.

If Git HTTPS authentication is involved, prefer the configured GitHub CLI credential helper. Do not paste tokens into chat or shell history.
