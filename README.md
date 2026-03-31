# QRL Technical Handbook

MkDocs site: operator and developer documentation for the [Quantum Resistant Ledger (QRL)](https://theqrl.org/): protocol (XMSS, OTS, addresses), node operations, mining (RandomX), wallets, on-chain features (tokens, messages, multisig, slaves, lattice keys), APIs, QIPs, and Zond.

**Repository:** [github.com/gorusys/qrl-handbook](https://github.com/gorusys/qrl-handbook)  
**GitHub Pages:** [gorusys.github.io/qrl-handbook](https://gorusys.github.io/qrl-handbook/)

## Content map

- **Protocol & Cryptography** — network/PoW, units, address descriptors, XMSS/OTS, PQC context
- **Using QRL** — wallets, node install/config, mining
- **On-Chain Features** — tokens, messages/notarization, multisig, slave keys, lattice keys
- **Building & APIs** — developer guide, private network & Docker, gRPC / `qrl.proto` reference tables
- **Governance & Evolution** — QIPs, Zond testnet pointer
- **Tutorials** — first transaction, running a node
- **Reference Catalog** — canonical URLs to official docs, repos, and standards

## Local development

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```

Open `http://127.0.0.1:8000`.

## Build

```bash
mkdocs build --strict
```

Output: `site/`.

## Upstream sources

Authoritative material lives in [QRL Docs](https://docs.theqrl.org/) and [github.com/theQRL](https://github.com/theQRL). This repository summarizes and cross-links those sources for operators and integrators.
