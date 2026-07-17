# Repository Baselines

Recorded 2026-07-17 UTC before substantive Phase 0 validation. All four working trees were clean. “Clean” below means `git status --short --branch` reported only branch/upstream tracking and no modified, staged, or untracked paths.

| Repository | Branch or state | HEAD | Upstream | Working tree | Required paths | Status |
|---|---|---|---|---|---|---|
| `/work/repos/EIPs` | `feat/eip8025` | `3aa6b30641ef8f98f8945b29ee92c1e8aed1070a` | `origin/feat/eip8025` | Clean; no pre-existing modified or untracked files | `EIPS/eip-8025.md`: readable, non-empty regular file | Pass |
| `/work/repos/consensus-specs` | `master` | `8c12caee279d77b322446d33440b37479117dcde` | `origin/master` | Clean; no pre-existing modified or untracked files | `specs/_features/eip8025/`: readable; 5 non-empty files | Pass |
| `/work/repos/lighthouse` | `optional-proofs-unstable` | `0dd6c3b8cf3b1eece82a0a7ee87282a222d93bf5` | `origin/optional-proofs-unstable` | Clean; no pre-existing modified or untracked files | Root `Cargo.toml` and `Makefile` identified | Pass |
| `/work/repos/grandine` | `develop` | `d8adce56ed2a324c5abd72af8c0ff94a492ae7d3` | `origin/develop` | Clean; no pre-existing modified or untracked files | Root `Cargo.toml`, `Makefile`, workspace crates, and `scripts/download_spec_tests.sh` identified | Pass |

## Remotes

| Repository | Fetch | Push |
|---|---|---|
| EIPs | `git@github-creese:frisitano/EIPs.git` | `git@github-creese:frisitano/EIPs.git` |
| consensus-specs | `git@github-creese:ethereum/consensus-specs.git` | `git@github-creese:ethereum/consensus-specs.git` |
| Lighthouse | `git@github-creese:eth-act/lighthouse.git` | `git@github-creese:eth-act/lighthouse.git` |
| Grandine | `git@github-creese:grandinetech/grandine.git` | `git@github-creese:grandinetech/grandine.git` |

## Task-specific source presence

- EIP: `EIPS/eip-8025.md` is 49,035 bytes and begins with front matter declaring `eip: 8025` and the title “Optional Execution Proofs.” This is only an identity/presence check, not technical analysis.
- Consensus specs: `beacon-chain.md` (3,152 bytes), `fork-choice.md` (4,204), `p2p-interface.md` (9,480), `proof-engine.md` (2,854), and `prover.md` (3,034) are present and non-empty. This is only an inventory.
- Lighthouse: the expected branch check passed. No comparison base was selected and no branch changes were classified in Phase 0.
- Grandine: build/test entry points are identifiable from the root Rust workspace, root `Makefile`, crate manifests, and the consensus-spec test download script. No architecture mapping was performed.

Raw command output is retained in `evidence/validation-log.md`.
