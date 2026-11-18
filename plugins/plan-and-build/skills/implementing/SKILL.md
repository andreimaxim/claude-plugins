---
name: implementing
description: Implements plans and agreed work through emergent scopes, architectural refinement, and independent verification. Use for substantial implementation or carrying out an agreed plan; not routine edits.
---

# Implementing

Implementing turns an agreed plan into working, reviewed code, following the building phase of
Shape Up. The work is built in scopes, which are small pieces that each work end to end, and the
scopes come out of the work instead of a task list made up front.

## Orient

Start from the plan or brief the user agreed to, and note the outcome, the appetite, the stories,
the no-gos, and any open decisions. Follow the entry point to the code that owns the rules, and use
the repository's guidance and similar flows to tell the intended design apart from accidental
legacy behavior.

Check the plan's assumptions against the code, contracts, or a quick experiment, and report
anything that could invalidate the plan or exceed the appetite before you build on it. If the
problem itself needs rethinking, go back to `shaping`. When the plan depends on how a library,
framework, or external service behaves, ask the `researcher` subagent instead of inferring it from
local code. Start building once you can choose a first scope.

## Choose scopes

Start with a small path through the middle of the system that works end to end and resolves an
important unknown. Use the plan's stories as starting points. A scope usually covers one story or a
few related ones, and each anti-story becomes a test.

Ask the `planner` subagent to map the first scope onto the code. Give it the plan as background,
the scope to plan, and what is already settled, such as the outcome, the stories, and the appetite,
and tell it not to plan later scopes. Ask it again when a later scope reaches code you haven't
explored.

Draw each scope around an outcome you can integrate and verify on its own, not around layers,
files, or people. Expect scopes to split, merge, and appear as you build, because the first map
isn't a complete task list.

Track each scope as unknown, understood, or verified. Understood means you can see how to finish
it, and verified means the integrated behavior works. New evidence can move a scope back, and
effort spent doesn't count as progress. Tell the user when the map changes in a way that matters.

## Build each scope

Design each class, module, or function from how callers use it. Decide what objects exist, what
messages callers send them, and what comes back. Prefer small objects that collaborate through
messages over deep inheritance trees, and use inheritance only when a subclass can stand in for its
parent everywhere. Validate and normalize untrusted input where it enters the system, so internal
objects can trust the values and collaborators handed to them instead of repeating format and nil
checks.

Build the smallest end-to-end path that produces the scope's visible result for today's
requirements, and run focused checks as you go. Write tests that specify the system's intended
behavior and state directly, rather than framing assertions around how it behaved before the
change. Exercise the public interface the way a caller does, and assert concrete expected values
and state changes rather than only checking that a value exists or a method was called. Because a
test is a caller of the public interface, a test that is hard to write or needs elaborate setup and
mocks shows that the public API itself isn't well thought out. Improve the interface and
responsibilities instead of working around them in the test.

## Refine the design

As soon as the flow is concrete enough to judge, step back and look at the new code and its
surroundings as one design before repeating it across the rest of the work. Integrate a new
requirement as if the design had always included it, reconsidering responsibilities and interfaces
instead of adding a special case. Judge simplicity across the operation and its callers, not by
code size or object count. Assume the next person or agent who edits the code will read only the
file they open, copy the nearest example, and take the shortest path that works:

- Give each piece of state and each domain rule one owner with a small public interface that
  completes the operation, rather than making callers coordinate internal steps or scattering flags
  and branches across callers.
- Keep internal methods, helpers, and collaborators private to the object or module that owns
  them, and inline wrappers that only forward arguments without changing the abstraction.
- When a scope replaces an internal method or object and all its callers can change together,
  update the callers and delete the old path in the same scope so nobody copies it later.
- Keep each list of valid states or options in one place, and derive other uses from it or check
  them in a test.

You can change how the plan organizes the code, but not the agreed outcome.

Refactor in small steps that keep behavior the same, separate from behavior changes, and leave
unrelated cleanup alone. Check why an existing guard, branch, or comment was added before deleting
it.

Do not add explanatory code comments unless the user explicitly requests them. Express intent
through names and interfaces. Enforce rules in code and cover them with tests. Preserve required
legal notices and comments interpreted by runtimes or tools.

A comment is not a substitute for fixing an implementation problem. When a constraint prevents the
fix, report the constraint and remaining limitation to the user.

## Handle blockers and plan changes

If a fix doesn't work, stop guessing variations of it. Inspect the running system or the code that
produces the input to find out which assumption was wrong before trying another change. Ask the
`reviewer` subagent when a hard technical question remains open.

When callers compensate for another component's implementation, trace the requirement to its source
and distinguish a required domain rule or external constraint from an avoidable implementation
choice. Fix the cause within the agreed appetite rather than repeating the workaround. When
compatibility or an external dependency prevents fixing the cause, contain the workaround inside
the component that owns the operation and make any remaining limitation explicit to callers.

Bring any consequential departure from the plan or appetite to the user before making it. Pause
only the affected work, and present the evidence with a recommendation. When something has to give,
cut flexible breadth or generality first, and never cut required behavior, quality, security, or
data integrity.

## Review and finish

Verify the integrated result with the repository's own checks, and exercise the changed behavior as
close to the user or caller's entry point as the environment allows. Then have the `reviewer`
subagent review the result in a fresh context. Give it the outcome, the stories, the constraints,
and the diff or branch to review, without the implementation conversation, and ask it to check:

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

Keep choosing scopes until the work meets the agreed outcome, unless the user asked you to stop
after one. The work is ready when it meets the outcome, not when every possible improvement is
done.

When you report, go through the plan's stories and anti-stories. Mark each story as `New`,
`Changed`, or `As before`, and say how you verified it. Name the test that covers each anti-story,
and list any remaining limits or blockers. Being ready doesn't authorize pushing, deploying,
releasing, or writing to production without explicit approval.
