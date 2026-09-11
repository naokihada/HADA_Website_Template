# Changelog

All notable **public** changes to HADA Website Template releases.

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
- Public-safe `Cursor/reports/` scaffold
- Clarified Cursor/ in Git vs Web Publish boundary

### Removed

- Development-only `tools/core/SPEC.md` from public template

## [0.1.0] — Initial public template

- Core validator, build pipeline, minimal i18n (jp/en)
- Foundation directory structure and tests
