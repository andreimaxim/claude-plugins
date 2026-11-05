# Plan template

Write the plan as one Markdown document with a descriptive title and the six sections below. A
reader who wasn't part of the shaping conversation should be able to understand and judge the
proposal from the plan alone. Keep the shape you agreed on, and don't add new decisions while you
write.

If the appetite is unsettled, or a behavior, design, or feasibility question could change the
agreed outcome, core solution, or appetite, label the plan **Draft** and say what has to be settled.
Having all six headings doesn't make the work solved. Keep those blockers apart from the ordinary
implementation choices that you leave to the team on purpose.

## Problem

Describe the specific situation that led to the work. Say who was trying to do what, and where it
failed. Show the current baseline, so readers can judge whether this solution, or another one,
improves on it.

Link the original request and the evidence for it when they help. Keep what you observed separate
from what people reported and from your own assumptions. Don't infer a failure from the proposed
solution, and don't borrow one from a reference example. The name of a requested feature isn't a
description of the problem. For internal technical work, describe the difficulty that the actual
caller or operator has, and don't invent a customer story.

End the section with the agreed outcome, in one or two sentences that say what's true when the work
is done.

## Appetite

State how much change we're willing to take on to solve the problem, and what the appetite rules
out. Say whether the appetite allows a small improvement to an existing workflow or a larger change.
Also say how much complexity we'll accept for users and maintainers.

Record the limit you agreed with the user, and don't invent a new estimate or deadline. Include a
time budget if the user gave one. Don't turn the appetite into an early choice of database schema,
interface control, or implementation. If the candidate needs more than the appetite allows, revisit
the appetite with the user instead of quietly growing the work.

## Solution

Present the main elements and how they connect, from the starting situation to a useful result.
Explain where the change fits in the existing system, and which existing behavior stays the same.
Use a rough sketch, a breadboard, or a concrete example when it makes the idea easier to follow.

Explain the trade-offs that make the solution fit the problem and the appetite. Label the parts,
and add enough context for someone outside the conversation to follow them, but don't repeat the
whole discussion. State the confirmed constraints, and show where designers and programmers can
still make their own choices. Number the main areas of the solution, so the stories and the sketch
can refer to them. Leave file lists, guessed class hierarchies, and implementation task lists for
later.

## Stories

List the stories in the order of the flow, with nine or fewer. Write each story as one sentence
that says who or what acts and what they can do. Mark each story as new, changed, or as before, so
the reader can see where the work is. For example:

1. An API client requests an export and gets a job ID back. New.
2. A worker builds the file and stores it. New.
3. An API client polls the job until the file is ready. New.
4. An API client downloads the file through the existing signed URL. As before.

Connect the rules and examples to the stories they clarify. Record a shared rule once and refer
to it from the affected stories. Include enough examples to distinguish the interpretations that
affected the solution, rather than an exhaustive test matrix.

For example, story 1 might have the following agreed behavior:

- Rule: A retry of the same export request returns the existing job without creating another.
- Example: The server creates job E, but the response is lost. The client retries and receives E's
  identifier, and no additional job is created.
- Anti-story: ✗ A retried request creates a second export.

List the anti-stories with ✗ and connect each one to the relevant stories or examples. They forbid
outcomes within supported behavior, while excluded features belong in No-gos. For story 4, another
anti-story could be "✗ A client downloads another account's export."

Keep unconfirmed outcomes labeled as proposals, and list unanswered questions separately from the
agreed rules and examples. For the retry example, an open question might be how to distinguish a
retry from a new request, or what should happen for simultaneous requests. Say what decision each
question affects and who or what can answer it, if known. Carry questions that could change the
shape into Rabbit holes.

Implementation derives runnable tests from the agreed behavior. The team can refine the stories,
examples, and their order as it learns more, but changes to agreed outcomes need the user's
agreement.

## Rabbit holes

Name the problems that could stall the work, and the decisions that avoid them. Base each technical
conclusion on the code, contracts, tests, or experiments you actually examined. Include an exact
detail when it closes a real rabbit hole, and otherwise leave implementation choices open.

Don't hide a known doubt about feasibility under "implementation detail." For example, if an
asynchronous operation needs a way to recover from failure and nobody has settled how, naming a
queue doesn't settle it. Investigate the gap, or simplify the solution. You can also leave out the
case that causes the trouble, if the solution still solves the problem without it. Otherwise, keep
the plan labeled as a draft. The reviewer's findings are advice, and they aren't proof that the
solution works.

## No-gos

List the features and use cases you left out on purpose, and say where the team should stop.
Explain an exclusion when its link to the appetite or the problem isn't obvious. Don't quietly drop
a confirmed requirement to make the proposal look smaller.

Source: [Write the Pitch](https://basecamp.com/shapeup/1.5-chapter-06).
