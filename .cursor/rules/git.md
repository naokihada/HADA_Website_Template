# Git rules (Cursor adapter)

Canonical source: `AGENTS.md` Git Rules.

## Commit

- Intentional changes only; one logical unit per commit when practical
- Verify diff before commit — no secrets, no unrelated files
- Do not commit `config/local.yaml`, `.env`, or cache contents

## Push

- Do not force-push `main` without explicit instruction
- Do not push if suspicious deletions or unexpected large diffs appear

## Branches

- Framework development: `HADA_Website_Template_Dev`
- This repository: public template for site projects (clone/fork and commit locally)

## Stop conditions

If unrelated changes, user-owned unexpected edits, or secrets appear — stop commit/push
and report NEEDS_REVIEW.
