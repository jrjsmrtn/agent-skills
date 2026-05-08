<!-- SPDX-FileCopyrightText: 2026 Georges Martin <jrjsmrtn@gmail.com> -->
<!-- SPDX-License-Identifier: MIT -->

# jrjsmrtn-skills

Personal Claude Code skills marketplace — a single install that brings in all my published skill plugins.

## Installation

### Claude Code

```
/plugin install github:jrjsmrtn/jrjsmrtn-skills
```

### Other agents (Copilot CLI, Cursor, Codex, Gemini CLI, Antigravity, Goose, …)

[Mistral Vibe](https://mistral.ai/products/vibe), [Copilot CLI](https://github.com/features/copilot/cli), and other agent hosts don't support Claude Code's marketplace format, but the plugins are valid [Agent Skills](https://agentskills.io/specification). Use the GitHub CLI (`gh` ≥ 2.90.0) — it auto-detects the agent host and installs into the right skills directory:

```
gh skill install jrjsmrtn/c4-skills
gh skill install jrjsmrtn/diataxis-skills
gh skill install jrjsmrtn/obsidian-skills
gh skill install jrjsmrtn/project-orchestration-skills
```

Pin a single skill or version: `gh skill install jrjsmrtn/<plugin> <skill-name>[@v<version>]`. Update later with `gh skill update --all`.

For [Mistral Vibe](https://mistral.ai/products/vibe) (not yet in `gh skill`'s host detection), pass `--dir`:

```
gh skill install jrjsmrtn/<plugin> --dir ~/.vibe/skills
```

## Included Plugins

| Plugin                                                                                   | Skills                                                                                                                                            | Description                                  |
| ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| [c4-skills](https://github.com/jrjsmrtn/c4-skills)                                       | c4-model, c4-review, structurizr-dsl, c4-deployment                                                                                               | C4 architecture modeling and Structurizr DSL |
| [diataxis-skills](https://github.com/jrjsmrtn/diataxis-skills)                           | diataxis-audit, diataxis-create, diataxis-convert, diataxis-plan                                                                                  | Diátaxis documentation framework             |
| [obsidian-skills](https://github.com/jrjsmrtn/obsidian-skills)                           | obsidian-cli                                                                                                                                      | Obsidian vault interaction via CLI           |
| [project-orchestration-skills](https://github.com/jrjsmrtn/project-orchestration-skills) | analyze-project, bootstrap-project, setup-adrs, setup-architecture-as-code, setup-pre-commit, validate-quality-config, plan-sprint, wrapup-sprint | AI-assisted project orchestration            |

## License

MIT
