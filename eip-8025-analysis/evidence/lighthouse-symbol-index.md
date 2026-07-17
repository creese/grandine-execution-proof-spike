# Lighthouse EIP-8025 Symbol Index

Scope: verified branch-local symbols in `494b00a3491e2c5e281f6972aa00694b17f16722..0dd6c3b8cf3b1eece82a0a7ee87282a222d93bf5`. Lines refer to the checked-out HEAD. The original Phase 1 inventory is preserved and the caller, test, and refinement notes below are superseding Phase 2 rerun findings.

| Symbol | Path | Lines | Classification | Requirement IDs | Purpose | Callers | Tests |
|---|---|---:|---|---|---|---|---|
| `MaxProofSize`, `ProofData`, `ProofType`, `MaxExecutionProofsPerPayload` | `consensus/types/src/execution/eip8025.rs` | 19-36 | Direct | TYPE-001/003, CONSTANT-001/002 | SSZ proof bounds and identifiers | Network codecs, proof engine, caches | Same file 213-354 |
| `DOMAIN_EXECUTION_PROOF`, `MIN_REQUIRED_EXECUTION_PROOFS` | same | 39-42 | Direct | DOMAIN-001, VALIDATION-006 | Signing domain and default proof count | Signature/domain and status code | Domain tests in chain spec; status tests |
| `PublicInput`, `ExecutionProof`, `SignedExecutionProof` | same | 51-92 | Direct | TYPE-002/003/004, HASH-001/002 | SSZ/tree-hash wire messages | Gossip, RPC, HTTP, proof engine, VC | Round trips, roots, accessors 213-354 |
| `ProofByRootIdentifier`, `ProofAttributes`, `ProofStatus` | same | 97-165 | Direct | RPC-002, ENGINE-004, VALIDATION-001 | Root requests, generation attributes, proof-node status | Sync, proof-node client, status handling | Type tests in same module |
| `NewPayloadRequest::to_owned` and Gloas conversions | `beacon_node/execution_layer/src/engine_api/new_payload_request.rs` | 53-235 | Direct | HASH-001, ENGINE-002/004 | Make payload request independently serializable/hashable | Payload registration and proof requests | Indirect engine/simulator tests |
| `ProofNodeClient` | `beacon_node/execution_layer/src/eip8025/proof_node_client.rs` | 28-47 | Direct | ENGINE-001/004, API-001 | Abstract request/verify/fetch/event proof-node API | `HttpProofEngine`, mock client | Mock and proof-engine suites |
| `HttpProofNodeClient` implementation | same | 67-196 | Direct | ENGINE-001/004, API-001 | HTTP+SSZ proof-node transport | `HttpProofEngine` | zkboost and mock tests |
| `HttpProofEngine` | `beacon_node/execution_layer/src/eip8025/proof_engine.rs` | 13-71 | Direct | ENGINE-001/004, VALIDATION-001 | Client facade for verify/get/request/SSE | `ExecutionLayer`, beacon chain, VC | `testing/proof_engine*` |
| `ProofEngineError` | `beacon_node/execution_layer/src/eip8025/errors.rs` | 5-92 | Direct | ENGINE-001/004 | Typed transport/status/SSZ errors | Proof client, beacon error conversion | Exercised by integration tests |
| `ExecutionLayer` proof-engine fields/methods | `beacon_node/execution_layer/src/lib.rs` | 461-1887 | Direct/mixed | ENGINE-001..004 | Configure and expose proof engine alongside EL | Beacon chain, VC/testing | Mock/rig/simulator |
| `ExecutionProofError` | `beacon_node/beacon_chain/src/eip8025/proof_verification.rs` | 20-79 | Direct | VALIDATION-002..005 | Reject classes for signatures/data/root/engine | Gossip/RPC verification path | Same file 178-466 |
| `compute_signing_root` | same | 84-91 | Direct | HASH-002 | Proof signing root | Validator store and verifier | Signature tests |
| `compute_execution_proof_domain` | same | 95-112 | Direct | DOMAIN-001 | Domain from fork version/genesis root | VC and verifier | Domain/signature tests |
| `verify_signed_execution_proof_signature` | same | 134-168 | Direct | VALIDATION-003/004 | Empty-data and BLS checks | Proof observation/import paths | Extensive positive/negative tests |
| `ExecutionProofStatusCache` | `beacon_node/beacon_chain/src/eip8025/proof_status.rs` | 52-181 | Direct | STORAGE-001, VALIDATION-005/006 | Map payload/block roots and cache valid proofs/status | `BeaconChain` EIP methods | Same file 621-682 |
| `register_execution_payload_request_root` | same | 224-233 | Direct | ENGINE-002, HASH-001 | Register payload/request context | Payload import/notifier hooks | Status cache tests |
| `observe_valid_execution_proof` | same | 249-302 | Direct | VALIDATION-005/006, STORAGE-001 | Record proof, count types, update status | Verification path | Status cache tests |
| `verify_and_observe_execution_proof` | same | 305-400 | Direct | VALIDATION-001..006 | Resolve context, validate, invoke engine, observe | Gossip/RPC/HTTP processing | Integration and simulator tests |
| `try_mark_proof_backed_payload_valid` | same | 462-485 | Direct | FORKCHOICE-001, VALIDATION-006 | Marks local payload status when quorum met | Proof observation | Phase 2 must test non-consensus boundary |
| `execution_proof(s)_by_*`, `missing_execution_proofs`, `latest_execution_proof_status` | same | 488-558 | Direct | STORAGE-001, RPC-001/002/003 | Serve/query/sync status from cache | RPC handlers and `ProofSync` | Status/sync tests |
| `ObservedExecutionProofs`, `ProofObservation` | `beacon_node/beacon_chain/src/observed_execution_proofs.rs` | 19-151 | Direct | VALIDATION-005, SECURITY-001 | Cheap dedup/attempt/invalid/valid tracking and pruning | Gossip validation | Tests 155-281 |
| `notify_new_payload` proof additions | `beacon_node/beacon_chain/src/execution_payload.rs` | 143-253 | Direct/mixed | ENGINE-002 | Register accepted payload with proof machinery | Existing payload import path | Simulator/engine tests |
| `PayloadNotifier` proof-status additions | `beacon_node/beacon_chain/src/payload_envelope_verification/payload_notifier.rs` | 69-128 | Direct/mixed | ENGINE-002, FORKCHOICE-001 | Feed request roots and proof-backed status | Gloas envelope notification | Simulator tests |
| `SubmitExecutionProofsRequest/Response/Status`, `post_beacon_pool_execution_proofs` | `beacon_node/http_api/src/beacon/pool.rs` | 50-164 | Direct | API-001, PROVER-001 | VC publishes completed signed proofs to BN | HTTP route/VC HTTP client | HTTP API integration tests |
| `EIP8025_ENR_KEY`, `Eth2Enr::execution_proof` | `beacon_node/lighthouse_network/src/discovery/enr.rs` | 32-113 | Direct | DISCOVERY-001 | Encode/read `eproof` capability | Discovery/service/peer manager | Existing ENR tests to inspect |
| `ExecutionProofsByRangeRequest`, `ExecutionProofsByRootRequest`, `ExecutionProofStatus` | `beacon_node/lighthouse_network/src/rpc/methods.rs` | 626-776 | Direct | RPC-001..004 | SSZ request/status representations | Protocol codecs, sync, handlers | Codec tests |
| `Protocol::{ExecutionProofsByRange, ExecutionProofsByRoot, ExecutionProofStatus}` | `beacon_node/lighthouse_network/src/rpc/protocol.rs` | 312-1125 | Direct | RPC-001..004 | Protocol IDs/versioning/request routing | RPC behavior | Protocol tests |
| proof RPC codec branches | `beacon_node/lighthouse_network/src/rpc/codec.rs` | 91-1468 | Direct/mixed | RPC-001..004 | SSZ-snappy lengths/encode/decode | RPC transport | Added range/status codec tests |
| proof RPC rate-limit fields/builders | `beacon_node/lighthouse_network/src/rpc/config.rs`; `rate_limiter.rs` | 95-332; 118-381 | Direct | RPC-001..004, SECURITY-001 | Per-protocol quotas | RPC service | Config parsing tests |
| execution-proof pubsub/topic variants | `beacon_node/lighthouse_network/src/types/pubsub.rs`; `topics.rs` | changed hunks | Direct | GOSSIP-001 | Wire topic/message classification | Network service/router | Pubsub tests to inspect |
| `process_gossip_execution_proof`, `process_rpc_execution_proof` | `beacon_node/network/src/network_beacon_processor/gossip_methods.rs` | 232-372 | Direct | GOSSIP-001, VALIDATION-002..005 | Gossip/RPC validation, acceptance and penalties | Router/network processor | Simulator/rig tests |
| `handle_execution_proofs_by_range_request` and helpers | `beacon_node/network/src/network_beacon_processor/rpc_methods.rs` | 1867-2014 | Direct | RPC-001, STORAGE-001 | Validate/serve canonical proof range | Router RPC dispatch | Proof engine/simulator tests |
| `handle_execution_proofs_by_root_request` and helpers | same | 2017-2144 | Direct | RPC-002/004, STORAGE-001 | Filter/serve root/type proofs | Router RPC dispatch | Proof engine/simulator tests |
| `ProofSync`, `ProofSyncState` | `beacon_node/network/src/sync/proof_sync.rs` | 29-339 | Direct | RPC-001..004, STORAGE-001 | Peer status handshake and by-root/range sync | Sync manager/context | Boundary test 410-452 plus simulator |
| `ProofService` | `validator_client/validator_services/src/proof_service.rs` | 37-385 | Direct | PROVER-001, API-001 | Event-driven request, completion, signing and publish service | VC startup | Proof-engine rig and zkboost suites |
| Validator-store proof signing additions | `validator_client/validator_store/src/lib.rs`; `lighthouse_validator_store/src/lib.rs` | changed hunks | Direct/mixed | HASH-002, PROVER-001 | Select validator/sign proof | `ProofService` | VC tests |
| `ProofEngineTestRig` | `testing/proof_engine/src/rig.rs` | 26-384 | Supporting | TEST-001 | Multi-node proof feature fixture | `testing/proof_engine/src/lib.rs` | Six async scenarios |
| `ZkboostTestHarness` | `testing/proof_engine_zkboost/src/zkboost_harness.rs` | 125-172 | Supporting | TEST-001, EL-001..005 | External proof-stack integration harness | zkboost test module | Seven async scenarios |

## Explicit incompleteness markers

**Verified Lighthouse facts:** `proof_verification.rs:7` says proof-engine end-to-end integration is TODO, `proof_verification.rs:115` says signature verification should migrate onto `BeaconChain`, and `eip8025/mod.rs:6` repeats an integration TODO. The rerun confirms the end-to-end comment is stale relative to the reachable call path; the comments alone are not evidence that integration is absent.

## Phase 2 rerun verified refinements

- `verify_and_observe_execution_proof` does call the configured proof engine after context, dedup, index/pubkey and BLS checks; the end-to-end TODO is stale. It does **not** check validator active status.
- `ExecutionProofStatusCache` is memory-only: bidirectional request/block LRUs and proof LRU are each bounded at 8,192; status metadata is a `HashMap`. No durable proof database representation was introduced.
- `ObservedExecutionProofs::prune` has no located branch-local caller, and no equivalent explicit status-cache finality prune was found.
- `try_mark_proof_backed_payload_valid` is reachable only when the non-default quorum config is enabled and K distinct proof types have been observed. It calls existing pre-Gloas/Gloas fork-choice payload-valid APIs.
- `ProofService` consumes ordinary block SSE, skips optimistic blocks, uses the first doppelganger-safe validator, tracks requests for five minutes, and is pre-Gloas-shaped despite BN-side Gloas request construction support.
- Proof-engine `notify_new_payload` and forkchoice-updated calls were not found; payload hooks register/derive request roots locally.
