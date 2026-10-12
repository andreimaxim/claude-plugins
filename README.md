# claude-plugins

The plugins here add skills and subagents that help software engineers with their work in Claude Code, e.g., planning gem upgrades in a Rails application, planning and building features, and writing text for people or for models. You can install each plugin on its own, so you only add the skills you need.

## Install

Add the marketplace, then install the plugins you want from the [list below](#plugins):

```bash
claude plugin marketplace add andreimaxim/claude-plugins
claude plugin install plan-and-build@andreimaxim
```

Inside a Claude Code session, use `/plugin marketplace add andreimaxim/claude-plugins` and `/plugin install plan-and-build@andreimaxim` instead.

Plugins don't set a `version`, so Claude Code treats each commit as a new version. To update manually, then restart Claude Code:

```bash
claude plugin marketplace update andreimaxim
claude plugin update plan-and-build@andreimaxim
```

## Plugins

| Plugin | Description |
| :- | :- |
| [`maintain-rails`](plugins/maintain-rails) | Maintenance workflows for Ruby on Rails applications |
| [`plan-and-build`](plugins/plan-and-build) | Shape work into bounded plans and implement them with independent review |
| [`write-for-people`](plugins/write-for-people) | Explain code, choose names, and write plain prose for people to read |
| [`write-for-models`](plugins/write-for-models) | Write and revise skills, system prompts, and other model instructions |

Claude loads a plugin's skills on its own when a request matches the skill's description, so you can describe the task in your own words. You can also invoke a skill directly as `/<plugin>:<skill>`.

### maintain-rails

- **`/maintain-rails:upgrade-gem <gem> to <version>`** returns an application-specific upgrade plan. Claude finds the installed version, resolves the target to an exact release, and delegates to `gem-upgrade-planner`. It does not implement the upgrade.

  ```text
  /maintain-rails:upgrade-gem rails to 8.1
  ```

- **`maintain-rails:gem-upgrade-planner`** is the subagent that writes the plan. It investigates without modifying your application or shared data such as databases, and it does not implement the upgrade.

### plan-and-build

- **`shaping`** turns a raw idea, such as a feature request or a hard bug report, into a bounded plan. You agree on the problem and how much change you're willing to take on, and the plan describes the main parts of the solution, the risks, and what is left out. The skill follows the shaping phase of [37signals' Shape Up](https://basecamp.com/shapeup/1.1-chapter-02).
- **`implementing`** builds an agreed plan in small changes that each work end to end, and it checks each change as it goes. An independent reviewer has to review the finished result. If no reviewer is available, Claude reports the review as missing instead of reviewing its own work.
- **`plan-and-build:oracle`** is a read-only subagent that reviews code, investigates hard bugs, and advises on architecture decisions. It runs on Fable at high effort and can't edit files. Inspired by [Amp's Oracle](https://ampcode.com/docs/tools#oracle).
- **`plan-and-build:librarian`** is a subagent that researches local and external code, including dependencies and commit history, and cites its sources. It runs on Sonnet at high effort and doesn't change your checkout. Inspired by [Amp's Librarian](https://ampcode.com/docs/tools#librarian).

### write-for-people

- **`explaining-code`** explains how code works and why it was designed that way. It traces the actual code path, and it keeps what the code shows separate from what Claude infers. Inspired by HumanLayer's [show-me](https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md) skill.
- **`naming-things`** chooses names and terms for variables, functions, tables, endpoints, and domain concepts. It reads how a thing is defined and used before it suggests a name. Adapted from Koleman Nix's [How to name things](https://kolemannix.com/blog/how-to-name-things/).
- **`plain-writing`** writes and edits prose in a plain and boring style, with everyday words, complete sentences, no dashes, and no jargon. It applies to documents, READMEs, emails, commit messages, PR descriptions, and code explanations, but not to code. To rewrite Claude's previous reply for a reader with no context, ask it to "deslopify" the reply.

### write-for-models

- **`building-skills`** defines a skill's scope, when it applies, how its `SKILL.md` is organized, and which details go into reference files.
- **`writing-prompts`** writes and revises model instructions, such as skill bodies, system prompts, `CLAUDE.md` files, tool descriptions, and subagent briefs. It removes or clarifies existing text before adding more, and it offers an eval for a change instead of running one without approval.

## Develop

Validate after every change:

```bash
claude plugin validate .
```

To try local edits, add the checkout as a marketplace and install from it. Claude Code then reads the plugin files in place, so edits apply at the next session start or after `/reload-plugins`:

```bash
claude plugin marketplace add ./
claude plugin install plan-and-build@andreimaxim
```
