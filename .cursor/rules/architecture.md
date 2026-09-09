# Architecture rules (Cursor adapter)

Canonical source: `AGENTS.md` Architecture sections. Do not create component master
specification files (e.g. `tools/core/SPEC.md`). Complete Master Implementation
Specification exists in `HADA_Website_Template_Dev` only.

## Key boundaries

| Concept | Location |
|---|---|
| Content Master | `content/` |
| Publication Root | `site/` |
| External references | `references/` |
| Capabilities (plugins) | `tools/plugins/` with spec + manifest |
| Agent workspace | `Cursor/` (in Git; excluded from Web Publish — `site/` only) |

## Pipeline

Source → Source Adapter → Content Master → Processing → Destination → `site/`

## Do not

- Merge WordPress.com and self-hosted WordPress into one plugin concept
- Treat static cache or `site/index.html` as source of truth
- Mix PWA client cache with static-cache capability
- Put site core logic inside cron or n8n definitions

## Repository flow

`HADA_Website_Template_Dev` → `HADA_Website_Template` (this repo) → `HADA_Website_Template_Sample`
