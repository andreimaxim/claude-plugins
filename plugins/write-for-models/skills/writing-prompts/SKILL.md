---
name: writing-prompts
description: "Writes and revises model instructions, including skill bodies and descriptions, system prompts, repository guidance, tool descriptions, and subagent briefs. Applies when drafting, editing, or reviewing these instructions."
---

# Writing model instructions

## What a model instruction is

A model instruction is any text a model reads to decide how to behave, e.g., a system prompt, a
tool's name and description, a parameter description, the text a tool returns, a CLAUDE.md or
AGENTS.md file, a skill's description or body, or a prompt template. The model reads all of these
together with the conversation, and its attention is limited, so every line competes with every
other line. Text that doesn't change behavior still uses some of that attention.

An instruction is judged by the behavior it produces. A good instruction produces the intended
behavior reliably across all the situations it covers, and it uses only the text that result needs.
When clarity for the model conflicts with another writing choice, choose clarity.

When the `plain-writing` skill is available, apply it to the prose of an instruction without
changing the behavior it asks for, its scope, or how strongly it asks.

## Principles

### Be clear and direct

Name the situation, the behavior, and the limits of the behavior, and define each term before you
use it. Use concrete nouns and verbs, and keep each sentence to one rule. To check the text, imagine
a new teammate with no context reading it. If they would need to ask a question to follow it, the
model will be confused too.

Unclear: "Handle git operations carefully."
Clear: "Ask before any git command that rewrites published history, such as push --force or
amending a pushed commit. Run local commands that can be undone without asking."

### Remove or clarify text before you add more

Before you add an instruction to fix a problem, read the text that already covers the situation,
and look for something to remove. For example, look for these:

- a rule the model already follows without being told
- two rules that say the same thing
- a special case that an existing general rule already covers
- a line that the model ignores, as its behavior shows

Remove the text if the removal doesn't change behavior that the instructions have to keep. Often
the fix is to clarify or move an existing line instead of writing a new one.

Adding: The model sometimes skips reading a config file, so a new bullet says to read config files
first.
Removing first: An existing bullet already says to read the file that defines a setting before
answering. The model skips it because a later bullet about answering quickly contradicts it, so
you delete the later bullet.

### Write a rule for the general behavior, not for one incident

An instruction covers a class of situations. When a bug or a conversation prompts a change, find
the general rule that would have prevented it and write that rule, instead of describing the
specific symptom. To check the rule, imagine the incident never happened, and ask whether the rule
would still read as obvious and useful.

Too specific: "When the user asks about the Stripe webhook retry setting, read config/stripe.ts
before answering."
General: "Before answering a question about configuration, read the file that defines it instead
of answering from memory."

A rule written for one incident also blocks legitimate work, because it fires on cases it was
never meant for. Give the model the rule for deciding, and let the model apply it.

### Say what to do

State the behavior you want. Add what to avoid only when the model actually does it, because a
prohibition on its own leaves the model to guess what to do instead.

Prohibition only: "Do not use markdown."
Behavior: "Write the response as plain paragraphs. Use inline code for identifiers."

### Give the reason when it helps the model apply the rule to new cases

A short reason lets the model handle cases the rule doesn't name. Cut a reason that doesn't change
behavior.

Rule only: "Never use ellipses."
With reason: "A text-to-speech engine reads the response aloud, and it can't pronounce ellipses,
so never use them."

### Match the strength of the wording to the rule

Words like "prefer", "usually", "consider", and "when appropriate" make an instruction optional, so
use them only when the behavior is meant to be optional. Keep MUST, NEVER, and ALWAYS for rules
that can't be broken and for behavior the model has been shown to get wrong. Current models follow
instructions literally and react strongly to emphasis, so capitalized warnings and phrases like "if
in doubt, do X" make a model apply an instruction more broadly than intended. When a behavior shows
up too often, remove the emphasis instead of adding a rule against it.

Too strong: "CRITICAL: If in doubt, load the skill."
Matched: "Load the skill when the task matches its description."

### Keep each instruction in one place, without contradictions

Each rule should be stated in exactly one place. Before you add text, read the instructions around
it, and clarify, merge, or replace an existing rule instead of adding another one. When two rules
pull in different directions, the model spends reasoning on every turn trying to reconcile them,
and you can't predict the result. Repeat an instruction only when an eval shows that the model
ignores it when it's stated once.

### Use examples on purpose

One concrete example can replace paragraphs of abstract instruction, and examples are the most
reliable way to steer format, tone, and structure. Make each example match the actual use, vary the
examples so the model doesn't copy an accidental pattern, and mark them clearly as examples. Cut an
example that doesn't measurably help.

### Order the text for the model

Put the sections in the order the model needs them. First, say what the thing is and when to act.
Second, say how to act and what to set in each case. Third, give examples. Group related rules under
one heading, and use headings or tags with consistent names so the model can find and refer to a
section. Put text that stays the same before text that changes with each request.

## Where an instruction belongs

Each kind of file holds one kind of instruction. An instruction in the wrong file either uses
attention on every turn or is missing when the model needs it.

- **System prompt.** General behavior and rules that apply to every task in the session go here.
  The system prompt is always in context, so it's the most expensive place to add text.
- **Skill.** A capability that the model needs only sometimes, and gets wrong without help, goes in
  a skill, e.g., a tool with unusual rules, a workflow with several steps, or a domain format. A
  skill loads only when the task matches it, so it has room for detail and examples.
- **Project CLAUDE.md.** Facts and rules for one repository or one directory go here, when the
  model can't work them out from the code and they don't apply anywhere else.
- **Tool description.** When to call the tool, how to call it, and what it returns go here. The
  broader task doesn't belong in a tool description.

To place an instruction, ask two questions. First, does it apply to every task in the session? If
it doesn't, it isn't a system prompt rule. Second, does it apply outside this repository? If it
doesn't, it belongs in the project CLAUDE.md. Anything left is either part of one tool's contract
or a capability needed only sometimes, which makes it a skill.

Misplaced: The system prompt has a paragraph on how to write RRULE strings for schedules. Most
turns never schedule anything, and the model gets the syntax wrong without examples, so the
paragraph belongs in a skill.
Misplaced: A project CLAUDE.md bullet repeats "ask before destructive git commands", and the system
prompt already gives that rule. Check the loaded instructions before you cut the bullet.

## By kind of instruction

### System prompts

- A system prompt sets the role, the rules for making decisions, and the limits. It isn't a recipe
  for a task or a reference manual.
- Write general instructions instead of step-by-step lists, because the model's own plan is
  usually better than a plan written by hand.
- Each line should change useful behavior on most tasks. Cut background, motivation that doesn't
  apply to new cases, and restated default behavior, and move detail that's needed only sometimes
  into a skill.
- Name a tool only when the session has it, and make instructions about a tool depend on the tool
  being available. Replacing Claude Code's system prompt changes the instructions, but it doesn't
  change which tools the session has.
- Put instructions that stay the same first and facts for each request last, so the part that stays
  the same can be cached.

### Skills

- The `building-skills` skill covers what a skill does, when it loads, how `SKILL.md` is organized,
  and which details go in reference files.
- The description says what the skill does and when to use it, in the third person, and it uses the
  terms a model would match on. The model sees only the description before it loads the skill, so
  the description decides whether the skill loads at all.
- The body teaches the whole capability. It says what the concept is and when to act, and then how
  to act, what to set in each case, and examples. Define every term before you first use it.
- A skill body can be long, but it shouldn't restate rules from the system prompt.

### CLAUDE.md and other guidance files

- Record what the model can't work out from the code, e.g., commands, invariants, rules for
  decisions, environment steps that aren't obvious, and product constraints. Remove general
  engineering advice.
- Write bullets instead of paragraphs. State what to do, and state why only when the reason changes
  the decision.
- Prefer rules that will stay true over details of the current implementation.
- Keep one topic per file, and link to the source of truth instead of copying a procedure.
- Put a rule for one directory in that directory's guidance file.
- Claude Code loads CLAUDE.md files on its own, but don't assume it loads AGENTS.md. When you adapt
  existing guidance, keep the project's instructions that import or read other files.

### Tool descriptions

- The first sentence says what the tool does and when to use it. Write it as you'd explain the tool
  to a new teammate, and spell out context that would otherwise be implied, e.g., the query format,
  unusual terms, and how the tool relates to similar tools.
- Add "Do not use for X" only for wrong uses that actually happen, and name the tool to use
  instead.
- Give parameters names that can't be misread, e.g., `user_id` instead of `user`. Describe each
  parameter's format with one example value.

## Process

1. Start from behavior you've seen. One case is an anecdote, two cases are a signal, and three
   cases are a pattern. Write down the cases before you write any instruction text.
2. Read the existing instructions that cover the situation. First look for text you can remove or
   clarify without changing behavior the instructions have to keep. Only then decide whether
   anything needs to be added, and where it belongs.
3. Write the smallest change that produces the behavior in the cases you wrote down.
4. Review the change against the principles above. Check that the change is clear, covers a general
   behavior, says what to do, and asks with the strength you intend. Also check that each
   instruction appears in one place, that the examples help, and that the text is in the order the
   model needs. Finally, check whether removing existing text would have solved the problem without
   adding anything.
5. Offer an eval, and don't run it without explicit approval. The plan names the case that
   prompted the change and one negative control, which is a case the change must not affect. It
   also names the model, the number of sessions (at most 12 by default), and what the eval doesn't
   cover. Run the eval through the actual agent with its actual system prompt and tools, in fresh
   isolated sessions, and report the pass rate for each case with the old and the new text. "Reads
   better" isn't a result.

## Examples

Tool description, before:

    Search tool. Use this to find things in the codebase. Very powerful. ALWAYS use it before
    editing.

After:

    Searches file contents with a regular expression and returns the matching lines with their
    paths and line numbers. Use it to find exact strings or symbols. For questions about behavior
    or flows that span several files, use Agent with a code search agent, such as Explore.
    Parameter `pattern`: a regular expression, e.g., `handleAuth\(`.

CLAUDE.md entry, before:

    We had an incident where the agent ran the test suite without the timeout and it looked like
    it hung, so please remember that tests can take a long time in this repo and be patient.

After:

    - Run `pnpm test` as a background Bash task, because the suite takes about 3 minutes. Read its
      output file to check progress and results instead of running it again.

System prompt rule, before:

    NEVER ask the user clarifying questions. Always make assumptions and proceed.

After:

    For small, well-defined tasks, pick sensible defaults and go ahead, and say which defaults you
    chose. Ask before you act only when a wrong guess would be expensive to undo, such as a schema
    migration or a destructive command.
