# Addresses & Descriptors

QRL addresses implement an **extensible, stateful hypertree** model: chained XMSS-style trees with **WOTS+** one-time signatures at the leaves ([address scheme](https://docs.theqrl.org/build/address/address-scheme)). The design targets post-quantum security while keeping address generation and signing practical versus a single gigantic XMSS instance.

## Display form vs internal form

- **User / explorer display**: hex string prefixed with **`Q`** — **79 hexadecimal characters** after `Q` in the common `sha256 2X` format (39-byte internal structure).
- **APIs and libraries**: often expect the **binary 39-byte** internal representation. The official docs note that the **Q-prefixed string may be rejected by the API** if passed where raw bytes are required — always follow your client library’s encoding rules.

## Internal layout (`sha256 2X`)

| Field | Bytes | Role |
|-------|-------|------|
| DESC | 3 | Descriptor: hash function, signature scheme, parameters, address format |
| HASH | 32 | `SHA2-256(DESC + PK)` |
| VERH | 4 | Last 4 bytes of `SHA2-256(DESC + HASH)` |

## Descriptor (first 3 bytes)

Bit-level fields (XMSS profile):

| Field | Bits | Meaning |
|-------|------|---------|
| HF | 0–3 | Hash function for the address |
| SIG | 4–7 | Signature scheme (XMSS = `0`) |
| P1 | 8–11 | **XMSS height / 2** (tree height parameter encoding) |
| AF (P2) | 12–15 | Address format (`sha256 2X` = `0`) |
| P3 | 16–23 | Reserved / unused in current XMSS profile |

### Hash function (`HF`)

| Value | Algorithm |
|-------|-----------|
| 0 | SHA2-256 |
| 1 | SHAKE-128 |
| 2 | SHAKE-256 |
| 3–15 | Reserved |

### Signature type (`SIG`)

| Value | Scheme |
|-------|--------|
| 0 | XMSS |
| 1–15 | Reserved (future agility) |

### Address format (`AF`)

| Value | Format |
|-------|--------|
| 0 | SHA256 2X (current standard) |
| 1–15 | Reserved |

## Tree height and OTS capacity

The **tree height** is fixed at address creation. It determines how many **OTS indices** exist for **outgoing** signatures from that address:

| Tree height | OTS keys (outgoing txs) |
|-------------|-------------------------|
| 8 | 256 |
| 10 | 1,024 (common default) |
| 12 | 4,096 |
| 14 | 16,384 |
| 16 | 65,536 |
| 18 | 262,144 |

Larger trees take longer to open because the Merkle tree must be regenerated when loading the wallet ([OTS documentation](https://docs.theqrl.org/build/fundamentals/ots-keys)).

## Incoming vs outgoing

- **Receiving** funds **does not** consume the recipient’s OTS pool.
- **Sending** (any transaction type signed from an address) consumes **one** OTS index from that signer address.

## Private key representations

Common formats ([wallet overview](https://docs.theqrl.org/use/wallet/overview)):

- **`wallet.json`** — may include mnemonic, hexseed, public address; support encryption.
- **Mnemonic** — 34 words from the [QRL wordlist](https://github.com/theQRL/qrllib/blob/master/src/qrl/wordlist.cpp).
- **Hexseed** — long hex string derived from the same secret material.
- **Ledger** — keys remain on device; user backs up the Ledger recovery seed.
- **`slaves.json` / payment slaves** — additional signing trees for **slave keys** ([slave keys](feature-slave-keys.md)).

## Crypto agility

The descriptor reserves space for **future hash functions and signature types** without breaking the addressing model ([address scheme](https://docs.theqrl.org/build/address/address-scheme)).

## See also

- [XMSS, OTS & Merkle Trees](xmss.md)
- [Wallet Ecosystem](wallet-ecosystem.md)
- [QRL CLI `create-wallet`](https://docs.theqrl.org/build/qrl-cli/overview/) (`-h` height, `-1`/`-2`/`-3` hash mechanism)
