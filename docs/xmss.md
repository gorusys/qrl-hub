# XMSS, OTS & Merkle Trees

QRL uses **XMSS** (eXtended Merkle Signature Scheme, [RFC 8391](https://www.rfc-editor.org/rfc/rfc8391)) with **WOTS+** one-time signatures at the leaves of a **Merkle tree**. The result is a **stateful** signature scheme: security depends on **never signing twice with the same OTS index**.

Official QRL primers: [OTS keys](https://docs.theqrl.org/build/fundamentals/ots-keys), [address scheme](https://docs.theqrl.org/build/address/address-scheme).

## One-time signatures (OTS)

- Each OTS index can produce **one** secure signature for a transaction.
- Signing reveals information; **reuse** of the same index breaks the security argument and can allow forgeries.
- The QRL network **rejects** transactions that reuse an OTS index already present on-chain for that address ([node overview](https://docs.theqrl.org/use/node/overview)).

!!! danger
    If you ever **double-sign** with the same OTS index (for example after restoring an old wallet backup), treat the address as **compromised** and move funds to a **new** address with a fresh seed.

## Merkle tree aggregation

Instead of publishing one public key per OTS key, XMSS publishes a single **root** that commits to many OTS public keys (leaves). A signature includes:

- the **WOTS+** signature for the chosen leaf, and
- a Merkle **authentication path** proving that leaf is under the published root.

## Tree height and capacity

Tree height is chosen at **address creation** and **cannot be upgraded** in place. It fixes the maximum number of **outgoing** signatures:

| Height | OTS keys |
|--------|----------|
| 8 | 256 |
| 10 | 1,024 |
| 12 | 4,096 |
| 14 | 16,384 |
| 16 | 65,536 |
| 18 | 262,144 |

Trade-off: larger trees mean **more OTS capacity** but **slower wallet open** times because the tree is regenerated from the seed when loading ([wallet overview](https://docs.theqrl.org/use/wallet/overview)).

## Exhaustion

When all OTS indices are consumed, the address **cannot sign** further transactions. Funds remaining on that address are effectively **stuck** unless you had previously moved them. Plan rotations early.

## Hypertrees and slave keys

QRL extends usable signing capacity by allowing a master address to **delegate** additional XMSS subtrees as **slave** addresses via `RelaySlaveTxn`. This is the practical “infinite address” pattern documented under [slave keys](feature-slave-keys.md) and the [whitepaper](https://github.com/theQRL/Whitepaper/raw/master/QRL_whitepaper.pdf).

## Hash functions in QRL

QRL supports **SHA2-256**, **SHAKE-128**, and **SHAKE-256** for the address/XMSS stack ([address scheme](https://docs.theqrl.org/build/address/address-scheme)). The web wallet default hash function is documented as **SHAKE-128** when unspecified ([wallet overview](https://docs.theqrl.org/use/wallet/overview)); CLI `create-wallet` exposes `-1` SHA2-256, `-2` SHAKE-128, `-3` SHAKE-256 ([CLI overview](https://docs.theqrl.org/build/qrl-cli/overview/)).

## Standards context

- **RFC 8391** — XMSS / XMSS^MT definitions.
- **NIST SP 800-208** — recommendations for **stateful** hash-based signatures (XMSS, LMS/HSS), emphasizing **state management** and deployment constraints ([SP 800-208](https://csrc.nist.gov/publications/detail/sp/800-208/final)).

## Operational checklist

- [ ] Track **next unused OTS** off-chain and on-chain (`qrl-cli ots`, or `GetOTS` via API).
- [ ] Persist wallet state **after** every sign; use **single-writer** signing processes.
- [ ] Alert before OTS capacity runs out; automate migration to new addresses or slave pools.
