# Phase 1 — Specification Baseline and Lighthouse Scope

## Objective

Establish the specification baseline and isolate the exact Lighthouse branch-local changes related to EIP-8025.

Do not perform full Lighthouse architecture analysis or Grandine architecture analysis.

## Inputs

Read:

- `/work/AGENTS.md`
- `/work/eip-8025-analysis/TASK.md`
- `/work/eip-8025-analysis/00-environment-validation.md`
- `/work/eip-8025-analysis/evidence/instruction-validation.md`
- `/work/eip-8025-analysis/evidence/repository-baselines.md`
- `/work/eip-8025-analysis/evidence/validation-log.md`
- `/work/repos/EIPs/EIPS/eip-8025.md`
- all relevant files under `/work/repos/consensus-specs/specs/_features/eip8025/`

## Outputs

Write:

- `/work/eip-8025-analysis/01-spec-and-lighthouse-scope.md`
- `/work/eip-8025-analysis/evidence/spec-requirement-index.md`
- `/work/eip-8025-analysis/evidence/spec-discrepancy-matrix.md`
- `/work/eip-8025-analysis/evidence/lighthouse-git-baseline.txt`
- `/work/eip-8025-analysis/evidence/lighthouse-changed-files.txt`
- `/work/eip-8025-analysis/evidence/lighthouse-relevant-diff.patch`
- `/work/eip-8025-analysis/evidence/lighthouse-symbol-index.md`
- `/work/eip-8025-analysis/evidence/validation-log.md`

## Work

### 1. Phase 0 readiness check

Read the Phase 0 report and instruction-validation evidence.

Proceed only when the Phase 0 readiness result is:

- `Ready for Phase 1`; or
- `Ready for Phase 1 with warnings`.

Do not proceed if:

- Phase 0 reports `Not ready for Phase 1`; or
- `evidence/instruction-validation.md` contains unresolved blockers.

Before continuing, verify that the current Lighthouse branch, HEAD, and
working-tree status still match `evidence/repository-baselines.md`.

Verify that the current EIPs and consensus-specs HEAD revisions and working-tree states match the Phase 0 repository baselines before extracting requirements.

If either repository has revision or working-tree drift, record the exact difference in the Phase 1 report and stop unless the drift is explicitly accepted by the workspace instructions.

If Phase 0 has unresolved blockers or any checked repository baseline has
changed in a way that invalidates the analysis, record the problem in
`01-spec-and-lighthouse-scope.md` and stop.

### 2. Requirement model

Extract motivation, terminology, data structures, state and block fields, constants, bounds, serialization, hashing, transitions, validation, activation, duties, EL interaction, networking, APIs, storage, security invariants, lifecycle, and tests.

Assign stable IDs such as `EIP8025-TYPE-001`, `EIP8025-VALIDATION-002`, and `EIP8025-TRANSITION-003`.

Write the requirement index using:

| Requirement ID | Area | EIP statement | Consensus-spec implementation | Status | Notes |

### 3. Specification comparison

Record agreement, terminology differences, omissions, extra behavior, changed constants, changed type shapes, validation-order differences, placeholders, TODOs, and contradictions.

Use:

| Topic | EIP | Consensus specs | Difference | Significance | Open question |

### 4. Lighthouse comparison base

Verify the branch and HEAD. Determine the base from upstreams, remotes, graph, merge bases, branch-only commits, diff composition, commit messages, and locally available PR metadata.

Record the selected base, base commit, merge base, diff range, branch-only commits, alternatives, uncertainty, and working-tree caveats.

### 5. Lighthouse inventory

Inventory every branch-local changed file, rename, commit, changed symbol, and mixed hunk.

Classify every changed file as direct, supporting, or unrelated.

For included files, record requirement IDs, changed symbols, evidence, callers and callees to inspect, and tests.

### 6. Focused evidence

Create a relevant-only patch where practical and a symbol index:

| Symbol | Path | Lines | Classification | Requirement IDs | Purpose | Callers | Tests |

## Main report

# Phase 1: EIP-8025 Specification and Lighthouse Scope

## 1. Executive scope conclusion
## 2. Repository revisions and working-tree state
## 3. EIP requirement summary
## 4. Consensus-spec implementation summary
## 5. EIP versus consensus-spec discrepancies
## 6. Lighthouse Git baseline
## 7. Complete Lighthouse branch change inventory

| File | Change | Classification | Requirement IDs | Reason | Symbols |

## 8. In-scope Lighthouse files and hunks
## 9. Excluded Lighthouse changes
## 10. Preliminary functional areas
## 11. Investigation plan for Phase 2
## 12. Open questions

## Completion checks

Verify Phase 0 approved proceeding to Phase 1, instruction validation contained
no unresolved blockers, the Lighthouse baseline was checked for drift, every
Lighthouse changed file is classified, every included change has evidence,
discrepancies remain visible, no Grandine architecture claims were made, no
repository was modified, and all outputs exist.

Do not begin Phase 2.
