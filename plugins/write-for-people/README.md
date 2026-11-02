# write-for-people

The write-for-people plugin helps Claude Code write text that people read, such as explanations of code, names in code, documentation, and website text.

## Skills

- `explaining-code` explains how code works, why it was designed that way, and what a change does.
- `naming-things` chooses names for code and domain concepts after it works out what each thing means and does.
- `plain-writing` writes and revises text for standalone messages, documents, websites, and interfaces, using plain language and consistent structure.

## Install

```bash
claude plugin marketplace add andreimaxim/claude-plugins
claude plugin install write-for-people@andreimaxim
```

## Example prompts

- `/write-for-people:explaining-code Explain how a request reaches the database in this repository. Read the relevant code and show the call path.`
- `/write-for-people:naming-things Review the name documentDate in this checkout. Trace where it is set and read, then recommend a name without editing files.`
- `/write-for-people:plain-writing Rewrite this sentence in plain language: We leverage a robust orchestration layer to seamlessly facilitate data ingestion.`
- `/write-for-people:plain-writing Review the text on this website so people can understand it easily. Check sentence structure, word order, and terminology, and preserve the facts and important qualifications.`

For a code question, open the relevant repository and identify an entry point or symbol that exists in it.

## Tools and permissions

The plugin contains Markdown instructions and a license notice. It includes no hooks, executable scripts, MCP servers, package dependencies, or background services.

Claude can read the code and documents relevant to your question. For historical explanations, it can read Git history and related pull requests, issues, or design records through the tools available in your session. Those lookups may contact GitHub or another source you use for the task.

A requested rename can edit files and run the repository's checks. Explanations and naming advice don't authorize unrelated code changes. When useful, the explaining skill can create a rendered page with an available artifact tool or offer an HTML file you can open. Claude Code's permissions and your project settings still apply.

## Sources

`explaining-code` is inspired by HumanLayer's [show-me](https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md) skill and the explaining-code skill in [Amp's official plugins](https://github.com/andreimaxim/amp-official-plugins/blob/main/skills/explaining-code/SKILL.md). `naming-things` is adapted from Koleman Nix's [How to name things](https://kolemannix.com/blog/how-to-name-things/).

`plain-writing` adapts Shreya Shankar's [Five Principles for Generating Readable AI Text in Bulk](https://fsdatalab.github.io/blog/agent-text-at-scale/), published October 29, 2026, from the [article source](https://github.com/fsdatalab/fsdatalab.github.io/blob/0c01cb07a244e7df461a7da51615fc3131bd1b79/_posts/2026-10-29-agent-text-at-scale.md). It combines the article's principles with the 25 rules and inline examples from the upstream [plain-writing skill](https://github.com/docwriter-org/plain-writing-skill/blob/f0d3630983ac7a82aa580f1c1509d72df739ee12/skills/plain-writing/SKILL.md). Related guidance is grouped by topic, with instructions before explanations and examples.

All five figures appear as inline Markdown tables, with their example text and counts preserved. The dashboard's bar chart is represented by a review-count column, and the research citations remain in Markdown footnotes. The wording, headings, and captions are adapted for the skill rather than copied verbatim. Publication framing, interactive figure instructions, and the compression analogy are omitted. The skill needs no HTML or external images.

The sentence and list-length rules distinguish prose from short labels and required data. The introduction requires edits to preserve facts and qualifications, and it excludes code, identifiers, and exact quotations from style changes.

## License

The plugin's original material and the original plain-writing rules are MIT-licensed. [LICENSE](LICENSE) contains the MIT terms and copyright notices for Andrei Maxim and Shreya Shankar.

The article text and transcribed figures in `plain-writing` remain attributed to Shreya Shankar. The article invites readers to use its checklist in coding agents or make a skill from it. Its source repository does not include a license file, and the plugin's MIT notice does not relicense the article.
