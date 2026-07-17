# Instruction Validation

## Summary

**Valid with warnings.** All required controls are present, readable, and non-empty; the five-phase workflow is coherent, feasible, correctly ordered, and proposal-complete. Two minor maintainability/verifiability issues do not block Phase 1.

## Required files

| File | Exists | Readable | Non-empty | Status | Notes |
|---|---|---|---|---|---|
| `/work/AGENTS.md` | Yes | Yes | Yes | Pass | 182 lines |
| `/work/eip-8025-analysis/TASK.md` | Yes | Yes | Yes | Pass | 121 lines |
| `phases/00-environment-validation.md` | Yes | Yes | Yes | Pass with warning | 449 lines; closing heading is `Completion rules`, not the exact `Completion checks` phrase the phase asks this validation to confirm |
| `phases/01-spec-and-lighthouse-scope.md` | Yes | Yes | Yes | Pass with warning | 124 lines; source-drift gate explicitly rechecks Lighthouse only |
| `phases/02-lighthouse-implementation.md` | Yes | Yes | Yes | Pass | 112 lines |
| `phases/03-grandine-gap-analysis.md` | Yes | Yes | Yes | Pass | 94 lines |
| `phases/04-grandine-proposal.md` | Yes | Yes | Yes | Pass | 85 lines |

## Phase consistency

| Check | Result | Evidence | Severity |
|---|---|---|---|
| Phases 0 through 4 are defined uniquely and contiguously | Pass | `TASK.md` lists five sequential phases; exactly `00` through `04` exist under `phases/` | — |
| Filenames in `TASK.md` exactly match present phase files | Pass | Directory inventory matches all five paths listed at `TASK.md:58-62` | — |
| Each phase has title, objective, inputs, outputs, scoped work/checks, and a completion section | Pass with warning | All required concepts are present; Phase 0 calls its final section `Completion rules` | minor |
| Phases 0–3 stop before the next phase; Phase 4 names no successor | Pass | Explicit stop language appears in Phases 0–3; Phase 4 ends with final completion checks | — |
| Declared phase outputs agree with `TASK.md` | Pass | All declared artifacts occur in the expected layout; shared evidence updates are consistent with the layout | — |
| Dependencies are ordered and non-circular | Pass | Phase 1 consumes Phase 0; Phase 2 consumes Phase 1; Phase 3 consumes Phases 1–2; Phase 4 consumes all prior reports/evidence | — |
| Phase 1 consumes rather than recreates Phase 0 baseline work | Pass with warning | It consumes the three Phase 0 reports, then performs the phase-specific Lighthouse drift check and detailed Git baseline; this is justified, but only Lighthouse drift is explicit | minor |
| Earlier phases gather evidence required by the final proposal | Pass | Requirements/discrepancies and Lighthouse scope → implementation behavior → Grandine mapping/gaps → workstreams, tests, risks, and final report | — |
| Outputs are consumed | Pass | Phase 1 consumes Phase 0 evidence; Phase 2 consumes Phase 1; Phase 3 consumes Phase 1/2 evidence; Phase 4 consumes all phase reports/evidence | — |

## Path and policy consistency

| Check | Result | Evidence | Severity |
|---|---|---|---|
| All live workflow references use `phases/` | Pass | No obsolete `passes/` path is used; occurrences in Phase 0 are validation terms only | — |
| Four source paths are consistent | Pass | `AGENTS.md`, `TASK.md`, and phase inputs use the prescribed EIP, consensus-specs, Lighthouse, and Grandine paths | — |
| Generated outputs remain under `/work/eip-8025-analysis` | Pass | Every declared report/evidence output is beneath the analysis root | — |
| No output resolves under `/work/repos` | Pass | `realpath` resolves analysis and evidence roots under `/work/eip-8025-analysis` | — |
| Repository modification is forbidden | Pass | `AGENTS.md` and Phase 0 explicitly prohibit source-repository edits | — |
| No obsolete external AGENTS requirement, EIP download, or unavailable-Grandine assumption | Pass | Full-text review found none | — |
| No stale three- or four-phase workflow | Pass | `TASK.md` explicitly defines five phases | — |

## Semantic workflow validation

| Location | Instruction | Problem | Consequence | Severity | Recommended correction |
|---|---|---|---|---|---|
| `phases/00-environment-validation.md:104-110,439` | Confirm every phase contains “completion checks.” | Phase 0’s own closing heading is `Completion rules`; the content is sufficient, but the literal naming rule is self-inconsistent. | Automated or literal structure validation can report a false failure. | minor | Rename the heading to `## Completion checks`, or define the check as accepting an equivalent completion section. |
| `phases/01-spec-and-lighthouse-scope.md:44-46` | Before continuing, verify current Lighthouse branch, HEAD, and status against Phase 0. | Equivalent drift checks are not explicitly required for the EIPs and consensus-specs repositories even though Phase 1 derives the requirement baseline from them. Shared reporting rules require exact revisions, but do not explicitly say to compare these sources with Phase 0 before extraction. | If either source changes between phases, the change may be recorded as the new revision but the Phase 0-to-Phase 1 drift is not necessarily surfaced as a caveat. | minor | Add EIPs and consensus-specs HEAD/status drift checks; permit continuation when drift is recorded and does not invalidate readiness. |

The substantive review found no blockers, major defects, circular dependencies, missing prerequisite work, ungrounded proposal stage, disappearing requirements, or final-report sections lacking a producing phase. Phase 0 baseline collection is distinct from Phase 1’s detailed specification and Lighthouse-scope work and Phase 3’s current Grandine architecture baseline.

## Missing, stale, or conflicting instructions

- Missing, unreadable, or empty control files: none.
- Stale paths or obsolete `passes/` references: none.
- Inconsistent phase names, numbers, or output paths: none.
- Obsolete source assumptions: none.
- Missing substantive phase sections: none.
- Duplicated work that should be factored into Phase 0: none; later repository baselines serve drift detection and phase-specific evidence.
- Instructions risking source-repository modification: none. Focused later validation may create normal ignored build artifacts only when explicitly permitted by the applicable phase and remains governed by `AGENTS.md`.
- Non-blocking inconsistencies: the two minor items in the semantic table above.
