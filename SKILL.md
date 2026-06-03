---
name: lean-blueprint-author
description: Generate insertion-ready leanblueprint LaTeX for a specified Lean/formalization target from local reference materials. Use when the user asks for a Lean or leanblueprint blueprint, proof route, dependency route, or formalization roadmap from papers, notes, or other reference files.
---

# Lean Blueprint Author

Use this skill to turn source materials into the minimal leanblueprint route needed to state and prove one user-specified target proposition.

## Required Inputs

- Target proposition: always require the exact theorem/proposition/result before generating blueprint LaTeX. If missing, ask for it and stop.
- Materials: use the user-specified material directory or file list. If none is specified, default to `PROJECT_ROOT/reference`.
- Selection: if the material location contains multiple plausible source files and the user did not select them, list the candidates and ask which one or more to use. Stop until they choose.

Interpret `PROJECT_ROOT` as the Lean project root, preferably the directory containing `lakefile.lean` or `lakefile.toml`.

## Pre-Output Guard

Before generating blueprint LaTeX, check that leanblueprint has already been initialized by verifying these files under `PROJECT_ROOT/blueprint/src`:

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

If any are missing, do not generate the blueprint. Tell the user to manually run `leanblueprint new` from the Lean project root, then retry.

## Workflow

Before choosing blueprint content, read and follow
`references/blueprint-generation-principles.md`.

1. Locate the target proposition in the selected materials. Quote or paraphrase only enough to identify it.
2. Build a recursive dependency route according to the blueprint generation principles.
3. Minimize and topologically order the route so every dependency appears before its first use.
4. Generate the blueprint LaTeX using the output contract below.

## Output Contract

Produce only LaTeX suitable for direct insertion into `blueprint/src/content.tex`; omit `\begin{document}` and other wrapper boilerplate.

- Use leanblueprint theorem-like environments such as `definition`, `lemma`, `proposition`, `theorem`, and `corollary`.
- Give every node a stable lowercase label, for example `def:local_name`, `lem:key_estimate`, `prop:intermediate_result`, `thm:target_result`, `obs:source_observation`.
- Put statement dependencies in `\uses{...}` on the theorem-like environment.
- Put proof-only dependencies in `\uses{...}` inside the `proof` environment.
- Use `\lean{...}` only when the Lean declaration name is known from existing code, known from Mathlib, or is a clearly proposed local declaration name.
- Never add `\leanok` unless that exact declaration is already formalized in the project.
- Use `\mathlibok` only for known Mathlib results.
- For source observations or cited external results that are not Lean declarations, state them as blueprint nodes with labels and citation/context, but do not pretend they are formalized.
- Keep prose concise and mathematical; write in the user's language when practical.

Prefer this shape:

```tex
\begin{lemma}\label{lem:key_step}\uses{def:main_object}
\lean{keyStep}
Statement of the key step.
\end{lemma}
\begin{proof}\uses{obs:source_estimate, lem:auxiliary_bound}
Proof route from the selected material.
\end{proof}

\begin{theorem}\label{thm:target}\uses{def:main_object}
\lean{targetTheorem}
Statement of the requested target proposition.
\end{theorem}
\begin{proof}\uses{lem:key_step}
Proof route of the target.
\end{proof}
```
