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

### Branch naming

Conventional-commit prefix in the branch name:
- `feat/<slug>` — user-visible additions (new skill, new feature, new docs section)
- `fix/<slug>` — corrections (bug, broken docs, frontmatter issue)
- `chore/<slug>` — non-functional housekeeping (marketplace SHA refresh, version-only bumps)

### Stage 1 — Release a plugin

In the plugin repo (e.g. `~/Projects/Skills/c4-skills`), with `REPO=$(pwd)` and `GH=jrjsmrtn/<plugin>`:

1. **Branch and stage:**
   ```
   git -C $REPO checkout -b <type>/<slug>
   git -C $REPO add <files>
   ```
2. **Bump versions:** `version` in `.claude-plugin/plugin.json` *and* the matching `metadata.version` in every touched `SKILL.md` (they must stay in lockstep).
3. **Commit** using a HEREDOC for proper formatting; include the `Co-Authored-By` footer when pair-programmed.
4. **Push to `github` and open a PR:**
   ```
   git -C $REPO push -u github <type>/<slug>
   gh pr create --repo $GH --head <type>/<slug> --base main --title "..." --body "..."
   ```
5. **Merge and sync `main`:**
   ```
   gh pr merge --repo $GH <type>/<slug> --merge --delete-branch
   git -C $REPO checkout main
   git -C $REPO pull github main
   git -C $REPO push origin main
   ```

Repeat for each plugin that has new work.

**Pre-commit hook:** each plugin repo runs gitleaks on staged changes. No action needed unless it flags a leak.

**If `origin` (the private git server) is unreachable** — push only to `github` and continue; catch up `origin` later. Safe because Stage 2 reads `github/main`, not `origin/main`. Batch fix-up:
```
for repo in c4-skills diataxis-skills obsidian-skills project-orchestration-skills jrjsmrtn-skills; do
  git -C ~/Projects/Skills/$repo push origin main
done
```

### Stage 2 — Refresh the marketplace

Back in this repo:

1. **Capture the GitHub-side SHA** for each updated plugin:
   ```
   for repo in c4-skills diataxis-skills obsidian-skills project-orchestration-skills; do
     echo "$repo: $(git -C ~/Projects/Skills/$repo rev-parse github/main)"
   done
   ```
   **Important:** use `github/main`, not local `main`. Local `main` may contain no-op merge commits introduced when reconciling a diverged `origin` — those commits do not exist on GitHub and would break installs that resolve via the `https://github.com/...` source URL.
2. **Update** each plugin's `sha` in `.claude-plugin/marketplace.json`.
3. **Bump** `version` in `marketplace.json` (patch bump on any SHA change).
4. **Branch / commit / PR / merge / sync** — same shape as Stage 1, branch named `chore/refresh-marketplace-<new-version>`.

### Downstream consumers

Once Stage 2 lands:
- **Claude Code** users running `/plugin install github:jrjsmrtn/jrjsmrtn-skills` resolve the new plugin SHAs on their next install.
- **Other agents** (Copilot CLI, Cursor, Codex, Gemini CLI, …) installed via `gh skill install jrjsmrtn/<plugin>` pick up changes via `gh skill update --all`.

Both paths benefit from a single Stage 2 — no separate publishing step needed.

### Versioning rules

- Plugin `version` (in `plugin.json`): patch bump for any user-visible change to skills, README, CLAUDE.md, or plugin.json metadata. Don't bump for purely internal changes that don't affect installs.
- Per-skill `metadata.version` (in `SKILL.md`): tracks the parent plugin's `version` exactly. Bump in lockstep.
- Marketplace `version` (in `marketplace.json`): patch bump on any SHA refresh, even if only one plugin changed.
- All three stay on `0.1.x` until the first deliberate `0.2.0` decision.

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
