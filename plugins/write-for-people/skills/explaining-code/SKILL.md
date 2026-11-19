---
name: explaining-code
description: Explains how a codebase or subsystem works, why it is structured that way, and what a change or pull request does. Use for architecture and code walkthroughs, design history questions, or writing and explaining pull request descriptions, diffs, and commits.
---

# Explaining code

Explaining code gives a reader a clear mental model of how a codebase or subsystem works, why it
was designed that way, or what a change does.

## What the explanation should deliver

For a codebase or subsystem, leave the reader with a clear picture of what the subsystem is
responsible for, which objects or modules own its state and rules, how a request, job, or state
transition moves through it from start to finish, and the reasons behind non-obvious boundaries,
invariants, and trade-offs.

For a change, branch, commit, or pull request, explain what `git diff` alone doesn't show,
organized around behavior and decisions rather than walking through edited files alphabetically.
Make clear the problem or goal that prompted the work, how behavior differs before and after, how
the changed parts fit into the execution path, and the design choices, trade-offs, edge cases, and
operational risks a reviewer needs to judge.

Use the `plain-writing` skill to write the prose, organizing the explanation as narrative
paragraphs with headings that state each section's takeaway rather than generic template labels or
long bullet inventories. Place focused code or `diff` excerpts beside the prose for the decisive
logic, queries, data shapes, or before-and-after changes.

Show multi-component flows, state transitions, and asynchronous steps with a diagram or execution
trace using the tools of the environment where the explanation will be read:

- **GitHub pull requests, issues, and repository Markdown files:** Use Mermaid diagrams for
  flowcharts, sequence diagrams, and state transitions.
- **App sessions with native visual or diagram tools:** Use the harness's visual or artifact tools
  to render flowcharts, sequence diagrams, and state transitions, embedding them in the reply or
  document.
- **CLI terminals with plain-text output only:** Use indented call traces, state flows, or Unicode
  box drawings in a `text` code block.

## Constraints and verification

Write impersonally about the system and the change, without first-person narration of what you
checked or didn't run. Base every claim about behavior on the code, contracts, and tests, and base
every claim about why the code was written that way on the commits, pull requests, or tickets that
introduced it rather than a later rename or refactor. When the historical record doesn't state why
a choice was made, describe what the code does without speculating about the author's intent.

Explain the system or change as it is. When the user also asks for an evaluation or critique, keep
the factual walkthrough separate from your recommendations.
