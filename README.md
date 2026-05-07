<!-- SPDX-FileCopyrightText: 2026 Georges Martin <jrjsmrtn@gmail.com> -->
<!-- SPDX-License-Identifier: MIT -->

# jrjsmrtn-skills

Personal Claude Code skills marketplace — a single install that brings in all my published skill plugins.

## Installation

### Claude Code

```
/plugin install github:jrjsmrtn/jrjsmrtn-skills
```

### Mistral Vibe

[Mistral Vibe](https://mistral.ai/products/vibe) doesn't support marketplaces, but the plugins are valid [Agent Skills](https://agentskills.io/specification). Bridge them by cloning each plugin into `~/.vibe/claude-skills/` and symlinking individual skills into `~/.vibe/skills/` (which Vibe scans as a flat directory):

```bash
mkdir -p ~/.vibe/claude-skills ~/.vibe/skills

# Clone all plugins
for plugin in c4-skills diataxis-skills obsidian-skills project-orchestration-skills; do
  git clone "https://github.com/jrjsmrtn/${plugin}.git" "$HOME/.vibe/claude-skills/${plugin}"
done

# Symlink each skill into Vibe's flat skills directory
cd ~/.vibe/skills
for plugin_dir in ~/.vibe/claude-skills/*/; do
  plugin=$(basename "$plugin_dir")
  for skill_dir in "$plugin_dir"skills/*/; do
    skill=$(basename "$skill_dir")
    ln -s "../claude-skills/${plugin}/skills/${skill}" "${skill}"
  done
done
```

Update later with `git -C ~/.vibe/claude-skills/<plugin> pull` — symlinks survive pulls.

## Included Plugins

| Plugin                                                                                   | Skills                                                                                                                                            | Description                                  |
| ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| [c4-skills](https://github.com/jrjsmrtn/c4-skills)                                       | c4-model, c4-review, structurizr-dsl, c4-deployment                                                                                               | C4 architecture modeling and Structurizr DSL |
| [diataxis-skills](https://github.com/jrjsmrtn/diataxis-skills)                           | diataxis-audit, diataxis-create, diataxis-convert, diataxis-plan                                                                                  | Diátaxis documentation framework             |
| [obsidian-skills](https://github.com/jrjsmrtn/obsidian-skills)                           | obsidian-cli                                                                                                                                      | Obsidian vault interaction via CLI           |
| [project-orchestration-skills](https://github.com/jrjsmrtn/project-orchestration-skills) | analyze-project, bootstrap-project, setup-adrs, setup-architecture-as-code, setup-pre-commit, validate-quality-config, plan-sprint, wrapup-sprint | AI-assisted project orchestration            |

## License

MIT
