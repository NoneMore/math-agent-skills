# Update Workflow

Follow this workflow when syncing a blueprint with changes in the
formalization code. Update operates at Modify Autonomy Level 1 (Label Sync) by
default; escalate to Modify when structural or content changes are needed.

## Input Requirements

- The existing blueprint (usually `blueprint/src/content.tex`).
- The project Lean code (scan `.lean` files for declarations).
- The user may optionally specify a scope (specific files, sections, or nodes).

## Workflow

### 1. Scan Formalization Code

Scan the project's `.lean` files to extract:

- Every theorem, lemma, proposition, and definition declared (name and type).
- The file path for each declaration.

Use `lakefile.lean` or `lakefile.toml` to locate the project source directory.
If no code is present (project not yet formalized), report that there is
nothing to sync and stop.

### 2. Parse Blueprint Labels

Parse the existing blueprint to extract:

- Every `\label{...}` and its associated `\lean{...}` or `\leanok` marker.
- Every `\mathlibok` marker.
- Every `\uses{...}` list.

Build a mapping: blueprint label → Lean declaration name (if any) → status
(has `\leanok`? has `\lean{}`? unlinked?).

### 3. Gap Analysis

Compare the code scan against the blueprint parse:

| Gap | Condition | Action |
|---|---|---|
| **Unmarked formalization** | Blueprint node has `\lean{...}` but no `\leanok`, and the Lean declaration exists in project code | Add `\leanok` |
| **Stale declaration name** | Blueprint node's `\lean{...}` name does not match any declaration in project code | Update the `\lean{}` name only when the replacement declaration is clear; add `\leanok` if that declaration exists |
| **Orphan blueprint node** | Blueprint node has no `\lean{}` and no matching declaration in code | Report for user attention; do not invent a link |
| **Unlinked code declaration** | Lean declaration exists in code but has no corresponding blueprint node | Report for user attention; suggest whether a blueprint node should be added |
| **Missing Mathlib marker** | Blueprint node uses a known Mathlib result but lacks `\mathlibok` | Add `\mathlibok` |

A replacement declaration is clear when label/name similarity, namespace,
nearby file location, and type/signature evidence identify a single candidate.
If multiple candidates remain, or no candidate is supported by the scanned
evidence, report the ambiguity immediately, stop the update, and ask the user
to choose the mapping. Do not guess a declaration name.

### 4. Produce Sync Output

For each gap found, output a specific, actionable change. Group by type:

**Marker Updates** — add `\leanok` or `\mathlibok` to existing nodes.

**Name Corrections** — replace stale `\lean{...}` names with current
declaration names only after a clear mapping is identified, adding `\leanok`
where appropriate.

**Reports** — list orphan nodes and unlinked declarations for user review. For
each, indicate whether the gap is likely harmless or needs attention.

### 5. Escalate to Modify When Needed

If the gap analysis reveals that the code has structurally diverged from the
blueprint (definitions changed, proofs restructured, theorems split or merged),
do not attempt to fix this within Update. Instead:

1. Complete all Level 1 (label sync) changes.
2. Report the structural divergences with specific references (which nodes,
   which code declarations).
3. Offer to escalate to the Modify workflow, describing the scope of changes
   needed.

## Output Shape

Produce a status change manifest, not a full blueprint rewrite.
Apply the shared output contract from `references/output-contract.md`.

For each affected node, output the minimal change:
- **Marker additions**: the node with `\leanok` or `\mathlibok` added.
- **Name corrections**: old `\lean{}` → new `\lean{}`.
- **Reports**: a bullet list of gaps that need user decisions.

Prefer this shape:

```text
## Sync Report

### Marker Updates (3 nodes)
- lem:key_step: added \leanok
- def:main_object: added \leanok
- prop:intermediate: added \mathlibok

### Name Corrections (1 node)
- lem:aux_bound: \lean{auxBound} → \lean{auxiliaryBound}\leanok

### Orphan Nodes (2 nodes, needs review)
- prop:unlinked_result: no matching Lean declaration
- lem:helper: no matching Lean declaration (likely harmless)

### Unlinked Declarations (1, needs review)
- newLemma in src/Chapter2.lean: no blueprint node
```

Always report totals and flag items needing user decisions.
