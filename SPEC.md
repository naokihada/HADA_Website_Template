# Public Template Specification Summary

This repository is the public, consumer-facing artifact of the HADA Website Template.
The machine-readable contract is `config/template.manifest.yaml`; its Template version
is `0.2.0` and its compatibility schema/data format versions remain `0.1`.

The publication root is configured by `config/project.yaml`. Template Upgrade preserves
project-owned content, settings, assets, and custom publication roots. Existing legacy
`.cursor/` and `Cursor/` workspaces must be preserved and classified before migration to
`AI/`. Run ReSPEC, tests, validator, build, and diff review after an upgrade.
