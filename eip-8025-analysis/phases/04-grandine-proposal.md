# Phase 4 — Grandine Implementation Proposal Review

## Objective

Review the user-authored Grandine EIP-8025 implementation proposal against the evidence produced in Phases 1 through 3.

Identify unsupported claims, missing or incomplete requirements, silently resolved specification conflicts, architecture claims not grounded in Grandine, Lighthouse-specific behavior copied without justification, and missing dependencies, acceptance criteria, tests, risks, or open questions.

Do not write, rewrite, edit, or replace the proposal.

## Inputs

Read:

- `/work/eip-8025-analysis/grandine-eip-8025-proposal.md`
- all Phase 1, Phase 2, and Phase 3 reports and evidence files;
- the live EIP and consensus-spec feature directory;
- the Lighthouse and Grandine repositories when important claims require verification.

Treat the proposal as immutable reviewer input.

If the proposal is missing, unreadable, or empty, write a short Phase 4 review report that records the blocker, update `evidence/validation-log.md`, and stop.

In this blocked state, do not create or replace
`evidence/proposal-traceability-matrix.md`, and mark Phase 4 incomplete.

If a traceability matrix already exists from an earlier Phase 4 run, leave it unchanged and explicitly state in the blocker report and validation log that it was not validated for the current run and must not be treated as current review evidence.


## Outputs

Write:

- `/work/eip-8025-analysis/04-grandine-proposal-review.md`
- `/work/eip-8025-analysis/evidence/proposal-traceability-matrix.md`

Update:

- `/work/eip-8025-analysis/evidence/validation-log.md`

Do not modify the proposal or any Phase 1 through Phase 3 report or evidence
file, except for replacing or adding the Phase 4 section in the shared
`evidence/validation-log.md`.

Do not create or update:

- `/work/eip-8025-analysis/04-grandine-proposal.md`
- `/work/eip-8025-analysis/final-report.md`

When rerunning Phase 4 with a present, readable, non-empty proposal, replace
the Phase 4 review report and proposal traceability matrix, and supersede the
previous Phase 4 section in the validation log. Do not append duplicate review
findings.

When the proposal-input check fails, follow the blocked-state rules above
instead.

## Review method

### 1. Review baseline

Record:

- proposal path, size, and SHA-256 hash;
- source repository branches, HEAD revisions, and working-tree states;
- Phase 1 through Phase 3 reports and evidence consumed;
- any accepted control-file drift since earlier phases.

Verify at phase completion that the proposal hash is unchanged and all source repositories remain at their accepted revisions and are clean.

### 2. Requirement traceability

For every stable requirement ID in `evidence/spec-requirement-index.md`, provide one distinct row in `evidence/proposal-traceability-matrix.md`.

Use:

| Requirement ID | Proposal location | Coverage | Proposal claim or commitment | Evidence basis | Reviewer finding | Severity |

Coverage must be one of:

- covered;
- partially covered;
- missing;
- contradicted;
- unclear;
- not applicable with justification.

Severity must be one of:

- blocker;
- major;
- minor;
- none.

Use `none` only when the reviewer found no corrective action for that
requirement. Do not leave severity blank.

Use the exact complete requirement ID. Do not combine IDs, use ranges, introduce new IDs, or omit requirements.

Mechanically verify that the matrix ID set exactly matches the requirement index, with no missing, additional, or duplicate IDs. State the verified count in the review report and validation log.

### 3. Claim and evidence audit

Review important factual, architectural, behavioral, and scope claims.

For each problem, record:

| Proposal location | Claim | Evidence category | Evidence checked | Finding | Severity | Required author action |

Check that:

- EIP statements and consensus-spec requirements are accurately represented;
- verified Lighthouse and Grandine facts cite real code, tests, configuration, or prior evidence;
- cross-client inferences and proposed Grandine changes are not presented as verified facts;
- source revisions and branch scope are accurate;
- Grandine targets refer to real architecture;
- unchanged code and reusable infrastructure are not described as existing EIP-8025 support.

### 4. Specification-decision audit

Check every unresolved conflict identified by Phases 1 through 3, including proof representation and maximum size, public input, signing domain, quorum, payload lifecycle, gossip validation and deduplication semantics, retention, activation, proof-type negotiation, and EL guest or host behavior.

For each conflict, determine whether the proposal:

- preserves it as an open question;
- explicitly chooses an assumption and labels the consequences;
- delegates it to a prerequisite specification decision; or
- silently resolves it.

Treat silent resolution of a wire, hashing, signing, validation, or consensus-boundary conflict as a blocker.

### 5. Grandine architecture audit

Check whether proposed workstreams and packages:

- are grounded in verified Grandine components;
- prefer Grandine-native ownership and lifecycle patterns;
- separate proof-engine behavior from EL payload validity;
- keep proof processing outside beacon-state transition and fork-choice weighting unless an explicit, evidence-backed scope change is stated;
- identify unaffected areas;
- distinguish required protocol behavior from suggested architecture;
- include confidence and unresolved design choices where placement remains uncertain.

### 6. Lighthouse comparison audit

Check that Lighthouse is used as implementation evidence rather than a normative source.

Flag:

- operator-configurable proof quorum affecting payload validity;
- incomplete active-validator validation;
- memory-only proof or status retention;
- incomplete finalized-window pruning;
- pre-Gloas prover lifecycle assumptions;
- temporary proof-sync behavior;
- local constants, APIs, caching, or topology copied without Grandine-specific justification.

### 7. Completeness audit

Evaluate whether the proposal contains reviewable:

- background and motivation;
- scope, non-goals, and unaffected areas;
- required protocol behavior;
- workstreams and likely Grandine targets;
- dependency-aware sequence and parallel work;
- work packages and deliverables;
- measurable acceptance criteria;
- unit, negative, boundary, integration, differential, and interoperability tests;
- release-blocking tests;
- migration, persistence, networking, observability, and operational considerations;
- risk register;
- open specification and Grandine design questions;
- milestones and review gates.

Do not invent missing proposal content. Record omissions and the author action needed.

### 8. Findings and severity

Classify findings as:

- **blocker:** the proposal is unsafe, internally contradictory, non-interoperable, materially untraceable, or cannot support implementation review;
- **major:** important requirements, architecture, dependencies, tests, risks, or decisions are missing or weak;
- **minor:** clarity, citation, organization, or maintainability issue.

Deduplicate findings. Prefer one root-cause finding with all affected locations over repeated symptoms.

## Review report

Write `/work/eip-8025-analysis/04-grandine-proposal-review.md` using:

# Phase 4: Grandine EIP-8025 Implementation Proposal Review

## 1. Review verdict

State one of:

- Ready for implementation review
- Ready with required revisions
- Not ready for implementation review

## 2. Proposal and source baseline
## 3. Evidence consumed and review limitations
## 4. Requirement traceability summary

| Coverage | Count | Notes |

## 5. Blocking findings

| ID | Proposal location | Finding | Evidence | Required author action |

## 6. Major findings

| ID | Proposal location | Finding | Evidence | Required author action |

## 7. Minor findings

| ID | Proposal location | Finding | Evidence | Suggested author action |

## 8. Unsupported or inaccurate claims
## 9. Silently resolved specification conflicts
## 10. Missing or weak Grandine architecture grounding
## 11. Lighthouse behavior copied without sufficient justification
## 12. Missing scope, sequence, work-package, or dependency detail
## 13. Acceptance-criteria and test gaps
## 14. Risk-register and open-question gaps
## 15. Strong sections to preserve
## 16. Author revision checklist
## 17. Re-review criteria
## 18. Appendix

The review may recommend targeted changes, but must not provide replacement proposal prose or rewrite complete sections.

## Completion checks

When the proposal was present, readable, and non-empty, verify:

- the proposal existed and remained byte-for-byte unchanged;
- every exact stable requirement ID has one traceability row;
- the traceability ID set exactly matches `evidence/spec-requirement-index.md`;
- every finding cites a proposal location and evidence basis;
- unresolved specification conflicts were checked for silent resolution;
- Grandine architecture claims were checked against verified architecture;
- Lighthouse was not treated as normative;
- missing acceptance criteria, tests, risks, and open questions were assessed;
- findings are deduplicated and severity is justified;
- all source repositories match their accepted branch and HEAD baselines and are clean;
- the review report, traceability matrix, and validation-log update exist.

When the proposal-input check failed, verify only that the blocker review and
validation-log update exist, the proposal was not created or modified, source
repositories remain clean, and Phase 4 is explicitly marked incomplete.
