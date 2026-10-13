---
name: building-skills
description: "Defines reusable skills, including their scope, activation conditions, SKILL.md structure, and references. Applies when creating a skill, revising its scope or organization, or turning an established practice into a skill."
---

# Building skills

Building a skill means writing a `SKILL.md` that defines a capability the agent can reuse, when the
agent should use it, and the context the agent needs to apply it. When you revise an existing skill,
keep the behavior it's meant to produce and the constraints it depends on.

The `writing-prompts` skill covers how to write model instructions that produce the intended
behavior, including the instructions inside a skill.

## Principles

### Let the agent decide when to use the skill

Describe when the skill applies, so the agent can use it whenever the situation calls for it, e.g.,
in the middle of a feedback loop, without the user having to ask for it.

### Describe the result instead of the steps

Define the result you want, the constraints it has to meet, and what success looks like. Prescribe
an order of steps only when the order is needed for correctness or safety.

### Give the agent context, and trust what it already knows

Name a practice instead of spelling out its steps, e.g., "prepare a handoff" or "red-team the
feature". Supply the scope, the local context, and the constraints, and leave the agent room to
choose how to do the work. Don't teach concepts the model already knows.

### Keep the skill simple, even when that takes more work to write

Keep separate concerns apart, and avoid complexity that the task doesn't need. For example, keep
what the task is for separate from the details of the tools used to do it. Spend more thought while
writing the skill rather than adding complexity to the skill itself. Keep the intended result and
the constraints it depends on, and don't aim for either complete coverage or the fewest words.

### Put details in reference files that load when needed

Keep in `SKILL.md` what the agent needs every time: when to use the skill, the result, and the
constraints. Put detailed documentation, examples, and templates in reference files, and say when
to read each one.

### Use existing tools for doing and checking the work

Find the tools that are already available, and build on them to do the work and check it. Where you
can, get feedback from automated checks such as linters and test runners, rather than relying on
the agent to inspect its own work. Don't restate a tool's rules in prose, and don't build custom
enforcement for a preference that depends on context.

### Make mistakes hard to make

Prefer an arrangement that prevents a likely mistake, or makes it obvious right away, over a warning
the agent has to remember. Use the simplest safeguard that works for that kind of failure.

## Writing the skill

### Folder, name, and description

Each skill is a folder that contains a `SKILL.md` file. Start the file with YAML frontmatter that
holds `name` and `description`, and follow it with the Markdown body.

- **Name.** Use a lowercase name that describes the capability, with hyphens between words. It has
  to match the folder name and be at most 64 characters. Prefer the "-ing" form of a verb, as in
  `shaping`, `implementing`, `naming-things`, and `explaining-code`. The "-ing" form is a naming
  convention, and the shared specification doesn't require it.
- **Description.** Write in the third person, and say both what the skill does and when it applies.
  Use the specific words a request would contain, and focus on what the skill can do and when it
  should load. The specification allows at most 1,024 characters. Quote a value that contains YAML
  punctuation, such as a colon, so it parses as one string.
- **Optional fields.** Include other fields from the shared specification when they serve a
  concrete purpose.
- **Credits.** When a skill is based on someone else's work, such as another skill, an article, or
  a book, credit that work in the plugin's README. A reference file can link to its source
  material, so the agent can read more when it needs context. Keep any license notice with the
  files it covers.

For example, `organizing-commits` uses this frontmatter:

```yaml
name: organizing-commits
description: Reorganizes a branch's committed and uncommitted changes into logical, reviewable commits that each leave the code working, without changing the final code. Use when asked to restack, clean up, split, squash, or reorganize a branch's commits before review.
```

The agent sees only the name and description before the body loads, so put the conditions for using
the skill in the description, and don't rely on instructions inside the body for that.

### Body

Start with a title and a short statement of the skill's purpose. Organize the rest around the
capability, the result, and the constraints, and choose headings that fit the task. Include a
concrete example when it makes an important difference clear.

A section can offer several approaches without making them a sequence, as in this example:

> Use the approaches below as needed. They aren't a required sequence or separate skills to load.

### Reference files and other supporting material

Keep the capability, the conditions for using it, and the constraints in `SKILL.md`. Link to
detailed explanations, examples, or templates when only part of the work needs them. Say when to
read or use each file, and write each link relative to the skill's folder.

When the skill mentions a script or a tool, state its purpose, and explain the inputs or outputs the
agent needs to understand. Keep a short example in `SKILL.md` when a separate file would add
nothing.

## Reviewing the skill

Check that the description separates the requests the skill should handle from similar tasks it
shouldn't handle. Check that the body agrees with the intended result and constraints. Remove
instructions that conflict, repeat each other, or don't belong, without losing context the model
needs.

Use an available parser or validator to check the frontmatter, the name, and the required fields
against the [Agent Skills specification](https://agentskills.io/specification), and check that every
referenced file exists. Format and link checks don't show that the skill produces the intended
behavior, so report what you checked without implying that you ran a behavioral evaluation.
