# cursor-agent-templates

Source of truth for reusable Cursor agent rules and skills. Projects **sync copies** into `.cursor/` — Cursor does not load this repo directly.

## Layout

```text
global/                 # Cross-repo behavior (thin router base, output contract, safe change)
dbt/                    # dbt domain pack
repo-overlays/pew-dbt/  # Pew DDM dbt overlay bootstrap (copy into target repos as repo-owned)
```

**Router split:** `global/agent-router-base.mdc` (synced, thin) + `repo-overlays/pew-dbt/pew-dbt-router-overlay.mdc` (repo-owned task table).

## Usage

From a project repo (e.g. `ddm-dbt`):

```bash
./scripts/sync-cursor-rules.sh              # copy template-owned files
./scripts/sync-cursor-rules.sh --check      # fail if synced files drift
```

Set `CURSOR_TEMPLATES_REPO` if the template repo is not at `../cursor-agent-templates`.

## Versioning

Bump `template_version` in each project's `.cursor/cursor-manifest.yaml` when rolling out template changes, then tag `cursor-agent-templates` (`v<version>`) so CI pins the same revision.

Rules under `.cursor/rules` are review policy for the active session. Executable verification lives in repo skills (e.g. `verify-ddm-dbt`). The only Pew custom subagent today is `deep-bug-finder`.
