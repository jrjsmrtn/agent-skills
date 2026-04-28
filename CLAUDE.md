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

Update the `sha` field whenever the referenced plugin is updated.

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
