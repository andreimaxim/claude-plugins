---
name: shaping
description: Shapes raw ideas into rough, solved, bounded plans. Use when asked to shape or plan work, or explore consequential scope, behavior, or architecture choices before implementation; not for routine coding changes.
---

# Shaping

Shaping turns a raw idea into a short plan that solves one specific problem within agreed limits.
It adapts 37signals' Shape Up, which calls the plan a pitch. Shape Up was written for product
work, and here we apply it to engineering work by treating each change as a small product. Its
users can be people, API clients, other services, or the engineers who maintain the code.

A shaped plan has three properties:

- **Rough.** It leaves visible room for designers and programmers to make their own decisions.
- **Solved.** The main elements fit together, and no open question could sink the solution.
- **Bounded.** It says what we're not doing (no-gos) and where the team should stop.

Work through the sections below in order. When a later step finds a problem, go back to an
earlier one.

## Frame the problem

Start from the raw idea, e.g., a user request, a Jira ticket, a GitHub issue, or an
observability signal. Before you discuss solutions, agree with the user on the problem, the
outcome, and the appetite.

**Problem.** Treat the request as a symptom, and ask what was happening when the need came up. For
example, a request for permission roles came from someone who archived a file without knowing it
would disappear for the whole team. Push back on requests that name an area instead of a problem,
e.g., "improve the files page" or "refactor billing". Ask what isn't working, in what context, and
what can stay the same. If you can't find a specific pain point, set the idea aside rather than
invent one. For help narrowing a broad request, read the
[dot calendar case study](reference/dot-calendar.md).

**Outcome.** Write one or two sentences that say what's true when the work is done, e.g., "An
export never blocks the request that started it."

**Appetite.** The appetite is how much change we're willing to take on, in scope and complexity,
and it isn't a time budget or an estimate. It can span from a small tactical fix within the
existing design, even if it isn't the ideal solution, to a broad refactoring across the codebase.
If a proposal includes broader or more complex changes than agreed, narrow the proposal or get the
user's agreement to expand the appetite.

Shape Up uses a time budget to reduce the risk of shipping late. Here we bound the change to reduce
the risk of unintended effects and work that's hard to understand, verify, deploy, or reverse.
The reference case studies retain Shape Up's time-based appetite.

## Map stories and examples

Before choosing a solution, define the stories and clarify ambiguous behavior with the user using
Example Mapping:

- **Story.** A story is one sentence that says who or what acts and what they can do, e.g., "An
  API client can poll the export until the file is ready." For internal work, the actor may be a
  developer, a caller, or an operator.
- **Rule.** A rule states a condition or constraint on the behavior.
- **Example.** An example gives a concrete starting situation, an action, and an observable
  expected result.
- **Anti-story.** An anti-story describes an outcome that must not happen within supported
  behavior, marked with ✗, e.g., "✗ A retried request creates a second export." Excluded features
  belong in No-gos, not in anti-stories.

Propose cases and expected outcomes from the request and confirmed contracts, and vary them to test
boundaries and interactions. Keep proposed outcomes distinct from agreed behavior, and don't assume
current behavior is intended. If an answer is unknown, keep the question visible and note what
decision it affects.

Use enough examples to settle behavior that could change the solution or its scope, rather than
enumerating every combination. Leave runnable test lists to implementation.

## Find and test the elements

Explore several candidates quickly, and give frank feedback instead of writing down the first
proposal. Look for the main elements of the solution and how they connect, without designing
screens, schemas, or classes yet. Number the main elements so the plan and the stories can refer to
them.

Pick the sketch that fits the idea:

- For a user flow, draw a breadboard of places, affordances, and connections. Read the
  [invoice autopay case study](reference/invoice-autopay.md) for an example.
- For a backend or JSON API, follow the caller from the first request to a useful result. Read
  [backend breadboarding](reference/backend-breadboarding.md) for an example.
- When the layout is part of the idea, draw a fat marker sketch, which is too coarse to hold fine
  detail. Read the [dot calendar case study](reference/dot-calendar.md) for an example.

Follow each story through the sketch in the order of the flow, from start to a useful result. Drop
any candidate that misses a story or allows an anti-story, and refine the stories and examples as
you compare candidates. When a connection in the sketch has no story running along it, either a
story is missing or the connection isn't needed. Move on when one candidate supports every story
and its agreed examples without allowing an anti-story.

## Find risks and rabbit holes

Walk through each story and its unanswered questions to find risks that could invalidate the
solution or push the work beyond the appetite:

- A capability or technical pattern that is new to the system.
- An unverified assumption about a rule or how the parts connect.
- A missing design answer that the solution relies on.
- A hard decision that can be settled now.

Check technical assumptions against the code, contracts, tests, or a small experiment approved by
the user, because a plausible explanation isn't evidence. When the solution depends on how a
library, framework, or external service behaves, ask the `researcher` subagent instead of inferring
it from local code. Resolve uncertain business behavior with the user.

Resolve each risk with evidence, settle a simpler answer now, or cut the risky part as a no-go. If
an open question could still change the agreed outcome, core solution, or appetite, the plan isn't
solved yet.

## Write and review the plan

Write the plan with the [plan template](reference/plan-template.md) for a reader who wasn't part of
the conversation.

Before presenting the plan as solved, ask the `reviewer` subagent to look for holes in the plan.
Give the reviewer the written plan and the file paths behind your evidence, without the shaping
conversation, and ask it to look for:

- A story or agreed example the solution doesn't support, or an anti-story it allows.
- A rule, expected result, or technical claim that the agreement or evidence doesn't support.
- A path that stops before the actor gets a useful result.
- A missed rabbit hole, or work that goes beyond the appetite.
- Anything an outside reader can't follow from the plan alone.

Weigh each finding against the evidence instead of accepting it automatically. When a fix changes
the solution, the agreed behavior, or the appetite, ask the reviewer to check the revised plan. If
an open question still threatens the outcome, core solution, or appetite, label the plan **Draft**
and state what must be settled. When you present the plan, list the findings that changed it and
any you rejected, with a reason for each rejection.

Keep the plan in the conversation unless the user asks for another destination. A request for
shaping alone ends with the plan, or with an explanation of why the idea isn't ready. The plan
doesn't authorize publishing it, updating a ticket, or building it, unless the user also asked for
implementation and the consequential choices are settled.
