# write-for-models

The write-for-models plugin helps Claude Code write and revise instructions that an agent reads, such as skills, subagent system prompts, and subagent briefs.

## Skills

- `building-skills` decides a skill's scope, when it loads, and how its `SKILL.md`, `reference/`, `scripts/`, and `assets/` files are organized.
- `writing-prompts` writes and revises skill bodies, subagent system prompts, and subagent briefs, and offers an eval for each change.

## Install

```bash
claude plugin marketplace add andreimaxim/claude-plugins
claude plugin install write-for-models@andreimaxim
```

## Example prompts

- `/write-for-models:building-skills Create a skill for writing release notes from the current branch's changes. Define when it should load and when it shouldn't.`
- `/write-for-models:writing-prompts Review this subagent prompt for repeated or conflicting instructions, and propose a version that states the expected result instead of a step list.`
- `/write-for-models:writing-prompts Write a brief for a reviewing subagent to check the current branch's changes without biasing its conclusion.`

For a repository-specific request, open that repository and identify the file or workflow you want to change.

## Tools and permissions

The plugin contains Markdown instructions. It includes no hooks, executable scripts, MCP servers, package dependencies, or background services.

Claude can read and edit the instruction files relevant to your request. The building skill uses available parsers or validators to check skill frontmatter and references, and may consult the linked Agent Skills specification at `https://agentskills.io/specification`. The writing skill offers a behavioral evaluation plan, but requires explicit approval before running an evaluation through an agent or model. An approved evaluation uses the provider and tools you choose and can incur their normal charges.

Claude Code's permissions and your project settings still apply to file edits and tool calls.

## License

MIT. See [LICENSE](LICENSE), included in the plugin folder.
