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

## Folder, name, and description

Each skill is a folder that contains a `SKILL.md` file. Start the file with YAML frontmatter that
holds `name` and `description`, and follow it with the Markdown body:

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

For example, `organizing-commits` uses this frontmatter:

```yaml
name: organizing-commits
description: Reorganizes a branch's committed and uncommitted changes into logical, reviewable commits that each leave the code working, without changing the final code. Use when asked to restack, clean up, split, squash, or reorganize a branch's commits before review.
```

The agent sees only the name and description before the body loads, so put the conditions for using
the skill in the description, and don't rely on instructions inside the body for that. Describe
when the skill applies so the agent can use it whenever the situation calls for it, e.g., in the
middle of a feedback loop, without the user having to ask for it.

## Writing the body

Start with a title and a short statement of the skill's purpose. Organize the rest around the
capability, the result, and the constraints, and choose headings that fit the task. Use the
`writing-prompts` skill to write the instructions in the body and reference files.

### Disclose context progressively

Keep in `SKILL.md` what the agent needs every time, so the agent loads detailed material only when
a step calls for it. Put detailed documentation, examples, and templates in `reference/`,
executable helpers in `scripts/`, and static files such as icons in `assets/`:

```text
skill-name/
├── SKILL.md
├── reference/
│   ├── guide.md
│   └── template.md
├── scripts/
│   └── validate.py
└── assets/
    └── icon.png
```

Say in `SKILL.md` when to read or run each file, and write each path relative to the skill's
folder. Keep a short example in `SKILL.md` when a separate file would add nothing.

## Reviewing the skill

Check that the description separates the requests the skill should handle from similar tasks it
shouldn't handle. Check that the body agrees with the intended result and constraints. Remove
instructions that conflict, repeat each other, or don't belong, without losing context the model
needs.

Use an available parser or validator to check the frontmatter, the name, and the required fields
against the [Agent Skills specification](https://agentskills.io/specification), and check that every
referenced file exists. Format and link checks don't show that the skill produces the intended
behavior, so report what you checked without implying that you ran a behavioral evaluation.
