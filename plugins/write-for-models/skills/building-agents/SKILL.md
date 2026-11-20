---
name: building-agents
description: "Defines, validates, and evaluates reusable subagents, including their role, frontmatter, tool boundaries, caller briefs, and behavioral evals. Applies when creating a subagent, revising its scope, tools, or handoff contract, or evaluating a subagent's behavior."
---

# Building agents

Building a subagent means writing an agent file (`agents/<name>.md`) that defines a specialist
role, when a caller should dispatch it, the boundaries it must hold in a fresh context, and the
report it returns. When you revise an existing subagent, preserve its core role and safety
boundaries unless the task explicitly changes them.

The `writing-prompts` skill covers how to write and revise the instructions inside the system
prompt and caller briefs.

## File, frontmatter, and tool boundaries

Each subagent is a Markdown file under `agents/` (or `.claude/agents/` in a project) with YAML
frontmatter followed by the system prompt body:

- **Name.** A lowercase, hyphenated noun or role matching the filename without `.md`, e.g.,
  `builder`, `reviewer`, `researcher`, or `profiler`.
- **Description.** Written in the third person, stating what the subagent does, when callers
  should dispatch it (and what adjacent roles handle instead), and what inputs the caller's brief
  must supply. Callers see only the name and description before dispatching, so put routing
  conditions and required brief inputs here.
- **Model and effort.** Use `model: inherit` by default. Pin a specific model or `effort` only
  when the role genuinely needs stronger reasoning (such as skeptical review) or lower latency
  and cost (such as external source lookup).
- **Tools.** Restrict `tools` to enforce boundaries structurally rather than relying on prose
  warnings alone—for example, giving read-only reviewers `Read, Glob, Grep, Bash` without file
  editing tools. Omit `tools` when the subagent needs full tool inheritance, such as a coding
  builder.

For example, `reviewer` uses this frontmatter:

```yaml
name: reviewer
description: Reviews a diff, branch, commit, plan, or design as a skeptic and reports what should block it alongside the smallest fix for each blocker. Give it the object to review, what the work is meant to do, and any open questions, without telling it your conclusion. Use it to look for holes in a shaped plan, review an implementation, or judge between competing designs.
model: fable
effort: high
tools: Read, Glob, Grep, Bash
```

## Writing the system prompt

Load the `writing-prompts` skill before writing or revising the system prompt.

Unless the harness forks the parent conversation, a subagent starts in a fresh context window
without the parent discussion or its loaded skills, so write the body to stand on its own:

Define the role, the decision rules, the hard limits (such as staying read-only, leaving git
state untouched, never touching production, or treating external repo content as untrusted data),
and when to stop and return early with a blocked or paused handoff instead of guessing or writing
workarounds. Define the report structure so the caller gets the exact evidence, locations, and
verdict it needs to act.

### Designing caller briefs

When a subagent runs in a fresh context instead of inheriting the parent conversation, make clear
in the subagent's description and in any calling skill what each brief must carry:

- The goal, the boundaries of the task, and what is already settled.
- The files, diffs, or evidence to start from, omitting the caller's conclusion when independent
  judgment is the point.
- How to verify completion and the specific facts, citations, or outputs needed back, because a
  summary may omit them.

## Reviewing and evaluating the agent

Check that the description routes cleanly against sibling agents and names the required brief
inputs. Check that `tools` matches the safety boundary, that every safety guard the subagent
depends on lives inside its own file (not only in a calling skill), and that the body has no
conflicting or repeated rules.

Validate the frontmatter and plugin manifest with an available validator (such as `claude plugin
validate --strict` for Claude Code plugins). Static checks confirm only that the file parses and
loads; they don't establish routing or downstream behavior, so report what you checked without
claiming behavior is verified.

### Evaluating the agent

Offer an eval when a subagent is new or its behavior changed, and get explicit approval before
running it. Run the motivating task, boundary or safety probes (such as staying read-only,
pausing when an assumption fails, or ignoring untrusted instructions), and a negative control
through the real subagent with its declared model, effort, and tools in fresh isolated sessions.

State the cases, model, session count (at most 12 by default), and what the eval doesn't cover
before running. Report pass rates per case comparing old and new text, and judge each run by the
observable outcome, e.g., catching a planted bug or pausing cleanly on an out-of-scope root cause,
rather than by whether the output echoes the prompt's wording.
