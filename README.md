# Lean Blueprint Author Skills

This repository is a multi-skill source repository for mathematical reading and
Lean blueprint authoring workflows.

## Skills

### `lean-blueprint-author`

Path: `skills/lean-blueprint-author`

Generates insertion-ready leanblueprint LaTeX for a specified Lean
formalization target from local reference materials. Use it for Lean
blueprint routes, proof routes, dependency routes, and formalization roadmaps.

Default prompt:

```text
Use $lean-blueprint-author to generate a leanblueprint route for the specified theorem from these reference materials.
```

### `math-paper-reader`

Path: `skills/math-paper-reader`

Reads mathematical papers and formulas with visual PDF checks, notation
ledgers, theorem/proof interpretation, equation references, and ambiguity
review. Use it before formalization when the task is understanding what a
paper says.

Default prompt:

```text
Use $math-paper-reader to read this mathematical paper and explain the key formulas with page or equation references.
```

## Installation

Install a skill by placing the desired skill folder under your Codex skills
directory.

```sh
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R skills/math-paper-reader "${CODEX_HOME:-$HOME/.codex}/skills/"
cp -R skills/lean-blueprint-author "${CODEX_HOME:-$HOME/.codex}/skills/"
```

Install only the skill folders you want to expose.

## Repository Layout

```text
skills/
  lean-blueprint-author/
    SKILL.md
    agents/openai.yaml
    references/blueprint-generation-principles.md
  math-paper-reader/
    SKILL.md
    agents/openai.yaml
    references/formula-reading-workflow.md
    references/math-paper-reading-checklist.md
```

There is intentionally no root-level `SKILL.md`; each skill is installed from
its own folder under `skills/`.

## Validation

Validate each skill folder with the skill-creator validator:

```sh
python3 /home/raibunitsu/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/lean-blueprint-author
python3 /home/raibunitsu/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/math-paper-reader
```
