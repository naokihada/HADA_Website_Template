# HADA Website Operations Framework (v0.1.0)

Agent-operable framework for creating, recovering, migrating, maintaining, testing, and
releasing websites. This is the **public release template** — start here for a new project.

AI Agent対応Webサイト運用フレームワーク v0.1.0 公開テンプレート。

---

## What this is

A **Web Site Operations Framework** — not a single website. It separates Content Master,
processing tools, plugins (capabilities), and the Publication Root so agents and humans
can operate sites deterministically with audit trails.

## v0.1.0 scope

- Foundation structure and agent contract (`AGENTS.md`)
- Core Validator (`tools/core/validate_framework.py`)
- Minimal i18n (jp / en)
- Japanese Content Master (`content/jp/`)
- jp → en translation with mock provider
- Single site-wide term dictionary (`config/term_dictionary.yaml`)
- Markdown → HTML build (`tools/core/build_site.py`)
- Basic tests

Not included in v0.1.0: real AI translation API, WordPress, RSS, PWA, FTP, n8n, cron,
plugins, full site import, or production automation.

## Getting started

1. Copy `config/term_dictionary.example.yaml` to `config/term_dictionary.yaml` and edit entries.
2. Add Markdown content under `content/jp/` (Japanese master).
3. Install dependencies and validate:

```text
pip install -r tools/core/requirements.txt
python tools/core/validate_framework.py --root .
python tools/core/build_site.py --root .
```

4. Generated HTML appears under `site/`.

## Repository roles

| Repo | Role |
|---|---|
| `HADA_Website_Template` | This repo — clean v0.1.0 template |
| `HADA_Website_Template_Dev` | Development workspace |
| `HADA_Website_Template_Sample` | Verified example with fictional sample site |

For a working example, see `HADA_Website_Template_Sample`.

## Safety

- Secrets never committed (`config/local.yaml`, `.env` — see `.gitignore`)
- Only `site/` (Publication Root) is deployed by default
- Read `AGENTS.md` for full security, scope, and deployment rules

See `AGENTS.md` and `Cursor/tasks/CURRENT.md` for agent operations.

## Disclaimer

See [DISCLAIMER.md](DISCLAIMER.md).
