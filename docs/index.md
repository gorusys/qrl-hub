# QRL Technical Handbook

The **QRL Technical Handbook** is a structured, production-oriented reference for the [Quantum Resistant Ledger (QRL)](https://theqrl.org/): a public blockchain that uses **XMSS** (hash-based, stateful signatures) for transaction authentication, combined with **Proof-of-Work** using the **RandomX** algorithm on the live network.

## What QRL is

- A distributed ledger maintained by **QRL nodes** that validate blocks, sync history from **genesis (June 2018)**, and expose **gRPC APIs** for queries and transaction submission.
- An address model based on **one-time signature (OTS) indices** under XMSS: each outgoing transaction consumes exactly one unused OTS index; the network **rejects duplicate OTS reuse** to protect the address.
- A protocol with **on-chain features** beyond simple transfers: **tokens**, **message transactions** (including standardized encodings), **multisignature** workflows, **slave keys** (hypertree-style extension of signing capacity), and **lattice public-key material** for advanced use cases.
- A **crypto-agile** design in the address descriptor: multiple hash functions (SHA2-256, SHAKE-128, SHAKE-256) and room for future signature schemes in the encoding.

## Who this documentation is for

- **Node operators**: installation, configuration, ports, APIs, sync, and health.
- **Wallet users**: OTS limits, backups, tree height, and safe signing practices.
- **Miners**: RandomX pools, software, and operational cautions.
- **Integrators**: gRPC usage, transaction construction, wallet API and proxies, offline signing patterns.

## How to read this site

1. **Protocol & Cryptography** — how the chain agrees on blocks, how addresses and fees work, and why OTS state matters.
2. **Using QRL** — wallets, nodes, mining.
3. **On-Chain Features** — tokens, messages, multisig, slaves, lattice keys.
4. **Building & APIs** — integration patterns and API surface.
5. **Governance & Evolution** — QIPs and the separate **Zond** test stack (PoS + EVM).
6. **Reference Catalog** — canonical links to official docs, repositories, protobuf definitions, and standards.

!!! tip
    Primary sources are [QRL Docs](https://docs.theqrl.org/) and the [theQRL GitHub organization](https://github.com/theQRL). This handbook summarizes and cross-links them for operators and developers; always verify behavior against the node and tooling version you run in production.
