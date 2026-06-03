# Lean Blueprint Author

`lean-blueprint-author` is a Codex skill for generating insertion-ready
leanblueprint LaTeX from local mathematical reference materials.

It is intended for Lean formalization projects that use
[leanblueprint](https://github.com/PatrickMassot/leanblueprint) to track theorem
statements, dependencies, proof routes, and formalization status.

## What It Does

Given a user-specified target proposition and a directory of reference
materials, the skill guides an agent to:

- locate the target result in the selected materials;
- recursively extract the statement and proof dependencies;
- keep only the minimal route needed to state and prove the target;
- omit unused background theory and unrelated exposition;
- produce paste-ready LaTeX for `blueprint/src/content.tex`.

The generated blueprint uses theorem-like environments, stable labels,
`\uses{...}` dependency annotations, and conservative Lean metadata such as
`\lean{...}`, `\leanok`, and `\mathlibok`.

## Trigger

Use this skill when asking an agent to generate a Lean or leanblueprint
blueprint, proof route, dependency route, or formalization roadmap from papers,
notes, or local reference files.

Example prompts:

```text
Generate a leanblueprint route for Theorem 3.2 from reference/paper.pdf.
```

```text
Use the notes in reference/ and write the blueprint dependencies for the
compactness theorem.
```

## Inputs

The skill expects:

- a target proposition, theorem, or result;
- reference materials, either explicitly selected by the user or defaulting to
  `PROJECT_ROOT/reference`;
- an initialized leanblueprint project.

If the target is missing, the agent must ask for it before generating output.
If multiple materials are available and none are selected, the agent must ask
which files to use.

## Safety Guard

Before generating LaTeX, the skill checks that leanblueprint has already been
initialized by verifying the standard `blueprint/src` files, including
`content.tex`, `web.tex`, `print.tex`, `blueprint.sty`, and the default macro
files.

If those files are missing, the agent stops and asks the user to manually run:

```sh
leanblueprint new
```

from the Lean project root.

## Output Shape

The skill produces only LaTeX suitable for direct insertion into
`blueprint/src/content.tex`. It does not emit document wrappers such as
`\begin{document}`.

Typical output:

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

## Installation

Install this repository as a Codex skill by placing it in a Codex skills
directory, or by using whichever local skill installation workflow your Codex
environment supports.

The required skill file is:

```text
SKILL.md
```
