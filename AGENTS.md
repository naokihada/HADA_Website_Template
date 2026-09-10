# AGENTS.md — HADA Website Operations Framework

**Agent-independent operation framework** for this repository. Applies to any AI coding
agent (Cursor, Claude Code, Codex, CLI, CI, or other) unless a task explicitly overrides.

| Document | Role |
|---|---|
| **`AGENTS.md`** (this file) | How agents operate safely — workflow, scope, Git, approval, reporting |
| **`SPEC.md`** | What the template is — implementation, architecture, invariants, lifecycles (Dev only) |
| **`Cursor/`** | Temporary project operation — tasks, reports, logs, state (not Content Master) |
| **`.cursor/rules/`** | Cursor adapter only — must not contradict `AGENTS.md` or redefine `SPEC.md` |
| **`docs/UPGRADE.md`** | Public Upgrade Contract (Public Release only; not in Dev) |
| **`docs/RELEASE.md`** | Dev Maintainer runbook — Dev → Public extraction (Dev only) |

**Future:** `AI/` may supersede `Cursor/` as the agent-neutral operation workspace name.
Until then, `Cursor/` is the operation workspace path in this repository.

Do **not** copy agent-independent rules into `.cursor/rules/` or agent-specific configs.
Do **not** copy template implementation detail from `SPEC.md` into this file.

---

## Project Identity

| Item | Value |
|---|---|
| Name | HADA Website Operations Framework |
| Development repo | `HADA_Website_Template_Dev` |
| Release repo | `HADA_Website_Template` |
| Sample repo | `HADA_Website_Template_Sample` |
| Purpose | Agent-operable website creation, recovery, migration, maintenance, testing, and release |

Relationship: **Development → Release → Sample**. Implement and validate in Dev; extract
stable engine to Release; demonstrate usage in Sample.

---

## Template architecture (reference only)

Template structure, content pipeline, plugins, i18n, validator, build, configuration schemas,
ownership, and lifecycles: **`SPEC.md`**. Do not duplicate here.

Agent-relevant deploy boundary only:

- **Publication Root** (`site/` by default) — only this tree is Web-published.
- Never deploy `Cursor/`, `content/`, `tools/`, `references/`, `environments/`, `docs/`,
  or `config/` to a production web document root unless explicitly instructed for a
  non-production purpose.

### Deterministic vs semantic work

- **Deterministic** — scripts, validators, builds, tests in `tools/`; prefer code over prose.
- **Semantic** — analysis, planning, drafting; allowed with review; must not bypass safety rules.
- **Generated artifacts** must be rebuildable from documented inputs (`SPEC.md` §4, §9).

---

## Directory Responsibilities

| Path | Role |
|---|---|
| `SPEC.md` | Template implementation specification (Dev only; not in Public Release) |
| `AGENTS.md` | Agent-independent operation framework (this file) |
| `.cursor/rules/` | Cursor adapter — not canonical |
| `Cursor/` | Operation workspace (tasks, reports, logs, state); in Public Git scaffold; excluded from Web Publish (`SPEC.md` §1.6) |
| `config/template.manifest.yaml` | Template version and upgrade ownership (Public from v0.1.2; `SPEC.md` §1.7) |
| `content/` | Content Master by locale (`en/`, `jp/`) |
| `site/` | Publication Root — deployable web output |
| `references/` | External reference registry, cache, reports |
| `environments/` | Environment definitions (LOCAL / STAGING / PRODUCTION) |
| `tools/core/` | Deterministic framework tools |
| `tools/plugins/` | Installed capability plugins (spec + manifest required) |
| `tests/` | Automated and manual test definitions |
| `docs/` | Human-facing documentation (`docs/RELEASE.md` — Maintainer runbook, Dev only) |
| `config/` | Project and site configuration (examples committed; secrets excluded) |

---

## Master Specification boundary

Root `SPEC.md` is the **only Master Implementation Specification** (Dev only). It defines
template architecture, tools, validator, build, configuration, upgrade lifecycles, and
implementation invariants. It does **not** replace this agent operation framework.

**Do not:**

- Create `SPEC_LITE.md`, `tools/core/SPEC.md`, or other master specification files
- Treat `AGENTS.md`, README, or `Cursor/` reports as implementation specification
- Duplicate `SPEC.md` implementation detail in `AGENTS.md` — reference the section instead
- Duplicate agent safety/workflow rules in `SPEC.md` — reference `AGENTS.md` instead

**Development workflow (behavior changes):**

```text
Update SPEC.md → Implement per SPEC.md → Test → Validation → Scope/Security check → Diff review
```

---

## Agent authority

Unless the human **explicitly instructs** otherwise, agents must **not**:

| Forbidden | Includes |
|---|---|
| Git write on target | commit, push, tag, merge, rebase, reset, force-push |
| Git branch ops | create branch, switch branch (including during Template Upgrade) |
| Public Release Git | commit/push/tag/GitHub Release on `HADA_Website_Template` |
| Production | Web Publish, FTP, DNS, deployment automation not in `SPEC.md` |
| Scope expansion | Unrequested features, refactors, dependency changes, README regeneration |
| Destructive ops | discard pre-existing changes, delete user content, auto-delete temp clones |

Agents **may** (read-only unless task authorizes writes):

- Run validators, tests, build (`tools/core/`)
- Read `git status`, `git diff`, `git log`, remotes
- Modify files **only** within declared task scope
- Run Template Upgrade tool when tasked (tool must not mutate Git — `SPEC.md` §1.7)

**Commit / push / release:** human-controlled unless the user explicitly requests the agent
to commit in that task.

---

## Pre-existing changes

Before editing, inspect `git status` and `git diff`. Changes that existed before the current
task are **pre-existing** — not authored by the current agent step.

- Do **not** assume pre-existing changes are yours
- Do **not** reset, restore, checkout, or discard them unless explicitly instructed
- Report pre-existing changes separately from task-induced diff
- If pre-existing changes conflict with task scope → **NEEDS_REVIEW** / **BLOCKED**

---

## Stop conditions

Stop and report with a clear status when:

| Status | When |
|---|---|
| **BLOCKED** | Secrets detected; network/release failure; invalid scope; cannot preserve user data; security risk; missing explicit approval for gated operation |
| **NEEDS_REVIEW** / **REVIEW_REQUIRED** | Ambiguous merge; large unexpected diff; pre-existing change conflict; inference required for requirements |
| **FAILED** | Tool/internal error after preflight |

Do not continue with partial unsafe state. Do not invent requirements to unblock.

Template Upgrade engine exit codes (`SUCCESS`, `REVIEW_REQUIRED`, `BLOCKED`, `FAILED`):
`SPEC.md` §1.7.5, `docs/UPGRADE_ARCHITECTURE.md`.

---

## Permanent knowledge vs temporary task information

| Permanent (repository) | Temporary (operation workspace) |
|---|---|
| `SPEC.md`, `AGENTS.md`, `tools/`, `tests/`, `config/` examples | `Cursor/tasks/`, `Cursor/reports/`, `Cursor/logs/`, `Cursor/state/` |
| Public docs (`docs/VALIDATION.md`, `docs/UPGRADE.md` on Release) | Session analysis, chat handoff notes |
| Implementation code | Upgrade temp clone paths (retained; `SPEC.md` §1.7) |

Do not store secrets or one-off task conclusions in permanent files unless the task requires it.
Do not treat temporary reports as implementation specification.

---

## Core Principles

1. Agent independent — no Cursor-only assumptions in core design.
2. Small, intentional diff — minimal scope per task.
3. Scope control — stay within the requested phase and files.
4. Human approval for production — never release or deploy to production implicitly.
5. No guessing — report unknowns; do not invent requirements or paths.
6. Deterministic processing should be code — scripts, validators, linters in `tools/`.
7. Semantic processing may use AI — analysis, planning, content drafting with review.
8. Generated artifacts should be rebuildable — document inputs and regeneration steps.
9. Secrets never enter Git — use local-only config and environment-specific secrets.
10. Production must never be modified implicitly — explicit approval and target required.

### Existing Library Rule

When a standard, proven library can fulfill a requirement safely, do not reimplement the
same capability from scratch.

Applies especially to: YAML, JSON, HTTP, XML, archives, Markdown, HTML, and similar
general-purpose parsing or protocol handling.

For Python tools:

1. Check libraries available in the current environment.
2. Check dependencies already declared in the project.
3. Use an appropriate existing library when suitable.
4. If a new dependency is needed, declare it before implementation (e.g. `tools/core/requirements.txt`).
5. Replacing a library with a custom implementation requires a reported reason and approval.

**Do not implement a custom substitute merely because a library is not yet installed.**
Install and declare the dependency instead.

Standard-library-only solutions remain acceptable for genuinely simple processing. When
unsure, ask before implementing a custom parser or client.

---

## Project Modes

| Mode | Use when |
|---|---|
| `NEW` | Creating a new site from framework templates |
| `RECOVERY` | Restoring or salvaging an existing site |
| `MIGRATION` | Moving content or stack between systems |
| `MAINTENANCE` | Ongoing updates, fixes, and verification |

State the active mode at the start of significant work.

---

## Core Actions

| Action | Meaning |
|---|---|
| `CREATE` | Produce new artifacts or site structure |
| `IMPORT` | Bring external content or reference material into the framework |
| `UPDATE` | Change existing source or configuration |
| `SYNC` | Align derived artifacts with Content Master or external sources |
| `VERIFY` | Check correctness without mutating production |
| `TEST` | Run defined tests and record results |
| `RELEASE` | Prepare or publish a release candidate to Release repo |
| `ARCHIVE` | Freeze or preserve a site snapshot for long-term storage |

---

## Operational Roles

Three roles — do not conflate Maintainer extraction with Public template usage or Web Publish.

| Role | Repository | Lifecycle |
|---|---|---|
| **A — Dev Maintainer** | `HADA_Website_Template_Dev` → `HADA_Website_Template` | Repository Release (Git artifact) |
| **B — Public Template User** | `HADA_Website_Template` (clone/fork) | Develop → Test → Validate → Review → Commit |
| **C — Web Publish Operator** | Project using framework (any) | Web Publish — `site/` only |
| **D — Template Upgrade Operator** | Existing site project (any) | Template Upgrade — GitHub stable Release (`SPEC.md` §1.7) |

**Public `AGENTS.md`** serves Roles B, C, D (and general agent contract). Dev→Public
extraction (Role A) belongs in **`docs/RELEASE.md`** (Dev only).

### Role A — Dev Maintainer (Repository Release)

Prepare Public Release extraction from Dev. Invariants: `SPEC.md` §1.5–§1.6. Procedure:
`docs/RELEASE.md`.

```text
Task → Validate → Analyze → Plan → Approval? → Implement → Test
  → Scope Check → Security Check → Diff Review → Release Candidate
  → Human Approval → Human Release (commit / push / tag on Public repository)
```

Release Candidate checks (minimum):

- Implementation validation — `python tools/core/validate_framework.py`
- Tests — `python tests/test_validate_framework.py`, `python tests/test_translation.py`, and `python tests/test_upgrade_from_release.py` (47 total at v0.1.2)
- Build — `python tools/core/build_site.py --root .`
- Scope check — diff limited to intended Public artifacts
- Public safety — no `SPEC.md`, Dev reports, secrets, Dev sample content
- Documentation consistency — Public docs align with extracted behavior

**AI agents must not** commit, push, tag, or create GitHub Releases on
`HADA_Website_Template` without explicit human instruction. Record each release in
`Cursor/reports/RELEASE.md` (template); follow `docs/RELEASE.md` for the runbook.

### Role B — Public Template User

Uses the Public template as a starting point for a site project. Normal workflow:

```text
Develop → Test → Validate → Review → Commit
```

This is **not** Dev→Public extraction. Do not apply Maintainer-only steps (Public safety
review for extraction, Dev internal artifact removal) unless the user is performing a
Maintainer task in Dev.

### Role C — Web Publish Operator

Publishes static web content from **Publication Root** (`site/`). Separate from
Repository Release (`SPEC.md` §1.6).

```text
Build → Validate → Review → Human Approval → Deploy Publication Root → Post-publish verification
```

v0.1.0: deployment automation is **not implemented** (`deployment.method: null`). Agents
must not invent FTP, CI/CD, or deploy commands. Human performs actual deployment after approval.

### Role D — Template Upgrade Operator

Upgrade an existing site from a **GitHub stable Release** of `HADA_Website_Template`.
Specification: **`SPEC.md` §1.7**; implementation detail: **`docs/UPGRADE_ARCHITECTURE.md`**;
Public runbook: **`docs/UPGRADE.md`** (Public Release only).

```text
Validate site → Network test → Resolve Release → Detect version → Temp clone
  → Migration plan → Apply safe updates → Preserve user-owned → Build → Test → Validate
  → Diff review → Report (temp clone retained)
```

- Default target: latest **non-prerelease** GitHub Release — **not** `main`
- No Git branch/commit/push on the site project during upgrade
- Do not auto-delete temporary clone
- `--assume-version` only when explicitly instructed (not default)

---

## Task Lifecycle

Standard workflow (all roles; adapt gates to task):

```text
Task → Validate → Analyze → Plan → Approval?
  → Implement → Test → Scope Check → Security Check → Diff Review → Report
  → Commit / Release (human or explicit instruction only)
```

Track active work in `Cursor/tasks/CURRENT.md`. Backlog in `BACKLOG.md`. Completed
items move through `TODO.md` or changelog as appropriate.

Before `Implement`:

- Confirm scope, mode, and target paths.
- Identify Content Master vs generated files affected.
- For behavior changes: confirm or update `SPEC.md` first (see Master Specification).

Before `Release` (Repository Release — Role A):

- Follow `docs/RELEASE.md` pre-release validation and extraction checklist.
- All required tests and checks documented in `Cursor/reports/` (TEST.md, SECURITY.md).
- Public safety review complete per `SPEC.md` §1.5.
- Human approval obtained before any commit/push/tag on `HADA_Website_Template`.

Before Web Publish (Role C):

- Build and validate Publication Root output where tools apply.
- Human approval for STAGING / PRODUCTION (`environments/`).
- Deploy **only** `site/` (or configured Publication Root) — never full repository.

Before Template Upgrade (Role D):

- Confirm site root and target Release version (or latest stable).
- Network preflight; stop on failure.
- Follow `SPEC.md` §1.7 and Public `docs/UPGRADE.md` when on Public template.
- Report `[TEMP]` location; do not delete temp clone.

---

## Scope Rules

- Do not expand scope beyond the current phase or explicit user request.
- Do not refactor unrelated files.
- Do not read or modify external site repositories unless the task requires it.
- Do not traverse large legacy directories (e.g. gallery album trees) without explicit need.
- Prefer updating Content Master over editing generated files in `site/` directly.
- Line-ending-only or whitespace-only diffs are unacceptable.

---

## Git Rules

- Commit only intentional, reviewed changes.
- One logical change per commit when possible.
- Never commit secrets, credentials, private local paths, or production tokens.
- Never force-push to `main` without explicit human instruction.
- Do not amend pushed commits unless explicitly requested.
- Foundation and feature work stay on Dev until extracted to Release.
- **Do not** commit, push, tag, or GitHub Release on `HADA_Website_Template` unless the
  human explicitly instructs — Repository Release is human-controlled (`docs/RELEASE.md`).

---

## Plugin Rules

Plugin lifecycle, manifest schema, and capability boundaries: **`SPEC.md` §2.6–§2.7**.

Agent install procedure:

```text
Preflight → Backup → Apply → Verify → Commit (if explicitly instructed)
```

- Do not create plugin directories without a written specification and `manifest.yaml`.
- Plugin logs: `Cursor/logs/`, `Cursor/state/`, `Cursor/reports/` — not inside plugin dir.
- **Disable** and **Uninstall** are different operations.

---

## Content, references, and generated files

Content Master, Publication Root, generated output, and file ownership: **`SPEC.md` §4**.

Agent rules:

- Prefer editing Content Master (`content/`) over generated `site/` HTML unless task targets output only.
- Do not treat generated HTML as authoritative source.
- `references/` — readonly by default; no private paths or secrets in committed registry files.
- Rebuild generated output via `tools/core/build_site.py` rather than manual drift repair when appropriate.

---

## Testing Rules

- Tests live under `tests/` and may be invoked from `tools/core/`.
- Record results in `Cursor/reports/TEST.md` or linked report files.
- `VERIFY` checks may be read-only; `TEST` may mutate fixtures in non-production paths only.
- Security checks documented in `Cursor/reports/SECURITY.md` before release candidates.

---

## Security Rules

Never commit:

- Passwords, API keys, tokens, or connection strings
- Private local filesystem paths tied to individuals (use examples)
- Production secrets or `.env` with real values
- Private logs containing sensitive data

Treat legacy imported content as untrusted until scanned. Do not execute untrusted PHP,
shell, or binary from reference or recovery imports without explicit review.

PWA and static-cache plugins must not cache authentication, private, or admin content
without explicit configuration and human approval.

---

## Deployment Rules

Web Publish (Role C) — distinct from Repository Release (Role A). See `SPEC.md` §1.6.

- Deployment target is **Publication Root only** (`site/` by default).
- Never deploy `Cursor/`, `content/`, `tools/`, `references/`, `environments/`, `docs/`,
  or `config/` to production web document root.
- FTP and other deployment adapters (Future) deploy publication content, not the full repository.
- STAGING and PRODUCTION require named environment config and human approval.
- LOCAL development uses `environments/` definitions; do not assume production parity.
- v0.1.0: no deployment automation — human-operated deploy only after approval.

---

## Human Approval

Required before:

- Production Web Publish or FTP upload (Role C)
- Repository Release extraction to `HADA_Website_Template` (Role A) — human commit/push/tag
- Destructive operations on Content Master or external references
- Enabling plugins that mutate live external systems (WordPress.com, cron, n8n)

Record Repository Release approval in `Cursor/reports/RELEASE.md` (template). Maintainer
procedure: `docs/RELEASE.md`.

---

## Logging

- Operational logs: `Cursor/logs/`
- Persistent state: `Cursor/state/`
- Human-readable reports: `Cursor/reports/`
- Inventories and audits: `Cursor/inventory/`
- Change history: `Cursor/changelog/`

Use structured, factual entries. Separate **fact**, **unverified**, and **inference**.

---

## Error Handling

On failure:

1. Stop the current automated step.
2. Record the error in `Cursor/logs/` or the active report.
3. Do not commit partial or broken foundation changes.
4. Do not push if diff contains secrets, unexpected deletions, or unrelated changes.
5. Report `BLOCKED` or `NEEDS_REVIEW` with concrete next steps.

---

## Plugin Installation / Uninstallation

**Install:** Preflight (manifest, dependencies, scope) → Backup (state + affected files) →
Apply → Verify (tests + diff review) → Commit.

**Disable:** Stop scheduling and external side effects; leave files installed.

**Uninstall:** Disable first → remove plugin artifacts → Verify → Commit.

Failed installs enter `FAILED` state; do not leave half-applied production config.

---

## Agent Communication

Reports must distinguish:

| Label | Meaning |
|---|---|
| Fact | Directly observed or verified |
| Unverified | Not yet confirmed |
| Inference | Reasoned but not confirmed — use sparingly |

Prefer machine-readable structure in reports where practical (YAML front matter, tables,
checklists) for future automation.

Do not paste secrets or full contents of large files in reports.

---

## Completion Criteria

A task is complete when:

- Requested scope is implemented within declared mode and phase.
- Tests and checks specified for the task have passed or are explicitly deferred with reason.
- Diff is limited to intended files; no secrets present.
- `Cursor/tasks/CURRENT.md` and relevant reports are updated.
- Human approval obtained if required.
- Git status is clean or commit message accurately describes remaining intentional state.

Foundation phase complete when all Foundation paths exist, `AGENTS.md` and README describe
the framework, and configuration examples are present without live secrets.
