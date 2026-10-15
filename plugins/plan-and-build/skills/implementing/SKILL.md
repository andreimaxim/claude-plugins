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
service behaves, ask the Librarian instead of inferring it from local code. Orienting isn't an
audit or an approval step, so start building once you can choose a first scope.

## Choose scopes

Start with a small path through the middle of the system that works end to end and resolves an
important unknown. Use the plan's stories as starting points. A scope usually covers one story or a
few related ones, and each anti-story should end up as a test.

Draw each scope around an outcome you can integrate and verify on its own, not around layers,
files, or people. Expect scopes to split, merge, and appear as you build, because the first map
isn't a complete task list.

Track each scope as unknown, understood, or verified. Understood means you can see how to finish
it, and verified means the integrated behavior works. New evidence can move a scope back, and
effort spent doesn't count as progress. Tell the user when the map changes in a way that matters.

## Build

Build the smallest end-to-end path that produces the scope's visible result, for today's
requirements rather than imagined ones. For each behavior change, write a test or check that fails
on the old behavior and passes on the new one, and run focused checks as you go. If the behavior is
hard to test, look at the responsibility and dependency boundaries before you reach for elaborate
mocks.

As soon as the flow is concrete enough to judge, step back and look at the new code and its
surroundings as one design, before you repeat that design across the rest of the work. Ask what
callers and operators have to know or coordinate, and look for a simpler design through deletion,
consolidation, and clearer ownership. You can change how the plan organizes the code, but not the
agreed outcome.

Refactor in small steps that keep behavior the same, with checks in place for the behavior you're
preserving. Keep refactoring separate from behavior changes, and leave unrelated cleanup alone.
Remove comments that only narrate the code, and keep the ones that explain a reason or a
constraint.

Bring any consequential departure from the plan to the developer before you make it. Pause only the
affected work, and present the evidence with a recommendation. When something has to give, cut
flexible breadth or generality first, and never cut required behavior, quality, security, or data
integrity.

## Review and finish

Verify the integrated result with the repository's own checks. Then have the Oracle subagent review
it in a fresh context. Give it the outcome, the stories, the constraints, and what changed, but not
the implementation conversation, and ask it to check these things:

- Correctness, and whether the change follows the repository's guidance.
- Fit with the surrounding design, and any complexity the change doesn't need.
- Whether the tests prove the required behavior.

The review has to cover the complete integrated result before you report the work as ready, and
your own review doesn't replace it. Weigh each finding instead of accepting it automatically, and
carry the supported fixes through the code, the checks, and a follow-up review without waiting for
the user to ask.

Keep choosing scopes until the work meets the agreed outcome, unless the user asked you to stop
after one. The work is ready when it meets the outcome, not when every possible improvement is
done.

When you report, go through the plan's stories. Say which ones work and how you verified them, mark
each one as new, changed, or as before, and name the test for each anti-story. Then list any
remaining limits or blockers. Being ready doesn't mean you can push, deploy, release, or write to
production without explicit approval.
