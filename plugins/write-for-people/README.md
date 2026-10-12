# write-for-people

The write-for-people plugin helps Claude Code write text that people read, e.g. explanations of code, names in code, and prose such as documentation and pull request descriptions. It adds three skills. Claude loads a skill when a request matches its description, and you can also invoke one directly as `/write-for-people:<skill>`.

## Skills

- `explaining-code` explains how code works and why it was designed that way. It traces the actual code path, and it keeps what the code shows separate from what Claude infers. Inspired by HumanLayer's [show-me](https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md) skill.
- `naming-things` chooses names and terms for variables, functions, tables, endpoints, and domain concepts. It reads how a thing is defined and used before it suggests a name, and it renames only when you ask. Adapted from Koleman Nix's [How to name things](https://kolemannix.com/blog/how-to-name-things/).
- `plain-writing` writes and edits prose in a plain style, with everyday words, complete sentences, no dashes, and no jargon. It doesn't apply to code.

## Plain writing applies by default

The `plain-writing` skill's description covers documents, READMEs, emails, commit messages, and pull request descriptions. Once the plugin is installed, Claude uses that style for most prose it writes for you, including when you only wanted `explaining-code` or `naming-things`. To turn it off, disable the plugin.

To rewrite Claude's previous reply for a reader with no project context, run `/write-for-people:plain-writing deslopify`, or ask Claude to "deslopify" the reply.

## What the plugin runs and fetches

The plugin contains only instructions. It has no hooks, MCP servers, or scripts, and it doesn't change your permission settings. `explaining-code` reads code and git history, and it may read pull request discussions or tickets when your session has tools for them. An approved rename from `naming-things` edits files and runs the repository's checks. Each of these steps uses Claude Code's normal tools and permission prompts.

## Install

```bash
claude plugin marketplace add andreimaxim/claude-plugins
claude plugin install write-for-people@andreimaxim
```

## License

MIT, as set in the plugin's `plugin.json`. The `plain-writing` skill comes from [docwriter-org/plain-writing-skill](https://github.com/docwriter-org/plain-writing-skill), copyright (c) 2026 Shreya Shankar, under the MIT license in `skills/plain-writing/LICENSE`. This copy changes the deslopify command name to the plugin's form, `/write-for-people:plain-writing deslopify`.
