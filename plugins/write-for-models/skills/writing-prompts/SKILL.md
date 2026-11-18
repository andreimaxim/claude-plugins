---
name: writing-prompts
description: "Writes and revises instructions for an agent, including skill bodies, subagent system prompts, and subagent briefs. Applies when drafting, editing, or reviewing these instructions."
---

# Writing model instructions

A model instruction is the text an agent reads to decide how to behave in a skill body, a subagent
system prompt, or a brief handed to a subagent. The model reads these instructions together with
its task, and its attention is limited, so every line competes with every other line. Text that
doesn't change behavior still uses some of that attention.

An instruction is judged by the behavior it produces. A good instruction produces the intended
behavior reliably across all the situations it covers, and it uses only the text that result needs.
When clarity for the model conflicts with another writing choice, choose clarity.

## Skills, subagent system prompts, and briefs

What an instruction needs depends on where it runs and what context the receiving agent already has.

### Skill bodies

A skill loads into an active conversation when the task matches its description (see the
`building-skills` skill for how skill folders, frontmatter, and reference files are organized).
Because the agent already has the conversation and its main system prompt, the skill body only
needs to define the capability, the intended result, and the constraints for that kind of work,
without restating general system-prompt rules.

### Subagent system prompts

A subagent starts in a fresh context window and doesn't inherit the main conversation or its loaded
skills. Its system prompt (`agents/*.md`) sets the role, the rules for making decisions, and the
limits (such as staying read-only, not creating commits, or when to stop and return early), along
with the structure of the report it returns to the caller.

### Subagent briefs

When you invoke a subagent, it hasn't seen the conversation that led to the call. Include in the
brief:

- The goal, the boundaries of the task, and what is already settled.
- The files or evidence to start from, without stating your conclusion when you want an independent
  judgment.
- How to verify completion, and the specific facts, citations, or outputs you need back, because a
  summary may omit them.

## Writing the instruction

Put the sections in the order the model needs them, grouping related rules under one heading and
putting text that stays the same before text that changes with each request:

- What the capability or rule is, and when it applies.
- The result to produce and the constraints it must meet.
- How to verify the result, and what good looks like.
- Examples, when they clarify an important distinction.

### Describe the result instead of the steps

Define the result you want, the constraints it has to meet, and what success looks like. State the
behavior you want, and add what to avoid only when the model actually does it. Prescribe an order
of steps only when the order is needed for correctness or safety, because the model's own plan is
usually better than a plan written by hand. A section can offer several approaches without making
them a sequence, as in this example:

> Use the approaches below as needed. They aren't a required sequence or separate skills to load.

### Give the agent context, and trust what it already knows

Name a practice instead of spelling out its steps, e.g., "prepare a handoff" or "red-team the
feature", and don't teach concepts the model already knows. Supply only what the model cannot work
out on its own:

- The scope and the local context for the task.
- The constraints the result must respect.
- A short reason, when it helps the model apply a rule to cases the text doesn't name.

### Keep the instruction simple, even when that takes more work to write

Keep separate concerns apart, and avoid complexity that the task doesn't need. For example, keep
what the task is for separate from the details of the tools used to do it. State each rule in one
place without contradictions, and clarify or remove existing text before adding more. Spend more
thought while writing the instruction rather than adding complexity to the instruction itself. Keep
the intended result and the constraints it depends on, and don't aim for either complete coverage
or the fewest words.

### Use existing tools for doing and checking the work

Find the tools that are already available, and build on them to do the work and check it. Where you
can, get feedback from automated checks such as linters and test runners, rather than relying on
the agent to inspect its own work. Don't restate a tool's rules in prose, and don't build custom
enforcement for a preference that depends on context. When the instruction mentions a script or a
tool, state its purpose, and explain the inputs or outputs the agent needs to understand.

### Make mistakes hard to make

Prefer an arrangement that prevents a likely mistake, or makes it obvious right away, over a warning
the agent has to remember. Use the simplest safeguard that works for that kind of failure.

### Write a rule for the general behavior, not for one incident

An instruction covers a class of situations. When a bug or a conversation prompts a change, find
the general rule that would have prevented it and write that rule, instead of describing the
specific symptom. To check the rule, imagine the incident never happened, and ask whether the rule
would still read as obvious and useful. A rule written for one incident also blocks legitimate
work, because it fires on cases it was never meant for.

### Match the strength of the wording to the rule

Words like "prefer", "usually", "consider", and "when appropriate" make an instruction optional, so
use them only when the behavior is meant to be optional. Keep MUST, NEVER, and ALWAYS for rules
that can't be broken and for behavior the model has been shown to get wrong. Current models follow
instructions literally and react strongly to emphasis, so capitalized warnings and phrases like "if
in doubt, do X" make a model apply an instruction more broadly than intended. When a behavior shows
up too often, remove the emphasis instead of adding a rule against it.

## Reviewing the instruction

Start from behavior you've seen across cases (one case is an anecdote, two cases are a signal, and
three cases are a pattern), and write down the cases before you change any instruction text. Read
the existing instructions that cover the situation, look for text you can remove or clarify first,
and write the smallest change that produces the intended behavior across those cases. Check that
the change is clear, covers a general behavior, states each rule in one place without
contradictions, and uses the strength you intend.

Offer an eval, and don't run it without explicit approval. State in the eval plan:

- The case that prompted the change, and one negative control that the change must not affect.
- The model and the number of isolated sessions to run (at most 12 by default).
- What the eval doesn't cover.

Run the eval through the actual agent with its actual system prompt and tools, in fresh isolated
sessions, and report the pass rate for each case with the old and the new text. "Reads better"
isn't a result.
