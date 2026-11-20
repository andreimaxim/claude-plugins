---
name: naming-things
description: Chooses, reviews, and fixes names by working out what a thing means, what it does, and who will read the name. Use when asked what to call something, to rename or review names, or when an unclear name in the code may hide a modeling problem or a bug.
---

# Naming things

A name tells the next reader what a thing means and does. Most of the text in a codebase is uses of
its own names, and later changes build on the names that are already there, so a vague or
misleading name leads later work to repeat the same misunderstanding.

## Understand the thing before choosing a name

Work out what the thing is, what it does, and where its value comes from by reading its definition,
its callers, and its contracts, tests, or stored data, rather than judging from its current name
alone. Consider its domain meaning, lifecycle, units, errors, and who will read the name, such as a
local variable, a public API, a database column, or a user-facing label.

When the definition and the callers disagree on what something means, investigate the disagreement
instead of assuming the existing name, the original intent, or the current code is right.

## Principles for choosing a name

### Name the concept or operation, not one caller's reason for using it

Put one caller's purpose in that caller's local variable names, not in the name of a general
function or type. For example, if a helper named `toPixels` only performs a general numeric
rounding operation, name the rounding operation and keep `pixels` in the caller. Before naming a
wrapper, check whether the language or an installed library already has an idiom for the operation,
such as a standard map union instead of `mergeTargetValuesIntoSourceTemplate`, because a wrapper
that adds no meaning may not need to exist.

Keep domain words when the behavior belongs to that domain, and don't give domain-specific behavior
a name that suggests it's general.

### Make related names tell one story as a set

Read neighboring names together, such as the fields on one struct or the date columns on one table,
so their relationship is obvious at a glance. `created_at` fits a synthetic record that begins to
exist when its row is inserted, whereas a row representing something from the real world needs a
name for the event each value records, such as `uploaded_at`, `issued_on`, and `last_reviewed_at`
on an uploaded document, using `_on` for dates and `_at` for timestamps.

Keep different facts separate even when their values happen to match today, and define each fact in
one place.

### Don't use a name to hide a modeling conflict

When a name is hard to choose or hard to read, it often points to a vague or conflicted concept in
the code rather than a vocabulary problem:

- **Split meanings:** Different parts of the code may use the same field with incompatible
  meanings. For example, ingestion sets `documentDate` to the day a file was uploaded while the
  review screen treats it as the date printed on the document, causing a 90-day staleness check to
  test the wrong date. No single rename makes those two meanings agree; report the split and the
  affected callers or stored data.
- **Ambiguous absence:** For an optional value, work out what its absence means instead of
  mechanically adding `maybe_` or `_opt`. Distinguish an expected optional override
  (`fallbackConfig`) from a missing value caused by a parse failure, and don't express absence
  twice, such as an optional map whose keys are also optional, unless the two kinds of absence mean
  different things.

Don't give an unresolved design a polished name that hides its problems. Settle the modeling
decision first, or report the conflict and the decision that needs to be made.

## Give advice or carry out the rename

Recommend the outcome that communicates the concept accurately with the least change: keeping the
current name, renaming the thing, replacing a pointless wrapper with an existing idiom, or
resolving a modeling conflict first. Give one recommended name with the evidence from the code and
callers, and offer alternatives only when they reflect an open question about meaning.

A request for naming advice doesn't authorize editing code. When asked to carry out a rename,
trace all references across code and external contracts (serialized fields, database schemas,
routes, configuration, and documentation), preserve required compatibility, and run the
repository's checks. Passing checks don't catch string-based references or external consumers, so
search for the old name before finishing and list any external contract kept for compatibility or
not verified. Keep a behavior-preserving rename separate from changes to behavior or data
migrations.
