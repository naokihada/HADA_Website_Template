# Tests

Automated tests for framework tools (v0.1.0).

## Purpose

Verify the core validator, translation pipeline, and build behavior against this repository.

## Test suite

| Module | Tests | Focus |
|---|---|---|
| `test_validate_framework.py` | 15 | Validator CLI, rules, JSON output, I18N |
| `test_translation.py` | 13 | Term dictionary, mock translation, build site |

**Total:** 28 tests.

## Run

From the repository root:

```text
pip install -r tools/core/requirements.txt
python tests/test_validate_framework.py
python tests/test_translation.py
```

Expected: 28/28 PASS.

## Fixtures

- `tests/fixtures/term_dictionary.yaml` — dictionary for unit tests
- `tests/fixtures/content/jp/` — sample content for build/integration tests

## Validation documentation

Validator rules, severity, exit codes, known baseline, and output format:
`docs/VALIDATION.md`

Record manual test results in `Cursor/reports/TEST.md` when completing significant work.
