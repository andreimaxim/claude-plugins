---
name: explaining-code
description: Explains how code and software systems work, why they were designed that way, and what a change does, using plain prose with call trees, diagrams, pseudocode, and diffs. Use when the user asks how something works, for architecture walkthroughs, runtime and data flows, state transitions, or ownership questions, for the history behind a design decision, or when the user asks what a change does, such as a diff, a commit, a pull request, or the work just done in this session.
---

# Explaining code

Explaining code helps the user understand how existing code works, why it was designed that way, or
what a change does. Lead with the answer in plain technical prose, and add a view such as a call
tree or a diagram only when it makes the answer easier to follow.

Inspired by HumanLayer's [show-me](https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md)
skill and by the explaining-code skill in [Amp's official plugins](https://github.com/andreimaxim/amp-official-plugins/blob/main/skills/explaining-code/SKILL.md).

## Find out what the code does

Read the relevant code before you explain it. Start from the entry point that best matches the
question, e.g., a public method, a command, a request, an event, a job, or a user action. Then
follow the path through the calls, state, data, and integrations that produce the result the user
asked about.

Read only as much of the system as the question needs. Use tests and contracts to confirm edge cases
and invariants when they affect the answer. Keep what you saw in the code separate from what you
infer, and say when you're unsure, instead of presenting a plausible explanation as fact.

Explain one path or one decision at a time. Start with the answer to the user's question, and then
show the code or evidence that supports it. Name the functions, types, files, data, and ownership
boundaries the reader needs, and leave out implementation details that don't bear on the question.
Use the repository's own terms for its concepts, and define a technical term when the user may not
know it.

## Find out why it was designed that way

When the user asks why a design exists or where it came from, investigate the decision as well as
the current code. A question about why the code produces a particular result usually needs only an
explanation of the current code.

For a question about history, trace the changes that introduced the behavior or changed it in an
important way. The last commit to touch a line is often unrelated to why the line exists. Read the
pull request discussions, tickets, design records, and incident reports that bear on the question,
when you can reach them. Look for the constraints, alternatives, and tradeoffs that shaped the
choice, and keep the investigation in proportion to the question.

Keep three kinds of claims apart in the answer. The first is what the code does, and the second is
what the historical record says. The third is what you infer from the evidence, and you should
explain the evidence for an inference instead of presenting it as recorded intent. For example, a
commit that changes a limit from 50 to 100 shows the new limit, but it doesn't show why 100 was
chosen. A benefit the code provides today doesn't show why it was first written, or that the
original constraint still applies. When the user suggests a reason, treat it as a hypothesis to
check, and don't set out to confirm it.

Cite the sources for historical claims, and explain any contradictions between them. When the
reason is still unknown, say what the sources you read establish and what is still open. Mention
missing records, places you couldn't search, or access you didn't have when they affect the answer.
Not finding a reason doesn't show that there wasn't one.

## Choose a view

Pick the smallest view that makes the answer clear, or use prose alone when prose is clearer.

| What the reader needs to see | View |
| --- | --- |
| Logic or an algorithm | Pseudocode |
| The order of calls at runtime | Call tree |
| Which component owns UI state or behavior | Component tree |
| Which file or module is responsible for what | Shallow file tree |
| An exact contract, data shape, or signature | Code |
| Interactions, data flow, states, or architecture | Line drawing |

Place each view next to the sentence it supports, and keep only the calls, files, values, states,
and boundaries that bear on the question. You can use one view or several, but you'll rarely need
all of them. Each view should add information that the prose doesn't already give.

Write code in the repository's language. When a path crosses languages, show each side in its own
language, and use pseudocode when the syntax would distract from the behavior. Show a complete
block when leaving parts out would hide the order of steps or who owns them.

A call tree shows the order and nesting of the calls that decide the result, and it doesn't need
every function on the stack. Add the file for each call when the reader needs to know which file
owns it, as in this TypeScript example:

```text
formatFile(path)                  src/formatter.ts
  readFile(path, "utf8")          node:fs/promises
  formatText(original)            src/text.ts
  if formatted !== original
    writeFile(path, formatted)    node:fs/promises
```

A component tree shows where UI behavior lives. Add only the state, hooks, files, and package
boundaries that explain it:

```tsx
<CheckoutPage>                    app/routes/checkout.tsx
  useCart()
  <PaymentForm>                   packages/payments
    usePaymentIntent()
  <OrderSummary>
```

A shallow file tree shows which part of the code is responsible for what, as in this Go project:

```text
.
├── cmd/format/main.go          # parses command line arguments
├── internal/format/format.go   # formats content and skips unchanged writes
└── internal/files/files.go     # reads and writes files
```

### Line drawings

Draw diagrams as Unicode line art in a `text` code block, because the terminal and the Claude apps
both show it exactly as written. Don't use Mermaid in a reply, because the terminal and many other
places where people read Claude Code show its source code instead of a drawing.

Draw the main structure with light lines (`─ │ ┌ ┐ └ ┘ →`). Draw the one path, state, or step that
answers the question with heavy lines (`━ ┃ ┏ ┓ ┗ ┛ ▶ ▼`), so the reader sees it first. Use concrete
labels, such as the function names, queue names, and states from the code.

```text
┌──────────┐     ┌───────────┐     ┌──────────┐
│ Request  │────→│ Validate  │────→│ Persist  │
└──────────┘     └─────┳─────┘     └──────────┘
                       ┃ invalid
                       ▼
                 ┏━━━━━━━━━━━┓
                 ┃ 422 error ┃
                 ┗━━━━━━━━━━━┛
```

### Rendered pages

Some ideas are too dense for line art, e.g., a visual UI, a page layout, or a comparison of several
states. If the session has a tool that shows the user a rendered page, such as an artifact tool, you
can make one focused HTML page for such an idea. Choose a diagram, an infographic, or a short slide
deck, depending on the point. Match the product's colors, type, spacing, and components, use the
product's own labels and data, and make the page work on both desktop and mobile. If the session
has no such tool, use line art, and offer to write the page as a file the user can open.

## Explain a change

A change is anything that makes the code behave differently than it did before, e.g., a diff, a
commit, a pull request, a fix, a proposal, or the work just done in this session. Show a change with
a drawing by default, because the reader then sees the difference at once, while prose alone makes
them rebuild both versions in their head. Draw what the system does, rather than which lines were
edited, e.g., the path a request takes, the states an object moves through, the order of calls, who
owns the data, or what the user sees on screen.

Choose the form by what the reader has to compare:

- When behavior was replaced, rerouted, or removed, draw a "Before" and an "After" in one block. Use
  the same layout and labels in both, so only the difference moves. Put them side by side when the
  block stays under about 70 columns, and otherwise put "Before" above "After". For a small change,
  two aligned lines labeled "Before" and "After" are enough.
- When behavior was added and the earlier state was only its absence, draw the result alone.
- When the exact text is the point, such as a signature, a config value, or a schema field, show a
  focused `diff` block instead of a drawing.

Draw the unchanged parts with light lines and the changed parts with heavy lines. Write an important
consequence, such as a latency, a count, or an error, next to the part that causes it.

State the change in one sentence, and put the drawing right after it. After the drawing, add only
what the drawing can't show, e.g., the reason for the change, the edge cases, and the risks. A
change with nothing to draw, such as a rename, a typo fix, or a new constant value, gets one
sentence and no drawing.

For example, the signup handler no longer sends the welcome email itself, and a background job sends
it instead:

```text
Before                          After

┌──────────┐                    ┌──────────┐
│ Signup   │                    │ Signup   │
└────┬─────┘                    └────┳─────┘
     │ deliver_now                   ┃ deliver_later
     ↓                               ▼
┌──────────┐                    ┏━━━━━━━━━━┓
│ SMTP     │                    ┃ Job      ┃
└──────────┘                    ┗━━━━┳━━━━━┛
                                     ┃ 5 retries
signup waits up to 10 s              ▼
                                ┌──────────┐
                                │ SMTP     │
                                └──────────┘

                                signup returns in 40 ms
```

When behavior was added, the result alone is enough. For example, an upload that fails its virus
scan now goes to quarantine instead of staying in the pending state:

```text
pending ──→ scanning ──→ stored
               ┃
               ┃ scan fails
               ▼
           ● quarantined
```

### Diffs of structure

A `diff` block doesn't have to be a diff of source code. Match it to the view the user is already
looking at, so they can see the change without translating edited lines back into the system. For
example, here is a change to a call tree:

```diff
 submitOrder
   validateCart
+  reserveStock
   chargeCard
-  sendReceipt
+  enqueueReceipt
```

Here is a change to a file layout:

```diff
 app/
 ├── models/
-├── billing.rb
+├── billing/
+│   ├── invoice.rb      # builds invoices
+│   └── charge.rb       # talks to the payment provider
 └── controllers/
```

When the exact source is the point, show a focused source diff. For example, the following Rust
diff skips the write when the content hasn't changed:

```diff
 fn format_file(path: &Path) -> io::Result<()> {
     let original = fs::read_to_string(path)?;
     let formatted = format!("{}\n", original.trim());
-    fs::write(path, formatted)
+    if formatted != original {
+        fs::write(path, formatted)?;
+    }
+    Ok(())
 }
```

Show the whole block instead of a diff when most of it is new, when a diff would hide the order of
steps or who owns them, or when the user needs a target they can copy.

## Stay within the question

Explain the system as it is, and don't redesign it in the middle of an explanation. If the user asks
for an evaluation, keep the factual walkthrough separate from your judgment. If the user asks about
a proposed change, keep the current behavior separate from the proposed behavior.

Before you finish, check the explanation against these points:

- It answers the question the user asked.
- Evidence you inspected supports each important claim, and inferences are marked as inferences.
- It uses the same term for the same thing throughout.
- No view is more complex than the behavior it explains.
