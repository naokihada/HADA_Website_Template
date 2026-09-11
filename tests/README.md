# Tests

Automated test definitions for framework tools.

Record results in `Cursor/reports/TEST.md`.

## Test files (v0.1.3)

| File | Tests | Scope |
|---|---|---|
| `test_validate_framework.py` | 15 | Framework validator |
| `test_translation.py` | 13 | Translation pipeline and build |
| `test_upgrade_from_release.py` | 20 | Template Upgrade engine (offline-safe) |
| `test_pwa_timer.py` | 5 | PWA Timer install contract and manifest icons |

**Total: 53 tests** (28 framework/translation + 20 upgrade + 5 PWA Timer).

## Run

From repository root:

```text
pip install -r tools/core/requirements.txt
python tests/test_validate_framework.py
python tests/test_translation.py
python tests/test_upgrade_from_release.py
python tests/test_pwa_timer.py
```

Upgrade tests are **offline-safe** by default (mocked GitHub API). Live GitHub Release
integration validation is a separate Maintainer step after Public Release (see `docs/RELEASE.md`).

## Manual PWA install checks

Use an HTTPS deployment and confirm:

1. A supported browser fires `beforeinstallprompt`; **Install this timer** appears and opens the browser install prompt.
2. After installation, `appinstalled` or standalone display hides the button.
3. On iPhone/iPad, the fallback says Safari Share → Add to Home Screen; other browsers receive a browser-menu hint.
4. Install the app, go offline, reopen it, and confirm the timer page and countdown still work.

## Fixtures

`tests/fixtures/` — shared fixtures for validator and translation tests.
