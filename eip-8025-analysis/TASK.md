# EIP-8025 Cross-Repository Analysis

## Goal

Analyze EIP-8025 across the EIP text, consensus specifications, Lighthouse implementation, and Grandine architecture. Build the evidence needed to review a user-authored proposal for implementing EIP-8025 in Grandine.

## Sources

- EIP: `/work/repos/EIPs/EIPS/eip-8025.md`
- Consensus specs: `/work/repos/consensus-specs/specs/_features/eip8025/`
- Lighthouse: `/work/repos/lighthouse`
- Grandine: `/work/repos/grandine`

Expected Lighthouse branch: `optional-proofs-unstable`. Verify it rather than assuming it.

## Workspace artifacts

All output belongs under `/work/eip-8025-analysis`.

Expected layout:

    /work/eip-8025-analysis/
    ├── TASK.md
    ├── phases/
    │   ├── 00-environment-validation.md
    │   ├── 01-spec-and-lighthouse-scope.md
    │   ├── 02-lighthouse-implementation.md
    │   ├── 03-grandine-gap-analysis.md
    │   └── 04-grandine-proposal.md
    ├── evidence/
    │   ├── instruction-validation.md
    │   ├── repository-baselines.md
    │   ├── spec-requirement-index.md
    │   ├── spec-discrepancy-matrix.md
    │   ├── lighthouse-git-baseline.txt
    │   ├── lighthouse-changed-files.txt
    │   ├── lighthouse-relevant-diff.patch
    │   ├── lighthouse-symbol-index.md
    │   ├── grandine-git-baseline.txt
    │   ├── grandine-symbol-index.md
    │   ├── lighthouse-grandine-map.md
    │   ├── validation-log.md
    │   └── proposal-traceability-matrix.md
    ├── 00-environment-validation.md
    ├── 01-spec-and-lighthouse-scope.md
    ├── 02-lighthouse-implementation.md
    ├── 03-grandine-gap-analysis.md
    ├── grandine-eip-8025-proposal.md
    └── 04-grandine-proposal-review.md

Create missing directories as needed.

`grandine-eip-8025-proposal.md` is user-authored input. Do not create, rewrite, or modify it. Phase 4 must report a blocker if it is absent, unreadable, or empty.

## Execution model

Run five sequential phases. Do not begin a later phase unless explicitly instructed. Later phases must read and verify relevant earlier outputs.

Phase instructions:

- `/work/eip-8025-analysis/phases/00-environment-validation.md`
- `/work/eip-8025-analysis/phases/01-spec-and-lighthouse-scope.md`
- `/work/eip-8025-analysis/phases/02-lighthouse-implementation.md`
- `/work/eip-8025-analysis/phases/03-grandine-gap-analysis.md`
- `/work/eip-8025-analysis/phases/04-grandine-proposal.md`

## Phase summaries

### Phase 0
Validate that the workspace, source repositories, analysis instructions, and required tools are ready for the EIP-8025 analysis.

### Phase 1
Establish repository revisions, EIP and consensus-spec requirements, specification discrepancies, Lighthouse comparison base, branch-local changes, classifications, and symbol inventory.

### Phase 2
Explain Lighthouse end to end: types, SSZ, hashing, configuration, activation, upgrade, processing, validation, production, networking, APIs, storage, metrics, tests, and gaps.

### Phase 3
Map every responsibility to Grandine's current architecture. Identify reusable abstractions, missing functionality, likely target modules and symbols, tests, dependencies, and design decisions.

### Phase 4
Review the user-authored proposal against all Phase 1 through Phase 3 evidence. Produce exact requirement traceability and actionable findings without rewriting the proposal.

## Shared reporting rules

Every major report must:

- state exact source revisions;
- distinguish facts, requirements, inferences, proposals, and open questions;
- cite paths and symbols;
- include line ranges when practical;
- expose unresolved discrepancies;
- explain behavior rather than only listing files;
- identify unaffected areas;
- record failed validation commands and environmental constraints.

## Phase 4 review outputs

Phase 4 writes:

- `/work/eip-8025-analysis/04-grandine-proposal-review.md`
- `/work/eip-8025-analysis/evidence/proposal-traceability-matrix.md`

The review must trace every stable requirement ID, identify unsupported or inaccurate claims, detect silently resolved specification conflicts, evaluate Grandine architecture grounding, assess tests and acceptance criteria, and provide actionable findings without rewriting the proposal.

## Completion
 
The overall task is complete only when Phases 0 through 3 are complete, the user-authored proposal has been reviewed without modification, every stable requirement ID has one exact traceability row, the Phase 4 review report and traceability matrix exist, findings trace to proposal locations and verified evidence, and all source repositories remain at their accepted revisions and are clean.
