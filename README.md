# claude-plugins

The plugins here add skills and subagents that help software engineers with their work in Claude Code, e.g., planning and building features, explaining code, and writing text for people or for models. You can install each plugin on its own, so you only add the skills you need.

## Install

Add the marketplace, then install the plugins you want from the [list below](#plugins):

```bash
claude plugin marketplace add andreimaxim/claude-plugins
claude plugin install plan-and-build@andreimaxim
```

Inside a Claude Code session, use `/plugin marketplace add andreimaxim/claude-plugins` and `/plugin install plan-and-build@andreimaxim` instead.

Each plugin sets a `version` in its `plugin.json`, and Claude Code installs a new copy of a plugin only when that version changes. To get a new version, update the marketplace and the plugin, and then restart Claude Code:

```bash
claude plugin marketplace update andreimaxim
claude plugin update plan-and-build@andreimaxim
```

## Plugins

| Plugin | Description |
| :- | :- |
| [`plan-and-build`](plugins/plan-and-build) | Shape work into bounded plans and implement them with independent review |
| [`write-for-people`](plugins/write-for-people) | Explain code, choose names, and write plain prose for people to read |
| [`write-for-models`](plugins/write-for-models) | Write and revise skills, system prompts, and other model instructions |

Claude loads a plugin's skills on its own when a request matches the skill's description, so you can describe the task in your own words. You can also invoke a skill directly as `/<plugin>:<skill>`.

### plan-and-build

- **`shaping`** turns a raw idea, such as a feature request or a hard bug report, into a bounded plan. You agree on the problem and how much change you're willing to take on, and the plan describes the main parts of the solution, the risks, and what is left out. The skill follows the shaping phase of [37signals' Shape Up](https://basecamp.com/shapeup/1.1-chapter-02).
- **`implementing`** builds an agreed plan in small changes that each work end to end, starting from the plan's stories, and it checks each change as it goes. The Oracle subagent reviews the finished result, and the final report goes through each story and says how it was verified.
- **`proving-changes`** proves that a fix, feature, or refactor works end to end and doesn't break existing behavior. It runs the same scenario before and after the change on the closest available environment to production, and it checks control cases. Then it reports screenshots, recordings, logs, state, and timings. The skill is adapted from the proving-changes skill in [Amp's official plugins](https://github.com/ampcode/official-plugins/blob/main/skills/proving-changes/SKILL.md).
- **`organizing-commits`** reorganizes a branch's committed and uncommitted changes into commits that are easy to review, and each commit has to build and pass the repository's checks on its own. Claude saves a backup first, confirms that the final code is unchanged, and leaves the push to you. It also loads when you ask Claude to restack a branch.
- **`plan-and-build:oracle`** is a subagent that gives a second opinion on hard problems, such as a design or plan before you commit to it, a bug that survives the first explanation, or finished work before you call it done. It runs on Fable at extra-high effort and can only read and search files. Inspired by [Amp's Oracle](https://ampcode.com/docs/tools#oracle).
- **`plan-and-build:librarian`** is a subagent that researches code outside the working tree, including dependencies at their installed version, other repositories, and commit history, and links to its sources. It runs on Sonnet at medium effort and doesn't change your checkout. Inspired by [Amp's Librarian](https://ampcode.com/docs/tools#librarian).

### write-for-people

- **`explaining-code`** explains how code works, why it was designed that way, and what a change does. It traces the actual code path, keeps what the code shows separate from what Claude infers, and shows changes as before and after drawings or diffs. Inspired by HumanLayer's [show-me](https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md) skill and the explaining-code skill in [Amp's official plugins](https://github.com/andreimaxim/amp-official-plugins/blob/main/skills/explaining-code/SKILL.md).
- **`naming-things`** chooses names and terms for variables, functions, tables, endpoints, and domain concepts. It reads how a thing is defined and used before it suggests a name. Adapted from Koleman Nix's [How to name things](https://kolemannix.com/blog/how-to-name-things/).
- **`plain-writing`** writes and edits prose in a plain and boring style, with everyday words, complete sentences, no dashes, and no jargon. It applies to documents, READMEs, emails, commit messages, PR descriptions, and code explanations, but not to code. Once the plugin is installed, Claude uses this style for most prose it writes for you. To rewrite Claude's previous reply for a reader with no context, run `/write-for-people:plain-writing deslopify` or ask Claude to "deslopify" the reply.

### write-for-models

- **`building-skills`** defines a skill's scope, when it applies, how its `SKILL.md` is organized, and which details go into reference files.
- **`writing-prompts`** writes and revises model instructions, such as skill bodies, system prompts, `CLAUDE.md` files, tool descriptions, and subagent briefs. It removes or clarifies existing text before adding more, and it offers an eval for a change instead of running one without approval.

## Develop

Validate after every change:

```bash
claude plugin validate .
```

When you change a plugin, raise the `version` in its `plugin.json`, because users don't get the change until the version changes.

To try local edits, add the checkout as a marketplace and install from it. Claude Code then reads the plugin files in place, so edits apply at the next session start or after `/reload-plugins`:

```bash
claude plugin marketplace add ./
claude plugin install plan-and-build@andreimaxim
```
