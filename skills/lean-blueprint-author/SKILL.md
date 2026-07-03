---
name: lean-blueprint-author
description: Generate leanblueprint routes, proof routes, dependency routes, or formalization roadmaps for Lean targets from local references. Use when the user asks to create a blueprint, modify an existing blueprint's structure or content, or sync blueprint labels with updated Lean code.
---

# Lean Blueprint Author

## Shared Prerequisites

Before any operation, determine which operation is requested (create, modify,
or update) and satisfy its prerequisites:

| Prerequisite           | Create | Modify | Update |
|------------------------|--------|--------|--------|
| Target proposition     | ✅ Required | Optional (may be unchanged) | ❌ |
| Reference materials    | ✅ Required | Optional (if sourcing alternative routes) | ❌ |
| Existing blueprint     | ❌ | ✅ Required (file or node range) | ✅ Required |
| Project `.lean` code   | ❌ | ❌ | ✅ Required |

### Project Scaffold Guard

Run this guard only when the task will read from, write to, or validate an
actual leanblueprint project tree. Do not run it for standalone insertion-ready
LaTeX, pasted blueprint node ranges, or roadmap-style chat output.

When the guard applies, verify these files exist under
`PROJECT_ROOT/blueprint/src`:

- `content.tex`
- `web.tex`
- `print.tex`
- `blueprint.sty`
- `plastex.cfg`
- `latexmkrc`
- `extra_styles.css`
- `macros/common.tex`
- `macros/print.tex`
- `macros/web.tex`

If any are missing, stop. Tell the user to run `leanblueprint new` from the
Lean project root, then retry.

Interpret `PROJECT_ROOT` as the Lean project root, preferably the directory
containing `lakefile.lean` or `lakefile.toml`.

## Shared Foundations

Before choosing blueprint content for any operation, read and follow
`references/foundations.md`.

## Shared Output Contract

Before producing blueprint LaTeX, status manifests, diffs, or roadmap output,
read and follow `references/output-contract.md`.

## Branch Dispatch

Determine the operation from the user's request:

- **Create**: building a blueprint route from reference materials for a target
  proposition. Read `references/create-workflow.md`.
- **Modify**: changing an existing blueprint's structure, content, or proof
  route. Read `references/modify-workflow.md`.
- **Update**: syncing a blueprint with changes in formalization code. Read
  `references/update-workflow.md`.

Follow only the workflow for the determined operation. If the operation is
unclear, ask the user to choose among create, modify, or update.
