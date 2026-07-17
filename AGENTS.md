# EIP-8025 Analysis Workspace Instructions

## Workspace

Source repositories:

    /work/repos/EIPs
    /work/repos/consensus-specs
    /work/repos/lighthouse
    /work/repos/grandine

Writable analysis directory:

    /work/eip-8025-analysis

Treat every repository under `/work/repos` as read-only. Write all reports, evidence, indexes, notes, and generated artifacts under `/work/eip-8025-analysis`.

Do not use `CODEX_HOME` for task output.

## Source roles

- `/work/repos/EIPs/eip-8025.md`: work-in-progress EIP text.
- `/work/repos/consensus-specs/specs/_features/eip8025/`: consensus-spec feature implementation.
- `/work/repos/lighthouse`: Lighthouse on the `optional-proofs-unstable` branch, containing a work-in-progress implementation.
- `/work/repos/grandine`: target client for the proposed implementation.

Only Lighthouse implementation analysis is branch-diff scoped. Grandine analysis is architecture and gap analysis at the checked-out revision.

## Repository safety

Do not modify files under `/work/repos`, including code, tests, specifications, EIP text, manifests, lockfiles, configuration, generated sources, or documentation.

Do not reset, clean, stash, amend, rebase, switch branches, discard changes, or update submodules.

Before substantial work, record each repository's branch, HEAD, remotes, and working-tree status. Preserve pre-existing changes.

Focused build or test commands may create normal ignored artifacts when a phase explicitly permits validation. Do not place analysis artifacts inside a repository.

## Evidence categories

Use these labels consistently:

### EIP statement
Text explicitly present in `/work/repos/EIPs/eip-8025.md`.

### Consensus-spec requirement
Behavior, types, constants, validation, transitions, activation, or tests defined under `/work/repos/consensus-specs/specs/_features/eip8025/`.

### Verified Lighthouse fact
Supported by Lighthouse code, branch-local diff, tests, configuration, call sites, or commit history.

### Verified Grandine fact
Supported by Grandine code, tests, configuration, call sites, or repository structure.

### Cross-client inference
A client-neutral implementation responsibility derived from the sources.

### Proposed Grandine change
A recommended Grandine work item derived from the gap analysis.

### Open specification question
A conflict, ambiguity, omission, placeholder, unfinished section, or mismatch among the EIP, consensus specs, and Lighthouse.

Never present an inference or proposal as a verified fact.

## Source conflicts

Do not assume the sources are synchronized.

When they differ:

1. State what each source says.
2. Identify whether Lighthouse follows the EIP, consensus specs, neither, or a hybrid.
3. Explain the likely significance.
4. Do not invent a resolution.
5. Carry unresolved conflicts into the final proposal and risk register.

Lighthouse code is authoritative for what Lighthouse implements. Grandine code is authoritative for Grandine architecture. Compare the EIP and consensus specs to determine intended behavior without assuming either is final.

## Lighthouse branch scoping

Scope Lighthouse analysis in two stages:

1. Determine the current branch's changes relative to its most defensible base.
2. Retain only changes directly or indirectly related to EIP-8025.

Do not assume the default branch is the base, every branch commit is relevant, every changed file is relevant, or every relevant symbol contains `8025`.

Use the current branch, upstream, remotes, commit graph, merge bases, branch-only commits, diff composition, commit messages, and locally available PR metadata.

Record the selected base, merge-base commit, exact diff range, alternatives, and uncertainty.

Classify Lighthouse branch-local changes as:

- **Direct:** implements an EIP-8025 or consensus-spec type, rule, transition, validation, activation behavior, or externally visible feature.
- **Supporting:** configures, integrates, serializes, hashes, persists, exposes, observes, or tests a direct change.
- **Unrelated:** no meaningful EIP-8025 role.

Every included file or hunk must have evidence. For mixed files, analyze only relevant symbols and hunks.

## Use of unchanged code

Unchanged Lighthouse or Grandine code may be inspected for callers, callees, ownership, lifecycle, state mutation, fork dispatch, serialization, hashing, storage, networking, APIs, and tests.

Label unchanged Lighthouse infrastructure as contextual rather than branch-introduced.

## Grandine mapping

Inspect Grandine directly. It is acceptable to name likely files, modules, traits, types, functions, and tests when supported by the checkout.

For every proposed change:

- cite the current Grandine component;
- identify the requirement basis;
- identify the Lighthouse analogue when useful;
- distinguish required behavior from suggested architecture;
- state confidence and open design choices;
- prefer Grandine's existing patterns over mechanically copying Lighthouse.

## Evidence standards

Support important conclusions with source text, spec code or tests, Lighthouse diff hunks, changed symbols, call sites, tests, configuration, serialization, hashing, upgrade logic, storage, networking, or commit history.

Record repository-relative paths, symbols or sections, line ranges when practical, tests, commits when useful, requirement IDs, and discrepancies.

Explain behavior, lifecycle, invariants, and architectural purpose rather than merely listing files.

## Analysis expectations

For important Lighthouse symbols and Grandine mapping targets, explain:

- representation and purpose;
- inputs and outputs;
- callers and callees;
- execution timing;
- state or storage effects;
- failure behavior;
- fork gating;
- serialization or hashing effects;
- networking, API, persistence, and metrics effects;
- tests;
- protocol invariants.

Trace EIP-8025 end to end. Cover happy paths, rejection paths, boundaries, activation, and interoperability. Explicitly state unaffected client areas.

## Commands and validation

Prefer focused searches, checks, and tests. Do not modify code to make commands pass.

Record commands, key outputs, failures, and environmental limitations in the validation log.

## Phase bookkeeping

At the start of each phase, state:

- phase number and name;
- source revisions and branches;
- working-tree caveats;
- input reports consumed;
- outputs to produce.

At the end:

- verify required outputs exist;
- list files created or updated;
- verify repositories were not intentionally modified;
- record failed validation commands.

## Quality checks

Before completing a phase, verify:

- evidence categories are not conflated;
- claims are evidence-backed;
- Lighthouse conclusions are branch-local;
- unrelated Lighthouse changes are excluded;
- unchanged Lighthouse code is labeled as context;
- Grandine facts come from Grandine;
- proposed Grandine changes trace to requirements or verified evidence;
- specification conflicts remain visible;
- generated files are under `/work/eip-8025-analysis`;
- repositories remain unmodified.
