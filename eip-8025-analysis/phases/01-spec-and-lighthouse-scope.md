# Phase 1 — Specification Baseline and Lighthouse Scope

## Objective

Establish the specification baseline and isolate the exact Lighthouse branch-local changes related to EIP-8025.

Do not perform full Lighthouse architecture analysis or Grandine architecture analysis.

## Inputs

Read:

- `/work/AGENTS.md`
- `/work/eip-8025-analysis/TASK.md`
- `/work/repos/EIPs/eip-8025.md`
- all relevant files under `/work/repos/consensus-specs/specs/_features/eip8025/`

Inspect repository metadata for:

- `/work/repos/EIPs`
- `/work/repos/consensus-specs`
- `/work/repos/lighthouse`
- `/work/repos/grandine`

Grandine inspection is limited to revision, branch, remotes, and working-tree state.

## Outputs

Write:

- `/work/eip-8025-analysis/01-spec-and-lighthouse-scope.md`
- `/work/eip-8025-analysis/evidence/repository-baselines.md`
- `/work/eip-8025-analysis/evidence/spec-requirement-index.md`
- `/work/eip-8025-analysis/evidence/spec-discrepancy-matrix.md`
- `/work/eip-8025-analysis/evidence/lighthouse-git-baseline.txt`
- `/work/eip-8025-analysis/evidence/lighthouse-changed-files.txt`
- `/work/eip-8025-analysis/evidence/lighthouse-relevant-diff.patch`
- `/work/eip-8025-analysis/evidence/lighthouse-symbol-index.md`
- `/work/eip-8025-analysis/evidence/validation-log.md`

## Work

### 1. Repository baselines

For each repository record path, branch or detached state, HEAD, remotes, upstream, working-tree status, and pre-existing changes.

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

Verify all repository revisions are recorded, every Lighthouse changed file is classified, every included change has evidence, discrepancies remain visible, no Grandine architecture claims were made, no repository was modified, and all outputs exist.

Do not begin Phase 2.
