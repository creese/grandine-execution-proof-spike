# Phase 3 — Grandine Architecture and Gap Analysis

## Objective

Inspect Grandine directly and map the complete EIP-8025 implementation surface to concrete Grandine architecture.

Identify existing capabilities, reusable abstractions, missing functionality, likely files and symbols, tests, dependencies, and design decisions.

Do not write or review the user-authored proposal.

## Inputs

Read all Phase 1 and Phase 2 reports and evidence. Inspect Grandine, Lighthouse, the consensus-spec feature directory, and the EIP.

## Outputs

Write:

- `/work/eip-8025-analysis/03-grandine-gap-analysis.md`
- `/work/eip-8025-analysis/evidence/grandine-git-baseline.txt`
- `/work/eip-8025-analysis/evidence/grandine-symbol-index.md`
- `/work/eip-8025-analysis/evidence/lighthouse-grandine-map.md`

Update `evidence/validation-log.md`.

## Work

### 1. Grandine baseline

Record branch or detached state, HEAD, upstream, remotes, working tree, structure, build/test entry points, and relevant fork patterns.

### 2. Architecture map

Identify Grandine components for protocol types, fork-specific state and block types, SSZ and tree hashing, presets and constants, fork identifiers and activation, upgrades, slot/epoch processing, production, validation, transition, EL interaction, fork choice, duties, pools, gossip, P2P, APIs, storage, caches, metrics, CLI, fixtures, and consensus-spec tests.

For each area identify paths, modules, types, traits, functions, dispatch, analogous forks, and tests. Mark unaffected areas.

### 3. Grandine symbol index

Use:

| Responsibility | Grandine symbol | Path | Lines | Current role | EIP-8025 relevance | Confidence |

### 4. Lighthouse-to-Grandine mapping

Use:

| Requirement ID | Responsibility | Lighthouse implementation | Grandine analogue | Gap | Likely target | Confidence | Open question |

For every stable requirement ID in
`evidence/spec-requirement-index.md`, provide one distinct mapping row.

Use the exact complete requirement ID, including the `EIP8025-` prefix.
Do not combine IDs, use ID ranges, introduce new IDs, or omit requirements
that are not directly applicable to Grandine.

Classify each as existing, extension required, new capability, no change
expected, or unresolved.

Verify that the set of mapping IDs exactly matches the requirement index,
with no missing, additional, or duplicate IDs. State the verified count in
the Phase 3 report and `evidence/validation-log.md`.

### 5. Gap analysis

For every requirement and Lighthouse functional area, explain current Grandine support, architectural differences, missing types or logic, conceptual reuse, Lighthouse-specific behavior not to copy, Grandine-native implementation options, dependencies, complexity, risk, and evidence-backed likely targets.

### 6. Design decisions

Identify unresolved choices around type representation, generic versus fork-specific APIs, storage, caching, validation layers, feature gating, migration, test-vector integration, and compatibility with pending Grandine work.

## Main report

# Phase 3: Grandine EIP-8025 Architecture and Gap Analysis

## 1. Executive summary
## 2. Grandine revision and repository state
## 3. Relevant Grandine architecture
## 4. Requirement-to-Grandine mapping

| Requirement ID | Required behavior | Current support | Gap | Integration point | Confidence |

## 5. Functional-area gap analysis
## 6. Concrete likely files, modules, and symbols
## 7. Lighthouse versus Grandine architecture

| Concern | Lighthouse | Grandine | Porting implication |

## 8. Reusable Grandine abstractions
## 9. New capabilities likely required
## 10. Areas likely unaffected
## 11. Testing and fixture gaps
## 12. Dependencies and ordering constraints
## 13. Complexity and consensus risk

| Work area | Complexity | Consensus risk | Reason | Dependencies |

## 14. Grandine design questions
## 15. Proposal-review inputs for Phase 4
## 16. Appendix

## Completion checks

Verify Grandine claims come from the checkout, every gap traces to a requirement or Lighthouse evidence, likely targets are evidence-backed, Lighthouse is not copied mechanically, unaffected areas are stated, no repository was modified, and outputs exist.

Verify that `evidence/lighthouse-grandine-map.md` contains one distinct row
for every exact stable requirement ID and that its ID set exactly matches
`evidence/spec-requirement-index.md`.

Do not begin Phase 4.
