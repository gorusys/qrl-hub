# Developer Guide

## Architecture pattern

1. **Run or lease a synced `qrl` node** — best practice per [API overview](https://docs.theqrl.org/api/qrl-api-overview).
2. **Query state** via gRPC (`GetHeight`, `GetAddressState`, `GetOTS`, …).
3. **Construct** unsigned transactions using helper RPCs (`Get*Txn` methods) or local builders.
4. **Sign** with XMSS using `qrllib`, `qrl-cli`, or wallet daemons — enforce **single-writer** OTS discipline.
5. **Broadcast** with `PushTransaction`.
6. **Confirm** via `GetTransaction` / block queries.

## Canonical schema

All message types are defined in **`qrl.proto`**:

- [github.com/theQRL/QRL/blob/master/src/qrl/protos/qrl.proto](https://github.com/theQRL/QRL/blob/master/src/qrl/protos/qrl.proto)

Regenerate language stubs when upgrading node versions.

## Language tooling

| Need | Resource |
|------|----------|
| Core cryptography / XMSS | [qrllib](https://docs.theqrl.org/build/qrllib) — Python `pyqrllib`, JS WASM |
| Node gRPC from Node.js | [@theqrl/node-helpers](https://docs.theqrl.org/build/helpers/node-helpers) |
| CLI automation | [`qrl-cli`](https://docs.theqrl.org/build/qrl-cli/overview/) |
| PoW hashing helpers | [pyqrandomx](https://docs.theqrl.org/build/mining/qrandomx) |

## gRPC transport

- Default local endpoint: **`127.0.0.1:19009`**
- TLS is deployment-specific; many examples use `grpc.insecure_channel`.
- **No authentication** on most public methods — treat network placement as your security boundary ([API overview](https://docs.theqrl.org/api/qrl-api-overview)).

## Transaction families

Beyond `Transfer`:

- **Message** / notarization ([feature-messages.md](feature-messages.md))
- **Token** create/transfer ([feature-tokens.md](feature-tokens.md))
- **Multisig** create/spend/vote ([feature-multisig.md](feature-multisig.md))
- **Slave** relay ([feature-slave-keys.md](feature-slave-keys.md))
- **Lattice** publication ([feature-lattice-keys.md](feature-lattice-keys.md))

Each consumes OTS from the appropriate signer address.

## Offline / cold signing

`qrl-cli send` supports saving signed transactions to disk (`-T`) and broadcasting later (`-F`) ([CLI](https://docs.theqrl.org/build/qrl-cli/overview/)). Mirror the same pattern in custom signers: **never reuse OTS** when retrying after failures — always reconcile chain state first.

## Testing strategy

- Integration tests against **testnet** (`start_qrl --network-type testnet`).
- Property tests for OTS monotonicity in your signer service.
- Contract tests for protobuf compatibility per node release.

## Support channels

Community help is linked throughout official docs (e.g. [Discord](https://theqrl.org/discord) from mining documentation).
