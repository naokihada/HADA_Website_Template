# HADA Website Operations Framework (v0.1.3)

Agent-operable framework for creating, recovering, migrating, maintaining, testing, and
releasing websites. This is the **public release template** — start here for a new project.

日本語の情報は、このページの下にあります。

**Working example:** [HADA_Website_Template_Sample](https://github.com/naokihada/HADA_Website_Template_Sample) — verified fictional sample site built with this template.

---

## What this is

A **Web Site Operations Framework** — not a single website. It separates Content Master,
processing tools, plugins (capabilities), and the Publication Root so agents and humans
can operate sites deterministically with audit trails.

## v0.1.3 scope

- Foundation structure and agent contract (`AGENTS.md`)
- Core Validator (`tools/core/validate_framework.py`)
- Minimal i18n (jp / en)
- Japanese Content Master (`content/jp/`)
- jp → en translation with mock provider
- Single site-wide term dictionary (`config/term_dictionary.yaml`)
- Markdown → HTML build (`tools/core/build_site.py`)
- **Template Upgrade** from GitHub stable Release (`tools/core/upgrade_from_release.py`)
- Template version contract (`config/template.manifest.yaml`)
- Tests (47 total — see `tests/README.md`)

Included: a client-only PWA Timer Demo with `MM:SS` display, Beep audio, and best-effort local notifications.

Not included: real AI translation API, WordPress, RSS, broader PWA capabilities, FTP, n8n, cron,
plugins, full site import, or production deployment automation.

Upgrade an existing project: see `docs/UPGRADE.md`.

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
| `HADA_Website_Template` | This repo — v0.1.3 public template |
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

---

# 日本語

## 概要

Webサイトの作成、復旧、移行、保守、テスト、リリースをエージェントが操作可能にする
フレームワークです。本リポジトリは **公開リリーステンプレート（public release template）** であり、
新規プロジェクトはここから開始してください。

AI Agent対応Webサイト運用フレームワーク v0.1.3 公開テンプレート。

**動作例:** [HADA_Website_Template_Sample](https://github.com/naokihada/HADA_Website_Template_Sample) — 本テンプレートで構築した架空データによる検証済みサンプルサイト。

---

## これは何か

**Web Site Operations Framework** — 単一のWebサイトではありません。Content Master、
処理ツール、plugins（capabilities）、Publication Root を分離し、エージェントと人間が
監査可能な手順でサイトを決定論的に運用できるようにします。

---

## v0.1.3 の範囲

- Foundation structure and agent contract（`AGENTS.md`）
- Core Validator（`tools/core/validate_framework.py`）
- Minimal i18n（jp / en）
- Japanese Content Master（`content/jp/`）
- jp → en translation with mock provider
- Single site-wide term dictionary（`config/term_dictionary.yaml`）
- Markdown → HTML build（`tools/core/build_site.py`）
- **Template Upgrade**（GitHub stable Release — `tools/core/upgrade_from_release.py`）
- Template version contract（`config/template.manifest.yaml`）
- Tests（47 total — `tests/README.md` 参照）

含まれるもの: client-only PWA Timer Demo、MM:SS表示、Beep音、best-effort local notification。

含まれないもの: real AI translation API、WordPress、RSS、broader PWA、FTP、n8n、cron、
plugins、full site import、production deployment automation。

既存プロジェクトのUpgrade: `docs/UPGRADE.md` を参照。

---

## はじめに

1. `config/term_dictionary.example.yaml` を `config/term_dictionary.yaml` にコピーし、entries を編集する。
2. Markdown コンテンツを `content/jp/`（Japanese master）に追加する。
3. 依存関係をインストールし、検証する:

```text
pip install -r tools/core/requirements.txt
python tools/core/validate_framework.py --root .
python tools/core/build_site.py --root .
```

4. 生成された HTML は `site/` 配下に出力される。

---

## リポジトリの役割

| Repo | Role |
|---|---|
| `HADA_Website_Template` | 本リポジトリ — v0.1.3 public template |
| `HADA_Website_Template_Dev` | Development workspace |
| `HADA_Website_Template_Sample` | Verified example with fictional sample site |

動作例は `HADA_Website_Template_Sample` を参照してください。

---

## 安全性

- Secrets never committed（`config/local.yaml`、`.env` — `.gitignore` 参照）
- Only `site/`（Publication Root）is deployed by default
- 完全な security / scope / deployment rules は `AGENTS.md` を参照

Agent operations の開始点: `AGENTS.md` および `Cursor/tasks/CURRENT.md`。

---

## Disclaimer（免責事項）

詳細な免責事項は [DISCLAIMER.md](DISCLAIMER.md) を参照してください。
