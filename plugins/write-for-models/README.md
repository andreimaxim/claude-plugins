# write-for-models

The write-for-models plugin helps Claude Code write and revise instructions that a model reads, e.g. skills, system prompts, `CLAUDE.md` files, tool descriptions, and briefs for subagents. It adds two skills. Claude loads a skill when a request matches its description, and you can also invoke one directly as `/write-for-models:<skill>`.

## Skills

- `building-skills` defines a skill's scope, when it applies, how its `SKILL.md` is organized, and which details go into reference files. It checks a skill's front matter against the [Agent Skills specification](https://agentskills.io/specification).
- `writing-prompts` writes and revises model instructions. It reads the existing instructions first and removes or clarifies text before it adds more. After a change, it offers an eval with a positive case and a negative control, and it doesn't run the eval without your approval.

When the `write-for-people` plugin is also installed, `writing-prompts` applies its `plain-writing` skill to the prose of an instruction without changing what the instruction asks the model to do.

## What the plugin runs and fetches

The plugin contains only instructions. It has no hooks, MCP servers, or scripts, and it doesn't change your permission settings. Claude edits instruction files and may run a validator such as `claude plugin validate` through Claude Code's normal tools and permission prompts. An eval runs only after you approve its plan.

## Install

```bash
claude plugin marketplace add andreimaxim/claude-plugins
claude plugin install write-for-models@andreimaxim
```

## License

MIT, as set in the plugin's `plugin.json`.
