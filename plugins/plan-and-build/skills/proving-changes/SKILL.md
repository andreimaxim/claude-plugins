---
name: proving-changes
description: Proves that a change works end to end and breaks nothing by running the same scenario before and after the change. Use when asked to prove a change works, show before and after evidence or a demo recording, or verify a change when unit and integration checks aren't enough.
---

# Proving changes

Proving changes produces evidence that convinces a skeptical reviewer that a change does what was
claimed and breaks nothing, without the reviewer having to read the code. Every claim needs an
observation from a run that would have come out differently if the claim were false. Unit tests,
type checks, and linters catch local mistakes, but they aren't end-to-end proof. Report them as
checks, and don't call a change verified end to end on their results alone.

## Choose the cases and the level

Start from the plan's agreed outcome, stories, examples, and anti-stories when a plan exists.
Otherwise, list what has to be true for the change to be right, and pick a concrete case for each
claim:

- **The target scenario.** Use the conditions from the bug report, feature request, or agreed
  examples, such as the same input, repository shape, platform, browser, or account state.
- **Every affected surface.** Cover each interface the change touches, including loading, error,
  and recovery states that a user or caller can see.
- **Control cases and neighbors.** Check a nearby case whose behavior must stay the same, and check
  adjacent features that share the edited code.
- **Operational checks.** When the change could affect them, check resource cleanup and behavior
  after a restart, reload, or reconnect. For a performance claim, ask the `profiler` subagent to
  measure before and after.

Drive the change as close to how a user or caller reaches it as the environment and the user's
authorization allow, e.g., through the deployed or staging interface, the full local stack, or the
changed component with its real neighbors. A request for proof doesn't authorize deploying, writing
to production, or running disruptive checks against shared services. When the software ships as a
built package, container, or binary, run the built output rather than running directly from the
source checkout, so the proof also covers the build and packaging step. Name the test level in your
report, and state what any shortcut or mock skipped.

## Run before and after

For a bug fix, run the failing scenario on the unchanged code first and capture the failure. For a
new feature, capture the starting state so the result can be compared against it. Put the steps in
a script or repeatable procedure that you will run unchanged after you apply the edit.

Confirm that each run executes the version you intend. Restart long-running processes after
switching code, and check a build hash, version string, or log line that differs between the two
versions. For a refactor or new test tooling, plant a small deliberate bug once and confirm that
the check fails.

If a failure doesn't reproduce on the unchanged code, a passing run after the edit proves nothing.
Adjust the environment, timing, or data until the failure reproduces. If it still doesn't
reproduce, say so and narrow your claim.

After you apply the change, rerun the identical procedure across all chosen cases with the same
input, timing, and viewport. When a race or transition is too fast to see, force the timing the
same way in both runs. When the outcome varies between runs, such as a feature that calls a
language model, run each case multiple times on both sides and report pass rates.

## Collect and check the evidence

Gather more than one kind of evidence for each claim, so each observation checks the others:

- **Visuals and state.** Capture screenshots of each state, or a recording with a moving pointer
  that also crosses neighboring controls when interaction or motion is part of the change. Back
  each visual with a state fact that confirms what it shows, such as a DOM attribute, a database
  row, a file on disk, or an API response. Use realistic inputs instead of placeholder labels.
- **Logs and numbers.** Capture timestamped lines across components that show which code path ran.
  Record exact counts, sizes, and timings when the claim depends on them.

Inspect every screenshot, recording, log, and number yourself before trusting the run. Discard and
repeat a run invalidated by stale processes, a silent setup failure, leftover state from an earlier
run, or a capture taken mid-transition. Note any discarded run in your report. Treat unexpected
application errors and unexplained differences between runs as findings rather than discarding
them.

Before finishing, read the whole diff and account for every removed line, changed comment, touched
shared file, and configuration edit. Delete any temporary test data, accounts, sandboxes, or
staging rows that the proof created.

## Report the proof

Start the report with one sentence that says what you proved and at what level, e.g., "Verified end
to end on the local stack, and not yet deployed." Then include the following:

- One list item per case, covering the stories, anti-stories, and control cases, with the before
  result, the after result, and the expected result.
- The screenshots and recordings, each labeled with the state it shows, together with the log
  lines, state values, and numbers that support them.
- The paths to the artifacts and reproduction scripts so a reviewer can rerun the proof, and any
  runs you discarded and why.
- What isn't proven and why, such as an untested platform, production timing, or a skipped surface.

Keep your claim no larger than the evidence. Separate a bug you reproduced and fixed from a path
that simply didn't fail during the run. Silence in logs only shows that a case was handled when the
logs also show that the case ran.
