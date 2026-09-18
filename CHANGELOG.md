# Changelog

All notable **public** changes to HADA Website Template releases.

## [0.2.1] — 2026-09-17

### Changed

- Added the common machine-readable manifest schema
- Added `manifest_version`, `template.release`, compatibility metadata, repository metadata, and `files.template_base`
- Added manifest/Base consistency validation and regression tests
- Preserved `schema_version` and `data_format_version` at `0.1`
- Added the `0.2.0 → 0.2.1` migration path

## [0.2.0] — 2026-09-17

### Changed

- Replaced the agent-operation workspace convention of `.cursor/` and `Cursor/` with the agent-neutral `AI/` workspace
- Updated validator and repository structure expectations to require `AI/` and keep it outside the Web Publish boundary
- Added the documented legacy workspace migration procedure for existing consumer projects
- Added required upgrade checkpoints: preserve SPEC/manifest/affected-file history, classify legacy contents, migrate useful history to `AI/history/`, then run ReSPEC, tests, validator, build, security, and diff review
- Undefined ownership, migration meaning, or legacy content is now explicitly `REVIEW_REQUIRED`; agents must not guess or delete blindly
- Added migration path: **v0.1.3 → v0.2.0**

### Upgrade Notes

- `config/project.yaml` → `project.version` remains site-project metadata and is not changed by this Template version upgrade
- Existing `.cursor/` and `Cursor/` directories belong to the consumer project migration process; they are not part of the v0.2.0 Template artifact
- Preserve useful historical evidence in `AI/history/` before removing legacy directories

## [0.1.3] — 2026-09-11

### Added

- Client-only PWA Timer Demo at `site/timer.html`
- Digital `MM:SS` timer from `00:00` through `99:59`, default `03:00`
- Web Audio API completion beep
- Best-effort local notifications through a Service Worker
- Offline app-shell cache for the timer demo
- Upgrade migration path: **v0.1.2 → v0.1.3**

### Notes

- No server, database, cron, or external API is required.
- Notification delivery while the PWA is fully terminated is best-effort and not guaranteed.

## [0.1.2] — 2026-09-09

### Added

- **Template Upgrade** from GitHub stable Release (`tools/core/upgrade_from_release.py`)
- **`config/template.manifest.yaml`** — template version and ownership contract
- **`docs/UPGRADE.md`** — public upgrade guide
- Three-way migration (Previous Template / Current Site / New Template)
- Network preflight and retained temporary clone for inspection
- First migration path: **v0.1.1 → v0.1.2**

### Safety

- User-owned content preserved (`content/`, term dictionary, site config, assets)
- Conflicts produce **REVIEW_REQUIRED** instead of blind overwrite
- Upgrade tool does not modify site Git history (no branch/commit/push)

### Documentation

- Public validation unchanged (`docs/VALIDATION.md`)
- Agent-independent framework in `AGENTS.md`

## [0.1.1] — 2026-09-09

### Added

- Public validation guide (`docs/VALIDATION.md`)
- Agent-neutral `AI/` workspace and handoff boundary
- Clarified internal AI workspace versus Web Publish boundary

### Removed

- Development-only `tools/core/SPEC.md` from public template

## [0.1.0] — Initial public template

- Core validator, build pipeline, minimal i18n (jp/en)
- Foundation directory structure and tests
