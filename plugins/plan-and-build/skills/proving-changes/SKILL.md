---
name: proving-changes
description: Proves that a fix, feature, or refactor works end to end and doesn't break existing behavior, by running the same scenario before and after the change and collecting evidence such as screenshots, recordings, logs, state, and timings. Use when the user asks for proof ("prove it", "irrefutable proof", "show me it works"), asks to test or verify a change end to end, asks for before and after numbers or a demo recording, or asks whether a change breaks anything, or when unit and integration checks aren't enough to verify a change.
---

# Proving changes

Proving changes produces evidence that convinces a skeptical reviewer that a change does what was
claimed and breaks nothing, without the reviewer having to read the code. Every claim in the proof
needs an observation from a run that would have come out differently if the claim were false.

Unit tests, type checks, and linters catch local mistakes, but they aren't end-to-end proof. For
example, a test that mocks the boundary where a bug lives will pass whether or not the bug is
fixed. Run the checks and report them as checks, and don't call a change verified end to end on
their results alone.

## Choose the cases and the level

Start by listing what has to be true for the change to be right, and pick a concrete case that
checks each claim:

- **The target scenario.** Use the conditions from the bug report or feature request, such as the
  same input, repository shape, platform, browser, text size, account state, or version.
- **Every affected surface.** Cover each interface the change touches, such as web, desktop,
  mobile, CLI, API, or SDK, and include loading, error, and recovery states when a user can see
  them.
- **Control cases and neighbors.** Check a nearby case whose behavior must stay the same, and check
  adjacent features that share the edited code. A control case shows that the change is targeted,
  e.g., that a menu item still appears when it should instead of disappearing everywhere.
- **Operational checks.** When the change could affect them, check resource cleanup for watchers,
  sockets, sessions, and processes, check behavior after a restart, reload, or reconnect, and
  compare performance before and after.

Drive the change the way a user or caller reaches it, from the strongest level down to the weakest:

1. Run the deployed product or a staging copy through its public interface.
2. Run the full local stack, including the UI, server, background workers, clients, and external
   services or their staging or sandbox versions.
3. Run the changed component with its direct neighbors, such as the CLI talking to the local
   server.
4. Run integration tests through a public interface, and use unit tests or mocks only when a higher
   level can't run.

Use the highest level the environment and the user's authorization allow, and name the level in
your report. A request for proof doesn't authorize deploying, writing to production, or running
disruptive checks against shared services. Calling an internal function, rendering an isolated
component story, or injecting a mocked client skips code that a user hits. For packaged software,
test the built artifact that ships, such as the binary, container, bundle, or installed package,
because source-tree tests miss files left out of a build. When you have to take a shortcut, say
what the check skipped.

## Run before and after

For a bug fix, run the failing scenario on the unchanged code first and capture the failure. For a
new feature, capture the starting state so the result can be compared against it. Put the steps in
a script or repeatable procedure that you will run unchanged after you apply the edit.

When you switch between the unchanged code and the changed code, confirm that each run executes the
version you intend:

- Restart servers, workers, and clients after you switch code, because long-running processes keep
  serving the old code in memory.
- Check a version string, build hash, log line, or file checksum that differs between the two
  builds.
- For a refactor or a new test harness where behavior should stay the same, plant a small
  deliberate behavior change once and confirm that the check fails. A check that still passes when
  behavior changes proves nothing.

If a failure doesn't reproduce on the unchanged code, a passing run after the edit proves nothing.
Adjust the platform, timing, or data until the failure reproduces, or say that you couldn't
reproduce it and lower your claim.

After you apply the change, rerun the identical procedure across all the cases you chose. Keep the
input, the timing, any injected delays, and the viewport the same in both runs. When a race or
transition is too fast to see, slow it down the same way in both runs and show that the slowdown
took effect in each.

## Collect and check the evidence

Gather more than one kind of evidence for each claim, so each observation checks the others:

- **Visuals and state.** Capture screenshots of each state, or a recording with a moving pointer
  when hover, clicks, focus, or motion are part of the change. Back each visual with a state fact
  that confirms what it shows, such as a DOM attribute, a database row, a file on disk, or an API
  response. Use realistic inputs instead of placeholder labels, so the run shows what users will
  see in practice.
- **Logs.** Capture timestamped lines from the server, client, or worker that show which code path
  ran across components.
- **Numbers.** Record exact counts, sizes, and timings when the claim depends on them.

Inspect every screenshot, recording, log, and number yourself before you trust the run. Discard a
run, repeat it, and note the discarded run when the setup or capture was invalid:

- A process was still running old code, or a setup step such as a feature flag or account sign-in
  failed silently.
- State carried over from an earlier run, such as a cache entry, a selection, or an open dialog.
- A capture landed during an animation, a scroll, or a retry timer.

Treat unexpected application errors and unexplained differences between runs as findings, rather
than discarding the run. Run both sides enough times to tell flakiness that already existed on the
base branch from flakiness the change introduced.

Before you finish, confirm that the evidence covers the change integrated with the current target
branch. If the target branch has moved, test the integrated result in a separate worktree or
temporary checkout and rerun the decisive cases, instead of rewriting the user's branch. Read the
whole diff and account for every removed line, changed comment, touched shared file, and
configuration edit, so the change doesn't delete intentional behavior or leave debug code behind.
Delete any test accounts, organizations, sandboxes, branches, or staging rows that the proof
created.

## Benchmark performance changes

When a claim is about a speedup, a regression, or a performance choice between options, vet the
measurement before you report or act on the number. For a quick single-run estimate that the user
asked for, still confirm that the work ran without errors, and state that the number came from one
run.

1. **Tune both sides for production.** Run both versions the way production runs them, with release
   builds, production flags, batching, connection pools, and realistic data sizes and cache warmth.
   If one side runs on unconfigured defaults, the test compares settings rather than code. Explain
   any caching or configuration change that is part of the change under test.
2. **Alternate runs and compare against the spread.** Run the before and after versions at least
   five times each, alternating between them (`before`, `after`, `before`, `after`) so warmup, lazy
   initialization, and background load affect both sides equally. Report the measured operation,
   the unit, the run count, the median, and the range, and report no measurable difference when the
   gap is smaller than the run-to-run variation.
3. **Confirm that the work happened without errors.** Count failed requests, timeouts, and retries,
   and check that the outputs are valid, because fast rejections look like speedups and retries
   look like slowdowns. Confirm that the work ran inside the timed region, e.g., by checking that
   background jobs finished, lazy enumerators were consumed, and no unintended cache skipped the
   work.
4. **Find the limiter and check the math.** Profile or read system counters in a separate untimed
   run, because profilers skew timings. Map the limiting resource or hot spot to the source code,
   and check that the load generator didn't saturate first. Compare the speedup against the share
   of time the changed code originally took, because removing a step that takes 10% of a run can
   speed the run up by at most about 11%. Next to any microbenchmark, measure the end-to-end path a
   user waits on and report the microbenchmark's share of the whole.

## Evidence by kind of change

| Kind of change | Evidence to collect |
| --- | --- |
| Visual or layout | Before and after screenshots of the same state, measured positions or sizes, and each affected viewport, theme, or input method |
| Interaction or flow | A recording of the flow with a moving pointer that also crosses neighboring controls, state facts after each step, and the state after a reload |
| Race, crash, or timing | The failure reproduced with forced timing, logs showing the bad ordering before and its absence after, and a check that fails when the fix is reverted |
| Backend or distributed flow | Timestamped logs across components, database and API state before and after, and the failure and recovery paths |
| Security or access | The denied case rejected and the allowed case accepted against the identity provider or its staging copy, with no unintended revocations |
| Deployment | The deployed version contains the commit, and production logs or metrics show the new path running |

## Report the proof

Start the report with one sentence that says what you proved and at what level, e.g., "Verified end
to end on the local stack, and not yet deployed." Then include the following:

- Show a table of the cases with the before result, the after result, and the expected result.
- Include the screenshots and recordings, each labeled with the state it shows, together with the
  log lines, state values, and numbers that support them.
- Point to the artifacts and reproduction scripts so a reviewer can rerun the proof, and note any
  runs you had to discard and why.
- State what isn't proven and why, such as an untested platform, production timing, or a skipped
  surface.

Keep your claim no larger than the evidence. When a gap remains, name the gap and say what would
close it. Separate a bug you reproduced and fixed from a path that simply didn't fail during the
run, because silence in logs only shows that a case was handled if you also show that the case ran.
