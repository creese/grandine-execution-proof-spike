# EIP-8025 Cross-Repository Analysis

## Goal

Analyze EIP-8025 across the EIP text, consensus specifications, Lighthouse implementation, and Grandine architecture. Produce an evidence-based proposal for implementing EIP-8025 in Grandine.

## Sources

- EIP: `/work/repos/EIPs/EIPS/eip-8025.md`
- Consensus specs: `/work/repos/consensus-specs/specs/_features/eip8025/`
- Lighthouse: `/work/repos/lighthouse`
- Grandine: `/work/repos/grandine`

Expected Lighthouse branch: `optional-proofs-unstable`. Verify it rather than assuming it.

## Outputs

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
    │   └── validation-log.md
    ├── 00-environment-validation.md
    ├── 01-spec-and-lighthouse-scope.md
    ├── 02-lighthouse-implementation.md
    ├── 03-grandine-gap-analysis.md
    ├── 04-grandine-proposal.md
    └── final-report.md

Create missing directories as needed.

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
Produce concrete Grandine workstreams, dependency-aware sequencing, reviewable packages, acceptance criteria, interoperability tests, risk register, and a consolidated final report.

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

## Final report structure

# EIP-8025: Lighthouse Implementation and Grandine Implementation Proposal

## 1. Executive summary
## 2. Source revisions and scope
## 3. EIP and consensus-spec requirements
## 4. Specification discrepancies and unresolved questions
## 5. Lighthouse branch change inventory
## 6. Lighthouse architecture and end-to-end behavior
## 7. Data structures, serialization, hashing, and consensus rules
## 8. Fork activation and compatibility
## 9. Lighthouse tests and implementation evidence
## 10. Grandine architecture mapping
## 11. Grandine implementation gaps
## 12. Proposed Grandine workstreams
## 13. Recommended implementation sequence
## 14. Work packages and acceptance criteria
## 15. Test and interoperability strategy
## 16. Risks and open questions
## 17. Proposal-ready conclusions
## 18. Evidence appendix

Link to detailed phase reports. Avoid duplicating whole sections verbatim.

## Completion

The overall task is complete only when all five phases are complete, required evidence files exist, proposals trace to requirements and verified code, all repositories remain unmodified, and the final report records exact revisions.
