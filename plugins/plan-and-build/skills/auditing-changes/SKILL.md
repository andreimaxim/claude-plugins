---
name: auditing-changes
description: Audits a high-stakes change line by line before it ships, such as a dependency upgrade that picks up a security fix, a data migration, or a change to authentication, payments, or data deletion, and reports whether it can ship without regressions, with evidence for every changed line. Started only by the user.
disable-model-invocation: true
argument-hint: "[branch, commit, or dependency and target version]"
---

# Auditing changes

Auditing a change produces a verdict on whether it can ship without regressions, and the evidence
behind that verdict. It's meant for changes where a missed regression is expensive, such as
upgrading a library to pick up a fix for a CVE, a data migration, or a change to authentication,
payments, or data deletion. An audit is slower than a normal review on purpose. Every changed line
is read and accounted for, every change in behavior is traced to the code in this application that
depends on it, and every claim that something is safe rests on code you read or a check you ran.

## Pin the change

Before you read anything else, write down the following:

- **Before and after.** For a branch or commit, the base and head SHAs. For a dependency, the
  version the lockfile resolves now, the target version, and the platform production runs on.
- **Intent.** What the change has to do, such as closing a named CVE.
- **What must not change.** The behavior that users, callers, and stored data rely on.
- **How it ships.** Whether old and new code run side by side during a deploy, and what the old
  code leaves behind for the new code to read, such as queued jobs, cache entries, serialized
  objects, and signed tokens.

Ask the user for anything the repository can't tell you that would change the verdict. Otherwise,
state what you assumed.

For a dependency upgrade, read [reference/dependency-upgrades.md](reference/dependency-upgrades.md)
before you go on. It covers choosing the target version, getting the diff of the code that will
actually run, and finding every place the application depends on the library.

Run upgrades, builds, and tests in a separate worktree (`git worktree add`), so the user's checkout
stays as it was. The audit doesn't change the user's branch, and it doesn't commit, push, or
deploy. Tests you write during the audit stay in the worktree, and the report can offer them.

## Account for every changed line

Read the whole diff yourself. Don't sample it, and don't hand the reading to a subagent, because a
summary drops the details an audit exists to catch. Keep a ledger with one row for each changed
hunk, and give each row one of these statuses:

- **No behavior change.** Formatting, comments, tests, documentation, or a rename with every
  reference updated. Say how you know.
- **Not reachable.** The application never runs this code. Name the search that shows it, and
  account for calls that a text search misses, such as `send`, `public_send`, `const_get`,
  `method_missing`, class names in configuration, and calls from other dependencies.
- **Reachable.** The application runs this code. List the call sites, and end the row either
  cleared, with the evidence, or as a finding.
- **Not reviewed.**

Also account for changes that don't appear as source code, such as lockfile entries, generated
files, build settings, defaults that depend on the environment, vendored native code, and data
that is already stored.

If the diff is too large to finish, say how much you covered rather than skimming the rest.

## Try to break each reachable change

For each reachable row, look for the case that breaks it, and trace that case through the code:

- **Inputs the application actually sends,** including empty and nil values, unusual encodings,
  large sizes, boundary values, and malformed user input. Security fixes often reject or escape
  input that used to pass, so look for the inputs this application sends that a stricter check
  would now treat differently.
- **Errors.** What's raised now compared with before, what rescues it, and whether a new exception
  class escapes a `rescue` written for the old one.
- **State and timing.** Transactions, partial failures, retries, concurrent callers, ordering, and
  time zones.
- **Interfaces.** Changed defaults, deprecations that now raise, removed or renamed methods,
  changed return types, and signatures that still accept the old call but now mean something
  different.
- **Deployment.** Old and new code running together, data written by one version and read by the
  other, locks taken by migrations on large tables, and rolling back.

## Check behavior by running it

Reading clears a row only when the code leaves no room for doubt. For each reachable row on a path
the application depends on, run a check that would fail if your conclusion were wrong:

- Run the repository's full test suite before and after the change, in the same environment, and
  compare the failures, skips, warnings, and deprecation messages. Failures that exist before the
  change are a baseline, not a pass.
- For each reachable row, find a test that calls the application's code with realistic input and
  passes through the changed code. Where none exists, write a characterization test in the
  worktree, run it before the change to record the current behavior, and run it again after.
  Confirm that the test fails when the behavior differs, because a test that passes no matter what
  the code does proves nothing.
- Boot the application and load all of its code the way production does, e.g., with
  `bin/rails zeitwerk:check` in a Rails application, and run the production build, because some
  regressions show up only at load or build time.
- When tests can't reach the risk, such as with native libraries, external services, or a full
  user flow, use `proving-changes` to run the scenario end to end.

## Get an independent review

After your own pass, ask the reviewer subagent to review the change in a fresh context. Give it the
before and after, the intent, what must not change, and, for a dependency, the paths to both
unpacked versions and to the application. Ask it to find regressions this change could cause in
this application. Don't give it your ledger, findings, or verdict, so its review isn't anchored on
yours.

Check each point where its findings and yours disagree against the code until you can settle it.
A finding you can't refute stays open.

## Decide the verdict

Choose one verdict:

- **Ship.** Every row is accounted for, each reachable row is cleared with evidence, and no finding
  is open.
- **Ship with conditions.** The change is safe once the listed steps are done, such as changing a
  call site, setting a configuration option, or deploying in a given order.
- **Don't ship.** A regression is confirmed, or the change doesn't do what it's meant to do.
- **Not enough evidence.** Rows remain unreviewed, or reachable rows couldn't be checked.

When the evidence supports more than one verdict, choose the most cautious one and say what would
change it.

## Report

Start with the verdict and one sentence that explains it. Then include the following:

1. **What you audited.** The exact before and after, the intent, and your assumptions.
2. **Findings.** For each one, the input or situation that triggers it, the application code it
   affects (`path/to/file.rb:10-20`), the evidence, and the smallest fix.
3. **Cleared changes.** Each reachable row with its call sites and the evidence that cleared it,
   such as a test with its before and after results, or the code that rules the problem out.
4. **Coverage.** The number of ledger rows with each status, the checks you ran with their before
   and after results, and anything you didn't cover and why.
5. **Security fix.** For an upgrade that fixes a vulnerability, whether the target version closes
   it and whether this application was exposed to it.

Write the full ledger to a file and link it from the report. Skip sections that have nothing in
them.
