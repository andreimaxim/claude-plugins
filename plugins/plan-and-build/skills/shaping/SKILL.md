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

Before choosing a solution, start the stories. A story is one sentence that says who or what acts
and what they can do. For internal work, the actor may be a developer or an operator. Use Example
Mapping within the conversation to clarify ambiguous behavior. A rule states a condition or
constraint on the behavior. An example gives a concrete starting situation, an action, and an
observable expected result.

Develop examples together with the user. Propose cases and expected outcomes using the request
and confirmed contracts, and build on examples the user brings. Vary the cases to explore
boundaries and interactions, explaining which rule or assumption each variation tests. Use the
discussion to refine both the rules and the examples.

Keep proposed outcomes distinct from agreed behavior, and don't assume current behavior is
intended. If an answer is unknown, keep the question visible and note what decision it affects.

Use enough examples to settle behavior that could change the solution or its scope, rather than
enumerating every combination. Leave runnable test lists and predictive TDD cycles to implementation.
Refine the stories, rules, and examples while exploring candidates.

For help narrowing a request, read the [dot calendar case study](reference/dot-calendar.md).

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
  detail. Read the [dot calendar case study](reference/dot-calendar.md) for an example.

Number the main areas of the solution, so the plan and the stories can refer to them.

## Check the solution with stories

Follow each story through the breadboard, from where it starts to where the actor gets a useful
result, e.g., "An API client can poll the export until the file is ready." Put the stories in the
order of the flow, and keep the list to nine or fewer. If you need more, group them. Connect the
rules and examples to the stories they clarify, without repeating shared rules under every story.

Write the anti-stories, which describe what must not happen within supported behavior, and mark
each one with ✗, e.g., "✗ A retried request creates a second export." Connect them to the relevant
stories and examples too. Features excluded from the scope belong in No-gos, not in anti-stories.

Check each candidate solution against the same stories, rules, and agreed examples, and drop any
candidate that allows an anti-story. When a connection in the breadboard has no story running
along it, either a story is missing or the connection isn't needed.

Move on when one solution supports every story and its agreed examples without allowing an
anti-story. Carry unanswered questions into the risk check rather than treating them as settled.

## Find risks and rabbit holes

Walk through each story and its unanswered questions, and ask:

- Does it need technical work we've never done?
- Are we assuming a rule or how the parts fit together?
- Are we assuming a design solution exists that we haven't found?
- Is there a hard decision we could settle now?

Check technical assumptions against the code, contracts, tests, or a small experiment that the
user approved, because a plausible explanation isn't evidence. Resolve uncertain business behavior
with the user. Then deal with each risk in one of these ways:

- Resolve it with evidence.
- Settle a simpler answer now, even if it's less elegant.
- Cut the risky part, and list it as a no-go.

When the solution depends on how a library, a framework, or another service behaves, ask the
researcher subagent instead of inferring it from local code.

If an open question could change the agreed outcome, core solution, or appetite, the plan isn't
solved yet. Keep those blockers separate from ordinary implementation choices left open on purpose.

## Write the plan

Write the plan with the [plan template](reference/plan-template.md). Keep the shape you agreed on,
and write for a reader who wasn't part of the conversation.

## Review the plan adversarially

Before you present the plan as solved, ask the reviewer subagent to try to break it. The reviewer
starts in a fresh context, so it reads the plan the way an outside reader would. Give it the
written plan and the paths of the files behind your evidence, but not the shaping conversation.
Ask it to look for these holes:

- A story or agreed example the solution doesn't support, or an anti-story it allows.
- A rule, expected result, or technical claim that the agreement or evidence doesn't support.
- A path that stops before the actor gets a useful result.
- A rabbit hole the plan doesn't name, or work that goes beyond the appetite.
- A part that a reader can't follow without the shaping conversation.

Weigh each finding against the evidence instead of accepting it automatically. Handle a supported
finding like any other risk, and go back to the step it affects. When a fix changes the solution,
the agreed behavior, or the appetite, ask the reviewer to review the revised plan. If a finding still
threatens the core solution, label the plan **Draft** and name the open question.

When you present the plan, list the findings that changed it and the ones you rejected, with a
reason for each rejection. The reviewer gives advice, and we make the decision.

Keep the plan in the conversation unless the user asks for a file or another destination. A request
for shaping alone ends with the plan, or with an explanation of why the idea isn't ready. The plan
doesn't authorize publishing it, updating a ticket, or building it. If the user also asked for
implementation, continue once the consequential choices are settled.
