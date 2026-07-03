# Create Workflow

Follow this workflow when building a new blueprint route from source materials
for a user-specified target proposition.

## Core Boundary

Generate a blueprint route for one exact user-specified target proposition.
Every included node must be needed to state that target, prove that target, or
make an explicitly requested user modification traceable.

Use only the selected source materials, existing project code, known Mathlib
facts, and clearly marked proposed local statements. Do not introduce broad
background theory merely because it appears near the target in the source.

## Workflow

### 1. Locate the Target Proposition

Find the target in the selected materials. Quote or paraphrase only enough to
identify it.

### 2. Build a Recursive Dependency Route

Starting from the target, identify the dependencies needed to state it and the
dependencies needed to prove it. Repeat the same analysis for each newly
introduced dependency until every leaf is one of:

- an existing project declaration;
- a known Mathlib fact;
- a source observation from the selected material;
- a directly cited external result;
- a proposed local lemma or observation;
- a primitive notion that does not need its own blueprint node.

**Do not stop after finding the immediate dependencies of the target.** A
blueprint route is complete only when every non-leaf node has its own
statement and proof dependencies resolved or explicitly marked as
proposed/source/external.

When recursion exposes an unused branch, remove it. When recursion exposes a
missing bridge, add only the weakest proposed local statement needed to close
that branch.

### 3. Minimize and Topologically Order

Default to the weakest direct route that states and proves the requested
target. Keep only nodes required by that route.

When the source gives a more general theorem than the target needs, prefer the
specialized form if it is enough for the target and no later selected node
needs the general form.

When the source offers multiple proof routes, choose the route with the fewest
new formalization obligations unless the user selects another route.

Topologically order content so each node appears before its first use. Cycles
or late dependencies indicate that the route has not been resolved.

### 4. Generate the Blueprint LaTeX

Produce LaTeX according to the shared output contract and the output shape
below.

## Output Shape

Produce only LaTeX suitable for direct insertion into
`blueprint/src/content.tex`; omit `\begin{document}` and other wrapper
boilerplate. Apply the shared output contract from
`references/output-contract.md`.

Prefer this shape:

```tex
\begin{proposition}\label{fact:source_estimate}
Source-context statement of the estimate used by the proof route.
\end{proposition}

\begin{lemma}\label{lem:key_step}\uses{def:main_object}
\lean{keyStep}
Statement of the key step.
\end{lemma}
\begin{proof}\uses{fact:source_estimate, lem:auxiliary_bound}
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
