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
earlier scopes, and the files to start from, without prescribing a step-by-step task list.

Expect scopes to split, merge, and appear as you learn from each build. Track each scope as
unknown, understood, or verified. Understood means you can see how to finish it, and verified means
the integrated behavior and design hold up. New evidence can move a scope back, and effort spent
doesn't count as progress. Tell the user when the map changes in a way that matters.

Keep choosing scopes until the work meets the agreed outcome, unless the user asked you to stop
after one. The work is ready when it meets the outcome, not when every possible improvement is
done.

## Spot and fix design friction

A passing test suite doesn't establish that the design is right. After each `builder` run, inspect
the diff, the callers, and the tests yourself before repeating the pattern in later scopes. Judge
simplicity across the operation and its callers, not by code size or object count. When you find
any of the patterns below, have `builder` refactor the design in a separate pass that keeps
behavior the same before you brief the next scope. Ask the `reviewer` subagent when a hard design
or technical question remains open.

When `builder` pauses because an assumption failed or a fix reaches outside the scope, inspect the
cause and update the scope map if fixing the source component fits the appetite. You can change how
the plan organizes the code, but not the agreed outcome. Bring any consequential departure from the
plan or appetite to the user before making it. Pause only the affected work, and present the
evidence with a recommendation. When something has to give, cut flexible breadth or generality
first, and never cut required behavior, quality, security, or data integrity.

### Special cases and flag arguments

The diff adds a boolean parameter, an option flag, a conditional branch, or a nearly identical
sibling method for the new case, or extends an existing chain of branches in the file. Instead of
branching on flags or types, split a method that takes a flag into explicit methods for each caller
intent. Replace growing conditionals with polymorphism, a lookup map, or a special case object, or
widen the domain model so the cases work the same way. When a change replaces an internal method or
object and its callers can change together, update the callers and delete the old path in the same
scope.

### Reinventing the framework or standard library

The diff adds custom plumbing for a general concern outside the application's domain, such as
routing, serialization, validation, pagination, retries, or collection manipulation, or works
around the framework's lifecycle. Check how the language, standard library, framework, or an
installed dependency handles the concern in the version the project uses, asking `researcher` when
needed, and replace the custom code with the built-in mechanism.

### Leaky abstractions and pass-through wrappers

A single behavior change touches several callers, or callers have to invoke methods in a specific
order or dig through chained objects to compute an argument. Or the diff adds helpers with a single
caller and wrapper classes that only forward arguments. Move the multi-step sequence or calculation
into a single method on the object that holds the data, and inline wrappers or helpers that don't
hide complexity from callers. When callers work around another component's behavior, trace the
requirement to its source to see whether it comes from a domain rule or external constraint or from
an avoidable implementation choice. Fix the source component when the cause is an implementation
choice within the appetite, or contain the workaround inside the component that owns the operation
and make any remaining limitation explicit to callers.

### Duplicated state and anemic objects

The diff adds a boolean or status field alongside existing timestamps or state that already implies
it, or repeats a list of valid values across files. Or a service or caller reads an object's
attributes, checks conditions, and mutates them step by step. Replace derived flags with a query
method or computed property on the existing state, and derive checks for valid values from one
constant or enum. Move state transition rules onto the domain object or a value object so callers
invoke a named transition instead of setting fields directly.

### Redundant checks, speculative generality, and explanatory comments

Internal methods check for `nil`, empty values, or types that callers already guarantee, or catch
exceptions only to log or re-raise them. Or the diff adds configuration options or parameters used
by a single caller, or comments that narrate the code. Parse and validate untrusted input once
where it enters the system so internal code can rely on the resulting types and values without
defensive guards. Inline single-use parameters and hooks that the agreed stories don't need, and
replace explanatory comments by renaming the method or variable or encoding the rule in a test.

### Tests coupled to implementation details

A test stubs or mocks internal collaborators, or builds a large graph of unrelated objects just to
run. Or it asserts that internal methods were called or how the code used to behave, or breaks when
internals are refactored without changing behavior. Rewrite the test to call the public interface
with real collaborators and assert the returned value, persisted state, or outgoing boundary call.
When testing a rule still requires heavy setup or mocks because calculation is tangled with I/O or
external dependencies, separate the domain logic from the I/O or narrow the interface rather than
adding more mocks.

## Review and finish

Once all scopes are verified, run the repository's checks on the complete result and exercise the
changed behavior as close to the user or caller's entry point as the environment allows. Then have
the `reviewer` subagent review the integrated result in a fresh context. Give it the outcome, the
stories, the constraints, and the diff or branch to review, without the implementation
conversation, and ask it to check:

- Correctness, and whether the change follows the repository's guidance.
- Fit with the surrounding design, and any complexity the change doesn't need.
- Whether the tests prove the required behavior.

The review must cover the complete integrated result before you report the work as ready, and your
own review doesn't replace it. Weigh each finding instead of accepting it automatically, and carry
supported fixes through the code, the checks, and a follow-up review without waiting for the user
to ask.

When the user asks for end-to-end proof, or when unit and integration checks aren't enough to show
that the change works and breaks nothing, use `proving-changes` after the review's fixes are in. If
the proof leads to a fix, ask the reviewer to review that fix.

When you report, go through the plan's stories and anti-stories. Mark each story as `New`,
`Changed`, or `As before`, and say how you verified it. Name the test that covers each anti-story,
and list any remaining limits or blockers. Being ready doesn't authorize pushing, deploying,
releasing, or writing to production without explicit approval.
