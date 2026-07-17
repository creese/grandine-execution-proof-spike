# Grandine EIP-8025 Architecture Symbol Index

Revision: `d8adce56ed2a324c5abd72af8c0ff94a492ae7d3`. All rows are **Verified Grandine facts** about current architecture; relevance is a **Cross-client inference**, not existing EIP-8025 support. A repository search found no EIP-8025 symbols.

| Responsibility | Grandine symbol | Path | Lines | Current role | EIP-8025 relevance | Confidence |
|---|---|---|---:|---|---|---|
| Fork identity | `Phase` | `types/src/nonstandard.rs` | 61-70 | Enumerates phases through Gloas | EIP is optional, so do not add a phase unless specs change | High |
| Fork config | `Config::{gloas_fork_epoch,gloas_fork_version,fork_epoch}` | `types/src/config.rs` | 35-84, 902-929 | Activates and identifies forks | Reuse current fork version for signing domain; add operator capability, not fork | High |
| Gloas payload objects | `ExecutionPayloadBid`, `ExecutionPayloadEnvelope`, signed forms | `types/src/gloas/containers.rs` | 109-143, 255-264 | Separates execution payload from beacon block | Envelope acceptance is the likely proof-request registration point | High |
| Generic fork access | `PostGloasBeaconState`, block/body traits | `types/src/traits.rs` | 930-997, 1808-1823 | Abstracts Gloas fields | Useful context only; proof data must not enter state/block traits | High |
| Bounded bytes | `ByteList`, `ContiguousList` | `ssz/src/byte_list.rs`; `types/src/bellatrix/containers.rs` | 25-98; 88-127 | SSZ bounded variable bytes/lists with hashing | Natural fixed-ByteList representation; progressive list is absent/unresolved | High |
| Signing domains | `compute_domain`; domain constants | `helper_functions/src/misc.rs`; `types/src/*/consts.rs` | symbol; e.g. `gloas/consts.rs:13-14` | Builds fork/genesis-bound domains | Reuse for proof signing after domain conflict resolves | High |
| Execution boundary | `ExecutionEngine::{notify_new_payload,notify_forkchoice_updated}` | `execution_engine/src/execution_engine.rs` | 19-55 | Async trait boundary to EL | Add a separate proof-engine abstraction; do not overload EL validity | High |
| Engine messages | `ExecutionServiceMessage` payload variants | `execution_engine/src/messages.rs` | 20-35 | Channels payload/fork-choice operations | Pattern for request/response/SSE proof-node client | Medium |
| EL payload types | `PayloadStatusV1`, `ForkChoiceStateV1` | `execution_engine/src/types.rs` | 448-474, 675-765 | Engine API wire types | Payload request hashing needs an owned canonical request type | High |
| Central store | `Store<P,S>` | `fork_choice_store/src/store.rs` | 112-261 | Owns chain, gossip dedup and sidecar caches | Candidate owner for canonical proof context/cache, but proof must not change fork choice | High |
| Gloas validation | `validate_execution_payload_bid`, `apply_execution_payload_bid` | `fork_choice_store/src/store.rs` | 1374-1538, 3145 | Validates/applies payload bids | Validation/action pattern is reusable; envelope import hook needs tracing | High |
| Gloas envelope transition | `validate_execution_payload_envelope*`, `process_execution_payload` | `transition_functions/src/gloas/execution_payload_processing.rs` | 37-207 | Validates envelope structure/signature and applies it to state | Supplies request-shape context, but proof processing remains off-transition | High |
| Gloas controller limitation | post-Gloas envelope TODOs | `fork_choice_control/src/mutator.rs` | 460-475 | Payload/commitments/requests are not yet obtained from envelopes in this path | Proof notification cannot assume a finished envelope import event | High |
| Sidecar validation | blob/data-column validation methods | `fork_choice_store/src/store.rs` | 2177-2660 | IGNORE/REJECT/ACCEPT, dependencies, signatures | Closest validation/dedup model for off-block proof objects | High |
| Controller orchestration | `Controller` and spawn methods | `fork_choice_control/src/controller.rs` | 80+, 385-399, 800-806 | Routes work into task pool | Add proof verification and import tasks off critical path | High |
| Task outcomes | `ExecutionPayloadBidTask`; `ValidationOutcome` | `fork_choice_control/src/tasks.rs` | 199, 504-520 | Runs validation and returns network outcome | Reuse for proof gossip/API processing | High |
| Controller messages | `P2pMessage` | `fork_choice_control/src/messages.rs` | 211+ | P2P-to-controller command surface | Needs proof gossip/RPC request/response variants | High |
| Events | `Topic`, `Event`, `EventChannels` | `fork_choice_control/src/events.rs` | 45-173 | Broadcasts blocks, bids, finality, etc. to SSE | Add execution-payload/proof lifecycle events if prover is in-process | High |
| Gossip types | `GossipKind`, `GossipTopic` | `eth2_libp2p/src/types/topics.rs` | 157-190, 250-333 | Topic naming/subscription and fork rules | Add execution-proof topic and optional subscription gating | High |
| Gossip payload | `PubsubMessage` | `eth2_libp2p/src/types/pubsub.rs` | 50-80, 175, 500-630 | SSZ decode/encode dispatch | Add signed-proof variant and size-safe decode | High |
| Gossip cache | `GossipCache` | `eth2_libp2p/src/service/gossip_cache.rs` | 1-220 | Retries failed publications by kind | Proof retry policy may reuse it; validation dedup belongs higher | Medium |
| RPC protocol | `Protocol`, `SupportedProtocol`, `RequestType`, limits | `eth2_libp2p/src/rpc/protocol.rs` | 179-226, 248-354, 653-828 | Protocol IDs, negotiation, codec bounds, termination | Add by-range/by-root/status protocols and exact SSZ bounds | High |
| RPC rate limit | `RateLimiterConfig`, `RPCRateLimiter` | `eth2_libp2p/src/rpc/rate_limiter.rs` | 107-233, 333-385 | Per-protocol quotas | Add proof request quotas and response byte/count limits | High |
| Network service | `Network::publish`, inbound RPC dispatch | `eth2_libp2p/src/service/mod.rs` | 873-905, 1553-1735 | libp2p transport integration | Extend dispatch and stream termination for proofs | High |
| P2P integration | `Network`, `ApiToP2p` | `p2p/src/network.rs`; `p2p/src/messages.rs` | `network.rs:756-764,2219+`; `messages.rs:93+` | Routes API/controller/network objects | Add publish and proof RPC routing | High |
| Sync | `SyncManager` request trackers | `p2p/src/sync_manager.rs` | 900-1047, 1412-1485 | By-root/range sync for blocks/sidecars | New proof sync state/status handshake; reuse trackers conceptually | High |
| ENR | `Eth2Enr`, discovery service local ENR | `eth2_libp2p/src`; `service/mod.rs` | `service/mod.rs:725,1170-1171` | Advertises fork/subnet metadata | Add `eproof` key plus peer capability parsing | Medium |
| HTTP routing | `standard` routes | `http_api/src/routing.rs` | 239, 406-492, 568-634 | Beacon/validator/pool endpoints | Add submit/query endpoint(s), modes and size handling | High |
| SSE API | `beacon_events` | `http_api/src/standard.rs` | 2475-2515 | Streams controller events | Prover can consume payload event; exact event unresolved | High |
| Validator lifecycle | `Validator` slot loop/publish methods | `validator/src/validator.rs` | 757-805, 1064+, 1397+ | Produces and signs validator duties | Proof proving is not a duty; separate service should avoid slot critical path | High |
| Persistence | `Storage` implementations / database crate | `database/src`; `fork_choice_store/src/store.rs` | crate; store generic | Persists canonical consensus objects | Proof retention may need a new column/schema or bounded auxiliary store | Medium |
| Metrics | `Metrics`, gossip counters | `prometheus_metrics/src/metrics.rs` | 59-81, 307-347, 1205-1206 | Network/fork-choice observability | Add proof verify latency/result/cache/network metrics | High |
| Spec fixtures | Gloas SSZ macros/tests | `types/src/gloas/spec_tests.rs` | 217-225, 409-417 | Runs consensus-spec SSZ fixtures | Add proof SSZ/root vectors when published | High |
| CLI/features | main CLI and `features` crate | `grandine/src`; `features/src` | crates | Operator configuration/experimental flags | Add prover/verifier/proof-engine endpoint capability gates | Medium |

## Explicitly unaffected current symbols

**Cross-client inference:** beacon-state variants, beacon-block bodies, phase upgrades, proposer/attester duty computation, slashing protection, attestation verification, consensus rewards/penalties, and fork-choice weights require no EIP-8025 change. Gloas payload-envelope acceptance supplies context, but optional-proof failure must not reject the envelope or alter EL-derived payload validity.
