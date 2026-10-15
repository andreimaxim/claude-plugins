# plan-and-build

The plan-and-build plugin helps Claude Code plan a change before building it, build it in small verified steps, and prepare the result for review. It adds three skills and two subagents. Claude loads a skill when a request matches its description, and you can also invoke one directly as `/plan-and-build:<skill>`.

## Skills

- `shaping` turns a raw idea, such as a feature request or a hard bug report, into a bounded plan. You agree on the problem, the outcome, and how much change you're willing to take on. The plan then describes the main parts of the solution, the stories it has to support and the ones it must prevent, the risks, and what is left out. The skill follows the shaping phase of [37signals' Shape Up](https://basecamp.com/shapeup/1.1-chapter-02), and its reference files retell two case studies from the book.
- `implementing` builds an agreed plan in small changes that each work end to end, starting from the plan's stories, and it checks each change as it goes. The Oracle subagent reviews the finished result, and the final report goes through each story and says how it was verified.
- `organizing-commits` reorganizes a branch's committed and uncommitted changes into commits that are easy to review, and each commit has to build and pass the repository's checks on its own. It also loads when you ask Claude to restack a branch.

## Subagents

- `plan-and-build:oracle` gives a second opinion on hard problems, such as a design or plan before you commit to it, a bug that survives the first explanation, or finished work before you call it done. It runs on the `fable` model at extra-high effort, and it can use only Read, Glob, and Grep. `shaping` asks it to check a plan before the plan is presented as solved, and `implementing` asks it for the independent review. Inspired by [Amp's Oracle](https://ampcode.com/docs/tools#oracle).
- `plan-and-build:librarian` researches code outside the working tree, including dependencies at their installed version, other repositories, and commit history, and it links to its sources. It runs on the `sonnet` model at medium effort, and it can use Read, Glob, Grep, Bash, WebSearch, and WebFetch. Inspired by [Amp's Librarian](https://ampcode.com/docs/tools#librarian).

Oracle needs access to the Fable model.

## What the plugin runs and fetches

The plugin contains only instructions. It has no hooks, MCP servers, or scripts, and it doesn't change your permission settings. Every command below runs through Claude Code's normal tools and permission prompts.

- Oracle can only read and search files with Read, Glob, and Grep. It has no shell and no tools that change files.
- Librarian reads public documentation and source code with WebSearch and WebFetch, and it reads repositories with read-only `git` and `gh` commands, using any access you already have. When remote reads aren't enough, it clones a repository into a new temporary directory to read it, and then deletes the clone. It doesn't run the code it researches or change your checkout.
- `organizing-commits` rewrites the current branch's commit history by reordering, splitting, squashing, and rewording commits. Before it starts, it saves the starting state, including uncommitted changes, under a backup ref. Afterward, it confirms that the final code matches that state, and it runs the repository's checks on each new commit. It never pushes.
- `implementing` edits code and runs the repository's checks, like any implementation work in Claude Code.

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

## License

MIT, as set in the plugin's `plugin.json`.
