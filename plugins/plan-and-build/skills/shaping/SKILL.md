---
name: shaping
description: Shapes raw ideas into rough, solved, bounded plans. Use when asked to shape or plan work, or explore consequential scope, behavior, or architecture choices before implementation; not for routine coding changes.
---

# Shaping

Shaping turns a raw idea into a short plan that solves one specific problem within agreed limits.
It follows 37signals' Shape Up, which calls the plan a pitch. Shape Up was written for product
work, and here we use it for engineering work, so treat each feature as a small product. Its users
can be people, API clients, other services, or the engineers who maintain the code.

A shaped plan has three properties:

- **Rough.** It leaves visible room for designers and programmers to make their own decisions.
- **Solved.** The main elements fit together, and no open question could sink the solution.
- **Bounded.** It says what we're not doing and where the team should stop.

Work through the sections below in order. When a later step finds a problem, go back to an
earlier one.

## Frame the problem

Start from the raw idea, e.g., a user request, a Jira ticket, a GitHub issue, or an
observability signal. Before you discuss solutions, agree with the user on the problem, the
outcome, and the appetite.

**Problem.** Treat the request as a symptom, and ask what was happening when someone felt the need
to ask. For example, a request for permission roles came from someone who archived a file without
knowing it would disappear for the whole team. Push back on requests that name an area instead of
a problem, e.g., "improve the files page" or "refactor billing". Ask what isn't working, in what
context, and what can stay the same. If you can't find a specific pain point, set the idea aside
rather than invent one.

**Outcome.** Write one or two sentences that say what's true when the work is done, e.g., "An
export never blocks the request that started it." The stories test the solution against the
outcome later.

**Appetite.** The appetite is how much change we're willing to take on, and it isn't an estimate.
For example, decide whether a free text note is enough, or whether the problem justifies
structured data that people can filter and report on. Don't pick database columns or controls yet.
When a good idea exceeds the appetite, narrow the idea or revisit the appetite with the user,
instead of quietly growing the work.

For help narrowing a request, or for choosing how much visual detail to show, read the
[dot calendar case study](reference/dot-calendar.md).

## Find the elements

Explore several ideas quickly, and give frank feedback instead of writing down the first proposal.
Look for the main elements of the solution and how they connect. Add enough detail to walk through
the interaction, but don't design screens, schemas, or classes yet.

Pick the sketch that fits the idea:

- For a user flow, draw a breadboard of places, the affordances in each place, and the connections
  between them. Read the [invoice autopay case study](reference/invoice-autopay.md) for an example.
- For a backend or JSON API, follow the caller from the first request to a useful result. Read
  [backend breadboarding](reference/backend-breadboarding.md) for an example.
- When the layout is part of the idea, draw a fat marker sketch, which is too coarse to hold fine
  detail.

Number the main areas of the solution, so the plan and the stories can refer to them.

## Check the solution with stories

Write the stories the solution has to support. A story is one sentence that says who or what acts
and what they can do, e.g., "An API client can poll the export until the file is ready." For
internal work, the actor is often a developer or an operator, e.g., "A developer can add a payment
provider without changing the checkout code." Each story follows a path through the breadboard,
from where it starts to where it ends.

Then write the anti-stories, which describe what must not happen, and mark each one with ✗, e.g.,
"✗ A retried request creates a second export." Put all the stories in the order of the flow, and
keep the list to nine or fewer. If you need more, group them.

Check each candidate solution against the same stories, and drop any candidate that allows an
anti-story. When a connection in the breadboard has no story running along it, either a story is
missing or the connection isn't needed.

Move on when one solution supports every story and allows no anti-story.

## Find risks and rabbit holes

Walk through each story, and ask these questions:

- Does it need technical work we've never done?
- Are we assuming how the parts fit together?
- Are we assuming a design solution exists that we haven't found?
- Is there a hard decision we could settle now?

Check each assumption against the code, contracts, or tests, because a plausible explanation isn't
evidence. Then deal with each risk in one of these ways:

- Resolve it with evidence.
- Settle a simpler answer now, even if it's less elegant.
- Cut the risky part, and list it as a no-go.

When the solution depends on how a library, a framework, or another service behaves, ask the
Librarian instead of inferring it from local code.

Before you present the plan as solved, ask the Oracle subagent to check it. Give it the solution,
the appetite, the evidence, and any remaining doubts, and ask whether the solution works and fits
the appetite. Oracle gives advice, and we make the decision.

If the core solution still depends on an open question, it isn't solved yet.

## Write the plan

Write the plan with the [plan template](reference/plan-template.md). Keep the shape you agreed on,
and write for a reader who wasn't part of the conversation.
