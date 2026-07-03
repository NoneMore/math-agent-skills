# Modify Workflow

Follow this workflow when changing an existing blueprint's structure, content,
or proof route.

## Autonomy Levels

Before operating, determine the narrowest level that satisfies the user's
request. Stay within that level unless the user explicitly escalates.

### Level 1: Label Sync

**Allowed:**
- Fix `\lean{}` declaration names to match updated code.
- Correct label spelling, formatting, and cross-reference typos.
- Update `\leanok` and `\mathlibok` status markers where project code or
  Mathlib facts have changed.

**Forbidden:**
- Changing any mathematical statement, proof, or dependency.

### Level 2: Structural Reorganization

Includes Level 1, plus:

**Allowed:**
- Reorder sections, subsections, or nodes within a section.
- Merge or split nodes where the mathematical content is unchanged.
- Adjust `\uses{}` lists to reflect reordering (no new mathematical
  dependencies introduced).
- Rename labels with consistent cross-reference updates.

**Forbidden:**
- Changing theorem statements, lemma statements, or proof content.
- Introducing new dependencies not already implied by the existing content.

### Level 3: Content Modification

Includes Levels 1–2, plus:

**Allowed:**
- Modify a specific theorem or lemma statement (user must specify which).
- Replace or rewrite a specific proof (user must specify which).
- Add a new lemma or observation explicitly requested by the user.
- Delete a node explicitly marked by the user.

**Forbidden:**
- Modifying nodes the user did not specify.
- Changing the target theorem unless requested.

### Level 4: Route Restructuring

Includes Levels 1–3, plus:

**Allowed:**
- Replace a proof route with an alternative from source materials.
- Insert new intermediate lemmas to bridge a gap.
- Remove redundant or unused branches.
- Strengthen or specialize results as directed by the user.

**Forbidden:**
- Changing the target theorem statement beyond user-specified
  strengthening/specialization.
- Introducing dependencies not traceable to sources or the user's request.

If the user's request is ambiguous about the level, ask before operating.

## Scope Determination

Before modifying:

1. Read the existing blueprint to understand the current structure.
2. Identify the exact nodes, sections, or proof routes the user wants changed.
3. If the scope is unclear, present the relevant portion of the blueprint and
   ask the user to specify.

Operate only within the confirmed scope.

## Local Re-computation Rules

When a node's statement or proof changes, recompute affected dependencies
downstream:

1. For each downstream node that `\uses` the modified node:
   - If the modification is a **statement change**, classify whether the
     downstream node can still be stated.
   - If the modification is a **proof change**, classify whether the downstream
     proof route is affected. If the modified node was a proof-only dependency,
     the downstream statement is unaffected but its proof may need adjustment.
2. Continue downstream until reaching unaffected nodes or the target theorem.
3. Classify each affected downstream statement or proof as one of:
   - **verified from source/code**: the supplied sources, existing blueprint, or
     Lean code justify the adjusted statement or proof route;
   - **dependency-consistent but unverified**: the `\uses{}` graph is coherent,
     but the supplied materials do not verify the mathematical proof;
   - **blocked/needs user decision**: the change creates a mathematical,
     source-traceability, or scope ambiguity that cannot be resolved locally.
4. Report every node whose statement, proof route, or dependency list required
   adjustment, even if the adjustment was trivial.

Do not assume downstream nodes are unaffected without checking. The
re-computation is complete only when every affected downstream statement and
proof has one of the three classifications above. If any item is
blocked/needs user decision, stop and request intervention before guessing a
mathematical repair.

## User-Directed Strengthening Or Specialization

The user may explicitly ask to strengthen or specialize a result. Treat that
request as a high-priority input, but keep the deviation traceable.

For a user-requested specialization:
- Prefer the specialized statement when it is enough to prove downstream
  targets.
- Do not retain the more general version unless it is still needed elsewhere
  in the route.

For a user-requested strengthening that is supported by source materials:
- Use the strengthened statement and recompute downstream dependencies around
  that stronger node.

For a user-requested strengthening that is not supported by source materials:
- The statement may appear only as a proposed local lemma or proposed local
  observation.
- Its proof route must state that the strengthening is user-specified rather
  than sourced.

## Output Shape

Produce a minimal diff against the existing blueprint, not a full rewrite.
Apply the shared output contract from `references/output-contract.md`.

### Structural changes (reordering, splitting, merging)

Show only the affected region in its new arrangement. Include a brief note
describing what moved where.

### Content changes (modified statements, proofs, dependencies)

For each modified node, output the complete replacement node (environment,
label, statement, proof, uses). Mark deleted nodes explicitly:
`% REMOVED: \label{old:label}`. List new nodes with their full content.

### Dependency updates

After all node changes, output the updated `\uses{...}` for any downstream
node whose dependency list changed.

Prefer this shape for a modified node:

```tex
% REPLACES: \label{lem:old_key_step}
\begin{lemma}\label{lem:key_step}\uses{def:main_object}
\lean{keyStep}
Revised statement of the key step.
\end{lemma}
\begin{proof}\uses{fact:source_estimate, lem:new_bound}
Revised proof route.
\end{proof}
```

Always report:
- How many nodes were added, modified, removed.
- Which downstream nodes had their `\uses{}` updated.
- The verification classification for each affected downstream statement or
  proof.
- Any unresolved issues (nodes whose proofs could not be verified after the
  change).
