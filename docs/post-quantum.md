# Post-Quantum Context

QRL’s design choice is to use **hash-based signatures (XMSS / WOTS+)** for on-chain authentication so that security does not rest on **discrete logarithm** hardness against quantum attacks.

## Classical risk (high level)

**Shor’s algorithm** breaks RSA and elliptic-curve discrete log problems on a sufficiently large, fault-tolerant quantum computer. That threatens ECDSA/EdDSA-style chains that reuse long-lived keys for unbounded signatures.

## Why hash-based signatures

- Security reduces to properties of **cryptographic hash functions** and the **XMSS construction**, not to number-theoretic problems targeted by Shor.
- Mature line of research; XMSS is standardized in **RFC 8391** and discussed for controlled deployments in **NIST SP 800-208**.

## Trade-offs QRL accepts

- **Stateful signing**: OTS indices must be consumed **once** and tracked monotonically.
- **Finite OTS per address** unless extended via **slave keys** or new addresses.
- **Larger signatures and state** than typical ECDSA — acceptable for a dedicated ledger design.

## Broader PQC landscape (for comparison)

NIST’s first finalized **post-quantum** standards (2024) include **ML-KEM** (FIPS 203), **ML-DSA** (FIPS 204), and **SLH-DSA** (FIPS 205, stateless hash-based signatures). QRL’s mainnet signature stack predates that wave and follows the **XMSS** track documented in SP 800-208 and RFC 8391.

QRL additionally exposes **lattice** (Kyber/Dilithium-class) **public material** in dedicated transactions for advanced protocols — see [Lattice Keys](feature-lattice-keys.md).

## Further reading

- [RFC 8391 — XMSS](https://www.rfc-editor.org/rfc/rfc8391)
- [NIST SP 800-208](https://csrc.nist.gov/publications/detail/sp/800-208/final)
- [FIPS 203 / 204 / 205](https://csrc.nist.gov/projects/post-quantum-cryptography) — module-lattice KEM/sigs and SLH-DSA
