# claude-plugins

A [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces) named `andreimaxim`.

## Plugins

| Plugin | Description |
| :- | :- |
| [`maintain-rails`](plugins/maintain-rails) | Maintenance workflows for Ruby on Rails applications |

### maintain-rails

- **`/maintain-rails:upgrade-gem <gem> to <version>`** returns an application-specific upgrade plan. Claude finds the installed version, resolves the target to an exact release, and delegates to `gem-upgrade-planner`. It does not implement the upgrade.

  ```text
  /maintain-rails:upgrade-gem rails to 8.1
  ```

- **`maintain-rails:gem-upgrade-planner`** is the subagent that writes the plan. It investigates without modifying your application or shared data such as databases, and it does not implement the upgrade.

## Install

Add the marketplace, then install the plugin:

```bash
claude plugin marketplace add andreimaxim/claude-plugins
claude plugin install maintain-rails@andreimaxim
```

Inside a Claude Code session, use `/plugin marketplace add andreimaxim/claude-plugins` and `/plugin install maintain-rails@andreimaxim` instead.

Plugins don't set a `version`, so Claude Code treats each commit as a new version. To update manually, then restart Claude Code:

```bash
claude plugin marketplace update andreimaxim
claude plugin update maintain-rails@andreimaxim
```

## Layout

```text
.claude-plugin/marketplace.json        marketplace catalog
plugins/<plugin>/
  .claude-plugin/plugin.json           plugin manifest
  skills/<skill>/SKILL.md              skills, invoked as /<plugin>:<skill>
  agents/<agent>.md                    subagents, named <plugin>:<agent>
```

## Develop

Validate after every change:

```bash
claude plugin validate .
```

To try local edits, add the checkout as a marketplace and install from it. Claude Code then reads the plugin files in place, so edits apply at the next session start or after `/reload-plugins`:

```bash
claude plugin marketplace add ./
claude plugin install maintain-rails@andreimaxim
```

## License

[MIT](LICENSE)
