# QRL Overview

The Quantum Resistant Ledger (QRL) is a blockchain whose authenticity and ownership proofs rely on **post-quantum-friendly hash-based signatures (XMSS)** rather than classical elliptic-curve schemes. The live network uses **Proof-of-Work** with the **RandomX** (`rx/0`) algorithm for block production.

## Role of a QRL node

Per the [node overview](https://docs.theqrl.org/use/node/overview), each node:

- Validates transactions and blocks
- Participates in agreeing on a single immutable history
- Maintains chain state (e.g. LevelDB-backed data after sync)
- Exposes APIs so wallets and applications can query state and submit transactions

Every spend hits the network through a node: the client signs locally; the node verifies (including **OTS non-reuse** and balance/fee checks) before gossiping the transaction toward inclusion in a block.

## Network model

- **Peer-to-peer**: nodes discover and exchange blocks and transactions ([whitepaper reference](https://docs.theqrl.org/build/fundamentals/whitepaper)).
- **Sync from genesis**: operational docs note history from **June 2018**; plan disk and bandwidth accordingly ([node requirements](https://docs.theqrl.org/use/node/requirements)).
- **Consensus**: PoW with RandomX; see [Network & Consensus](network-and-consensus.md) and [Mining](mining.md).

## Address and signature model (summary)

- A QRL **address** encodes hash function choice, XMSS parameters, and a commitment to a Merkle tree of **WOTS+** one-time keys.
- Each **outgoing** transaction must be signed with a **new OTS index**. **Incoming** transfers do not consume the recipient’s OTS pool.
- **Tree height** is chosen at creation time and **cannot be changed** later; it caps the total number of spend signatures from that address unless extended via **slave keys**.

Details: [Addresses & Descriptors](addresses-and-descriptors.md), [XMSS, OTS & Merkle Trees](xmss.md), [slave keys](feature-slave-keys.md).

## Comparison to Bitcoin / Ethereum (high level)

| Aspect | Typical ECDSA/EdDSA chains | QRL |
|--------|---------------------------|-----|
| Signature family | Discrete-log hard problems | Hash-based XMSS (WOTS+ + Merkle tree) |
| Key reuse | One long-lived key signs many txs | One OTS index → one signature |
| Quantum threat model | Shor-class attacks on ECC | Designed around hash assumptions + XMSS analysis |
| Operational burden | Key backup | Backup **plus** monotonic OTS state |

## On-chain capabilities

Beyond `Quanta` transfers, QRL supports **tokens**, **message transactions**, **multisig**, **slave delegation**, and **lattice key** publication flows. See the **On-Chain Features** section in the navigation.

## Formal design references

- [QRL whitepaper (PDF)](https://github.com/theQRL/Whitepaper/raw/master/QRL_whitepaper.pdf) — cryptographic and protocol rationale ([docs entry](https://docs.theqrl.org/build/fundamentals/whitepaper)).
- [RFC 8391](https://www.rfc-editor.org/rfc/rfc8391) — XMSS.
- [NIST SP 800-208](https://csrc.nist.gov/publications/detail/sp/800-208/final) — stateful hash-based signature guidance.

## Related stack: Zond

QRL also develops **Zond**, a separate **Proof-of-Stake** network with **EVM**-style contracts, currently documented as a test network (see [Zond](zond.md)).
