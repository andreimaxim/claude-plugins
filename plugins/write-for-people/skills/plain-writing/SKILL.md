---
name: plain-writing
description: >-
  Writes and revises text in AI-generated artifacts and products in a plain
  style so people can read it easily. Use when creating or reviewing text for
  websites, dashboards, documents, interfaces, or standalone messages, including
  prompts that generate the text. Applies to prose and short labels as well as
  groups of comparable items.
---

# Plain writing

Write text people can understand in one pass, from standalone messages to whole
websites and documents.

Preserve facts and qualifications, including uncertainty and attribution. Leave
code, identifiers, and exact quotations unchanged. When revising existing text,
compare the draft against the source so every fact, number, qualification, and
attribution keeps its original strength, rather than letting a style fix turn
"not established" into "not".

## Word choice and tone

### Use simple, everyday words

Don't pick a fancy synonym when a plain word works. Also avoid words AI tools
overuse, e.g., "delve", "tapestry", "landscape", "robust", "leverage", and "reach".

Before: We leverage the cache to unlock a more robust query experience.

After: We use the cache to make repeated queries faster.

### No jargon

Use everyday language, the way two people talk to each other, instead of invented
jargon or shorthand. Use an established technical term only when it is more
precise than an everyday word, and briefly define it when readers may not know
it.

Before: The score is a calibrated proxy for whether the property holds.

After: The score estimates how likely the property is to hold.

### No puffery or empty emphasis

Drop words that add emphasis but no information, e.g., "really", "real",
"matters", "worth", "carries weight", "boasts", "a testament to", "pivotal",
"renowned", and "quietly". State the actual point, or cut the sentence.

Before: This result matters, and it carries weight for the design.

After: The scores barely moved, so we can skip the model on most documents.

### Use consistent terminology and constrain your vocabulary

Use one word for each concept throughout the artifact. If a concept is an
"issue", don't also call it a "page". Consistent terminology keeps readers from
having to decide whether different words mean the same thing.

Before: Upload the document. The file is parsed, and the record is saved.

After: Upload the document. The document is parsed and saved.

### It's ok to use contractions

They match everyday speech, so use them freely.

Before: Do not worry, it is not going to overwrite your file.

After: Don't worry, it's not going to overwrite your file.

### Do not invent hyphenated adjectives

Avoid a phrase you make up by joining words with a hyphen to sound compact or
clever. If you would not find it in a dictionary, don't use it. A common compound
adjective that people already use is fine, e.g., "well-crafted".

Before: We added a reveal-style colon to the output.

After: We added a colon that shows the schema.

### Keep the writing boring, descriptive, and explanatory

Do not use a catchy phrase, slogan, clever label, or wording meant to sound
memorable. Apply the rule to headings, topic sentences, callouts, labels,
summaries, and ordinary prose.

Before: Legal requirements as a floor.

After: Applicable legal constraints.

Before: # The alignment loop

After: # Iterative refinement using development disagreements

### Match the connotation of verbs to the purpose of the text

Use verbs that make the sentiment clear. For customer complaints, use
clearly negative verbs like "complain". For praise, use clearly positive verbs
like "love". Readers who skim can then tell whether an item describes praise or
a problem from the verb alone.

Before: Chess is a popular new draw, though users report a checkmate rule that
seems wrong and no way for skilled players to skip ahead.

After: Users enjoy the new chess course, but complain about a checkmate rule
that seems wrong and no way to skip ahead.

## Sentences and paragraphs

### Write complete sentences in ordinary prose

Each sentence should have a subject and a verb. Do not write fragments, and do
not stitch unrelated ideas together with colons or semicolons. But it is ok to
join closely related ideas with plain conjunctions, like "and", "because", or
"so". Headings, interface labels, and status text may use shorter forms when the
context supplies the missing words.

Before: The agent polls the file and reacts to changes, and the team meets on
Tuesdays.

After: The agent polls the file and reacts to changes. The team meets on
Tuesdays.

### Make the subject and action easy to identify

Start each item with its subject or its verb. If the subject is the same in all
items and clear from context, drop it and start with the verb. Put the first
verb in the tense that best matches the reader's expectations, e.g., a
continuous form for an active session.

English readers often take the first noun as the subject, the next verb as its
action, and the next noun as its object. A misleading opening can make readers
go back and parse the sentence again. For example, when every active session
has the same subject (the agent), start each title with a verb in the `-ing`
form instead of mixing nouns and imperatives:

Before: KV persistence across queries / Liquid 1.7B model integration /
Fine-tune model on personal writing

After: Persisting KV across queries / Integrating Liquid 1.7B model /
Fine-tuning model on personal writing

### Put the most important words first

Find the words that distinguish each item from the others, and move them as
close to the start as possible. Readers scanning a list often see only the
first two words of each item:

Before: Your order #1001 has been delivered / Your order #1001 has shipped /
Your order #1001 is confirmed

After: Delivered: order #1001 / Shipped: order #1001 / Confirmed: order #1001

### Present sequences in order

When you present a workflow or sequence, walk through it in order. Use "First",
"Second", "Third", and give each step its own sentence so the reader can follow
it, or break up steps with semicolons.

Before: The groups the features were sorted into were the authors' own reading,
the example posts were written by hand, and finer detail meant training extra
small models and labeling again.

After: First, the authors sorted the features into groups themselves, based on
their own reading of the outputs. Second, they wrote the example posts by hand.
Third, when they wanted finer detail, they trained another small model, and
they labeled the posts again.

### Organize a paragraph as a topic sentence and then support

Start each paragraph or section with a topic sentence that states the main
point. Then, the next sentence should be a supporting example or fact, with an
extra sentence about it if it needs one. Then, introduce more support with a
plain connective like "For example", "Moreover", or "Or".

Before: The parser skips files with no changes. The cache holds the previous
output. Most renders are fast.

After: Most renders are fast. For example, the parser skips files with no
changes, so the server returns early. Moreover, the cache keeps the previous
output, so a repeated render does no work.

### Limit clauses and repeated examples

In ordinary prose, a sentence may have one or two related clauses. Do not pack
three or more clauses into one prose sentence. If you need that many points,
use a numbered First / Second / Third sequence as described under [Present
sequences in order](#present-sequences-in-order), or short bullet points when
you are writing a brief. If list points are examples and you want to inline
them, introduce with "e.g.". Also do not give three or more example sentences
back to back to support the same point.

Before: The parser reads the file, the validator checks the fields, and the
writer saves the record.

After: The parser reads the file, and the validator checks the fields. The
writer then saves the record.

### Prefer long, explanatory sentences over short, punchy ones

In ordinary prose, write the way people explain things out loud: longer
sentences with commas, and the simplest way to say the point. Do not break one
thought into a stack of short sentences, and don't write catchy short phrases.
Short lines are fine in headings, labels, status text, labeled briefs, bullets,
or a First / Second / Third sequence, e.g., "To do: validate recall on long
queries."

Before: The gate runs on every merge. It blocks regressions. Nobody bypasses it.

After: The gate runs on every merge, and it blocks changes that fail a
regression case. A regression cannot make it to production, unless someone
deliberately overrides the check.

Before: Search ranking now uses a scored model instead of heuristics. The
change reduced p95 latency from 900 ms to 220 ms. We still need to validate
recall on long queries.

After: Search ranking now uses a scored model instead of heuristics, and
p95 latency fell from 900 ms to 220 ms. To do: validate recall on long queries.

### Be precise and unambiguous, and cut unnecessary clauses

Say exactly what changes, who does what, or by what mechanism. Prefer a concrete
statement over an evocative abstraction, e.g., don't say things like
"improvement stops being guesswork". Also drop trailing or side clauses that
add no fact, e.g., "before we call the work done", "as we move forward", or "for
the time being". Keep the sentence long when the content needs it, but do not
pad it.

Before: With trusted scores, improvement stops being guesswork.

After: With trusted scores, you can measure whether each change helped,
so you keep or revert each change based on the measured result.

Before: We still need to validate recall on long queries before we call the work
done.

After: To do: validate recall on long queries.

### In chat, give context on the problem

When you are chatting back and forth, write for a smart reader who does not have
context on the problem, or who forgot it. Give context on the problem and on
what was happening before. Then say what changed. Keep the setup short. Do not
dump the whole history. Ignore this rule if you are writing an essay.

Before: The exporter now waits on the reset header, and `dotnet test` is green.

After: The Okta System Log exporter was rereading whole hours, so a retry could
write the same event twice. It now waits using the response reset header, and
the six acceptance tests pass.

## Consistency across related text

### Use the same structure for comparable items

Use one sentence structure for comparable items, so readers can follow the same
pattern across them. Don't start some items with a noun phrase ("The issue
reports that..."), others with a different noun ("The page reports..."), and
others with an action ("Pressing Ctrl+C..."):

Before:
- The issue reports that `ollama show` writes an unparseable Modelfile when a
  value contains triple quotes.
- The page reports an error reading GGUF metadata for a large MoE model.
- Pressing Ctrl+C during multiline input leaves the REPL in multiline mode.

After:
- `ollama show` writes an unparseable Modelfile when a value contains triple
  quotes.
- Reading GGUF metadata for a large MoE model returns an error.
- Pressing Ctrl+C during multiline input leaves the REPL in multiline mode.

### Use variation only to show a meaningful difference

Vary length, structure, or wording only when the difference conveys a fact the
reader needs. For comparable summaries, include the same kind of detail in each
item instead of naming an app version in one item and review languages in
another, so readers don't have to guess whether the difference is meaningful:

Before:
- **Bugs (9 reviews):** Reports include the app freezing and finished lessons
  not counting, mostly on version 7.146.0.
- **Energy limits (8 reviews):** Users say they run out of energy fast and must
  wait or watch ads, including in Russian and Turkish reviews.

After:
- **Bugs (9 reviews):** Users complain about the app freezing and finished
  lessons not counting.
- **Energy limits (8 reviews):** Users complain that they run out of energy
  fast and must wait or watch ads.

## Punctuation and formatting

### No dashes or middle dots

Do not use em dashes or en dashes, including in number ranges. Join clauses with
a period or "and", and write ranges with "to". Do not use the middle dot (·) as a
separator; use a comma, "and", or separate lines instead.

Before: The build is fast — it finishes in 10 to 20 seconds.

After: The build is fast. It finishes in 10 to 20 seconds.

### Don't use colons to join clauses in ordinary prose

Do not use a colon to glue two clauses or to set up a point in essay-like
writing. A colon is fine when you introduce a list. A colon is also fine as a
short label in updates, briefs, status notes, and PR descriptions, e.g.,
"Summary:", "Changes:", or "Remaining work:".

Before: Read for the schema: the feature fires.

After: Read for the schema. The feature fires.

Before (allowed in a PR or update): Summary: Replace em dashes in generated docs.

After (same text is fine): Summary: Replace em dashes in generated docs.

### Use straight quotes, not curly quotes

Before: The system logs each “event” as it happens.

After: The system logs each "event" as it happens.

### Keep the formatting plain

Use sentence case in headings. Do not use bold for decoration.

Before: ## How To Install The Skill

After: ## How to install the skill

### You can use lists, but do not overuse them

In explanatory prose, keep a list to three or four points, and nest extra points
if you need more. When you are writing an essay, use lists and tables very
sparingly. Keep every required item or row in a requested checklist, inventory,
or data table.

Before: Shipped this week:

- dark mode
- an invite link fix
- a schema mismatch that blocked analytics export
- renderer cleanup
- copy edits
- a scored ranking model
- a p95 drop from 900 ms to 220 ms
- untested recall on long queries

After: Search ranking now uses a scored model, and p95 latency fell from 900 ms
to 220 ms. The old heuristic path is still in the repo as a fallback.

- Shipped
    - Dark mode
    - Invite link fix
- Still open
    - Test recall on long queries
    - Unblock analytics export

## Patterns to avoid

### Do not give inanimate things fake agency

Do not write as if a system or object transforms, decides, or intends on its own
when a person or process is the real actor. Ordinary factual verbs for tools
and systems are fine, e.g., "The API returns JSON", "The job writes the file",
or "The paper argues". Prefer a human or process subject when that is clearer.

Before: The logs become searchable records, once the job finishes.

After: You can search the logs, once the job finishes.

### No analogies or imagery

Do not explain by comparing to something else, and do not use metaphor. Describe
the actual thing in literal terms. Write in a boring way.

Before: The feature index is like a card catalog that the optimizer can flip
through.

After: The feature index is a list of named features. The optimizer can look
up which feature matches a request.

### Avoid negative parallelism

Never use negative parallelism of the form "not just X, it is Y" or "not only X,
but Y". State what the thing is. A plain refusal or correction is fine,
including a short quote of a customer's hype phrase when you replace it with a
concrete fact.

Before: It is not just a parser, it is a full toolchain.

After: It is a parser and a formatter.

Before: We do not describe the API as "enterprise-grade robust," because
that phrase can mean different things.

After: The API has a 99.9% monthly uptime SLO.

### Do not stack rhetorical questions

AI writing often asks two or three rhetorical questions in a row to sound
thoughtful. Don't do this. Just state the problem directly.

Before: Does the tool keep the writer's voice? Does it make the argument
stronger or weaker?

After: We do not yet know whether the tool keeps the writer's voice, or
whether it makes the argument stronger or weaker.

### Do not use vague demonstrative pronouns

Do not use "This", "That", "These", or "Those", especially do not start a
sentence with a demonstrative pronoun, and never begin a paragraph with a
sentence that contains a demonstrative anywhere in it.

Before: That context carries into the next turn.

After: The agent applies the rules you saved on the next turn.

### Do not open with a count of things

Never start by announcing how many points are coming, e.g., "Two cautions." or
"Three things to keep in mind." State the first point directly. If you
absolutely must present many things, use a bullet list instead.

Before: Two cautions. First, the section can drift out of date. Second,
it can balloon if every item gets a sentence.

After: The section can drift out of date, because it duplicates facts
that live elsewhere. It can also balloon if every item gets a sentence.
