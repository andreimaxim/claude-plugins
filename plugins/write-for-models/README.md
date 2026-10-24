# write-for-models

The write-for-models plugin helps Claude Code write and revise instructions that a model reads, such as skills, system prompts, `CLAUDE.md` files, tool descriptions, and subagent briefs.

## Skills

- `building-skills` decides a skill's scope, when it loads, and how its `SKILL.md` and reference files are organized.
- `writing-prompts` writes and revises model instructions, removing or clarifying text before it adds more, and offers an eval for each change.

With `write-for-people` installed, `writing-prompts` also writes the instruction's prose in the plain style.

## Install

```bash
claude plugin marketplace add andreimaxim/claude-plugins
claude plugin install write-for-models@andreimaxim
```

## License

MIT, as set in the plugin's `plugin.json`.
