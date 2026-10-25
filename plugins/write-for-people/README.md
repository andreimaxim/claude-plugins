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

## Example prompts

- `/write-for-people:explaining-code Explain how a request reaches the database in this repository. Read the relevant code and show the call path.`
- `/write-for-people:naming-things Review the name documentDate in this checkout. Trace where it is set and read, then recommend a name without editing files.`
- `/write-for-people:plain-writing Rewrite this sentence in plain language: We leverage a robust orchestration layer to seamlessly facilitate data ingestion.`

For a code question, open the relevant repository and identify an entry point or symbol that exists in it.

## Tools and permissions

The plugin contains Markdown instructions and a license notice. It includes no hooks, executable scripts, MCP servers, package dependencies, or background services.

Claude can read the code and documents relevant to your question. For historical explanations, it can read Git history and related pull requests, issues, or design records through the tools available in your session. Those lookups may contact GitHub or another source you use for the task.

A requested rename can edit files and run the repository's checks. Explanations and naming advice don't authorize unrelated code changes. When useful, the explaining skill can create a rendered page with an available artifact tool or offer an HTML file you can open. Claude Code's permissions and your project settings still apply.

## Sources

`explaining-code` is inspired by HumanLayer's [show-me](https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md) skill and the explaining-code skill in [Amp's official plugins](https://github.com/andreimaxim/amp-official-plugins/blob/main/skills/explaining-code/SKILL.md). `naming-things` is adapted from Koleman Nix's [How to name things](https://kolemannix.com/blog/how-to-name-things/).

## License

MIT. See [LICENSE](LICENSE), included in the plugin folder. The `plain-writing` skill comes from [docwriter-org/plain-writing-skill](https://github.com/docwriter-org/plain-writing-skill), copyright (c) 2026 Shreya Shankar, under the MIT license in [skills/plain-writing/LICENSE](skills/plain-writing/LICENSE). This copy changes the deslopify command name to the plugin's form, `/write-for-people:plain-writing deslopify`.
