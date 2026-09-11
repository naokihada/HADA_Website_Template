# Template Upgrade Guide

Upgrade an existing site project from a **GitHub stable Release** of
[HADA_Website_Template](https://github.com/naokihada/HADA_Website_Template).

This guide is the **Public Upgrade Contract**. It is not the Master Implementation Specification.

---

## 1. Prerequisites

- Python 3.10+
- PyYAML (`pip install -r tools/core/requirements.txt`)
- Network access to `api.github.com` and GitHub
- Backup or Git working tree review before upgrading

---

## 2. Template version vs project version

| Field | Location | Meaning |
|---|---|---|
| Template version | `config/template.manifest.yaml` → `template.version` | HADA Website Template Release |
| Project version | `config/project.yaml` → `project.version` | Your site project metadata |

Do not use `project.version` as the template version.

---

## 3. Check current template version

1. If `config/template.manifest.yaml` exists → read `template.version` (**detected**)
2. Else legacy v0.1.1 sites may match release fingerprint (mechanical identification)
3. If unknown → upgrade stops with **BLOCKED** unless you pass `--assume-version` explicitly

---

## 4. Target release

| Mode | Behavior |
|---|---|
| Default | Latest **non-prerelease** GitHub Release |
| Explicit | `--version v0.1.3` |

Not used as upgrade source: `main`, branch HEAD, unreleased commits, prereleases (as stable default).

---

## 5. Run upgrade

From your site project root:

```text
pip install -r tools/core/requirements.txt
python tools/core/upgrade_from_release.py --root .
```

Explicit version:

```text
python tools/core/upgrade_from_release.py --root . --version v0.1.3
```

Legacy site without manifest (v0.1.1):

```text
python tools/core/upgrade_from_release.py --root . --assume-version v0.1.1
```

Dry run (plan only):

```text
python tools/core/upgrade_from_release.py --root . --dry-run
```

---

## 6. What is preserved (user-owned)

Never automatically overwritten:

- `content/` (Content Master)
- `config/term_dictionary.yaml`, `config/site.yaml`, `config/local.yaml`
- `site/assets/` (except template scaffold), `site/en/`, `site/jp/` HTML
- User-filled `Cursor/reports/`, `Cursor/tasks/`
- `references/registry/` (non-example), `references/cache/`

See `config/template.manifest.yaml` for the authoritative list.

---

## 7. Automatic updates

The engine compares **Previous Template**, **Current Site**, and **New Template**:

| Situation | Result |
|---|---|
| Template-only change | Automatic update |
| User-only change | Preserved |
| Both changed | **REVIEW_REQUIRED** |
| Unchanged | Skipped |

`config/project.yaml` uses safe key-level merge (preserves `project.name`, `project.mode`).

Generated HTML/content may require review if manually edited.

---

## 8. Exit status

| Code | Status | Meaning |
|---|---|---|
| 0 | SUCCESS | Safe changes applied |
| 1 | REVIEW_REQUIRED | Conflicts need human review |
| 2 | BLOCKED | Preflight failed; no safe migration |
| 3 | FAILED | Internal error |

---

## 9. Temporary clone

The tool fetches the Release to a temp directory (e.g. `%TEMP%\hada-template-upgrade-<uuid>\`).
**The temp clone is retained** for inspection. The report includes the absolute path.

---

## 10. Git safety

The upgrade tool does **not** create branches, commit, push, tag, merge, or reset your site repository.
Review `git diff` after upgrade before committing.

---

## 11. Post-upgrade validation

```text
python tests/test_validate_framework.py
python tests/test_translation.py
python tools/core/validate_framework.py --root .
python tools/core/build_site.py --root .
```

See `docs/VALIDATION.md` for validator rules and baseline.

---

## 12. v0.1.1 → v0.1.2

First automated migration path. Adds:

- `config/template.manifest.yaml`
- `tools/core/upgrade_from_release.py`
- `docs/UPGRADE.md`, `CHANGELOG.md`

v0.1.1 sites without manifest: use fingerprint detection or `--assume-version v0.1.1`.

---

## 13. v0.1.2 → v0.1.3

Adds the client-only PWA Timer Demo at `site/timer.html`, including Beep audio,
Service Worker notifications, and an offline app-shell cache. Notification delivery
after complete PWA termination is best-effort and not guaranteed.

## 14. Troubleshooting

| Issue | Action |
|---|---|
| BLOCKED: network | Check connectivity to GitHub |
| BLOCKED: unknown version | Add manifest or use `--assume-version` |
| REVIEW_REQUIRED | Inspect listed paths; merge manually |
| Large unexpected diff | Stop; restore from backup if needed |

---

## 15. Not included

- Web deployment / FTP / production publish
- Automatic Git commit or push
- Using `main` branch as upgrade source
