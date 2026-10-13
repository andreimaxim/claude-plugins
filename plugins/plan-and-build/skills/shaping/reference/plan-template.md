# Plan template

Write a self-contained Markdown plan with a descriptive title and the six sections below. The
reader should be able to understand and evaluate the proposal without having been in the shaping
conversation. Preserve the agreed shape rather than introducing new decisions while writing the plan.

If the appetite, a consequential design choice, or a feasibility question still blocks the shape,
label the plan **Draft** and state what must be settled. Including all six headings does not
mean the work is solved. Distinguish those blockers from ordinary implementation choices
deliberately left to the team.

## Problem

Describe the specific situation that motivated the work: who was trying to do what, where it
failed, and what improvement would matter. Show the baseline so readers can judge whether this
solution, or another one, improves the situation.

Link the original request and supporting evidence where useful. Separate observed behavior from
reported experience or assumptions. Do not infer a failure from the proposed solution or borrow one
from a reference example. The name of a requested feature is not a problem definition. For internal
technical work, use the real caller or operator's difficulty rather than inventing a customer story.

End the section with the agreed outcome, which is one or two sentences that say what's true when
the work is done.

## Appetite

State how much change we're willing to take on to solve this problem and what that rules out.
Explain whether the investment supports a small improvement to an existing workflow or a larger
change, including the complexity we're willing to introduce for users and maintainers.

Record the boundary agreed with the user, not a newly invented estimate or deadline. Include a time
budget if one was supplied. Do not turn willingness to invest into a premature choice of database
schema, interface control, or implementation. If the candidate needs more investment, revisit the
appetite rather than silently expanding it.

## Solution

Present the main elements and how they connect, from the starting situation to a useful outcome.
Explain where the change fits into the existing system and which existing behavior stays the same.
Use a rough sketch, breadboard, or concrete example where it makes the idea easier to understand.

Explain the trade-offs that make this solution fit the problem and appetite. Add enough context and
labels for someone outside the conversation to follow it, without reproducing the whole discussion.
Keep confirmed constraints explicit and show where designers and programmers still have choices.
Number the main areas of the solution so the stories and the sketch can refer to them. Leave file
inventories, speculative class hierarchies, and implementation task lists for later.

## Stories

List the stories in the order of the flow, with nine or fewer. Write each story as one sentence
that says who or what acts and what they can do. Mark each story as new, changed, or as before, so
the reader can see where the work is. For example:

1. An API client requests an export and gets a job ID back. New.
2. A worker builds the file and stores it. New.
3. An API client polls the job until the file is ready. New.
4. An API client downloads the file through the existing signed URL. As before.

Then list the anti-stories, and mark each one with ✗. For example:

- ✗ A retried request creates a second export.
- ✗ A client downloads another account's export.

Implementation starts from the list, and the team can change the stories or their order as they
learn more.

## Rabbit holes

Call out the traps that could derail this solution and the decisions that avoid them. Ground
technical conclusions in the code, contracts, tests, or experiments actually examined. Include an
exact detail when it closes a real rabbit hole. Otherwise, keep implementation choices open.

Do not hide a known feasibility gap under "implementation detail." For example, if an asynchronous
operation requires a recovery path and that path is still unresolved, naming a queue does not solve
it. Investigate the gap, simplify the solution, exclude the troublesome case if the solution still
solves the problem without it, or leave the plan marked as a draft. An independent review provides
advice, not proof of feasibility.

## No-gos

State the functionality and use cases deliberately excluded, and where the team should stop.
Explain exclusions when their connection to the appetite or problem would otherwise be unclear.
Do not quietly discard a confirmed requirement to make the proposal appear smaller.

Keep the plan in the conversation unless the user requests a file or destination. A shaping-only
request ends with the plan or an explanation of why the idea is not ready. The plan does not
authorize publishing it, updating a ticket, or building it. If implementation was also requested,
continue once the consequential choices are settled.
