---
name: implementing
description: Implements plans and agreed work through emergent scopes, architectural refinement, and independent verification. Use for substantial implementation or carrying out an agreed plan; not routine edits.
---

# Implementing

Implementing turns an agreed plan into working, reviewed code, following the building phase of
Shape Up. The work is built in scopes, which are small pieces that each work end to end, and the
scopes come out of the work instead of a task list made up front.

## Orient

Start from the plan or brief the user agreed to, and note the outcome, the stories, the boundaries,
and any open decisions. Then follow the entry point to the code that owns the rules, and use the
repository's guidance and similar flows to tell the intended design apart from accidental legacy
behavior.

Check the plan's assumptions against the code, contracts, or a quick experiment, and report
anything that could invalidate the plan before you build on it. If the problem itself needs
rethinking, go back to `shaping`. When the plan depends on how a library, a framework, or another
service behaves, ask the researcher subagent instead of inferring it from local code. Orienting
isn't an audit or an approval step, so start building once you can choose a first scope.

## Choose scopes

Start with a small path through the middle of the system that works end to end and resolves an
important unknown. Use the plan's stories as starting points. A scope usually covers one story or a
few related ones, and each anti-story should end up as a test.

Ask the planner subagent to map the first scope onto the code. Give it the plan as background, the
scope to plan, and what is already settled, such as the outcome, the stories, and the appetite, and
tell it not to plan later scopes. Ask it again when a later scope reaches code you haven't explored.
Its steps are a starting point, and building the scope can change them.

Draw each scope around an outcome you can integrate and verify on its own, not around layers,
files, or people. Expect scopes to split, merge, and appear as you build, because the first map
isn't a complete task list.

Track each scope as unknown, understood, or verified. Understood means you can see how to finish
it, and verified means the integrated behavior works. New evidence can move a scope back, and
effort spent doesn't count as progress. Tell the user when the map changes in a way that matters.

## Build

Before you write the internals of a class, module, or function, start from how callers will use
it, including what objects exist, what messages callers send them, and what comes back. Give each
object one clear responsibility and a small public interface that completes the operation, instead
of making callers coordinate internal steps or change its state directly. Put domain rules in the
objects and data structures that own them, rather than scattering boolean flags and `if` or `case`
branches across callers, or splitting one concept across procedural steps such as load, validate,
transform, and save.

Prefer small objects that play a clear role and collaborate through messages over deep inheritance
trees. Use inheritance only when a subclass can stand in for its parent everywhere without callers
checking its class or overriding parent behavior in incompatible ways. Validate and normalize
untrusted input where it enters the system, such as controller parameters, CLI arguments, webhook
payloads, or external API responses, so objects inside the system can trust the values and
collaborators handed to them instead of repeating nil and format checks everywhere.

Build the smallest end-to-end path that produces the scope's visible result, for today's
requirements rather than imagined ones. For each behavior change, write a test or check that fails
on the old behavior and passes on the new one, and run focused checks as you go. Send the same
messages a caller sends through the public interface, and compare the returned value, the state
change, or the arguments of an outgoing command to a concrete expected result. Don't keep a test
that only checks whether a value is present or whether a method was called without checking its
arguments, or that computes the expected value with the code under test. If the behavior is hard to
test, look at the responsibility and dependency boundaries before you reach for elaborate mocks.

As soon as the flow is concrete enough to judge, step back and look at the new code and its
surroundings as one design, before you repeat the design across the rest of the work. Integrate a
new requirement as if the design had always included it, instead of adding a special case to the
existing flow. Assume the next person or agent who edits the code will read only the file they
open, copy the nearest example, and take the shortest path that works:

- Give each piece of state and each business rule one owner, so callers send a message to the
  owner instead of updating the same state or repeating the same rule in several places.
- Keep internal methods, helpers, and collaborators private to the object or module that owns
  them, and inline wrappers that only forward arguments without changing the abstraction.
- When a scope replaces an internal method or object and all its callers can change together
  without a compatibility requirement, update the callers and delete the old path in the same
  scope so nobody copies it later.
- Keep each list of valid states or options in one place, and derive other uses from it or check
  them in a test, so a change in one file can't silently miss another file.

You can change how the plan organizes the code, but not the agreed outcome.

Refactor in small steps that keep behavior the same, with checks in place for the behavior you're
preserving. Keep refactoring separate from behavior changes, and leave unrelated cleanup alone.
Check why an existing guard, branch, or comment was added before you delete it, so you don't remove
intentional behavior. Remove comments that only narrate the code, and keep the ones that explain a
reason or an external constraint. When a comment warns callers about a rule they have to remember,
enforce the rule in the object's interface, a test, or a check if you can.

When two attempts that share an assumption fail the same check, or when the same workaround
appears in several places, stop writing variations of the fix. Write down the assumption the
attempts relied on, and check whether it holds in the running system or in the code that produces
the input. Then fix the cause instead of adding code that compensates for its effect, and ask the
reviewer subagent when a hard technical question remains open.

Bring any consequential departure from the plan to the developer before you make it. Pause only the
affected work, and present the evidence with a recommendation. When something has to give, cut
flexible breadth or generality first, and never cut required behavior, quality, security, or data
integrity.

## Review and finish

Verify the integrated result with the repository's own checks, and exercise the changed behavior as
close to the user or caller's entry point as the environment allows. Then have the reviewer
subagent review the result in a fresh context. Give it the outcome, the stories, the constraints,
and the diff or branch to review, but not the implementation conversation. Don't tell it that the
change works or what you expect it to find. Ask it to check the following:

- Correctness, and whether the change follows the repository's guidance.
- Fit with the surrounding design, and any complexity the change doesn't need.
- Whether the tests prove the required behavior.

The review has to cover the complete integrated result before you report the work as ready, and
your own review doesn't replace it. Weigh each finding instead of accepting it automatically, and
carry the supported fixes through the code, the checks, and a follow-up review without waiting for
the user to ask.

When the user asks for end-to-end proof, or when unit and integration checks aren't enough to show
that the change works and breaks nothing, use `proving-changes` after the review's fixes are in, so
the proof covers the final code. If the proof leads to a fix, ask the reviewer to review that fix.

Keep choosing scopes until the work meets the agreed outcome, unless the user asked you to stop
after one. The work is ready when it meets the outcome, not when every possible improvement is
done.

When you report, go through the plan's stories. Say which ones work and how you verified them, mark
each one as new, changed, or as before, and name the test for each anti-story. Then list any
remaining limits or blockers. Being ready doesn't mean you can push, deploy, release, or write to
production without explicit approval.
