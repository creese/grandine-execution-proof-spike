# Phase 1: EIP-8025 Specification and Lighthouse Scope

## 1. Executive scope conclusion

Phase 0 authorized proceeding with warnings and no blocker. EIPs, consensus-specs, and Lighthouse exactly match their Phase 0 commits and clean working-tree states.

The defensible Lighthouse comparison is `494b00a3491e2c5e281f6972aa00694b17f16722..0dd6c3b8cf3b1eece82a0a7ee87282a222d93bf5`: seven linear commits beginning with `Add optional execution proofs`, whose parent is the selected base. The range changes 109 files (18,923 insertions, 251 deletions): 41 Direct, 68 Supporting, 0 Unrelated. No rename exists. Every file is classified below and in `evidence/lighthouse-changed-files.txt`.

The sources are not synchronized. Important **Open specification questions** include the proof bound (EIP 400 KiB; consensus feature 4 MiB; Lighthouse 1,344 KiB), signature domain (EIP/Lighthouse `0x0D`; consensus feature `0x0F`), public input (EIP root/result/config; consensus feature/Lighthouse root only), progressive versus fixed bounded proof bytes, unresolved quorum `k`, and Gloas envelope versus BeaconBlock lifecycle wording. Lighthouse is therefore a hybrid, not evidence that either textual source is canonical.

## 2. Repository revisions and working-tree state

| Source | Branch | HEAD | Upstream | State | Drift from Phase 0 |
|---|---|---|---|---|---|
| EIPs | `feat/eip8025` | `3aa6b30641ef8f98f8945b29ee92c1e8aed1070a` | `origin/feat/eip8025` | Clean | None |
| consensus-specs | `master` | `8c12caee279d77b322446d33440b37479117dcde` | `origin/master` | Clean | None |
| Lighthouse | `optional-proofs-unstable` | `0dd6c3b8cf3b1eece82a0a7ee87282a222d93bf5` | `origin/optional-proofs-unstable` | Clean | None |

The Phase 1 control file itself was updated after Phase 0 to add the EIPs and consensus-specs drift checks recommended by Phase 0 (current SHA-256 `33e7d37f…` versus recorded `7a854479…`). This is an analysis-instruction change, not source-repository drift, and the current instruction was followed.

## 3. EIP requirement summary

**EIP statements:** EIP-8025 defines an opt-in, supplementary proof path that must not replace EL re-execution or affect fork choice/attestation. It introduces proof containers and signing, a proof-engine abstraction, gossip, range/root/status req/resp, ENR capability, prover lifecycle, and an EL stateless guest/host/witness/SSZ model. Its core invariants are payload-root binding, guest/result/config binding, active-validator attribution, bounded/deduplicated networking, canonical proof retention to finality, and no waiting on the attestation critical path.

Forty-two stable requirement IDs cover scope/roles, types/constants/hashing, engine operations, validation, transition/fork-choice/liveness, storage/gossip/RPC/discovery, prover/API lifecycle, EL guest/host/witness/encoding, activation, testing, and security. The canonical index with source lines is `evidence/spec-requirement-index.md`.

**Cross-client inference:** no beacon-state or block field, proposer/attester duty, slashing condition, or consensus fork-choice weighting changes. Disabled nodes should incur no proof networking or proof-node work. These are responsibilities for Phase 2 verification, not yet Lighthouse conclusions.

## 4. Consensus-spec implementation summary

**Consensus-spec requirements:** the five WIP feature documents are based on Gloas and define:

- root-only `PublicInput`, progressive proof bytes, signed proof container, 4 MiB maximum, four proof types per payload, and domain `0x0F`;
- implementation-dependent proof-engine verify/notify/forkchoice/request operations;
- active-validator, maximum-size, signature, and engine checks in `process_execution_proof`;
- Gloas execution-payload-envelope notification to the proof engine after EL acceptance;
- honest-prover SSE/request/fetch/sign/broadcast lifecycle;
- gossip validation/dedup; by-range, by-root and status protocols; canonical serving window; request bounds; and `eproof` ENR.

The feature has no local test files, no EIP-8025 fork epoch/version/upgrade, no concrete quorum, and no checked-out EL guest/host spec. It explicitly marks itself WIP and several execution values non-definitive.

## 5. EIP versus consensus-spec discrepancies

The full matrix is `evidence/spec-discrepancy-matrix.md`. The highest-impact differences are:

| Topic | EIP | Consensus feature | Significance |
|---|---|---|---|
| Proof maximum/type | 400 KiB fixed `ByteList` | 4 MiB `ProgressiveByteList` | Incompatible SSZ/roots, codec and bandwidth |
| Signature domain | `0x0D000000` | `0x0F000000` | Incompatible signatures |
| Public input | request root + success + chain config | request root only | Different roots and security binding |
| Quorum | `k` promised but unset | “sufficient” unset | Status/sync semantics not interoperable |
| Payload lifecycle | BeaconBlock/process-block prose | Gloas payload envelope/fork-choice hook | Different timing and construction surface |
| Serving boundary | checkpoint “slot” wording | start slot of finalized epoch | Boundary ambiguity |
| Tests | links feature directory as cases | no tests in directory | Missing local CL vectors |

The matrix also records gossip polarity/order, deduplication, empty-data layering, signed-size, dynamic proof-type registry, activation/base-fork, and unavailable pinned execution-specs questions.

## 6. Lighthouse Git baseline

Selected base: `494b00a3491e2c5e281f6972aa00694b17f16722`, exact parent of `ad61231736ecf955f0a7ef87174fb419a8685b39 Add optional execution proofs`.

Exact range: `494b00a3491e2c5e281f6972aa00694b17f16722..0dd6c3b8cf3b1eece82a0a7ee87282a222d93bf5`.

Branch commits:

1. `ad6123173` Add optional execution proofs
2. `d8a80b985` Fix optional proof CI failures
3. `d7e234419` Isolate optional proof CI caches
4. `d44086aa1` restore optional proof test coverage
5. `70396010d` Fix SSZ encoding of ExecutionProofsByRange requests
6. `c07e8c280` Fix CI compile, docs and zkboost dependency failures
7. `0dd6c3b8c` Minimal CI fixes after proof-sync revert

`origin/unstable` is a descendant 25 commits ahead; its merge base with HEAD is HEAD and cannot isolate this feature. `origin/optional-proofs`, `origin/feat/eip8025`, and `origin/optional-proofs-old` are divergent sibling histories. `origin/stable` is too old and includes hundreds of unrelated commits. Full graph evidence, counts and uncertainty are in `evidence/lighthouse-git-baseline.txt`.

## 7. Complete Lighthouse branch change inventory

| File | Change | Classification | Requirement IDs | Evidence/reason | Changed symbols or hunk context |
|---|---:|---|---|---|---|
| `.github/workflows/kurtosis-eip8025.yml` | A | Supporting | EIP8025-TEST-001 | CI coverage | workflow jobs |
| `.github/workflows/nightly-tests.yml` | M | Supporting | EIP8025-TEST-001 | CI coverage | env:; jobs: |
| `.github/workflows/test-suite.yml` | M | Supporting | EIP8025-TEST-001 | CI coverage | env:; jobs: |
| `.github/workflows/zkboost-tests.yml` | A | Supporting | EIP8025-TEST-001 | CI coverage | workflow jobs |
| `Cargo.lock` | M | Supporting | EIP8025-TEST-001, EIP8025-ENGINE-001 | build/dependency integration | dependencies = [ |
| `Cargo.toml` | M | Supporting | EIP8025-TEST-001, EIP8025-ENGINE-001 | build/dependency integration | members = [; ethereum_serde_utils = "0.8.0"; snap = "1"; quick-protobuf = { git = "https://github.com/sigp/quick-protobuf.git", rev = "87 |
| `Makefile` | M | Supporting | EIP8025-TEST-001, EIP8025-ENGINE-001 | build/dependency integration | test-release:; test-debug: |
| `beacon_node/beacon_chain/src/beacon_chain.rs` | M | Supporting | EIP8025-VALIDATION-002..006, EIP8025-STORAGE-001 | beacon-chain wiring and errors | use crate::early_attester_cache::EarlyAttesterCache;; use crate::observed_data_sidecars::ObservedDataSidecars;; pub struct BeaconChain<T: BeaconChainTypes> {; impl<T: BeaconChainTypes> BeaconChain<T> { |
| `beacon_node/beacon_chain/src/builder.rs` | M | Supporting | EIP8025-VALIDATION-002..006, EIP8025-STORAGE-001 | beacon-chain wiring and errors | use crate::{; where |
| `beacon_node/beacon_chain/src/chain_config.rs` | M | Supporting | EIP8025-VALIDATION-002..006, EIP8025-STORAGE-001 | beacon-chain wiring and errors | use std::{collections::HashSet, sync::LazyLock, time::Duration};; pub struct ChainConfig {; impl Default for ChainConfig { |
| `beacon_node/beacon_chain/src/eip8025/mod.rs` | A | Direct | EIP8025-VALIDATION-002..006, EIP8025-STORAGE-001, EIP8025-FORKCHOICE-001 | proof observation, verification, status, and storage | proof status/verification |
| `beacon_node/beacon_chain/src/eip8025/proof_status.rs` | A | Direct | EIP8025-VALIDATION-002..006, EIP8025-STORAGE-001, EIP8025-FORKCHOICE-001 | proof observation, verification, status, and storage | proof status/verification |
| `beacon_node/beacon_chain/src/eip8025/proof_verification.rs` | A | Direct | EIP8025-VALIDATION-002..006, EIP8025-STORAGE-001, EIP8025-FORKCHOICE-001 | proof observation, verification, status, and storage | proof status/verification |
| `beacon_node/beacon_chain/src/errors.rs` | M | Supporting | EIP8025-VALIDATION-002..006, EIP8025-STORAGE-001 | beacon-chain wiring and errors | use crate::data_availability_checker::AvailabilityCheckError;; pub enum BeaconChainError {; easy_from_to!(ForkChoiceStoreError, BeaconChainError); |
| `beacon_node/beacon_chain/src/execution_payload.rs` | M | Direct | EIP8025-ENGINE-002, EIP8025-FORKCHOICE-001 | payload notification and proof-status interaction | use execution_layer::{; use slot_clock::SlotClock;; pub async fn notify_new_payload<T: BeaconChainTypes>( |
| `beacon_node/beacon_chain/src/internal_events.rs` | A | Supporting | EIP8025-VALIDATION-002..006, EIP8025-STORAGE-001 | beacon-chain wiring and errors | BeaconChain wiring |
| `beacon_node/beacon_chain/src/lib.rs` | M | Supporting | EIP8025-VALIDATION-002..006, EIP8025-STORAGE-001 | beacon-chain wiring and errors | mod early_attester_cache;; pub mod historical_data_columns;; pub mod observed_data_sidecars; |
| `beacon_node/beacon_chain/src/observed_execution_proofs.rs` | A | Direct | EIP8025-VALIDATION-002..006, EIP8025-STORAGE-001, EIP8025-FORKCHOICE-001 | proof observation, verification, status, and storage | proof status/verification |
| `beacon_node/beacon_chain/src/payload_envelope_verification/import.rs` | M | Direct | EIP8025-ENGINE-002, EIP8025-FORKCHOICE-001 | payload notification and proof-status interaction | impl<T: BeaconChainTypes> BeaconChain<T> { |
| `beacon_node/beacon_chain/src/payload_envelope_verification/payload_notifier.rs` | M | Direct | EIP8025-ENGINE-002, EIP8025-FORKCHOICE-001 | payload notification and proof-status interaction | use fork_choice::PayloadVerificationStatus;; use tracing::warn;; impl<T: BeaconChainTypes> PayloadNotifier<T> { |
| `beacon_node/beacon_chain/src/test_utils.rs` | M | Supporting | EIP8025-TEST-001 | test support | where |
| `beacon_node/execution_layer/Cargo.toml` | M | Supporting | EIP8025-TEST-001, EIP8025-ENGINE-001 | build/dependency integration | arc-swap = "1.6.0"; ethereum_ssz = { workspace = true }; fork_choice = { workspace = true }; reqwest = { workspace = true } |
| `beacon_node/execution_layer/src/eip8025/errors.rs` | A | Direct | EIP8025-ENGINE-001..004, EIP8025-VALIDATION-001, EIP8025-API-001 | proof-engine client/API | proof engine/client/types/errors |
| `beacon_node/execution_layer/src/eip8025/mod.rs` | A | Direct | EIP8025-ENGINE-001..004, EIP8025-VALIDATION-001, EIP8025-API-001 | proof-engine client/API | proof engine/client/types/errors |
| `beacon_node/execution_layer/src/eip8025/proof_engine.rs` | A | Direct | EIP8025-ENGINE-001..004, EIP8025-VALIDATION-001, EIP8025-API-001 | proof-engine client/API | proof engine/client/types/errors |
| `beacon_node/execution_layer/src/eip8025/proof_node_client.rs` | A | Direct | EIP8025-ENGINE-001..004, EIP8025-VALIDATION-001, EIP8025-API-001 | proof-engine client/API | proof engine/client/types/errors |
| `beacon_node/execution_layer/src/eip8025/types.rs` | A | Direct | EIP8025-ENGINE-001..004, EIP8025-VALIDATION-001, EIP8025-API-001 | proof-engine client/API | proof engine/client/types/errors |
| `beacon_node/execution_layer/src/engine_api/new_payload_request.rs` | M | Direct | EIP8025-HASH-001, EIP8025-ENGINE-002 | owned/hashable payload request conversion | use crate::versioned_hashes::verify_versioned_hashes;; use superstruct::superstruct;; use types::{; pub struct NewPayloadRequest<'block, E: EthSpec> {; impl<'block, E: EthSpec> NewPayloadRequest<'block, E> { |
| `beacon_node/execution_layer/src/lib.rs` | M | Direct | EIP8025-ENGINE-001..004, EIP8025-ENGINE-002 | proof engine integration with execution layer | mod block_hash;; pub enum Error {; impl From<EngineError> for Error {; struct Inner<E: EthSpec> {; pub struct Config { |
| `beacon_node/execution_layer/src/test_utils/mock_event_stream.rs` | A | Supporting | EIP8025-TEST-001 | test support | mocks/builders |
| `beacon_node/execution_layer/src/test_utils/mock_proof_node_client.rs` | A | Supporting | EIP8025-TEST-001 | test support | mocks/builders |
| `beacon_node/execution_layer/src/test_utils/mod.rs` | M | Supporting | EIP8025-TEST-001 | test support | pub use mock_builder::{MockBuilder, Operation, mock_builder_extra_data};; pub use mock_execution_layer::MockExecutionLayer;; mod mock_builder;; mod mock_execution_layer; |
| `beacon_node/http_api/src/beacon/pool.rs` | M | Direct | EIP8025-API-001, EIP8025-PROVER-001 | proof publication/retrieval HTTP API | use operation_pool::ReceivedPreCapella;; use types::{; pub type BeaconPoolPathAnyFilter<T> = BoxedFilter<( |
| `beacon_node/http_api/src/lib.rs` | M | Direct | EIP8025-API-001, EIP8025-PROVER-001 | proof publication/retrieval HTTP API | pub fn prometheus_metrics() -> warp::filters::log::Log<impl Fn(warp::filters::lo; pub fn serve<T: BeaconChainTypes>( |
| `beacon_node/lighthouse_network/src/config.rs` | M | Supporting | EIP8025-GOSSIP-001, EIP8025-DISCOVERY-001, EIP8025-RPC-003 | network configuration/wiring | pub struct Config {; impl Default for Config { |
| `beacon_node/lighthouse_network/src/discovery/enr.rs` | M | Direct | EIP8025-DISCOVERY-001 | ENR proof-awareness | pub const PEERDAS_CUSTODY_GROUP_COUNT_ENR_KEY: &str = "cgc";; pub trait Eth2Enr {; impl Eth2Enr for Enr {; pub fn build_enr<E: EthSpec>(; fn compare_enr(local_enr: &Enr, disk_enr: &Enr) -> bool { |
| `beacon_node/lighthouse_network/src/peer_manager/mod.rs` | M | Supporting | EIP8025-GOSSIP-001, EIP8025-DISCOVERY-001, EIP8025-RPC-003 | network configuration/wiring | impl<E: EthSpec> PeerManager<E> { |
| `beacon_node/lighthouse_network/src/rpc/codec.rs` | M | Direct | EIP8025-RPC-001..004, EIP8025-SECURITY-001 | req/resp protocols, codecs, limits | use tokio_util::codec::{Decoder, Encoder};; use types::{; impl<E: EthSpec> SSZSnappyInboundCodec<E> {; impl<E: EthSpec> Encoder<RequestType<E>> for SSZSnappyOutboundCodec<E> {; fn handle_rpc_request<E: EthSpec>( |
| `beacon_node/lighthouse_network/src/rpc/config.rs` | M | Direct | EIP8025-RPC-001..004, EIP8025-SECURITY-001 | req/resp protocols, codecs, limits | pub struct RateLimiterConfig {; impl RateLimiterConfig {; impl Default for RateLimiterConfig {; impl Debug for RateLimiterConfig {; impl FromStr for RateLimiterConfig { |
| `beacon_node/lighthouse_network/src/rpc/methods.rs` | M | Direct | EIP8025-RPC-001..004, EIP8025-SECURITY-001 | req/resp protocols, codecs, limits | use superstruct::superstruct;; use types::data::BlobIdentifier;; impl LightClientUpdatesByRangeRequest {; pub enum RpcSuccessResponse<E: EthSpec> {; pub enum ResponseTermination { |
| `beacon_node/lighthouse_network/src/rpc/mod.rs` | M | Direct | EIP8025-RPC-001..004, EIP8025-SECURITY-001 | req/resp protocols, codecs, limits | pub struct RPC<Id: ReqId, E: EthSpec> {; impl<Id: ReqId, E: EthSpec> RPC<Id, E> {; where |
| `beacon_node/lighthouse_network/src/rpc/protocol.rs` | M | Direct | EIP8025-RPC-001..004, EIP8025-SECURITY-001 | req/resp protocols, codecs, limits | use tokio_util::{; pub static LIGHT_CLIENT_UPDATES_BY_RANGE_ELECTRA_MAX: LazyLock<usize> =; pub enum Protocol {; impl Protocol {; pub enum SupportedProtocol { |
| `beacon_node/lighthouse_network/src/rpc/rate_limiter.rs` | M | Direct | EIP8025-RPC-001..004, EIP8025-SECURITY-001 | req/resp protocols, codecs, limits | pub struct RPCRateLimiter {; pub struct RPCRateLimiterBuilder {; impl RPCRateLimiterBuilder {; impl RPCRateLimiter { |
| `beacon_node/lighthouse_network/src/service/api_types.rs` | M | Supporting | EIP8025-GOSSIP-001, EIP8025-DISCOVERY-001, EIP8025-RPC-003 | network configuration/wiring | use types::{; pub enum SyncRequestId {; pub struct DataColumnsByRangeRequestId {; pub enum Response<E: EthSpec> {; impl<E: EthSpec> std::convert::From<Response<E>> for RpcResponse<E> { |
| `beacon_node/lighthouse_network/src/service/gossip_cache.rs` | M | Direct | EIP8025-GOSSIP-001, EIP8025-VALIDATION-005 | gossip topic/message/cache | impl GossipCache { |
| `beacon_node/lighthouse_network/src/service/mod.rs` | M | Supporting | EIP8025-GOSSIP-001, EIP8025-DISCOVERY-001, EIP8025-RPC-003 | network configuration/wiring | impl<E: EthSpec> Network<E> { |
| `beacon_node/lighthouse_network/src/service/utils.rs` | M | Supporting | EIP8025-GOSSIP-001, EIP8025-DISCOVERY-001, EIP8025-RPC-003 | network configuration/wiring | pub(crate) fn create_whitelist_filter( |
| `beacon_node/lighthouse_network/src/types/globals.rs` | M | Supporting | EIP8025-GOSSIP-001, EIP8025-DISCOVERY-001, EIP8025-RPC-003 | network configuration/wiring | impl<E: EthSpec> NetworkGlobals<E> { |
| `beacon_node/lighthouse_network/src/types/pubsub.rs` | M | Direct | EIP8025-GOSSIP-001, EIP8025-VALIDATION-005 | gossip topic/message/cache | use types::{; pub enum PubsubMessage<E: EthSpec> {; impl<E: EthSpec> PubsubMessage<E> {; impl<E: EthSpec> std::fmt::Display for PubsubMessage<E> { |
| `beacon_node/lighthouse_network/src/types/topics.rs` | M | Direct | EIP8025-GOSSIP-001, EIP8025-VALIDATION-005 | gossip topic/message/cache | pub const PROPOSER_PREFERENCES: &str = "proposer_preferences";; pub struct TopicConfig {; pub fn core_topics_to_subscribe<E: EthSpec>(; pub fn is_fork_non_core_topic(topic: &GossipTopic, _fork_name: ForkName) -> bool; pub fn all_topics_at_fork<E: EthSpec>(fork: ForkName, spec: &ChainSpec) -> Vec<G |
| `beacon_node/network/src/network_beacon_processor/gossip_methods.rs` | M | Direct | EIP8025-GOSSIP-001, EIP8025-RPC-001..003, EIP8025-VALIDATION-002..005 | gossip/RPC processing and routing | use beacon_chain::data_column_verification::{; use beacon_chain::{; use types::{; fn clone_message_acceptance(a: &MessageAcceptance) -> MessageAcceptance {; impl<T: BeaconChainTypes> NetworkBeaconProcessor<T> { |
| `beacon_node/network/src/network_beacon_processor/mod.rs` | M | Direct | EIP8025-GOSSIP-001, EIP8025-RPC-001..003, EIP8025-VALIDATION-002..005 | gossip/RPC processing and routing | use lighthouse_network::rpc::methods::{; impl<T: BeaconChainTypes> NetworkBeaconProcessor<T> { |
| `beacon_node/network/src/network_beacon_processor/rpc_methods.rs` | M | Direct | EIP8025-GOSSIP-001, EIP8025-RPC-001..003, EIP8025-VALIDATION-002..005 | gossip/RPC processing and routing | use lighthouse_network::rpc::methods::{; use types::data::BlobIdentifier;; impl<T: BeaconChainTypes> NetworkBeaconProcessor<T> { |
| `beacon_node/network/src/router.rs` | M | Direct | EIP8025-GOSSIP-001, EIP8025-RPC-001..003, EIP8025-VALIDATION-002..005 | gossip/RPC processing and routing | use futures::prelude::*;; use types::{; impl<T: BeaconChainTypes> Router<T> { |
| `beacon_node/network/src/sync/backfill_sync/mod.rs` | M | Supporting | EIP8025-RPC-001..004 | sync integration | impl<E: EthSpec> BatchConfig for BackFillBatchConfig<E> {; pub struct BackFillSync<T: BeaconChainTypes> {; impl<T: BeaconChainTypes> BackFillSync<T> { |
| `beacon_node/network/src/sync/custody_backfill_sync/mod.rs` | M | Supporting | EIP8025-RPC-001..004 | sync integration | pub struct CustodyBackFillSync<T: BeaconChainTypes> { |
| `beacon_node/network/src/sync/manager.rs` | M | Supporting | EIP8025-RPC-001..004 | sync integration | use super::peer_sync_info::{PeerSyncType, remote_sync_type};; use lighthouse_network::rpc::RPCError;; use lighthouse_network::service::api_types::{; use types::{; pub enum SyncMessage<E: EthSpec> { |
| `beacon_node/network/src/sync/mod.rs` | M | Supporting | EIP8025-RPC-001..004 | sync integration | mod peer_sync_info; |
| `beacon_node/network/src/sync/network_context.rs` | M | Supporting | EIP8025-RPC-001..004 | sync integration | use beacon_chain::block_verification_types::{AsBlock, RangeSyncBlock};; use custody::CustodyRequestResult;; use fnv::FnvHashMap;; use lighthouse_network::service::api_types::{; use slot_clock::SlotClock; |
| `beacon_node/network/src/sync/proof_sync.rs` | A | Direct | EIP8025-RPC-001..004, EIP8025-STORAGE-001 | proof synchronization state machine | ProofSync |
| `beacon_node/src/cli.rs` | M | Supporting | EIP8025-ACTIVATION-001, EIP8025-ENGINE-001 | beacon-node runtime configuration | pub fn cli_app() -> Command { |
| `beacon_node/src/config.rs` | M | Supporting | EIP8025-ACTIVATION-001, EIP8025-ENGINE-001 | beacon-node runtime configuration | pub fn get_config<E: EthSpec>( |
| `book/src/help_bn.md` | M | Supporting | EIP8025-API-001 | operator documentation | Options: |
| `book/src/help_vc.md` | M | Supporting | EIP8025-API-001 | operator documentation | Options: |
| `common/eth2/src/lib.rs` | M | Supporting | EIP8025-API-001 | HTTP client endpoint support | use types::{; impl BeaconNodeHttpClient { |
| `consensus/types/src/core/chain_spec.rs` | M | Supporting | EIP8025-DOMAIN-001, EIP8025-ACTIVATION-001 | type exports and chain configuration | pub enum Domain {; pub struct ChainSpec {; impl ChainSpec {; mod tests { |
| `consensus/types/src/core/config_and_preset.rs` | M | Supporting | EIP8025-DOMAIN-001, EIP8025-ACTIVATION-001 | type exports and chain configuration | pub fn get_extra_fields(spec: &ChainSpec) -> HashMap<String, Value> { |
| `consensus/types/src/execution/eip8025.rs` | A | Direct | EIP8025-TYPE-001..004, EIP8025-CONSTANT-001..002, EIP8025-DOMAIN-001, EIP8025-RPC-002 | protocol types, bounds, domains, and SSZ/tree hash | EIP-8025 types |
| `consensus/types/src/execution/mod.rs` | M | Supporting | EIP8025-DOMAIN-001, EIP8025-ACTIVATION-001 | type exports and chain configuration | pub use bls_to_execution_change::BlsToExecutionChange; |
| `lighthouse/environment/Cargo.toml` | M | Supporting | EIP8025-TEST-001, EIP8025-ENGINE-001 | build/dependency integration | edition = { workspace = true } |
| `lighthouse/environment/src/lib.rs` | M | Supporting | EIP8025-TEST-001, EIP8025-ENGINE-001 | runtime/test environment wiring | use {futures::channel::oneshot, std::cell::RefCell};; impl<E: EthSpec> EnvironmentBuilder<E> { |
| `lighthouse/environment/src/test_utils.rs` | A | Supporting | EIP8025-TEST-001 | test support | mocks/builders |
| `scripts/local_testnet/network_params_eip8025.yaml` | A | Supporting | EIP8025-TEST-001, EIP8025-ACTIVATION-001 | local-network configuration/launch | network parameters |
| `scripts/local_testnet/network_params_eip8025_zkboost.yaml` | A | Supporting | EIP8025-TEST-001, EIP8025-ACTIVATION-001 | local-network configuration/launch | network parameters |
| `scripts/local_testnet/network_params_eip8025_zkboost_gpu.yaml` | A | Supporting | EIP8025-TEST-001, EIP8025-ACTIVATION-001 | local-network configuration/launch | network parameters |
| `scripts/local_testnet/start_eip8025_testnet.sh` | A | Supporting | EIP8025-TEST-001, EIP8025-ACTIVATION-001 | local-network configuration/launch | network parameters |
| `testing/proof_engine/Cargo.toml` | A | Supporting | EIP8025-TEST-001, EIP8025-ENGINE-001 | build/dependency integration | workspace/dependencies/targets |
| `testing/proof_engine/src/lib.rs` | A | Supporting | EIP8025-TEST-001 | test harness, fixtures, and simulation | tests/fixtures/harness |
| `testing/proof_engine/src/rig.rs` | A | Supporting | EIP8025-TEST-001 | test harness, fixtures, and simulation | tests/fixtures/harness |
| `testing/proof_engine_zkboost/Cargo.lock` | A | Supporting | EIP8025-TEST-001, EIP8025-ENGINE-001 | build/dependency integration | workspace/dependencies/targets |
| `testing/proof_engine_zkboost/Cargo.toml` | A | Supporting | EIP8025-TEST-001, EIP8025-ENGINE-001 | build/dependency integration | workspace/dependencies/targets |
| `testing/proof_engine_zkboost/src/lib.rs` | A | Supporting | EIP8025-TEST-001 | test harness, fixtures, and simulation | tests/fixtures/harness |
| `testing/proof_engine_zkboost/src/zkboost_harness.rs` | A | Supporting | EIP8025-TEST-001 | test harness, fixtures, and simulation | tests/fixtures/harness |
| `testing/proof_engine_zkboost/tests/fixture/chain_config.json` | A | Supporting | EIP8025-TEST-001 | test harness, fixtures, and simulation | tests/fixtures/harness |
| `testing/proof_engine_zkboost/tests/fixture/execution_witness.json` | A | Supporting | EIP8025-TEST-001 | test harness, fixtures, and simulation | tests/fixtures/harness |
| `testing/proof_engine_zkboost/tests/fixture/new_payload_request.ssz` | A | Supporting | EIP8025-TEST-001 | test harness, fixtures, and simulation | tests/fixtures/harness |
| `testing/simulator/Cargo.toml` | M | Supporting | EIP8025-TEST-001, EIP8025-ENGINE-001 | build/dependency integration | edition = { workspace = true }; clap = { workspace = true }; kzg = { workspace = true }; logging = { workspace = true }; serde_json = { workspace = true } |
| `testing/simulator/src/basic_sim.rs` | M | Supporting | EIP8025-TEST-001 | test harness, fixtures, and simulation | use crate::local_network::LocalNetworkParams;; pub fn run_basic_sim(matches: &ArgMatches) -> Result<(), String> { |
| `testing/simulator/src/fallback_sim.rs` | M | Supporting | EIP8025-TEST-001 | test harness, fixtures, and simulation | pub fn run_fallback_sim(matches: &ArgMatches) -> Result<(), String> { |
| `testing/simulator/src/lib.rs` | A | Supporting | EIP8025-TEST-001 | test harness, fixtures, and simulation | tests/fixtures/harness |
| `testing/simulator/src/local_network.rs` | M | Supporting | EIP8025-TEST-001 | test harness, fixtures, and simulation | use kzg::trusted_setup::get_trusted_setup;; use node_test_rig::{; use std::{; use types::{ChainSpec, Epoch, EthSpec};; const QUIC_PORT: u16 = 43424; |
| `testing/simulator/src/test_utils/builder.rs` | A | Supporting | EIP8025-TEST-001 | test harness, fixtures, and simulation | tests/fixtures/harness |
| `testing/simulator/src/test_utils/event_stream.rs` | A | Supporting | EIP8025-TEST-001 | test harness, fixtures, and simulation | tests/fixtures/harness |
| `testing/simulator/src/test_utils/mod.rs` | A | Supporting | EIP8025-TEST-001 | test harness, fixtures, and simulation | tests/fixtures/harness |
| `validator_client/Cargo.toml` | M | Supporting | EIP8025-TEST-001, EIP8025-ENGINE-001 | build/dependency integration | eth2 = { workspace = true }; tracing = { workspace = true } |
| `validator_client/http_api/src/lib.rs` | M | Direct | EIP8025-PROVER-001, EIP8025-HASH-002, EIP8025-API-001 | validator proof production/signing lifecycle | use validator_services::block_service::BlockService;; impl From<String> for Error {; pub struct Context<T: SlotClock, E> { |
| `validator_client/http_api/src/test_utils.rs` | M | Supporting | EIP8025-TEST-001 | test support | impl ApiTester { |
| `validator_client/http_api/src/tests.rs` | M | Direct | EIP8025-PROVER-001, EIP8025-HASH-002, EIP8025-API-001 | validator proof production/signing lifecycle | impl ApiTester { |
| `validator_client/lighthouse_validator_store/src/lib.rs` | M | Direct | EIP8025-PROVER-001, EIP8025-HASH-002, EIP8025-API-001 | validator proof production/signing lifecycle | use account_utils::validator_definitions::{PasswordStorage, ValidatorDefinition}; use types::{; impl<T: SlotClock + 'static, E: EthSpec> ValidatorStore for LighthouseValidatorS |
| `validator_client/signing_method/src/lib.rs` | M | Direct | EIP8025-PROVER-001, EIP8025-HASH-002, EIP8025-API-001 | validator proof production/signing lifecycle | pub enum SignableMessage<'a, E: EthSpec, Payload: AbstractExecPayload<E> = FullP; impl<E: EthSpec, Payload: AbstractExecPayload<E>> SignableMessage<'_, E, Payload; impl SigningMethod { |
| `validator_client/signing_method/src/web3signer.rs` | M | Direct | EIP8025-PROVER-001, EIP8025-HASH-002, EIP8025-API-001 | validator proof production/signing lifecycle | pub enum MessageType {; pub enum Web3SignerObject<'a, E: EthSpec, Payload: AbstractExecPayload<E>> {; impl<'a, E: EthSpec, Payload: AbstractExecPayload<E>> Web3SignerObject<'a, E, Pa |
| `validator_client/src/cli.rs` | M | Supporting | EIP8025-PROVER-001, EIP8025-ACTIVATION-001 | prover configuration | pub struct ValidatorClient { |
| `validator_client/src/config.rs` | M | Supporting | EIP8025-PROVER-001, EIP8025-ACTIVATION-001 | prover configuration | use eth2::types::{Graffiti, GraffitiPolicy};; use tracing::{info, warn};; use types::GRAFFITI_BYTES_LEN;; pub struct Config {; impl Default for Config { |
| `validator_client/src/lib.rs` | M | Direct | EIP8025-PROVER-001, EIP8025-HASH-002, EIP8025-API-001 | validator proof production/signing lifecycle | use validator_services::{; pub struct ProductionValidatorClient<E: EthSpec> {; impl<E: EthSpec> ProductionValidatorClient<E> { |
| `validator_client/validator_metrics/src/lib.rs` | M | Supporting | EIP8025-PROVER-001 | prover observability | pub static SIGNED_VALIDATOR_REGISTRATIONS_TOTAL: LazyLock<Result<IntCounterVec>> |
| `validator_client/validator_services/Cargo.toml` | M | Supporting | EIP8025-TEST-001, EIP8025-ENGINE-001 | build/dependency integration | either = { workspace = true } |
| `validator_client/validator_services/src/lib.rs` | M | Direct | EIP8025-PROVER-001, EIP8025-HASH-002, EIP8025-API-001 | validator proof production/signing lifecycle | pub mod preparation_service; |
| `validator_client/validator_services/src/proof_service.rs` | A | Direct | EIP8025-PROVER-001, EIP8025-HASH-002, EIP8025-API-001 | validator proof production/signing lifecycle | validator service/store |
| `validator_client/validator_store/src/lib.rs` | M | Direct | EIP8025-PROVER-001, EIP8025-HASH-002, EIP8025-API-001 | validator proof production/signing lifecycle | use types::{; pub trait ValidatorStore: Send + Sync { |

## 8. In-scope Lighthouse files and hunks

All 109 files are in scope: Direct changes implement externally visible/types/rules/lifecycle; Supporting changes wire, configure, document, test, simulate, or resolve dependencies for them. For mixed files, only the exact diff hunks listed in the inventory are branch-introduced evidence; surrounding unchanged code is contextual.

`evidence/lighthouse-relevant-diff.patch` contains the exact diff for all 41 Direct files. Supporting changes are retained in the complete inventory and exact Git range but omitted from the focused patch for practicality, especially generated locks and bulky test fixtures. `evidence/lighthouse-symbol-index.md` indexes important definitions, lines, expected callers, and tests.

Preliminary **Verified Lighthouse facts** from direct branch code:

- protocol types are fixed bounded SSZ/tree-hash types with root-only public input, domain `0x0D`, four proof types, 1,344 KiB proof maximum, and minimum quorum constant 1;
- HTTP proof-node client/engine supports asynchronous request, verification, retrieval, and proof-event SSE;
- beacon-chain caches map request roots/block roots, observe proofs, expose range/root/status queries, and include proof-backed payload-status logic;
- network code adds gossip, ENR, three req/resp protocols, serving handlers, rate limiting, and proof sync;
- validator client adds an event-driven proof service and signing/publication integration;
- tests include unit, HTTP/mock engine, simulator, local testnet, and zkboost integration infrastructure;
- TODO comments claim incomplete proof-engine integration/migration, but call sites exist; Phase 2 must resolve whether those comments are stale.

## 9. Excluded Lighthouse changes

No branch-local file is classified Unrelated, so none is excluded from the branch inventory. The focused patch excludes Supporting file bodies only as an evidence-size decision, not as a scope exclusion.

Unchanged Lighthouse infrastructure may be inspected in Phase 2 for callers, ownership and lifecycle, but must be labeled contextual rather than branch-introduced. Divergent sibling branches are not part of the selected implementation range.

## 10. Preliminary functional areas

| Area | Direct branch surface | Supporting surface | Phase 2 focus |
|---|---|---|---|
| Types/SSZ/hashing/domain | `types/execution/eip8025.rs`, owned payload request | exports/chain spec | exact roots, 1,344 KiB rationale, compatibility |
| Proof engine/API | execution-layer `eip8025/*`, EL integration | deps/mocks | transport, status semantics, timeouts/errors |
| Payload/fork-choice status | proof status/cache, payload notifier/import | chain wiring | supplementary vs load-bearing boundary |
| Validation/dedup | proof verification, observed proofs | errors/tests | order, active validator, root binding, retries |
| Gossip/discovery | pubsub/topic, processor, ENR | service/config | gating, scoring, disabled behavior |
| Req/resp/sync/storage | RPC methods/codecs/handlers, `ProofSync` | manager/context | ranges, bounds, canonicality, pruning |
| Prover/validator | `ProofService`, signing/store, publish API | CLI/metrics/docs | SSE lifecycle, validator selection, cleanup |
| Testing/operations | direct test hooks | workflows, rigs, simulator, zkboost, scripts | actual coverage and failures |

## 11. Investigation plan for Phase 2

1. Map every requirement ID to implemented/partial/different/not-found/not-applicable/ambiguous.
2. Trace payload acceptance → request-root registration → proof generation or receipt → gossip checks → proof-node verification → observation/quorum → local payload status → storage/sync/serving/cleanup.
3. Trace rejection order, cache mutation and peer outcomes for unknown roots, duplicates, bad indices/signatures, empty/oversized/invalid/unsupported proofs.
4. Derive exact SSZ encodings, roots, domain fork version, maximum request/response calculations, and status wire types.
5. Determine runtime gating and behavior with no proof endpoint/types/quorum; verify no fork epoch/state upgrade or attestation wait.
6. Resolve the apparent tension between proof-backed payload-valid marking and the EIP’s supplementary-only claim.
7. Inventory persistence versus memory-only caches, finality pruning, canonical filtering, API endpoints, metrics and operator configuration.
8. Run focused tests/checks where practical and distinguish existing coverage from missing vectors.
9. Explain all Supporting changes and mark unchanged infrastructure as context.

## 12. Open questions

- Which of 400 KiB, 1,344 KiB, or 4 MiB is intended, and fixed versus progressive SSZ?
- Is `0x0D` or `0x0F` the assigned signature domain?
- Must public input contain success/config, or does the proof system bind these outside the CL container?
- What is `k`, and is it globally normative or local/operator-configurable?
- Can a verified proof change local payload validity/fork choice in this optional phase, or must EL re-execution remain the sole load-bearing result?
- Is EIP-8025 deployable without a fork only after Gloas, and how should pre-Gloas types behave?
- Which Block/execution-payload SSE event and object shape is normative?
- How are dynamic proof-type numbers allocated and bound to guest/verifier programs?
- Are proof caches persistent enough to satisfy finalized-range serving across restart?
- Where are canonical CL vectors and the pinned execution-specs checkout needed to validate EL semantics?
- Why do Lighthouse TODO comments describe integration as pending despite branch-local engine/verification call sites?

