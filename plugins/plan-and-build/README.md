# plan-and-build

The plan-and-build plugin helps Claude Code turn a raw idea into working, reviewed code. It adds four skills and three subagents.

## How it works

You start with a raw idea, such as a feature request, a ticket, or a hard bug report. The `shaping` skill works through it with you until it becomes a plan that is ready to build. The plan covers the problem, the outcome, how much change you're willing to take on, the main parts of the solution, and the stories it has to support. Shaping is a conversation, so expect to go back and forth until the shape is right.

Once you have the plan, run `implementing` with it, ideally in a new session so the work starts from the plan and not from the shaping conversation. Claude builds the plan in small pieces that each work end to end, checks them as it goes, and has the result reviewed before it reports the work as ready. When you need stronger evidence, `proving-changes` shows the change working, and `organizing-commits` prepares the branch for review.

## Skills

- `shaping` turns a raw idea into a bounded plan, following the shaping phase of Shape Up.
- `implementing` builds an agreed plan in small verified pieces and reports how each story was verified.
- `proving-changes` shows with before and after evidence that a change works and breaks nothing.
- `organizing-commits` reorganizes a branch into commits that are easy to review and that each pass the checks.

## Subagents

- `planner` maps part of a plan onto the code and returns the steps to build it.
- `reviewer` reviews a change, a plan, or a design as a skeptic and reports what should block it. It needs access to the Fable model.
- `researcher` answers questions about dependencies and other repositories by reading their source.

## Install

```bash
claude plugin marketplace add andreimaxim/claude-plugins
claude plugin install plan-and-build@andreimaxim
```

## Sources

The `shaping` skill and its reference files are based on chapters of [37signals' Shape Up](https://basecamp.com/shapeup):

- The plan template follows [Write the Pitch](https://basecamp.com/shapeup/1.5-chapter-06).
- The invoice autopay case study and the backend breadboarding technique follow [Find the Elements](https://basecamp.com/shapeup/1.3-chapter-04).
- The dot calendar case study draws on [Principles of Shaping](https://basecamp.com/shapeup/1.1-chapter-02), [Set Boundaries](https://basecamp.com/shapeup/1.2-chapter-03), and [Find the Elements](https://basecamp.com/shapeup/1.3-chapter-04).

The `planner`, `reviewer`, and `researcher` subagents are adapted from Amp's planning, reviewing, and researching subagent skills.

The `implementing` skill follows the building phase of [Shape Up](https://basecamp.com/shapeup/3.0-chapter-10) and draws on engineering principles from Lauren Tan's [pstack](https://github.com/cursor/plugins/tree/main/pstack). The `proving-changes` skill is adapted from the skill of the same name in [Amp's official plugins](https://github.com/ampcode/official-plugins/blob/main/skills/proving-changes/SKILL.md), and its benchmarking section draws on `pstack`'s [benchmark-checklist](https://github.com/cursor/plugins/blob/main/pstack/skills/benchmark-checklist/SKILL.md), which is based on Brendan Gregg's benchmarking checklist.

## License

MIT, as set in the plugin's `plugin.json`.
