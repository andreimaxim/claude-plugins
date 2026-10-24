# claude-plugins

Plugins that help software engineers work in Claude Code, such as planning and building features, explaining code, and writing text for people or for models. You can install each plugin on its own.

## Plugins

| Plugin | Description |
| :- | :- |
| [plan-and-build](plugins/plan-and-build) | Shape work into bounded plans and implement them with independent review |
| [write-for-people](plugins/write-for-people) | Explain code, choose names, and write plain prose for people to read |
| [write-for-models](plugins/write-for-models) | Write and revise skills, system prompts, and other model instructions |

Each plugin's README describes its skills and subagents. Claude loads a skill when a request matches its description, and you can also invoke one directly as `/<plugin>:<skill>`.

## Install

```bash
claude plugin marketplace add andreimaxim/claude-plugins
claude plugin install plan-and-build@andreimaxim
```

Inside a Claude Code session, run these instead:

```
/plugin marketplace add andreimaxim/claude-plugins
/plugin install plan-and-build@andreimaxim
```

It's recommended to turn on auto-update, which is off by default for marketplaces outside Anthropic's own. To turn it on, run `/plugin` in a Claude Code session, open the **Marketplaces** tab, select `andreimaxim`, and choose **Enable auto-update**.

## Develop

Validate after every change:

```bash
claude plugin validate .
```

When you change a plugin, raise the `version` in its `plugin.json`, because users get a change only when the version changes.

To try local edits, install the plugins from your checkout. Edits apply at the next session start or after `/reload-plugins`:

```bash
claude plugin marketplace add ./
claude plugin install plan-and-build@andreimaxim
```
