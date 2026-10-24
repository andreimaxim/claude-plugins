# write-for-people

The write-for-people plugin helps Claude Code write text that people read, such as explanations of code, names in code, and documentation or pull request descriptions.

## Skills

- `explaining-code` explains how code works, why it was designed that way, and what a change does.
- `naming-things` chooses names for code and domain concepts after it works out what each thing means and does.
- `plain-writing` writes and edits prose in a plain style, and `/write-for-people:plain-writing deslopify` rewrites Claude's previous reply.

## Install

```bash
claude plugin marketplace add andreimaxim/claude-plugins
claude plugin install write-for-people@andreimaxim
```

## Sources

`explaining-code` is inspired by HumanLayer's [show-me](https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md) skill and the explaining-code skill in [Amp's official plugins](https://github.com/andreimaxim/amp-official-plugins/blob/main/skills/explaining-code/SKILL.md). `naming-things` is adapted from Koleman Nix's [How to name things](https://kolemannix.com/blog/how-to-name-things/).

## License

MIT, as set in the plugin's `plugin.json`. The `plain-writing` skill comes from [docwriter-org/plain-writing-skill](https://github.com/docwriter-org/plain-writing-skill), copyright (c) 2026 Shreya Shankar, under the MIT license in `skills/plain-writing/LICENSE`. This copy changes the deslopify command name to the plugin's form, `/write-for-people:plain-writing deslopify`.
