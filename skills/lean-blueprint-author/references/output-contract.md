# Output Contract

Apply these constraints to blueprint LaTeX, minimal diffs, sync manifests, and
roadmap output.

## Leanblueprint Nodes

- Use leanblueprint theorem-like environments: `definition`, `lemma`,
  `proposition`, `theorem`, `corollary`.
- Give every node a stable lowercase label with one of these prefixes:
  `def:...`, `lem:...`, `prop:...`, `thm:...`, `cor:...`, `fact:...`.
- Use `cor:` only for corollaries.
- Use `fact:` only for source observations or cited external results that the
  route depends on but that are not existing Lean declarations.
- Do not use `obs:` or `ext:` labels.
- Put statement dependencies in `\uses{...}` on the theorem-like environment.
- Put proof-only dependencies in `\uses{...}` inside the `proof` environment.

## Lean Status Markers

- Use `\lean{...}` only when the Lean declaration name is known from existing
  project code, known from Mathlib, or is a clearly proposed local declaration
  name.
- Never add `\leanok` unless that exact declaration is already formalized in
  the project.
- Use `\mathlibok` only for known Mathlib results.
- For source observations or cited external results represented by `fact:`
  nodes, include citation or local source context, but do not pretend they are
  formalized.

## Style

- Keep prose concise and mathematical.
- Write in the user's language when practical.
