# Messages & Notarization

QRL can store short **arbitrary payloads** on-chain via **MessageTransaction**, up to **80 bytes** per transaction ([message encoding](https://docs.theqrl.org/build/messages/message-tx-encoding)).

## QIP002 — encoded message prefix

[QIP002](https://github.com/theQRL/qips/blob/master/qips/QIP002.md) defines a prefix so clients can recognize structured payloads inside message transactions.

Hex layout (message field, conceptual):

| Bytes | Meaning |
|-------|---------|
| 2 | Magic `0F0F` marks encoded data |
| 2 | Message **type** id (allocated per QIP) |
| 76 | Payload |

Total usable: **80 bytes** in the message transaction.

## Registered type ids (examples)

From [message encoding docs](https://docs.theqrl.org/build/messages/message-tx-encoding):

| Hex type | Purpose |
|----------|---------|
| `0000` | Reserved |
| `0001` | Reserved |
| `0002` | Keybase |
| `0003` | GitHub |
| `0004` | On-chain voting |

New types should be proposed via the QIP process ([Governance](governance-qips.md)).

## Document notarization

Notarization stores a **document hash** on-chain so third parties can verify integrity later. It is integrated into:

- [QRL Web Wallet](https://wallet.theqrl.org/)
- [QRL Block Explorer](https://explorer.theqrl.org/)

The encoding doc notes that **early notarization** predates QIP002 and uses a **slightly different encoding** than the standardized prefix ([message encoding](https://docs.theqrl.org/build/messages/message-tx-encoding)).

## CLI tooling

- `qrl-cli notarize DATAHASH` — submit a SHA256 document hash ([CLI](https://docs.theqrl.org/build/qrl-cli/overview/)).
- `qrl-cli send-message` — general short message path with fee and OTS index.

## Implementation references

Message transaction implementation (version-pinned link from docs):  
[MessageTransaction.py in QRL repo](https://github.com/theQRL/QRL/blob/v4.0.2/src/qrl/core/txs/MessageTransaction.py#L8)

Explorer/wallet helper code is linked from the same documentation page for client-side decoding patterns.
