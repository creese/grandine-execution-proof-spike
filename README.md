# Grandine Execution Proof Spike 

This spike studies what it would take to support [EIP-8025: Optional Execution Proofs](https://eips.ethereum.org/EIPS/eip-8025) in Grandine.

It starts with the draft specifications, then examines the Lighthouse prototype and maps the resulting responsibilities onto Grandine. 

## Source repositories

The repositories under `/work/repos` are treated as read-only:

* `EIPs` — draft EIP-8025 text
* `consensus-specs` — EIP-8025 feature documents
* `lighthouse` — the pinned prototype implementation
* `grandine` — the target client architecture

## Workflow

The spike is split into five phases:

1. Validate the workspace and pinned repositories.
2. Extract requirements and isolate the Lighthouse change set.
3. Study the Lighthouse implementation end to end.
4. Map the requirements and implementation areas to Grandine.
5. Review the user-authored implementation proposal against the collected evidence.

## Main artifacts

The main reports are:

* `01-spec-and-lighthouse-scope.md`
* `02-lighthouse-implementation.md`
* `03-grandine-gap-analysis.md`
* `04-grandine-proposal-review.md`

Supporting indexes, mappings, baselines, and validation records are stored under `evidence/`.

The proposal itself is user-authored at `grandine-eip-8025-proposal.md`. Phase 4 reviews it without modifying it.

## Evidence rules

The analysis distinguishes:

* specification requirements
* verified Lighthouse behavior
* verified Grandine architecture
* cross-client inferences
* proposed Grandine changes

Lighthouse is implementation evidence, not the specification. 
