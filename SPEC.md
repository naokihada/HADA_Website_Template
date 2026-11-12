# HADA Website Template — Public Contract

| Contract field | Value |
|---|---|
| Artifact | HADA Website Template Release |
| Template version | 0.5.0 |
| HADA Base compatibility | Minimum 0.2.0; tested 0.2.0 |
| Applies to | Public template repository and artifacts extracted for that release |

This document records the public, testable behavior of the Release artifact. It is
derived from the Development repository's Master `SPEC.md`; it does not define
Dev-only maintenance, release preparation, private reports, or Sample content.
Executable implementation and regression tests in this repository are the final
evidence for current behavior. Upgrade procedures are defined in `docs/UPGRADE.md`.

## 1. Supported artifact and data formats

- The framework builds static site output. A published site does not require a
  server-side runtime, third-party JavaScript, or a CDN for its primary content.
- `site/` is the default Publication Root. Only that output tree is intended for
  web publication; configuration, source content, tools, and private media masters
  remain outside it.
- Human-edited project configuration uses TOML. Machine-readable catalogs,
  manifests, schemas, and replace-in-place records use JSON. Append-only records
  may use JSONL where each line is one complete JSON value.
- YAML is unsupported in active configuration, runtime code, tests, and project
  data. Existing project data must be converted to its defined JSON/TOML form;
  obsolete or unconvertible sample artifacts are removed. Preserved historical
  snapshots are evidence, not active inputs.
- The template identity and its Base compatibility version are separate. The
  current Release is `hada-website-template` 0.5.0 and is verified against
  `hada-base-template` 0.2.0.

## 2. Media catalog and publication

`config/media.catalog.json` is the single master for media assets and collections.
Generated HTML is rebuildable output and must not be edited as the content master.
The catalog records stable media and collection IDs, ordered membership, localized
labels, public derivatives, thumbnails, processing policy, and private-source
provenance.

Private masters and source paths are never published. Only approved public
derivatives and thumbnails referenced by collections are copied into Publication
Root. Image derivatives use supported JPEG/WebP output; source masters remain
available for regeneration. Metadata stripping, watermarks, size limits, and
image ordering are controlled through the catalog's processing policy and intake
configuration. A watermark or right-click suppression is a deterrent, not a
guarantee against copying or downloading public images.

### 2.1 Routes and hierarchy

`config/project.toml` → `paths.media_route` controls the locale-local library root
and defaults to `media`. For example, the library landing page is `/jp/media/` or
`/en/media/`. The route is a safe relative path. Same-site links are root-relative
and contain no domain name.

Collection routes resolve below that root. Nested collections preserve their
hierarchy in generated URLs and breadcrumbs. Legacy catalog prefixes are
normalized once and must not duplicate the configured media root. The library
landing page lists top-level collections when the catalog has no explicit empty-
route root collection. Changing `paths.media_route` changes generated locations
and links without changing collection IDs or item metadata.

Collections with children render cover cards. A collection's first approved image
in configured order is its cover; nested books may use the first available image
in their descendants. Cards can sort by localized name, publication date, or
updated date in ascending or descending order. Without JavaScript, the cards and
links remain usable in catalog order.

### 2.2 Gallery, sequence, and manga presentation

Gallery pages use the JSON catalog order and generated thumbnails. Optional local
viewer behavior enhances normal same-tab image links and degrades to those links
without JavaScript. Images use the shared site shell, core stylesheet, locale
navigation, display controls, and footer. Generated media pages declare the configured
Display State persistence flag and storage key so preferences survive locale and
collection navigation.

Ordered sequence collections support single-page navigation and configured
two-page spreads. Japanese manga uses the `manga` viewer mode and right-to-left
reading order. A viewer may offer both first-page-on-right and first-page-on-left
layouts; choosing a layout changes spread grouping, not the underlying media order
or the meaning of forward/back navigation. `single` remains the one-image view.
Desktop and mobile defaults are collection-configurable.

Missing media or thumbnails fail catalog validation before publication. A media ID
may appear only once in a collection. The builder does not publish private source
files or insert synthetic blank-page images.

## 3. Verification and upgrade

The Release artifact is validated with its framework validator, automated tests,
and static build. Browser tests are an additional contract when the optional
Playwright capability is installed. A missing optional browser capability is not
a browser-test pass.

Existing site projects use the Public Upgrade Contract in `docs/UPGRADE.md`.
Upgrade applies only reviewed, ownership-safe template changes; conflicts and
unknown ownership require review. It does not authorize publication or deployment.
