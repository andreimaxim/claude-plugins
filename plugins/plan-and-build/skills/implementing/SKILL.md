---
name: implementing
description: Implements plans and agreed work by orchestrating emergent scopes, reviewing the design of each scope for friction, and verifying the integrated result. Use for substantial implementation or carrying out an agreed plan; not routine edits.
---

# Implementing

Implementing turns an agreed plan into working, reviewed code, following the building phase of
Shape Up. The work is built in scopes, which are small pieces that each work end to end, and the
scopes come out of the work instead of a task list made up front. You own the scope map and judge
the design after each scope, while the `builder` subagent writes the code and tests for one scope
at a time.

## Orient and choose scopes

Start from the plan or brief the user agreed to, and note the outcome, the appetite, the stories,
the no-gos, and any open decisions. Follow the entry point to the code that owns the rules, and
check the plan's assumptions against the code, contracts, or a quick experiment. Report anything
that could invalidate the plan or exceed the appetite before you build on it. If the problem itself
needs rethinking, go back to `shaping`. When the plan depends on how a library, framework, or
external service behaves, ask the `researcher` subagent instead of inferring it from local code.

Start with a scope that builds a small path through the middle of the system end to end and
resolves an important unknown. Use the plan's stories as starting points. A scope usually covers
one story or a few related ones, and each anti-story becomes a test. Draw each scope around an
outcome you can integrate and verify on its own, not around layers or files.

Give the `builder` subagent one scope at a time. Pass it the scope's outcome, its stories and
agreed examples, the anti-stories to cover with tests, the constraints and interfaces settled by
earlier scopes, the checks to meet, and the files to start from, without prescribing a step-by-step
task list.

Expect scopes to split, merge, and appear as you learn from each build. Track each scope as
unknown, understood, or verified. Understood means you can see how to finish it, and verified means
the integrated behavior and design hold up. New evidence can move a scope back, and effort spent
doesn't count as progress. Tell the user when the map changes in a way that matters.

Keep choosing scopes until the work meets the agreed outcome, unless the user asked you to stop
after one. The work is ready when it meets the outcome, not when every possible improvement is
done.

## Spot and fix design friction

A passing test suite doesn't establish that the design is right. After each `builder` run, inspect
the diff, the callers, and the tests yourself—checking the report's claims against them—before
repeating the pattern in later scopes. Judge simplicity across the operation and its callers, not
by code size or object count. Treat the patterns below as signals of design friction, not
automatic rewrites. When the code and callers confirm avoidable complexity, have `builder`
refactor it in a separate pass that keeps behavior the same before you brief the next scope; when
an external contract or framework gap justifies the shape, keep it and note why. Ask the
`reviewer` subagent when a hard design or technical question remains open.

When `builder` pauses because an assumption failed or a fix reaches outside the scope, inspect the
cause and update the scope map if fixing the source component fits the appetite. You can change how
the plan organizes the code, but not the agreed outcome. Bring any consequential departure from the
plan or appetite to the user before making it. Pause only the affected work, and present the
evidence with a recommendation. When something has to give, cut flexible breadth or generality
first, and never cut required behavior, quality, security, or data integrity.

### Special cases and flag arguments

The diff adds a boolean parameter, option flag, conditional branch, or near-duplicate sibling
method. Replace flag arguments with intent-revealing methods, growing conditionals with
polymorphism, a lookup map, or a Special Case object, or widen the domain model. Update internal
callers and delete the old path together.

### Reinventing the framework or standard library

The diff adds custom non-domain plumbing (routing, serialization, validation, pagination, retries,
collection helpers) or fights the framework's lifecycle. Check what the language, standard library,
framework, or installed dependencies provide in this version—asking `researcher` when needed—and
use the built-in facility when it covers the need.

### Leaky abstractions and pass-through wrappers

One change touches several callers, callers coordinate multi-step sequences or dig through chained
objects (Law of Demeter / Feature Envy), or the diff adds single-use pass-through wrappers. Deepen
the owning module behind one operation, and inline wrappers that hide nothing. When callers work
around another component, trace the cause to its source: fix an avoidable implementation choice
within the appetite, or contain the workaround at the owning boundary and state any remaining limit
explicitly.

### Duplicated state and anemic objects

The diff stores a boolean or status field implied by existing state, repeats valid-value lists
across files, or mutates an object's attributes from outside instead of telling it what to do
(Tell, Don't Ask / Anemic Domain Model). Derive computed state unless it must be stored or indexed,
keep a single source of truth for valid values, and move state transitions onto the owning domain
or value object.

### Redundant checks, speculative generality, and explanatory comments

Internal methods re-check invariants callers already guarantee, catch exceptions only to log and
re-raise, add single-use options (YAGNI), or narrate the code in comments. Parse, don't validate at
the system boundary so internal code trusts its types; inline speculative hooks; and replace
explanatory comments with clearer names or tests.

### Tests coupled to implementation details

A test mocks internal collaborators, builds heavy fixture graphs, or asserts internal call
sequences so refactoring breaks it without changing behavior. Test observable outcomes—return
values, persisted state, or outgoing boundary calls—through the public interface with real
collaborators. When heavy setup persists because logic is tangled with I/O, extract a pure seam
rather than adding more mocks.

## Review and finish

Once all scopes are verified, run the repository's checks on the complete result and exercise the
changed behavior as close to the user or caller's entry point as the environment allows. Then have
the `reviewer` subagent review the integrated result in a fresh context. Give it the outcome, the
stories, the constraints, and the diff or branch to review, without the implementation
conversation, and ask it to check:

- Correctness, and whether the change follows the repository's guidance.
- Fit with the surrounding design, and any complexity the change doesn't need.
- Whether the tests would fail if the required behavior broke, rather than mirroring the
  implementation.

The review must cover the complete integrated result before you report the work as ready, and your
own review doesn't replace it. If the review couldn't run, say so instead of calling the work
reviewed. Weigh each finding instead of accepting it automatically, and carry supported fixes
through the code, the checks, and a follow-up review without waiting for the user to ask.

When the user asks for end-to-end proof, or when unit and integration checks aren't enough to show
that the change works and breaks nothing, use `proving-changes` after the review's fixes are in. If
the proof leads to a fix, ask the reviewer to review that fix.

When you report, go through the plan's stories and anti-stories. Mark each story as `New`,
`Changed`, or `As before`, and say how you verified it. Name the test that covers each anti-story,
and list any remaining limits or blockers. Being ready doesn't authorize pushing, deploying,
releasing, or writing to production without explicit approval.
