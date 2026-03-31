# Getting Started

Pick a track and follow it end-to-end. All paths assume you understand that **QRL addresses have a finite number of outgoing signatures** (OTS keys) unless you use **slave keys** or rotate to new addresses.

## If you only want to hold and send QRL

1. Read [Wallet Ecosystem](wallet-ecosystem.md) and choose a wallet (web, desktop, mobile, or Ledger).
2. Read [Units (Quanta & Shor)](units-and-fees.md) so balances and fees are unambiguous.
3. Read [XMSS, OTS & Merkle Trees](xmss.md) and [Addresses & Descriptors](addresses-and-descriptors.md) at least through the OTS section.
4. Complete [First Transaction](tutorials/first-transaction.md).

## If you want to run infrastructure

1. [QRL Overview](qrl-overview.md) and [Network & Consensus](network-and-consensus.md)
2. [Node Installation & Config](node-setup.md)
3. [Running a Node](tutorials/running-node.md)
4. [API Reference](api-reference.md) for health checks and automation

## If you want to mine

1. [Mining (RandomX)](mining.md)
2. A QRL address for payouts and a RandomX-capable miner (e.g. [xmrig](https://github.com/xmrig/xmrig))

## If you are building an integration

1. [Developer Guide](developer-guide.md)
2. [Private Networks & Docker](developer-testing.md) for local and CI environments
3. [API Reference](api-reference.md) and the canonical [`qrl.proto`](https://github.com/theQRL/QRL/blob/master/src/qrl/protos/qrl.proto)
4. Feature pages for anything beyond simple transfers: [Tokens](feature-tokens.md), [Messages](feature-messages.md), [Multisignature](feature-multisig.md), [Slave Keys](feature-slave-keys.md), [Lattice Keys](feature-lattice-keys.md)

## Critical rules (production)

- **Never reuse an OTS index.** The network rejects duplicates; restoring an old wallet backup after signing can desynchronize state and risk compromise if you double-sign.
- **Plan for OTS exhaustion.** Move funds before the last OTS is consumed, or design with **slave keys** / new addresses.
- **Run your own node** or use endpoints you trust for high-value automation ([QRL API overview](https://docs.theqrl.org/api/qrl-api-overview)).
