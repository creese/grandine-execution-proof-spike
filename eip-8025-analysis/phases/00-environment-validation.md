# Phase 0 — Environment, Repository, and Instruction Validation

## Objective

Validate that the workspace, source repositories, analysis instructions, and required tools are ready for the EIP-8025 analysis.

This phase performs prerequisite and consistency checks only.

Do not begin:

- EIP or consensus-spec analysis;
- Lighthouse merge-base or diff classification;
- Lighthouse implementation analysis;
- Grandine architecture mapping;
- proposal generation.

Do not edit instruction files or source repositories during this phase. Report required corrections instead.

## Inputs

Validate the following control files:

- `/work/AGENTS.md`
- `/work/eip-8025-analysis/TASK.md`
- `/work/eip-8025-analysis/phases/00-environment-validation.md`
- `/work/eip-8025-analysis/phases/01-spec-and-lighthouse-scope.md`
- `/work/eip-8025-analysis/phases/02-lighthouse-implementation.md`
- `/work/eip-8025-analysis/phases/03-grandine-gap-analysis.md`
- `/work/eip-8025-analysis/phases/04-grandine-proposal.md`

Validate the following sources:

- `/work/repos/EIPs/eip-8025.md`
- `/work/repos/consensus-specs/specs/_features/eip8025/`
- `/work/repos/lighthouse`
- `/work/repos/grandine`

Validate the writable analysis location:

- `/work/eip-8025-analysis`
- `/work/eip-8025-analysis/evidence`

## Outputs

Write:

- `/work/eip-8025-analysis/00-environment-validation.md`
- `/work/eip-8025-analysis/evidence/repository-baselines.md`
- `/work/eip-8025-analysis/evidence/instruction-validation.md`
- `/work/eip-8025-analysis/evidence/validation-log.md`

Create `/work/eip-8025-analysis/evidence` if it does not exist.

Do not write generated artifacts anywhere under `/work/repos`.

## Part 1 — Analysis instruction validation

### Required-file checks

Confirm that each required control file:

- exists;
- is a regular file;
- is readable;
- is non-empty.

Required files:

- `/work/AGENTS.md`
- `/work/eip-8025-analysis/TASK.md`
- `/work/eip-8025-analysis/phases/00-environment-validation.md`
- `/work/eip-8025-analysis/phases/01-spec-and-lighthouse-scope.md`
- `/work/eip-8025-analysis/phases/02-lighthouse-implementation.md`
- `/work/eip-8025-analysis/phases/03-grandine-gap-analysis.md`
- `/work/eip-8025-analysis/phases/04-grandine-proposal.md`

### Cross-file consistency

Check that:

- `TASK.md` defines Phases 0 through 4;
- phase numbering is unique and contiguous;
- the filenames listed in `TASK.md` exactly match the phase files present;
- all references use `phases/`, not the obsolete `passes/` directory;
- the source paths are consistently defined as:
  - `/work/repos/EIPs/eip-8025.md`
  - `/work/repos/consensus-specs/specs/_features/eip8025/`
  - `/work/repos/lighthouse`
  - `/work/repos/grandine`
- all generated-output paths are beneath `/work/eip-8025-analysis`;
- no control file treats `/state/codex/AGENTS.md` as required;
- no control file instructs Codex to download an EIP that is already available locally;
- no control file states that the Grandine repository is unavailable;
- no control file permits modifying a repository under `/work/repos`;
- no stale three-phase or four-phase workflow references remain;
- Phase 1 consumes Phase 0 outputs instead of recreating the same baseline work unnecessarily.

### Phase-file structure

Confirm that each phase file contains:

- a phase number and title;
- an objective;
- inputs;
- outputs;
- scoped work or required checks;
- completion checks.

Confirm that:

- Phases 0 through 3 instruct Codex not to begin the next phase;
- Phase 4 does not claim that another phase follows;
- each phase's declared outputs agree with `TASK.md`;
- later phases declare the earlier reports or evidence they depend on;
- no phase contains obsolete assumptions or source paths.

## Semantic workflow validation

Review the substantive instructions in `AGENTS.md`, `TASK.md`, and every phase
file. Do not limit validation to filenames, paths, headings, and declared
inputs or outputs.

For each phase, determine whether its instructions are:

- coherent: the objective, scope, work, outputs, and completion checks agree;
- feasible: the requested work can be performed from the declared inputs;
- sufficient: the work is enough to produce the declared outputs;
- properly scoped: the phase does not perform work assigned to another phase;
- correctly sequenced: all required evidence is produced before it is consumed;
- verifiable: completion checks can demonstrate that the objective was met;
- non-contradictory: no instruction conflicts with another instruction in the
  same phase, `TASK.md`, or `AGENTS.md`;
- non-circular: no phase requires an output that depends on that same phase or
  a later phase;
- traceable: major outputs can be traced to source evidence and earlier
  deliverables;
- proposal-complete: the combined phases gather all evidence needed by the
  final Grandine proposal.

Check the workflow as a whole for:

- missing prerequisite work;
- duplicate analysis across phases;
- gaps between specification analysis, Lighthouse analysis, Grandine mapping,
  and proposal generation;
- outputs produced but never consumed;
- inputs referenced but never produced;
- inconsistent terminology or evidence categories;
- requirements that disappear between phases;
- Grandine proposals not grounded in the gap analysis;
- final-report sections that no phase gathers evidence for.

For every problem, report:

| Location | Instruction | Problem | Consequence | Severity | Recommended correction |

Severity must be one of:

- blocker: the workflow cannot reliably proceed;
- major: a phase can run, but important output may be incomplete or incorrect;
- minor: clarity, duplication, or maintainability issue.

Do not rewrite the instruction files during Phase 0.

### Instruction validation report

Write:

- `/work/eip-8025-analysis/evidence/instruction-validation.md`

Use this structure:

# Instruction Validation

## Summary

State one of:

- Valid
- Valid with warnings
- Invalid

## Required files

| File | Exists | Readable | Non-empty | Status | Notes |

## Phase consistency

| Check | Result | Evidence | Severity |

## Path and policy consistency

| Check | Result | Evidence | Severity |

## Missing, stale, or conflicting instructions

List:

- missing files;
- unreadable or empty files;
- stale paths;
- obsolete `passes/` references;
- inconsistent phase names or numbers;
- conflicting output paths;
- obsolete assumptions;
- missing phase sections;
- duplicated work that should be factored into Phase 0;
- any instruction that risks modifying source repositories.

Do not repair the files during this phase.

## Part 2 — Workspace and repository validation

### Workspace checks

Confirm that:

- `/work` exists and is readable;
- `/work/eip-8025-analysis` exists or can be created;
- `/work/eip-8025-analysis/evidence` exists or can be created;
- the analysis directory is writable;
- generated files can be created beneath `/work/eip-8025-analysis`;
- no output path resolves beneath `/work/repos`.

Record filesystem or permission problems as blockers.

### Repository baseline checks

For each repository, record:

- absolute path;
- whether it is a valid Git repository;
- current branch or detached-HEAD state;
- HEAD commit;
- configured remotes;
- configured upstream, when present;
- working-tree status;
- pre-existing modified files;
- pre-existing untracked files;
- whether required task-specific paths exist.

Repositories:

- `/work/repos/EIPs`
- `/work/repos/consensus-specs`
- `/work/repos/lighthouse`
- `/work/repos/grandine`

Write the normalized baseline to:

- `/work/eip-8025-analysis/evidence/repository-baselines.md`

Use:

| Repository | Branch or state | HEAD | Upstream | Working tree | Required paths | Status |

Append relevant raw command output to:

- `/work/eip-8025-analysis/evidence/validation-log.md`

### EIPs repository checks

Verify that:

- `/work/repos/EIPs/eip-8025.md` exists;
- it is readable;
- it is non-empty;
- it appears to be an EIP-8025 document.

Do not analyze its technical requirements in this phase.

### Consensus-specs checks

Verify that:

- `/work/repos/consensus-specs/specs/_features/eip8025/` exists;
- it is readable;
- it contains at least one file;
- list the files present;
- relevant files are non-empty where applicable.

Do not perform requirement extraction or semantic comparison in this phase.

### Lighthouse checks

Verify that:

- `/work/repos/lighthouse` is a valid Git repository;
- the current branch is `optional-proofs-unstable`.

If the branch differs:

- report the actual branch;
- mark the expected-branch check as failed;
- do not switch branches.

Also verify that the repository is readable and that common project manifests or build entry points are identifiable.

Do not determine the final comparison base or classify branch changes in this phase.

### Grandine checks

Verify that:

- `/work/repos/grandine` is a valid Git repository;
- it is readable;
- its primary build manifest or entry points can be identified;
- likely test entry points can be identified without running a full build.

Do not inspect Grandine architecture in depth.

## Part 3 — Tool availability

Check availability of:

- `git`
- `rg`
- `find`
- `sed`
- `awk`

Also identify relevant build or language tools when present, such as:

- `cargo`
- `rustc`
- `python`
- `make`
- project-specific scripts.

Do not install missing tools.

Record:

- command path;
- version when available;
- whether the tool is required or optional for later phases.

Use:

| Tool | Available | Path or version | Required later | Notes |

## Main report

Write `/work/eip-8025-analysis/00-environment-validation.md` with:

# Phase 0: Environment, Repository, and Instruction Validation

## 1. Readiness summary

State one of:

- Ready for Phase 1
- Ready for Phase 1 with warnings
- Not ready for Phase 1

Provide separate results for:

| Dimension | Result | Blocking issues | Warnings |
|---|---|---|---|
| Instruction files | | | |
| Source repositories | | | |
| Tools and writable outputs | | | |

Phase 1 may proceed only when no dimension has a blocking issue.

## 2. Instruction-file validation summary

Summarize:

- required-file status;
- phase numbering and filename consistency;
- path consistency;
- output consistency;
- obsolete assumptions;
- required corrections.

Link or refer to:

- `/work/eip-8025-analysis/evidence/instruction-validation.md`

## 3. Workspace and output validation

Use:

| Path | Expected role | Status | Notes |

## 4. Repository baselines

Use:

| Repository | Branch or state | HEAD | Working tree | Remotes or upstream | Status |

## 5. Required source checks

Cover:

- local EIP file;
- consensus-spec feature directory;
- Lighthouse branch;
- Grandine repository and build entry points.

## 6. Tool availability

Use:

| Tool | Available | Path or version | Needed by later phases |

## 7. Blockers

List only issues that should prevent Phase 1 from starting.

Examples:

- missing or unreadable control files;
- inconsistent phase definitions;
- missing source repository;
- Lighthouse on the wrong branch;
- missing EIP file;
- missing consensus-spec feature directory;
- output directory not writable;
- required tools unavailable.

## 8. Non-blocking warnings

Examples:

- dirty working trees;
- missing optional tools;
- detached HEAD where acceptable;
- unexpected but usable repository remotes;
- minor instruction inconsistencies that do not change execution.

## 9. Recommendation

State explicitly whether Phase 1 should proceed.

When recommending against proceeding, identify the exact corrections required.

## Completion rules

Before finishing:

- verify all required Phase 0 outputs exist;
- verify control files were not edited;
- verify no repository under `/work/repos` was modified;
- list files created or updated;
- record failed commands and environmental limitations;
- do not begin Phase 1.

