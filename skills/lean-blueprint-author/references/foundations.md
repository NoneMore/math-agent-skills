# Shared Foundations

These principles govern blueprint content decisions for all operations:
creating, modifying, and updating blueprint routes. Operation-specific rules
live in the corresponding workflow references.

## Dependency Classes

Classify every dependency by its role:

- **Statement dependency**: a definition, structure, notation-bearing object, or
  prior result needed to state a node.
- **Proof-only dependency**: a result used in the proof route but not needed to
  state the node.
- **Source observation**: a fact, construction detail, estimate, convention, or
  local argument that the selected material uses without presenting it as a
  separately named theorem.
- **Cited external result**: an external theorem or proposition directly invoked
  by the selected material or existing blueprint.
- **Proposed local lemma or observation**: a bridge statement needed for the route
  but not determined by existing sources (materials, project code, or known
  Mathlib).

Do not blur these classes. Source observations and proposed local statements
are not existing Lean declarations unless project code already contains the
exact declaration.

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
- tools or definitions that appear in sources but are unused by the target;
- one-line steps that Lean should discharge by unfolding, direct rewriting,
  `simp`, routine algebra, or immediate application of an already listed node.

Put small routine steps in proof prose instead of promoting them to lemmas.

## Missing Dependencies

If a necessary bridge is not determined by existing sources (materials, project
code, or known Mathlib), introduce it only as a proposed local lemma or
proposed local observation.

A proposed statement must:

- be explicitly marked as proposed;
- be necessary for the current route;
- be as weak as possible while still connecting the proof;
- avoid pretending to be a cited theorem, Mathlib result, or already
  formalized project declaration.

Do not invent external theorem names or citations to fill gaps.

## Source Traceability

Every non-routine node should be traceable to one of:

- the requested target (for create) or the modification scope (for modify);
- a specific part of the source materials;
- existing project Lean code;
- a known Mathlib fact;
- a user-directed strengthening or specialization;
- a clearly marked proposed local bridge.

If a node cannot be traced this way, remove it.

For modify and update operations, additionally trace each change to:
- the user's explicit modification request; or
- a specific change in the formalization code (for update).
