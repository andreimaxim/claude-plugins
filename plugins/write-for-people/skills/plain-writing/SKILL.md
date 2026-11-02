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
code, identifiers, and exact quotations unchanged.

## Word choice and tone

### Use simple, everyday words

Don't pick a fancy synonym when a plain word works. Also avoid words AI tools
overuse, e.g., "delve", "tapestry", "landscape", "robust", "leverage", and "reach".

Before: We leverage the cache to unlock a more robust query experience.

After: We use the cache to make repeated queries faster.

### No jargon

Always use human-understandable language, the way two people talk to each other.
Don't invent jargon or shorthand (that is, if a word or phrase is not in the
Merriam Webster dictionary, don't use it). Use established technical terms only
when they are most precise, and briefly define them when readers may not know
them.

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
having to decide whether different words mean the same thing.[^7]

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
a problem from the verb alone.[^13]

In the [user feedback example](#user-feedback-themes), the original chess
summary starts with "Chess is a popular new draw", even though most of the
summary lists complaints. The rewrite begins "Users enjoy the new courses but
complain about problems", so the opening reflects the mixed feedback.

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
action, and the next noun as its object.[^1] A misleading opening can make
readers go back and parse the sentence again.[^2] For example, "The agent fixed
the login bug" identifies the actor before the action. "Login bug fixed by the
agent" delays the actor until the end.

#### Session titles

The original Claude session titles mix nouns, gerunds, and imperatives. Each
active session has the same subject, the agent, so the rewrites omit the subject
and start with a verb in the continuous form.

| Group | As written | Rewritten |
| --- | --- | --- |
| Pinned | KV persistence across queries | Persisting KV across queries |
| Today | vLLM performance on eviction queries | Measuring vLLM performance on eviction queries |
| Today | Nemotron-3.5-Parse on OfficeQA benchmark | Running Nemotron-3.5-Parse on OfficeQA |
| Yesterday | Liquid 1.7B model integration | Integrating Liquid 1.7B model |
| Yesterday | Quail grant application strategy | Planning Quail grant application |
| Yesterday | Fine-tune model on personal writing | Fine-tuning model on personal writing |
| Oct 26 | Switching to main and fast-forwarding | Switching to main and fast-forwarding |

### Put the most important words first

Find the words that distinguish each item from the others, and move them as
close to the start as possible. Readers scanning web pages pay more attention
to the start of each line,[^3] and readers scanning a list may see only the
first two words of each item.[^4]

#### Order email subjects

When every subject starts with "Your order", readers have to read further to
find the status. Amazon puts the status first, so the opening word tells readers
whether the order was placed, shipped, or delivered.

| Date | Status last (Store) | Status first (Amazon.com) |
| --- | --- | --- |
| Oct 27 | Your order #1001 has been delivered | Delivered: "Bounty Essentials..." |
| Oct 25 | Your order #1001 has shipped | Shipped: "Bounty Essentials..." |
| Oct 24 | Your order #1002 has shipped | Shipped: "Desk lamp..." |
| Oct 23 | Your order #1001 is confirmed | Ordered: "Bounty Essentials..." |

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

Never write three or more clauses in one sentence, or three or more example
sentences in a row. In ordinary prose, a sentence may have one or two related
clauses. Do not pack three or more clauses into one prose sentence. If you need
that many points, use a numbered First / Second / Third sequence as described
under [Present sequences in order](#present-sequences-in-order), or short bullet
points when you are writing a brief. If list points are examples and you want
to inline them, introduce with "e.g.". Also do not give three or more example
sentences back to back to support the same point.

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
pattern across them. In a study of sentences with two clauses joined by "and",
readers read the second clause faster when it had the same structure as the
first, e.g., when both clauses were active or both were passive.[^5]

#### Issue summaries

Notion AI generated the summaries below from public Ollama issues.[^8] The
summaries start inconsistently with "The issue reports that", "The page
reports", or an action, "Pressing Ctrl +C". They also call the same thing an
"issue" in some rows and a "page" in another, which breaks the
[terminology rule](#use-consistent-terminology-and-constrain-your-vocabulary).
The bold text marks each opening.

| Issue | Summary | Annotation |
| --- | --- | --- |
| Claude Desktop Conversation: ~17K initial prefill, HTTP 500 retries, MLX cache loss (0.40.11–0.40.12) | **The issue reports that** Claude Desktop Conversation experiences a long (~70–90 s) initial prefill of ~17 K tokens, frequent HTTP 500 errors, and cache loss on MLX versions 0.40.11–0.40.12, despite later messages being faster. | |
| ollama show --modelfile writes a Modelfile that ollama create rejects when a SYSTEM/TEMPLATE/LICENSE/MESSAGE value contains """ followed by whitespace | **The issue reports that** `ollama show --modelfile` incorrectly wraps values containing triple quotes followed by whitespace, causing unparseable Modelfiles that fail to recreate the model. | |
| Error reading GGUF metadata on large MoE model with MXFP4 format | **The page reports** an error reading GGUF metadata for a large MoE model in MXFP4 format due to an oversized unsupported tensor, references a similar bug, and includes a link to the related GitHub issue. | Calls the issue a “page” |
| ollama run: Ctrl+C during """ multiline input leaves session in multiline mode, so later input is swallowed or saved as the system message | **Pressing Ctrl +C** while entering a """ multiline message in ollama run clears the buffer but leaves the REPL in multiline mode, so the following input is mistakenly treated as a continuation or system message. | Starts with an action |

#### News headlines

The Perplexity Discover headlines below use a consistent opening, with the
subject followed by an action or a planned action.[^9] For example, "Bank of
Canada hikes rate to 2.5%" and "Brewers beat Rays in Game 5 for first World
Series title" both identify the subject before stating what happened.

| Headline | Summary | Sources |
| --- | --- | --- |
| Brewers beat Rays in Game 5 for first World Series title | Jacob Misiorowski blanked Tampa Bay over 5 2/3 innings as Milwaukee won 4-1, capping an 11-2 postseason run. | 59 |
| Bank of Canada hikes rate to 2.5%, first since July 2023 | | 30 |
| Weil Gotshal to wind down after partner exodus | | 31 |
| HHS lets pharmacies make six unapproved peptides | | 15 |

### Use variation only to show a meaningful difference

Vary length, structure, or wording only when the difference conveys a fact the
reader needs. For comparable summaries, use the same components and show
important differences explicitly, e.g., with review counts.

The rule applies a principle from visualization design to text. A chart should
express all the facts in its data, and only those facts.[^10] A difference in
color or shape suggests a difference in meaning.[^11] In related text items,
differences in length, structure, or detail can create similar expectations.

#### User feedback themes

Claude generated 11 themes from 100 Duolingo App Store reviews; five appear
below.[^12] The original summaries include different kinds of detail. Only the
bugs summary names an app version, 7.146.0, while only the energy summary names
Russian and Turkish reviews. Readers cannot tell whether the differences are
significant or whether other themes also involve the same version or languages.

The rewritten summaries use short statements about what users say, with
examples from the reviews. Review counts appear separately in a column. The
original summaries remain alongside the rewrites so readers can compare the
detail.

| Theme | Claude's version | Rewritten version | Reviews per theme |
| --- | --- | --- | --- |
| Fun, easy, and effective learning | Most reviews praise Duolingo as fun, easy, and helpful for learning, often in short 5-star notes. Users cite new vocabulary, pronunciation help, self-paced lessons, and being able to talk with family or friends. Spanish is the most named language, followed by Japanese, Italian, Korean, German, and others. Several of these reviewers still raise a complaint, such as energy limits or cost. | Users love that the app is fun and helps them learn, e.g., new vocabulary and pronunciation. | 74 |
| Chess, music, and math courses | Chess is a popular new draw, and some users mention music, piano, and math too. Feedback is mostly positive, but there are specific complaints: a checkmate rule that seems wrong, no way for skilled players to skip ahead, a distracting new owl voice and hard-to-see pieces in the chess puzzle, and many clicks between games. | Users enjoy the new courses but complain about problems, e.g., a checkmate rule that seems wrong. | 12 |
| Bugs and technical problems | Reports include voice answers not registering, the app freezing (one user lost a streak), the app crashing on launch, finished lessons not counting, leaderboard members disappearing, a new event missing for one family-plan member, and a shared iPad starting a new learner at the wrong level. Most come from the current version, 7.146.0. | Users complain about bugs that cost them progress, e.g., a freeze that lost a daily streak. | 9 |
| Energy and battery limits on lessons | The energy (battery) system, and the older hearts system, limits how much users can practice. Users say they run out fast, must wait or watch ads, and see it as a push to buy Super. Some reviews are in Russian and Turkish, so the complaint shows up across markets. One user suggests earning energy by doing chess, music, or math. | Users complain that they run out of energy fast and must wait to keep practicing. | 8 |
| New language requests | Users ask for new courses. Two ask for Icelandic, noting that Duolingo already offers other Nordic languages, and one asks for Elvish from The Lord of the Rings. | Users request new courses, e.g., Icelandic. | 3 |

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

## Sources and notes

[^1]: Thomas G. Bever. The Cognitive Basis for Linguistic Structures. In J. R. Hayes, editor, *Cognition and the Development of Language*, pages 279–362. Wiley, New York, 1970. Bever describes the strategy as reading a noun, a verb, and a noun as the actor, the action, and the object of the action.

[^2]: Lyn Frazier and Keith Rayner. [Making and Correcting Errors During Sentence Comprehension: Eye Movements in the Analysis of Structurally Ambiguous Sentences](https://doi.org/10.1016/0010-0285(82)90008-1). *Cognitive Psychology*, 14(2):178–210, 1982. In an eye-tracking study, readers of sentences with a misleading start moved their eyes back to earlier words to parse the sentence again.

[^3]: Jakob Nielsen. [F-Shaped Pattern for Reading Web Content (Original Study)](https://www.nngroup.com/articles/f-shaped-pattern-reading-web-content-discovered/). Nielsen Norman Group, April 16, 2006. The study tracked the eyes of 232 users as they read thousands of web pages.

[^4]: Jakob Nielsen. [First 2 Words: A Signal for the Scanning Eye](https://www.nngroup.com/articles/first-2-words-a-signal-for-scanning/). Nielsen Norman Group, April 6, 2009.

[^5]: Lyn Frazier, Lori Taft, Tom Roeper, Charles Clifton, and Kate Ehrlich. [Parallel Structure: A Source of Facilitation in Sentence Comprehension](https://link.springer.com/article/10.3758/BF03198303). *Memory & Cognition*, 12(5):421–430, 1984. In 2010, Sturt, Keller, and Dubey found that readers also read a phrase faster when it has the same structure as an earlier phrase, even when "and" does not join the two phrases: Patrick Sturt, Frank Keller, and Amit Dubey. [Syntactic Priming in Comprehension: Parallelism Effects with and without Coordination](https://doi.org/10.1016/j.jml.2010.01.001). *Journal of Memory and Language*, 62(4):333–351, 2010.

[^7]: E.g., a core rule of [ASD Simplified Technical English](https://www.asd-ste100.org/about.html), a writing standard for aircraft maintenance manuals, is "one word, one meaning." Also, Jakob Nielsen's fourth usability heuristic says that users "should not have to wonder whether different words, situations, or actions mean the same thing." See Jakob Nielsen. [10 Usability Heuristics for User Interface Design](https://www.nngroup.com/articles/ten-usability-heuristics/). Nielsen Norman Group, 1994.

[^8]: Shreya Shankar copied 10 open issues from [ollama/ollama](https://github.com/ollama/ollama/issues) on October 28, 2026, and added a Notion AI autofill property named "Summary" with its default settings. The [issue summaries](#issue-summaries) table preserves the summaries of issues [#19359](https://github.com/ollama/ollama/issues/19359), [#19342](https://github.com/ollama/ollama/issues/19342), [#19375](https://github.com/ollama/ollama/issues/19375), and [#19340](https://github.com/ollama/ollama/issues/19340).

[^9]: [Perplexity Discover](https://www.perplexity.ai/discover), accessed October 28, 2026.

[^10]: Jock Mackinlay. [Automating the Design of Graphical Presentations of Relational Information](https://doi.org/10.1145/22949.22950). *ACM Transactions on Graphics*, 5(2):110–141, 1986.

[^11]: Jeff Heer. [CSE 512: Data Visualization](https://courses.cs.washington.edu/courses/cse512/24sp/). University of Washington, Spring 2024. See the lectures on data and image models and on visualization design.

[^12]: Shreya Shankar downloaded the 100 most recent US App Store reviews of Duolingo from Apple's public review feed on October 28, 2026; the reviews are from October 26 and 27, 2026. Claude ran the `data:analyze` skill of Claude's data analysis plugin on them and wrote 11 theme cards. The [user feedback themes](#user-feedback-themes) table preserves five of the original summaries and Shankar's rewrites.

[^13]: Shankar cites related work by Kousta, Vinson, and Vigliocco, while noting that the work was suggested by Claude and had not been read closely for the article. The study found that people recognize emotion words, positive or negative, faster than neutral words. See Stavroula-Thaleia Kousta, David P. Vinson, and Gabriella Vigliocco. [Emotion Words, Regardless of Polarity, Have a Processing Advantage over Neutral Words](https://doi.org/10.1016/j.cognition.2009.06.007). *Cognition*, 112(3):473–481, 2009.
