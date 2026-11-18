# Plan template

Write the plan as one Markdown document with a descriptive title and the six sections below, so a
reader outside the shaping conversation can understand and judge the proposal from the plan alone.
Keep the agreed shape, and don't add new decisions while writing.

If an open question could change the agreed outcome, core solution, or appetite, label the plan
**Draft** and state what must be settled. Keep questions that block the plan separate from ordinary
implementation choices left open for the team.

## Problem

Describe the specific situation that led to the work and the current baseline. Say who or what was
trying to do what, and where it failed. Link the original request and supporting evidence when they
help, and keep observations separate from reports and assumptions. Don't infer a failure from the
proposed solution or feature name, and don't invent a customer story for internal technical work.

End with the agreed outcome in one or two sentences that say what's true when the work is done.

## Appetite

State the agreed scope and complexity of the change, including how far the work may extend and what
must remain unchanged.

## Solution

Present the numbered main elements and how they connect from the starting situation to a useful
result. Explain where the change fits in the existing system and what stays the same. State the
trade-offs that make the solution fit the problem and the appetite, and use a breadboard, rough
sketch, or concrete example when it makes the flow easier to follow.

State the confirmed constraints, and leave out file lists, guessed class hierarchies, and
implementation task lists.

## Stories

List the stories in the order of the flow, keeping them to nine or fewer so a reader can hold the
whole flow in mind at once. If you have more than nine, group related steps rather than turning the
stories into a detailed task list. Mark each story as `New`, `Changed`, or `As before`:

1. An API client requests an export and gets a job ID back. New.
2. A worker builds the file and stores it. New.
3. An API client polls the job until the file is ready. New.
4. An API client downloads the file through the existing signed URL. As before.

Attach the agreed rules, concrete examples, and anti-stories (marked with `✗`) to the stories they
clarify. Record a shared rule once and refer to it from the affected stories. For example, story 1
might include:

- Rule: A retry of the same export request returns the existing job without creating another.
- Example: The server creates job E, but the response is lost. The client retries and receives E's
  identifier, and no additional job is created.
- Anti-story: ✗ A retried request creates a second export.

Keep unconfirmed outcomes labeled as proposals, and list open questions separately. For each open
question, state the decision it affects and who or what can answer it. Carry questions that could
change the shape into Rabbit holes.

## Rabbit holes

Name the risks and unknowns that could invalidate the solution or push the work beyond the
appetite, and the decisions or evidence from the code, contracts, tests, or experiments that close
them. Include an exact technical detail when it closes a rabbit hole, and otherwise leave
implementation choices open.

Don't hide an unresolved doubt about feasibility under "implementation detail." Investigate the
gap, simplify the solution, or cut the case that causes it.

## No-gos

List the features and cases left out on purpose, and say where the team should stop. Explain an
exclusion when its link to the problem or appetite isn't obvious, and don't drop a confirmed
requirement without agreement.

Source: [Write the Pitch](https://basecamp.com/shapeup/1.5-chapter-06).
