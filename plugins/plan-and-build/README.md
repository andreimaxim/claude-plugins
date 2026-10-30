# plan-and-build

The plan-and-build plugin helps Claude Code turn a raw idea into working, reviewed code. It adds five skills and three subagents.

## How it works

You start with a raw idea, such as a feature request, a ticket, or a hard bug report. The `shaping` skill works through it with you until it becomes a plan that is ready to build. The plan covers the problem, the outcome, how much change you're willing to take on, the main parts of the solution, and the stories it has to support. Shaping is a conversation, so expect to go back and forth until the shape is right.

Once you have the plan, run `implementing` with it, ideally in a new session so the work starts from the plan and not from the shaping conversation. Claude builds the plan in small pieces that each work end to end, checks them as it goes, and has the result reviewed before it reports the work as ready. When you need stronger evidence, `proving-changes` shows the change working, and `organizing-commits` prepares the branch for review. For a change where a missed regression is expensive, such as a dependency upgrade that fixes a CVE, run `auditing-changes` yourself before it ships.

## Skills

- `shaping` turns a raw idea into a bounded plan, following the shaping phase of Shape Up.
- `implementing` builds an agreed plan in small verified pieces and reports how each story was verified.
- `proving-changes` shows with before and after evidence that a change works and breaks nothing.
- `organizing-commits` reorganizes a branch into commits that are easy to review and that each pass the checks.
- `auditing-changes` checks every line of a high-stakes change, such as a security upgrade of a dependency, and reports whether it can ship without regressions. Claude never loads it on its own, so you start it with `/plan-and-build:auditing-changes`.

## Subagents

- `planner` maps part of a plan onto the code and returns the steps to build it.
- `reviewer` reviews a change, a plan, or a design as a skeptic and reports what should block it. It needs access to the Fable model.
- `researcher` answers questions about dependencies and other repositories by reading their source.

## Install

```bash
claude plugin marketplace add andreimaxim/claude-plugins
claude plugin install plan-and-build@andreimaxim
```

## Example prompts

Use the prompts in a development checkout, and supply the plan or change each prompt refers to:

- `/plan-and-build:shaping Our CSV export times out for large accounts. Shape a solution that keeps the current download format and doesn't add a new service.`
- `/plan-and-build:implementing Implement the plan below. Keep the public API unchanged and verify each story with the repository's checks.`
- `/plan-and-build:proving-changes Prove that the current export fix handles an empty account and a large account. Compare the base revision with the changed code using local test data.`
- `/plan-and-build:organizing-commits Reorganize the current branch into reviewable commits without changing its final contents. Keep a backup and don't push.`
- `/plan-and-build:auditing-changes Upgrade rack to the release that fixes its latest CVE. Tell me whether we can ship it without regressions.`

## Tools and permissions

The plugin contains Markdown instructions and reference files. It includes no hooks, executable scripts, MCP servers, package dependencies, or background services.

When you use its skills, Claude can read project files and Git history, edit code, and run the project's builds and tests through the tools available in your session. The proving skill can start local services and collect screenshots, recordings, logs, and timings. The commit-organizing skill rewrites local Git history after saving a backup and leaves pushing to you. The auditing skill runs upgrades, builds, and tests in a separate Git worktree and leaves your checkout unchanged. For a dependency upgrade, it downloads the released packages of both versions from the package registry and clones the library's source.

The researcher can clone the repositories relevant to your question, fetch their history, and read related issues, pull requests, and web documentation. It uses Git, optionally the GitHub CLI, and Claude's web tools. Private repository access uses your existing Git authentication. Remote requests go to the repository host, GitHub, or the sources relevant to the research. Research clones remain under `/tmp/claude-researcher/`, or `%TEMP%\claude-researcher\` on Windows, until you remove them.

The skills require explicit approval for publishing, deploying, or writing to production. Claude Code's permissions and your project settings still apply to every tool call.

## Sources

The `shaping` skill and its reference files are based on chapters of [37signals' Shape Up](https://basecamp.com/shapeup):

- The plan template follows [Write the Pitch](https://basecamp.com/shapeup/1.5-chapter-06).
- The invoice autopay case study and the backend breadboarding technique follow [Find the Elements](https://basecamp.com/shapeup/1.3-chapter-04).
- The dot calendar case study draws on [Principles of Shaping](https://basecamp.com/shapeup/1.1-chapter-02), [Set Boundaries](https://basecamp.com/shapeup/1.2-chapter-03), and [Find the Elements](https://basecamp.com/shapeup/1.3-chapter-04).

The `planner`, `reviewer`, and `researcher` subagents are adapted from Amp's planning, reviewing, and researching subagent skills.

The `implementing` skill follows the building phase of [Shape Up](https://basecamp.com/shapeup/3.0-chapter-10) and draws on engineering principles from Lauren Tan's [pstack](https://github.com/cursor/plugins/tree/main/pstack). The `proving-changes` skill is adapted from the skill of the same name in [Amp's official plugins](https://github.com/ampcode/official-plugins/blob/main/skills/proving-changes/SKILL.md), and its benchmarking section draws on `pstack`'s [benchmark-checklist](https://github.com/cursor/plugins/blob/main/pstack/skills/benchmark-checklist/SKILL.md), which is based on Brendan Gregg's benchmarking checklist.

## License

MIT. See [LICENSE](LICENSE), included in the plugin folder.
