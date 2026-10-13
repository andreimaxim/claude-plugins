# Backend breadboarding

Read this before you decide how to show a backend interaction. Follow the person or system that
starts the interaction until it reaches a result it can see and use, because a single request and
response may cover only one step of that path. When an API is the product, follow the workflow of
the API's consumer, and don't invent a UI for it. Showing the whole interaction doesn't put all of
it in scope for the work.

The export scenario below shows the technique. It isn't evidence about the current system, and it
isn't a recommended architecture.

## Breadboard from the consumer's point of view

Suppose a client needs a file of selected records, but preparing the file takes too long to keep
one request open. One candidate accepts an export request and lets the client fetch the result
later. The example assumes both of these facts, and other export systems may differ.

The important difference is that accepting a request isn't the same as completing it. A response
that says the request was accepted doesn't yet give the client the file it needs.

In this interaction, the parts of a breadboard map to what the client can see and use:

- **Places** are the resources and states the client can see, such as an export that's being
  prepared, an export that's ready, or an export that failed.
- **Affordances** are what the client can do, e.g., submit a selection of records, check the
  status, read the result, and download the file when it's available.
- **Connections** show how the client's actions and the background work lead to the next step.
  Checking the status shows progress, but it doesn't make the export ready.

Walking through the candidate raises questions about the interaction before any implementation
detail, e.g., the following:

- What happens if the client loses the first response and the export identifier in it.
- How long the client can still download the file.
- Whether a notification would suit the client better than repeated status checks.

Explore these choices before you settle endpoint paths, JSON fields, or a queue design.

## Draw the whole interaction as a short flow

```text
┌──────────────────┐   request  ┌───────────────────┐
│ Client submits   │───────────▶│ Export preparing  │
│ record selection │◀───────────│                   │
└──────────────────┘ identifier └─────────┬─────────┘
                                          │ background preparation
                                 ┌────────┴────────┐
                                 ▼                 ▼
                           ┌───────────┐     ┌───────────┐
                           │ Ready     │     │ Failed    │
                           │ file link │     │ reason    │
                           └─────┬─────┘     └───────────┘
                                 │ client downloads
                                 ▼
                           ┌───────────┐
                           │ File      │
                           └───────────┘
```

After it receives the identifier, the client can check whether the export is being prepared, is
ready, or has failed. The file is prepared in the background, and the status changes when
preparation succeeds or fails, whether or not the client checks it. The link to the file appears
when the export is ready, and a failed result tells the client that waiting won't produce a file.
The state names are provisional, and they aren't a specification of the wire format.

The flow shows more than "the API starts a job." It shows how accepting the request leads to
something the client can use, and where the path can stop. It also raises a question about whether
the design can work, because accepted work might be lost between accepting the request and starting
preparation. Investigate that handoff before you treat the candidate as solved, because drawing an
arrow doesn't prove the step works.

## Use another view when it answers the question better

Pick the view that answers the question, and don't produce every view for every idea.

A call tree helps when the question is which action starts which piece of work. Label each point
where work continues later, so the nesting doesn't suggest that the first request waits for
everything:

- The client submits a selection.
  - Accept the export, and return an identifier and a way to check on it.
- Background preparation runs later, separately from any status checks.
  - Prepare the file.
  - Make the result available to download, or record that it failed.
- The client checks the export.
  - Show its current state and the next action the client can take.
- The client downloads the ready file.

The items are actions in the domain, and they aren't proposed functions or classes. A call tree
that stops when the work is scheduled still leaves out the result the client needs.

Pseudocode helps when a branch is the important part. For example, the status check has three
cases:

```text
on status check
  if preparing   tell the client to check again later
  if failed      report the failure
  if ready       give the client a way to download the file
```

Keep pseudocode at the level of behavior, unless an exact technical contract is needed to settle a
risk.

A diff helps when the reader already knows the starting point. For this candidate, a before and
after in words would be:

- Before: the request waits for preparation and returns the file.
- After: the request returns an export identifier. The client checks on preparation separately and
  downloads the file when it's ready.

A code diff can show a small change to a known interaction, but it shouldn't bury the client's path
under implementation details or suggest that the feature is ready to build.

## Keep the open questions visible

Recovering a lost response, how long to keep the file, and whether to notify or poll are choices to
explore, and not every backend feature needs them. Follow the scenarios that could
change this solution or how well it fits the appetite. Keep existing access rules and other
confirmed contracts when you compare candidates.

A small experiment that the user approved can settle a technical question, but it doesn't authorize
building the whole feature or show that the feature is ready for production. Leave endpoint names,
schemas, class structure, and infrastructure choices open, unless evidence shows that you need to
settle one to avoid a rabbit hole.

Technique source: [Find the Elements](https://basecamp.com/shapeup/1.3-chapter-04).
