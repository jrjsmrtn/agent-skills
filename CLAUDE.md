# jrjsmrtn-skills

Personal Claude Code skills marketplace. Contains a single `marketplace.json` manifest that references all published skill plugins.

## Structure

```
.claude-plugin/
  marketplace.json     # Marketplace manifest listing all published plugins
README.md
```

## Adding a Plugin

Add an entry to `.claude-plugin/marketplace.json` under `plugins`:

```json
{
  "name": "<plugin-name>",
  "description": "<one-line description>",
  "source": {
    "source": "url",
    "url": "https://github.com/jrjsmrtn/<plugin-name>.git",
    "sha": "<latest commit sha>"
  },
  "homepage": "https://github.com/jrjsmrtn/<plugin-name>"
}
```

Update the `sha` field whenever the referenced plugin is updated — see the Release Workflow below.

## Release Workflow

A release happens in two stages: each plugin is released independently, then the marketplace is refreshed to point at the new plugin commits.

### Stage 1 — Release a plugin

In the plugin repo (e.g. `~/Projects/Skills/c4-skills`):

1. Make changes on a feature/fix branch.
2. Bump `version` in `.claude-plugin/plugin.json` (semver patch during 0.1.x development).
3. Commit, push to `github`, open a PR, merge.
4. Sync `main` locally and push to `origin` (private git server):
   ```
   git checkout main && git pull github main && git push origin main
   ```

Repeat for each plugin that has new work.

### Stage 2 — Refresh the marketplace

Back in this repo:

1. Capture the **GitHub-side** SHA for each updated plugin:
   ```
   cd ~/Projects/Skills/<plugin> && git rev-parse github/main
   ```
   **Important:** use `github/main`, not local `main`. Local `main` may contain no-op merge commits introduced when reconciling a diverged `origin` (private git server) — those commits do not exist on GitHub and would break installs that resolve via the `https://github.com/...` source URL.
2. Update each plugin's `sha` in `.claude-plugin/marketplace.json`.
3. Bump `version` in `marketplace.json` (patch bump on any SHA change).
4. Commit, push to `github`, open a PR, merge.
5. Sync `main` and push to `origin`:
   ```
   git checkout main && git pull github main && git push origin main
   ```

### Versioning rules

- Plugin `version`: patch bump for any user-visible change to skills, README, CLAUDE.md, or plugin.json metadata. Don't bump for purely internal changes that don't affect installs.
- Marketplace `version`: patch bump on any SHA refresh, even if only one plugin changed.
- Both stay on `0.1.x` until the first deliberate `0.2.0` decision.

### Sync rule

When `origin` (private git server) diverges from `github` (e.g. an old commit only on origin), reconcile *before* the next release. Always merge `github` first so `main` matches GitHub, then push to `origin`. Never put a local-only merge SHA into `marketplace.json`.

## Published Plugins

| Plugin | Repo |
|--------|------|
| c4-skills | github:jrjsmrtn/c4-skills |
| diataxis-skills | github:jrjsmrtn/diataxis-skills |
| obsidian-skills | github:jrjsmrtn/obsidian-skills |
| project-orchestration-skills | github:jrjsmrtn/project-orchestration-skills |

## Remotes

- origin: private git server
- github: https://github.com/jrjsmrtn/jrjsmrtn-skills
