# API Reference

QRL exposes blockchain functionality primarily through **gRPC** using **Protocol Buffers**. Official overview: [QRL API Overview](https://docs.theqrl.org/api/qrl-api-overview). Detailed method tables: [QRL Public API](https://docs.theqrl.org/api/qrl-public-api).

## Source of truth

- **Protobuf**: [`src/qrl/protos/qrl.proto`](https://github.com/theQRL/QRL/blob/master/src/qrl/protos/qrl.proto)
- **Python example** (from public API docs): connect to `127.0.0.1:19009`, instantiate `QRLServiceStub`, call `GetBlockInfo`, etc.

## Transport & security

- Default **public API** port: **19009** ([node config](https://docs.theqrl.org/use/node/config/)).
- **Authentication** is generally **not** required ([API overview](https://docs.theqrl.org/api/qrl-api-overview)).
- **Do not expose** admin/mining/debug/wallet APIs to untrusted networks.

## Service: `QRL` (public)

Method inventory from [QRL Public API](https://docs.theqrl.org/api/qrl-public-api) (verify names against your `qrl.proto` revision):

### Node & network

| Method | Request | Response |
|--------|---------|----------|
| `GetNodeState` | `GetNodeStateReq` | `GetNodeStateResp` |
| `GetKnownPeers` | `GetKnownPeersReq` | `GetKnownPeersResp` |
| `GetPeersStat` | `GetPeersStatReq` | `GetPeersStatResp` |
| `GetStats` | `GetStatsReq` | `GetStatsResp` |
| `GetChainStats` | `GetChainStatsReq` | `GetChainStatsResp` |
| `GetHeight` | `GetHeightReq` | `GetHeightResp` |

### Address, balance, OTS

| Method | Request | Response |
|--------|---------|----------|
| `GetAddressState` | `GetAddressStateReq` | `GetAddressStateResp` |
| `GetOptimizedAddressState` | `GetAddressStateReq` | `GetOptimizedAddressStateResp` |
| `GetMultiSigAddressState` | `GetMultiSigAddressStateReq` | `GetMultiSigAddressStateResp` |
| `GetBalance` | `GetBalanceReq` | `GetBalanceResp` |
| `GetTotalBalance` | `GetTotalBalanceReq` | `GetTotalBalanceResp` |
| `GetOTS` | `GetOTSReq` | `GetOTSResp` |
| `GetAddressFromPK` | `GetAddressFromPKReq` | `GetAddressFromPKResp` |

### Objects, blocks, chain tips

| Method | Request | Response |
|--------|---------|----------|
| `GetObject` | `GetObjectReq` | `GetObjectResp` |
| `GetLatestData` | `GetLatestDataReq` | `GetLatestDataResp` |
| `GetBlock` | `GetBlockReq` | `GetBlockResp` |
| `GetBlockByNumber` | `GetBlockByNumberReq` | `GetBlockByNumberResp` |

### Transactions

| Method | Request | Response |
|--------|---------|----------|
| `GetTransaction` | `GetTransactionReq` | `GetTransactionResp` |
| `PushTransaction` | `PushTransactionReq` | `PushTransactionResp` |
| `GetMiniTransactionsByAddress` | `GetMiniTransactionsByAddressReq` | `GetMiniTransactionsByAddressResp` |
| `GetTransactionsByAddress` | `GetTransactionsByAddressReq` | `GetTransactionsByAddressResp` |

### Address-scoped collections

| Method | Notes |
|--------|--------|
| `GetTokensByAddress` | Token balances |
| `GetSlavesByAddress` | Registered slave addresses |
| `GetLatticePKsByAddress` | Lattice public keys |
| `GetMultiSigAddressesByAddress` | Multisig associations |
| `GetMultiSigSpendTxsByAddress` | Pending / related multisig spends |
| `GetInboxMessagesByAddress` | Message tx inbox helpers |

### Voting / governance helpers

| Method | Request | Response |
|--------|---------|----------|
| `GetVoteStats` | `GetVoteStatsReq` | `GetVoteStatsResp` |

### Transaction builders (`Get*Txn`)

These RPCs assemble **unsigned** transaction payloads for local signing:

| Method | Purpose |
|--------|---------|
| `GetMultiSigCreateTxn` | Create multisig |
| `GetMultiSigSpendTxn` | Propose multisig spend |
| `GetMultiSigVoteTxn` | Vote on multisig spend |
| `GetMessageTxn` | Message / encoded payload |
| `GetTokenTxn` | Token creation |
| `GetTransferTokenTxn` | Token transfer |
| `GetSlaveTxn` | Slave / relay operations |
| `GetLatticeTxn` | Lattice key publication |

(Additional transfer helpers may exist in your protobuf — always read `qrl.proto`.)

## Wallet API & proxies

- **Wallet API** (gRPC, default port **19010** in example configs): [wallet-api](https://docs.theqrl.org/api/wallet-api) — includes `RelaySlaveTxn`, `RelayTokenTxn`, `RelayTransferTokenTxn`, etc.
- **walletd-rest-proxy**: HTTP wrapper documented at [walletd-rest-proxy](https://docs.theqrl.org/api/walletd-rest-proxy) (automatic slave transaction support).
- **Zeus proxy**: [Zeus Proxy API](https://docs.theqrl.org/api/zeus-proxy/) for simplified HTTP access in some deployments.
- **Explorer API**: linked from [API overview](https://docs.theqrl.org/api/qrl-api-overview).

## Minimal Python sketch

```python
import grpc
from qrl.protos import qrl_pb2, qrl_pb2_grpc

channel = grpc.insecure_channel("127.0.0.1:19009")
stub = qrl_pb2_grpc.QRLServiceStub(channel)

state = stub.GetNodeState(qrl_pb2.GetNodeStateReq())
addr = stub.GetAddressState(qrl_pb2.GetAddressStateReq(address=b"..."))
```

Replace `address=b"..."` with the byte encoding expected by your client (often raw 39-byte addresses, not the `Q` string).

## Operational checklist

- [ ] Pin **node version** == **client stubs** version.
- [ ] Monitor RPC latency, error rate, and peer sync on the backing node.
- [ ] Run **TLS** or VPN if traffic crosses untrusted networks.
- [ ] Separate **read-only** query endpoints from **signing** infrastructure.
