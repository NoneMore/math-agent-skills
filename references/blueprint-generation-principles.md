# Blueprint Generation Principles

These principles govern blueprint content decisions only: what mathematical
nodes to include, how to classify dependencies, and how to treat missing or
user-directed statements. LaTeX syntax and leanblueprint formatting rules stay
in `SKILL.md`.

## Core Boundary

Generate a blueprint route for one exact user-specified target proposition.
Every included node must be needed to state that target, prove that target, or
make an explicitly requested user modification traceable.

Use only the selected source materials, existing project code, known Mathlib
facts, and clearly marked proposed local statements. Do not introduce broad
background theory merely because it appears near the target in the source.

## Dependency Classes

Classify every dependency by its role:

- Statement dependency: a definition, structure, notation-bearing object, or
  prior result needed to state a node.
- Proof-only dependency: a result used in the proof route but not needed to
  state the node.
- Source observation: a fact, construction detail, estimate, convention, or
  local argument that the selected material uses without presenting it as a
  separately named theorem.
- Cited external result: an external theorem or proposition directly invoked
  by the selected material.
- Proposed local lemma or observation: a bridge statement needed for the route
  but not determined by the selected material, existing project code, or known
  Mathlib.

Do not blur these classes. In particular, source observations and proposed
local statements are not existing Lean declarations unless project code already
contains the exact declaration.

## Recursive Closure

Build the route recursively from the target backward. For each node included in
the route, identify the dependencies needed to state it and the dependencies
needed to prove it. Then repeat the same analysis for each newly introduced
dependency until every leaf is one of:

- an existing project declaration;
- a known Mathlib fact;
- a source observation from the selected material;
- a directly cited external result;
- a proposed local lemma or observation;
- a primitive notion that does not need its own blueprint node.

Do not stop after finding the immediate dependencies of the target. A blueprint
route is complete only when every non-leaf node has its own statement and proof
dependencies resolved or explicitly marked as proposed/source/external.

When recursion exposes an unused branch, remove that branch during minimization.
When recursion exposes a missing bridge, add only the weakest proposed local
statement needed to close that branch.

## Node Granularity

Create a blueprint node for a mathematical object or conclusion only when it
affects the Lean formalization route.

Use separate nodes for:

- definitions, structures, constructions, or named objects needed later;
- key lemmas or propositions that carry real proof work;
- proof bottlenecks that deserve independent formalization;
- results reused by more than one later node;
- source observations or cited external results that the target proof actually
  depends on;
- the final target theorem.

Do not create nodes for:

- motivation, historical commentary, examples, or alternative methods;
- broad framework exposition not invoked by the chosen proof route;
- tools or definitions that appear in the source but are unused by the target;
- one-line steps that Lean should discharge by unfolding, direct rewriting,
  `simp`, routine algebra, or immediate application of an already listed node.

Put small routine steps in the proof route prose instead of promoting them to
lemmas.

## Minimal Route

Default to the weakest direct route that states and proves the requested
target. Keep only nodes required by that route.

When the source gives a more general theorem than the target needs, prefer the
specialized form if it is enough for the requested target and no later selected
node needs the general form.

When the source offers multiple proof routes, choose the route with the fewest
new formalization obligations unless the user selects another route.

Topologically order content so each node appears before its first use. This is
a content constraint, not merely a formatting preference: cycles or late
dependencies indicate that the route has not been resolved.

## Missing Dependencies

If a necessary bridge is not determined by the selected material, existing
project code, or known Mathlib, introduce it only as a proposed local lemma or
proposed local observation.

A proposed statement must:

- be explicitly marked as proposed;
- be necessary for the current target route;
- be as weak as possible while still connecting the proof;
- avoid pretending to be a cited theorem, Mathlib result, or already
  formalized project declaration.

Do not invent external theorem names or citations to fill gaps.

## User-Directed Strengthening Or Specialization

The user may explicitly ask to strengthen or specialize a result. Treat that
request as a high-priority input, but keep the deviation from the minimal route
traceable.

For a user-requested specialization, prefer the specialized statement when it
is enough to prove the target. Do not retain the more general version unless it
is still needed elsewhere in the selected route.

For a user-requested strengthening that is supported by the selected material,
use the strengthened statement and recompute downstream dependencies around
that stronger node.

For a user-requested strengthening that is not supported by the selected
material, the statement may appear only as a proposed local lemma or proposed
local observation. Its proof route must say that the strengthening is
user-specified rather than sourced from the selected material.

## Source Traceability

Every non-routine node should be traceable to one of:

- the requested target;
- a specific part of the selected material;
- existing project code;
- a known Mathlib fact;
- a user-directed strengthening or specialization;
- a clearly marked proposed local bridge.

If a node cannot be traced this way, remove it.
