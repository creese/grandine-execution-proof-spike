# Phase 3: Grandine EIP-8025 Architecture and Gap Analysis

## 1. Executive summary

**Verified Grandine fact:** Grandine at `d8adce56…` contains Gloas support but no EIP-8025 symbols, proof containers, proof engine, proof gossip/RPC/status, ENR capability, proof storage, prover service, or proof API. This is therefore a broad new auxiliary subsystem, not a small fork patch.

**Cross-client inference:** Grandine already has nearly every architectural seam needed: SSZ bounded bytes/tree hashing; Gloas payload envelopes; EL notification channels; fork-choice controller tasks; store validation/action patterns; gossip topics and message codecs; bounded req/resp and rate limiting; sync trackers; SSE; BLS signing; storage; metrics; and consensus fixture macros. The safest shape is a separately gated proof service integrated with those seams while leaving beacon state, blocks, transition functions, duties, slashing, attestation timing, EL validity and fork-choice weights unchanged.

The specification is not implementation-ready at interoperability boundaries. Proof maximum/type, public input, domain, quorum, payload lifecycle, gossip polarity/dedup order, retention, activation and EL guest semantics remain unresolved. Phase 4 must carry these as decisions/risks rather than silently adopting Lighthouse’s hybrid implementation.

## 2. Grandine revision and repository state

Phase start: Phase 3, Grandine Architecture and Gap Analysis. Inputs consumed: Phase 1 scope/requirements/discrepancies and Phase 2 Lighthouse report/index; source EIP, consensus feature, Lighthouse checkout and Grandine checkout inspected. Outputs: this report, `grandine-git-baseline.txt`, `grandine-symbol-index.md`, `lighthouse-grandine-map.md`, and validation-log update.

Grandine is clean on `develop` at `d8adce56ed2a324c5abd72af8c0ff94a492ae7d3`, tracking `origin/develop`. Optional submodules shown with `-` were already uninitialized. Exact commands and entry points are in `evidence/grandine-git-baseline.txt`. EIPs `3aa6b306…`, consensus-specs `8c12caee…`, and Lighthouse `0dd6c3b8…` also remained clean at phase start.

## 3. Relevant Grandine architecture

- **Types/SSZ/hashing:** fork containers live under `types/src/<fork>` and derive/use Grandine SSZ. `ssz::ByteList` supports bounded variable bytes (`ssz/src/byte_list.rs:25-98`). Gloas defines `ExecutionPayloadEnvelope` and its signed form (`types/src/gloas/containers.rs:130-143,262-264`) and transition validation (`transition_functions/src/gloas/execution_payload_processing.rs:37-207`). No progressive-byte-list analogue was found.
- **Forks/activation/upgrades:** `Phase` ends at Gloas (`types/src/nonstandard.rs:61-70`); `Config` carries Gloas epoch/version and dispatch (`types/src/config.rs:83-84,902-929`). EIP-8025 says no fork, so capability flags should gate services without a new state upgrade.
- **Execution interaction:** `ExecutionEngine` exposes `notify_new_payload` and `notify_forkchoice_updated` (`execution_engine/src/execution_engine.rs:19-55`) through message channels. Proof verification/generation needs a distinct interface because it must not redefine EL validity.
- **Validation/transition/fork choice:** `Store` owns chain context, dedup and sidecar caches; controller tasks validate asynchronously and report action/outcome. Gloas bid and sidecar validation show Grandine-native IGNORE/REJECT/ACCEPT patterns. Envelope types and transition helpers exist, but `fork_choice_control/src/mutator.rs:460-475` still has TODOs to obtain post-Gloas envelope payload/commitments/requests; Phase 3 therefore does not assume a complete envelope import hook. Existing transition functions should not gain proof processing.
- **Production/duties:** `validator::Validator` owns slot-critical duties and publications. Proof production is not a duty; use a separate event-driven service and existing signer access rather than extending proposer/attester scheduling.
- **Networking:** `eth2_libp2p` owns `GossipKind`, `PubsubMessage`, protocol IDs, codec limits, response termination and rate limiters. `p2p` routes messages and manages by-range/by-root sync. These require systematic variants for proof gossip and three RPCs.
- **API/events:** `http_api` uses Axum routing and `beacon_events`; `fork_choice_control::EventChannels` publishes block/bid/finality events. A proof service can consume a payload-accepted event and publish signed results through a bounded endpoint.
- **Storage/caches:** `Store<P,S>` and `database` provide persistence seams, while sidecars provide cache/prune examples. Proofs are auxiliary, but canonical finalized-to-head serving may require durable or reconstructable storage.
- **Metrics/CLI/tests:** Prometheus has generic gossip/fork-choice instrumentation; CLI/features compose optional services; `types/src/gloas/spec_tests.rs` demonstrates consensus-spec SSZ fixtures.

## 4. Requirement-to-Grandine mapping

| Requirement ID | Required behavior | Current support | Gap | Integration point | Confidence |
|---|---|---|---|---|---|
| EIP8025-SCOPE-001 | Opt-in, supplementary path | Services and EL validity are separable | Isolated feature modes and negative consensus boundary | service composition/controller tests | High |
| EIP8025-ROLE-001 | Prover/verifier independently selectable | CLI composition and validator signer | Two proof roles/configuration | `grandine`, `features`, proof service | High |
| EIP8025-TYPE-001 | SSZ uint8 proof type | SSZ scalar/newtype patterns | Proof-specific type | `types` | High |
| EIP8025-TYPE-002 | Signed proof container | SSZ/BLS container patterns | Proof-specific container | `types` | High |
| EIP8025-TYPE-003 | Proof container and bounded bytes | Fixed `ByteList`; no progressive list | Container plus unresolved representation | `types`; possibly `ssz` | High |
| EIP8025-TYPE-004 | Public input | None | Source-incompatible container shape | `types` | High |
| EIP8025-CONSTANT-001 | Maximum proof bytes | Type-level bounds/codec limits | Conflicting constant | types and RPC codecs | High |
| EIP8025-CONSTANT-002 | Four proofs/types per payload | Bounded-list/preset patterns | Proof constant | types/RPC/status | High |
| EIP8025-DOMAIN-001 | Proof signature domain | Domain helper/constants | Conflicting domain constant | types/helper call sites | High |
| EIP8025-HASH-001 | Payload-request root | Engine structs/tree hashing | Canonical owned request schema | proof service/engine boundary | High |
| EIP8025-HASH-002 | Proof signing root | Domain/signing-root/BLS helpers | Proof-specific call path | proof service/helpers | High |
| EIP8025-ENGINE-001 | Verify proof | Injectable EL trait pattern only | Separate proof-engine client | new service/crate | High |
| EIP8025-ENGINE-002 | Notify accepted payload | EL/Gloas acceptance pipeline | Non-blocking context registration/notify hook | controller/proof service | High |
| EIP8025-ENGINE-003 | Notify head/safe/finalized | EL fork-choice-update path | Independent proof-engine notification | controller/proof client | Medium |
| EIP8025-ENGINE-004 | Request asynchronous proof | Channels and HTTP/SSE patterns | Proof request/event transport | proof client/service | High |
| EIP8025-VALIDATION-001 | Verify proof result/bindings | None | Verification pipeline, spec conflict | controller task/proof engine | High |
| EIP8025-VALIDATION-002 | Active validator | State access/epoch helpers | Proof signer eligibility check | controller task/store snapshot | High |
| EIP8025-VALIDATION-003 | BLS signature | BLS/domain helpers | Proof signature check | controller task | High |
| EIP8025-VALIDATION-004 | Non-empty/bounded proof bytes | SSZ bounds/outcome model | Cheap pre-validation | decoder/controller | High |
| EIP8025-VALIDATION-005 | Unknown-root/dedup behavior | Seen maps and sidecar validation patterns | Proof context, queue and dedup rules | auxiliary store/controller | High |
| EIP8025-VALIDATION-006 | Quorum/status | None | Undefined observational status model | auxiliary proof store/API | High |
| EIP8025-TRANSITION-001 | No state transition effect | Transition/controller separation | No code change; enforce boundary | negative tests | High |
| EIP8025-FORKCHOICE-001 | No proof-based fork choice | EL-derived fork choice | No code change; prohibit mutation | controller/fork-choice tests | High |
| EIP8025-LIVENESS-001 | Never wait for proofs | Task pool/event architecture | Dedicated bounded async work | proof service/task pool | High |
| EIP8025-STORAGE-001 | Canonical finalized-to-current retention | Generic storage and canonical queries | Proof persistence/cache/pruning/reorg policy | database/auxiliary store | Medium |
| EIP8025-GOSSIP-001 | Proof topic and scoring | Generic gossip stack | Topic/message/validation/routing | `eth2_libp2p`, `p2p`, controller | High |
| EIP8025-RPC-001 | Proofs by range | By-range protocol patterns | Proof protocol/types/serving | RPC and `p2p` | High |
| EIP8025-RPC-002 | Proofs by root | By-root protocol patterns | Proof protocol/types/serving | RPC and `p2p` | High |
| EIP8025-RPC-003 | Proof status | Status/sync patterns | Status handshake and sync state | RPC/peer metadata/sync manager | High |
| EIP8025-RPC-004 | Derived proof request cap | Codec/rate limit framework | Exact proof limits | RPC codec/rate limiter | High |
| EIP8025-DISCOVERY-001 | `eproof` ENR | `Eth2Enr`/discovery | ENR key and peer capability | discovery/peer metadata | High |
| EIP8025-PROVER-001 | Prover lifecycle | Events/SSE/signer | Separate non-duty job manager | proof service/API/signer | High |
| EIP8025-EL-001 | Stateless EL input/output | No EL proof stack | External capability/source gap | proof-engine boundary | Low |
| EIP8025-EL-002 | Witness | No EL witness builder | External capability/source gap | external proof node/EL | Low |
| EIP8025-EL-003 | Guest validation | No EL guest | External capability/source gap | external proof node/EL | Low |
| EIP8025-EL-004 | Transaction key checks | No EL guest logic | External capability/source gap | external proof node/EL | Low |
| EIP8025-EL-005 | Host construction | No EL recorder/witness builder | External capability/source gap | external proof node/EL | Low |
| EIP8025-SSZ-001 | EL guest schema prefix | No EL guest codec | External capability/source gap | external proof engine | Low |
| EIP8025-SSZ-002 | EL guest bounds | CL SSZ only | External capability/source gap | external proof engine | Low |
| EIP8025-ACTIVATION-001 | Runtime opt-in, no fork | CLI/services; `Phase` ends at Gloas | Capability gates/subscriptions/ENR | composition/network | High |
| EIP8025-API-001 | BN/proof-node APIs/events | Axum/SSE patterns | Proof routes and event client | `http_api`, proof client | High |
| EIP8025-TEST-001 | Conformance/client tests | Spec-test macros; no 8025 vectors | Full proof test/interop matrix | affected crates/harness | High |
| EIP8025-SECURITY-001 | Bounds/rate limits/scoring | Network controls/metrics | Proof-specific budgets and outcomes | network/controller/metrics | High |
| EIP8025-SECURITY-002 | Consensus containment | Architecture permits isolation | No consensus change; negative tests | service/controller boundary | High |

The row-level mapping for all 44 stable IDs is in `evidence/lighthouse-grandine-map.md`. The exact-ID set was mechanically verified against `evidence/spec-requirement-index.md`.

## 5. Functional-area gap analysis

### Representation and hashing

**Proposed Grandine change:** define proof wire containers in `types`, preferably as a non-fork module because the feature is optional. Use Grandine derives and type-level bounds. Inputs are payload request data and proof metadata; outputs are canonical SSZ bytes/tree roots. Failure is decode/bound violation before expensive checks. No state/block serialization changes.

The fixed `ByteList` abstraction is reusable if fixed bounded bytes win. Progressive SSZ would be a new `ssz` capability with much wider review impact. Public-input and maximum conflicts directly change roots and interoperability; they must be compile-time/test-vector decisions, not runtime tolerance.

### Payload context and proof engine

At the eventually normative accepted pre-Gloas payload or Gloas-envelope point, construct an owned canonical payload request, hash it, and register `(block root, request root, slot/fork context)`. Grandine's checked-out controller still has post-Gloas payload-envelope TODOs, so this hook depends on pending Gloas integration and cannot yet be named as an existing caller. Notify/request proof work asynchronously. A new `ProofEngine` trait should return typed transport/verification errors and expose verify/request/status/events as selected by the final API. It should be injectable for tests like `ExecutionEngine`, but separate so a proof timeout/failure cannot become an EL rejection.

Head/safe/finalized notification can reuse data already sent through fork-choice-updated, but needs an independent proof-engine message only if retained by the final spec. Lighthouse did not demonstrate it, so this is not safe to omit merely for parity.

### Validation, status and retention

Validation ordering should cheaply enforce size/non-empty, known request root, proof type, dedup and signer index before BLS and proof-engine verification. The active-validator check missing in Lighthouse must use the specified epoch once clarified. Accepted proofs update auxiliary status keyed by payload/request/proof type/prover; rejected proofs never mutate consensus state.

Quorum must not call Grandine’s EL-payload-valid or fork-choice mutation APIs. Status is observational only. Retention needs canonical-chain association, reorg handling, finalized-start pruning, request-count bounds and explicit memory/disk budgets. Lighthouse’s 8,192-entry memory LRUs do not prove the EIP retention requirement.

### Gossip, RPC, discovery and sync

Add a signed-proof pubsub variant/topic, optional subscriptions, decode maxima, controller task, peer outcome and dedup. Unknown-root handling needs a bounded dependency queue if queueing is chosen. Add by-range, by-root and status protocol identifiers, request/response types, stream termination, codecs, rate quotas, handlers and peer sync state. Serve only canonical requested data in the allowed window, cap count and bytes, deduplicate roots/types, and penalize malformed/over-limit behavior consistently with existing RPC policy.

The `eproof` ENR key must be written only when proof networking is enabled and parsed into peer capability. Dynamic proof-type advertisement needs a registry/version rule not supplied by current sources.

### Prover/API lifecycle

Create a non-duty service that consumes the normative accepted-payload event, fetches/constructs the canonical request, selects an eligible local validator, requests proof generation, verifies result binding, signs, submits to the BN and gossips after local acceptance. Bound concurrent jobs, expiry, retries and shutdown. Never wait in proposal or attestation paths. Grandine’s existing controller events/SSE and signing infrastructure are reusable; Lighthouse’s “first doppelganger-safe validator” and five-minute tracking are implementation choices, not requirements.

### EL guest/host/witness

No Grandine component implements the EIP’s EL guest/host/witness semantics. The `zkvm` workspace crates prove CL state transitions and are not an analogue. Treat this as an external proof-engine integration dependency. Without the pinned execution-specs source, Phase 3 cannot validate witness formats or EL SSZ semantics.

## 6. Concrete likely files, modules, and symbols

Likely extensions are `types/src/lib.rs` plus a new proof module; `execution_engine` or a sibling proof-engine crate; `fork_choice_control/{controller,tasks,messages,events}.rs`; an auxiliary store/cache near `fork_choice_store`; `eth2_libp2p/src/types/{topics,pubsub}.rs`, `rpc/{protocol,rate_limiter,...}` and `service/mod.rs`; `p2p/src/{messages,network,sync_manager}.rs`; `http_api/src/{routing,standard,error}.rs`; service composition/CLI under `grandine/src`; persistence under `database`; metrics under `prometheus_metrics`; and tests beside each. These are evidence-backed targets, not a final file plan.

## 7. Lighthouse versus Grandine architecture

| Concern | Lighthouse | Grandine | Porting implication |
|---|---|---|---|
| Core orchestration | BeaconChain methods/caches | Controller tasks + Store actions | Model proofs as tasks/actions/messages |
| Execution boundary | proof client embedded beside execution layer | trait/channel execution service | Prefer a separate injectable proof service |
| Gloas | BN hooks partly support it; prover remains pre-Gloas | checked-out Gloas types/events/pipeline | Start from envelope-native timing once specified |
| Cache | dedicated memory LRUs/maps | Store plus storage abstraction | Design canonical retention explicitly |
| Networking | Lighthouse enums/router/sync | `eth2_libp2p` transport + `p2p` integration | Extend both layers consistently |
| Fork-choice effect | optional quorum can mark payload valid | EL payload status feeds fork choice | Do not port this experimental behavior |
| Validator prover | VC `ProofService` | integrated validator plus controller events | Separate service from duties, reuse signer |

## 8. Reusable Grandine abstractions

SSZ derives and `ByteList`; `Hc`/tree hashing; BLS/domain helpers; Gloas containers; execution trait/message injection; controller task pool and validation outcomes; Store context and canonical queries; sidecar caches; pubsub topic/message codec; RPC limits/rate limiter/termination; P2P sync request tracking; ENR extension; Axum routes/SSE; event channels; storage generic; Prometheus counters/histograms; spec-test macros.

## 9. New capabilities likely required

Proof types and constants; canonical payload-request representation/root; proof-engine interface/client; payload-context and proof-status store; active-signer and cryptographic verification pipeline; proof gossip; three RPC protocols and proof sync; `eproof` ENR; prover job manager; BN/proof-node APIs; retention/pruning; proof metrics/configuration; test vectors, mocks and interop harness.

## 10. Areas likely unaffected

**Cross-client inference:** beacon-state and block schemas, fork upgrades, epoch/slot transition logic, proposer/attester/sync duties, operation pools for consensus operations, slashing conditions/protection, rewards/penalties, attestation verification, fork-choice weights, block production contents, builder bidding semantics and EL re-execution remain unchanged. Generic network/API/metrics code changes only to carry the auxiliary feature. Disabled nodes should neither advertise, subscribe, request, prove, verify nor retain proofs.

## 11. Testing and fixture gaps

Required tests include SSZ/JSON/root/signing vectors; every bound; domain/fork/genesis cases; unknown root; inactive/out-of-range signer; empty/oversize/bad signature/bad proof; dedup races; quorum ambiguity fixtures; reorg/finality retention; canonical range/root serving; RPC caps/rate limits/termination; ENR gating; disabled modes; proof-engine timeout/retry/auth; prover cancellation and validator selection; no-delay and no-fork-choice-effect assertions; multi-client interoperability. No EIP-8025 consensus fixtures are present, so locally authored tests must be distinguished from normative vectors.

## 12. Dependencies and ordering constraints

Resolve wire constants/public input/domain/quorum/lifecycle first. Then land types/roots and vectors; proof-engine interface and mocks; payload-context cache; validation/status; storage policy; gossip/RPC/ENR; prover/API; observability/config; interop. Network codecs cannot stabilize before exact SSZ maxima. Status/sync/storage cannot stabilize before quorum and retention. Prover work cannot stabilize before event/request semantics and external proof API.

## 13. Complexity and consensus risk

| Work area | Complexity | Consensus risk | Reason | Dependencies |
|---|---|---|---|---|
| Wire types/hash/domain | High | High | Any mismatch breaks roots/signatures/interoperability | spec resolutions |
| Proof engine/API | High | Medium | External, large-data, async failures | canonical request/API |
| Validation/status | High | High | Eligibility/dedup/context mistakes accept invalid claims | types, engine, quorum |
| Retention/reorg | High | Medium | Canonical serving and resource exhaustion | status semantics |
| Gossip/RPC/ENR | High | Medium | DoS and cross-client framing risks | exact bounds/types |
| Prover lifecycle | High | Low-to-medium | Must not affect duties/liveness | events, signer, engine |
| Consensus state/fork choice | None expected | Critical boundary | Any mutation violates containment | architecture tests |

## 14. Grandine design questions

1. Fixed `ByteList` or new progressive list, and which maximum?
2. Root-only public input or root/result/config; how are guest and chain identity bound?
3. Domain `0x0D` or `0x0F`, and which fork version signs?
4. Gloas-only or pre-Gloas too; envelope, payload, or block event?
5. What is quorum `k`, per payload/type/prover, and is status purely observational?
6. Which epoch defines validator activity?
7. In-memory versus durable proof retention; exact finality/reorg pruning?
8. Queue unknown roots or ignore; exact dedup order/key?
9. Startup-only flags or live capability changes; how is ENR refreshed?
10. Separate crate/service versus execution-engine sibling; standard or vendor API?
11. How are proof types registered and verifier/guest versions negotiated?
12. How does pending Gloas work constrain envelope acceptance hooks?

## 15. Proposal inputs for Phase 4

Phase 4 should propose a staged, separately gated auxiliary subsystem; name the concrete Grandine-native seams above; make containment an explicit invariant; require resolution checkpoints before wire/network work; include resource budgets and reorg/finality behavior; distinguish required behavior from suggested architecture; and carry every unresolved source conflict into its risk register and acceptance criteria.

## 16. Appendix

Evidence artifacts: `evidence/grandine-git-baseline.txt`, `evidence/grandine-symbol-index.md`, `evidence/lighthouse-grandine-map.md`, Phase 1 requirement/discrepancy indexes, and Phase 2 Lighthouse report/index. No Phase 4 work was begun.
