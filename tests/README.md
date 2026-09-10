# Tests

Automated test definitions for framework tools.

Record results in `Cursor/reports/TEST.md`.

## Test files (v0.1.2)

| File | Tests | Scope |
|---|---|---|
| `test_validate_framework.py` | 15 | Framework validator |
| `test_translation.py` | 13 | Translation pipeline and build |
| `test_upgrade_from_release.py` | 19 | Template Upgrade engine (offline-safe) |

**Total: 47 tests** (28 existing + 19 upgrade).

## Run

From repository root:

```text
pip install -r tools/core/requirements.txt
python tests/test_validate_framework.py
python tests/test_translation.py
python tests/test_upgrade_from_release.py
```

Upgrade tests are **offline-safe** by default (mocked GitHub API). Live GitHub Release
integration validation is a separate Maintainer step after Public Release (see `docs/RELEASE.md`).

## Fixtures

`tests/fixtures/` — shared fixtures for validator and translation tests.
