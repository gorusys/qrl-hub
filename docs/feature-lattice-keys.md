# Lattice Keys (Kyber & Dilithium)

QRL’s CLI can generate **lattice-based** keypairs (**Kyber** and **Dilithium**) alongside an **ECDSA** key, and optionally **broadcast** them on-chain ([CLI `generate-lattice-keys`](https://docs.theqrl.org/build/qrl-cli/overview/)).

## Purpose

These keys enable **post-quantum-friendly auxiliary cryptography** (for example KEM-style shared secrets and PQ signatures) layered beside the legacy XMSS transaction authorization model. Typical users do not need this; integrators building advanced protocols do.

## CLI capabilities

`qrl-cli generate-lattice-keys` (from official CLI documentation):

- Requires a **QRL wallet** or **hexseed/mnemonic** (only one source).
- Can print keys to stdout or write an encrypted JSON file (`--crystalsFile`, `--crystalsPassword`).
- Optional **broadcast** (`-b`) with a valid **OTS index** (`-i`) to publish lattice keys for an address.
- Network selection: `--mainnet` / `--testnet`; custom node: `-g host:port`.

## Retrieving published keys

`qrl-cli get-keys` looks up lattice public keys by **transaction hash** or **address**, with pagination flags for address scans ([CLI](https://docs.theqrl.org/build/qrl-cli/overview/)).

## Shared secret helper

`qrl-cli generate-shared-keys` derives **shared key material** from combinations of lattice public/secret inputs and optional ciphertext/signed-message files — used for advanced handshake-style workflows ([CLI](https://docs.theqrl.org/build/qrl-cli/overview/)).

## API alignment

Public API includes helpers such as **`GetLatticeTxn`** and lattice lookup methods (see [API Reference](api-reference.md)).

## Security notes

- Lattice keys are **not** a replacement for XMSS transaction signing on the classic QRL chain; they extend what you can prove or encrypt **around** QRL activity.
- Protect lattice secret files with the same rigor as `wallet.json`.
