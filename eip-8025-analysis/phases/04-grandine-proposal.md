# Phase 4 — Grandine Implementation Proposal

## Objective

Produce a concrete, evidence-based proposal for implementing EIP-8025 in Grandine.

Synthesize all prior phases while keeping facts, requirements, inferences, proposals, and open questions distinct.

## Inputs

Read all phase reports and evidence files. Verify important claims against the repositories when needed.

## Outputs

Write:

- `/work/eip-8025-analysis/04-grandine-proposal.md`
- `/work/eip-8025-analysis/final-report.md`

Only correct or extend evidence files when necessary.

## Proposal requirements

### Workstreams

For each workstream include objective, requirement IDs, verified Grandine integration points, Lighthouse analogue, deliverables, dependencies, complexity, consensus risk, acceptance criteria, tests, and unresolved questions.

### Sequence

Provide a dependency-aware order covering specification alignment, types, constants, activation, upgrades, processing, validation, production/import, networking/APIs, persistence/caches, metrics, tests, and interoperability. Identify parallel work.

### Work packages

Each package must include title, objective, rationale, scope, out of scope, likely Grandine targets, deliverables, acceptance criteria, dependencies, risks, tests, Lighthouse evidence, requirement IDs, and open questions.

### Test strategy

Cover units, SSZ round trips, roots, bounds, defaults, upgrade, activation boundary, happy paths, malformed input, duplicates/conflicts, validation order, pre-fork and post-fork behavior, production, import, persistence, networking, consensus-spec vectors, differential Lighthouse testing, and cross-client interoperability. Mark release-blocking tests.

### Risks

Include specification drift, EIP/spec mismatch, Lighthouse/spec divergence, SSZ or hashing mismatch, activation errors, validation-order differences, upgrade errors, networking compatibility, storage migration, copying Lighthouse-specific behavior, weak negative testing, and unstable upstream interfaces.

### Scope

Identify unrelated Lighthouse changes, Lighthouse-specific machinery not needed by Grandine, unaffected areas, speculative future work, and behavior blocked by unresolved specification questions.

### Proposal-ready text

Provide polished background, motivation, objective, scope, non-goals, approach, milestones, deliverables, acceptance criteria, testing, risks, and open questions.

## Proposal report

# Phase 4: Grandine EIP-8025 Implementation Proposal

## 1. Proposal summary
## 2. Background and motivation
## 3. Source revisions and evidence basis
## 4. Scope and non-goals
## 5. Required protocol behavior
## 6. Grandine implementation workstreams

| Workstream | Requirement IDs | Grandine targets | Deliverables | Complexity | Risk |

## 7. Recommended implementation sequence
## 8. Detailed work packages
## 9. Acceptance criteria
## 10. Test and interoperability strategy
## 11. Risk register

| Risk | Likelihood | Impact | Mitigation | Blocking question |

## 12. Open specification questions
## 13. Open Grandine design questions
## 14. Lighthouse behavior not to copy blindly
## 15. Milestones and review strategy
## 16. Proposal-ready text

## Final report

Use the structure in `TASK.md`. Consolidate rather than duplicate, link to phase reports, include exact revisions, trace proposals to evidence, and preserve unresolved conflicts.

## Completion checks

Verify all prior outputs were read, all workstreams trace to requirement IDs, Grandine targets cite real architecture, acceptance criteria are measurable, tests include negative and activation-boundary coverage, risks include specification instability, no repository was modified, and both outputs exist.
