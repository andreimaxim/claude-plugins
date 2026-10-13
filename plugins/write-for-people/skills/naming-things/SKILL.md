---
name: naming-things
description: Chooses, reviews, and fixes names by first working out what a thing means, what it does, and who will read the name. Use when the user asks what to call something, asks to rename or review names, or asks for a naming rule or convention, for variables, functions, types, fields, database columns, endpoints, pages, products, or domain concepts. Also use when a name found during other work seems unclear or is used with more than one meaning, because it may hide a modeling problem or a bug.
---

# Naming things

A name tells the next reader, whether a person or a model, what a thing means. Most of the text in
a codebase is uses of the project's own names, so names are the main way a reader learns what the
code does. Later changes build on the names that are already there, and a vague name can lead later
work to repeat or extend the same misunderstanding. Name a thing for what it is or does, and not for
the reason one caller needed it at the time.

Adapted from Koleman Nix's [How to name things](https://kolemannix.com/blog/how-to-name-things/).

## Understand the thing before you suggest a name

Use the guidance in this skill to understand the thing, and don't treat it as a checklist or a
score. Base the recommendation on the difference in meaning that decides the name, rather than on
whether the name follows a convention. Spend more time on names with a wide scope or a high risk of
being misread, and less on a short-lived local name.

Gather evidence about the thing in order:

1. Read the definition and the code around it.
2. Read a few representative places that write the value, read it, or call it.
3. Where they help, check contracts, tests, schemas, documentation, and stored data.

When these sources disagree, find out why, and don't assume that the existing term, the original
intent, or the current code is the right one. A contract can limit what you can safely change, but
it doesn't prove that the concept behind it is coherent. For a new design, work from the behavior
you intend and from concrete examples instead. In both cases, don't decide what something means
from its current name alone.

Find out what the thing is and what it does, and where its value comes from. Where they matter,
also consider its domain meaning, lifecycle, units, errors, relationships to other things, and what
its absence means. Separate what the thing means from the details of how it's implemented today,
and from the particular use that prompted the name. The current uses are evidence, but no single use
defines a general operation. If the meaning is hard to pin down, write a short definition in plain
words. If two definitions fit and they conflict, investigate the conflict instead of choosing a word
that hides it.

Find out who will read the name and where they'll see it. For example, a local variable, a public
API, a URL parameter, and a product name each have different readers, and some names, such as URL
parameters, have to work for both users and code. Read the code or interface from the top, the way
a teammate would read it, instead of judging the name as a word on its own.

## Choose a name that fits its context

Name the concept or operation itself. Put one caller's purpose in that caller's local names, instead
of in the name of a general operation. Keep domain words when the behavior belongs only to that
domain, and don't give domain-specific behavior a name that suggests it's general.

A function named after one caller's need can only be found and used for that need. Someone looking
for the general operation won't find it under that name, and won't trust it if they do. A reader who
meets a call to it while fixing a bug can't trust the name either, so they have to read the body to
learn what it does. Before you name an ordinary operation, check whether the language or a library
already has an idiom for it, because a wrapper that adds no meaning may not need to exist.

Read related names together. Look for pairs and families of names, such as `src` and `dst`, that
mirror each other so the relationship shows on the page. Fields that sit together, such as the date
columns on one table, should tell one clear story as a set.

Use an established term when it has the right meaning. Don't keep a misleading term only for
uniformity, and don't introduce a second word for the same concept without a reason.

Decide on length, abbreviations, and type information in a name based on what the reader needs at
that spot. Rules like "always abbreviate", "never abbreviate", "use long names", or "mark every
optional value" let you skip the thinking, so don't apply them. Be consistent about the care you
take with each name, and don't require every name to follow the same pattern.

Keep different facts separate even when their values match. A fact should be defined in one place,
and every other place that states the same fact should refer to that definition. Two values that
happen to be equal aren't always the same fact, e.g., a maximum name length and a default queue
depth that are both 64 should stay two constants. A concept can have several representations or
terms for different audiences, but make the relationship between them explicit.

Recommend the outcome that communicates the concept accurately with the least change. The outcome
may be one of the following:

- Keep the existing name.
- Rename the thing, or merge competing terms into one.
- Replace a wrapper that adds no meaning with an existing idiom.
- Settle a modeling decision before anyone chooses a name.

Separate what needs to change from what the task lets you implement.

When a new name is needed, give one recommendation with the evidence for it. Offer alternatives only
when they reflect an open question about meaning, and explain that question instead of listing
synonyms. Check the candidate at its definition and at representative uses, and make sure it means
the same thing at each place to a reader who doesn't know why the author wrote it.

## When a name is hard to choose or hard to read

An unclear existing name often points to a vague concept. When you find a name that seems unclear
while you work on another task, investigate it before you rely on it, because your task may depend
on what it actually means. Usually the name is someone's best attempt to label a vague concept,
rather than a poor label on a clear one. Often different parts of the code use the name with
different meanings, and both meanings are reasonable readings of the name, but one of them is a bug.

It's normal to struggle to name a design that isn't finished, so let the name develop as you
understand the design better. If the design is meant to be settled and still has no clear name,
look for conflicting meanings, mixed responsibilities, redundant states, or differences the model
doesn't represent. When a good name appears during refinement, it's evidence that the design is
getting clearer, but it doesn't prove that the design is correct. Don't give an unresolved design a
polished name that hides its problems.

For an optional value, find out what its absence means, and don't follow a rule that adds `maybe_`
or `_opt` to every optional name. For example, overrides are optional by nature, because they're
layered on top of a complete baseline configuration. A parse failure is different, and it isn't
just another missing override. If absence means an error, handle the error early, or choose a name
that says the value can be missing because of an error. If an absent map and an empty map mean the
same thing everywhere the value is read, consider turning the absent map into an empty map where the
value enters the system. Don't merge the two states until you've confirmed that they mean the same
thing.

If a name hides inconsistent behavior or stored data with mixed meanings, report the conflict and
the decision someone needs to make. Renaming alone doesn't fix the behavior or the historical data.
Keep a rename that preserves behavior separate from changes to behavior, data shape, or migrations.
Make only the changes the task asks for, because a request about a name isn't permission to redesign
the system.

## Examples

### A rounding helper named `toPixels`

Check the arithmetic, the rounding mode, the valid range of inputs, and how the result is
converted. If the function only does a general numeric operation, name that operation, and say
"pixels" in the caller's variable names. Don't assume that a formula such as "add 0.5 and convert to
an integer" matches a standard `round` function, because it can give a different result for negative
numbers.

### `created_at`, `created_on`, `uploaded_at`, and `issued_at`

`created_at` is a good default name for the moment a row's thing began to exist, and the `_at`
suffix marks a point in time. When the column holds a date rather than a timestamp, `created_on`
reads better, because people say "on" a day and "at" a time. `created_at` fits a synthetic object,
such as a display configuration, because the object exists only inside the system and begins to
exist when its row is inserted.

A row that describes something in the real world needs a name for the event the value records. For
example, a birth certificate that someone uploaded in 2023 was issued in 1995, so a column named
`created_at` would claim the wrong year. Name each column for its event, e.g., `uploaded_at` for
when the file arrived or `issued_at` for when the document was issued, instead of giving every table
the same timestamp column.

Read the date columns on a table together. For a document the system received, `uploaded_at`,
`record_date`, and `last_reviewed_at` tell one clear story as a set, and `record_date` is a date
rather than a timestamp.

### `documentDate`

Trace where `documentDate` is set and where it's read. Suppose ingestion stores the day the file
arrived, while the review screen shows the value as the date printed on the document. A staleness
rule that rejects bank statements older than 90 days then checks the wrong date for some documents,
whichever meaning its author intended. No single rename makes those meanings agree. Report the split
in meaning and the affected checks, and report the risk to stored data, because a fix may need a
migration and a backfill.

### `mergeTargetValuesIntoSourceTemplate`

If the function's whole behavior is the language's ordinary map union, prefer the idiom, such as
Scala's `template ++ values`, over a wrapper named after one use. Nobody looking for a way to
combine two maps would find the function under its current name. Before you replace it, confirm
which map wins when both have the same key, and check for any other behavior the idiom doesn't
have.

### `fallbackConfig`

Find out whether `fallbackConfig` is a complete configuration used when another one fails, or a
sparse set of overrides layered on a baseline, and name the role it has. If its type is
`Option[Map[String, String]]`, absence is expressed twice, because both the map and any key in it
can be missing. Decide whether the two kinds of absence differ, as described for optional values
above. If different uses imply conflicting roles, resolve the conflict in the model before you
choose the name.

## Give advice or carry out the rename

For advice, give the recommended outcome and the main reason from the context. When the outcome
includes a name, state the name and exactly what it means, and add a short usage example if it makes
the recommendation clearer. Say which meanings are still unresolved and what evidence would settle
them. Keep a small naming decision short, without a formal report.

For a rename the user asked for, follow the references through the affected code and any external
contracts, e.g., serialized fields, schemas, routes, and documentation. Use symbol-aware reference
and rename tools when they're available, and check for dynamic or external references the tools may
miss. Treat a public or stored name differently from an internal identifier, and keep compatibility
where it's required. Run the repository's relevant checks, and tell the user which changes are a
verified rename and which are a proposed fix to the model.
