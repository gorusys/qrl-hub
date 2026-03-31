# Wallet Operations

Practical guide for safely holding and spending QRL. Read [Units (Quanta & Shor)](units-and-fees.md) and [XMSS, OTS & Merkle Trees](xmss.md) first.

## Before you receive funds

1. Create a wallet using an official app ([Wallet Ecosystem](wallet-ecosystem.md)).
2. Record **mnemonic** or **hexseed** (or secure `wallet.json` backup).
3. **Restore once** into a separate file/profile and confirm the **Q... address** matches.
4. Decide if default **tree height 10** (1,024 OTS) is enough for your expected **outgoing** tx count; increase height or plan **slave keys** if not ([OTS keys](https://docs.theqrl.org/build/fundamentals/ots-keys)).

## Checking balance and OTS

Using [QRL CLI](https://docs.theqrl.org/build/qrl-cli/overview/):

```bash
qrl-cli balance Q0105... --mainnet
qrl-cli ots Q0105... --mainnet
```

- `balance` supports `-q` / `-s` for Quanta vs Shor.
- `ots` reports the **next unused** OTS index for the address or `wallet.json`.

## Sending Quanta

```bash
qrl-cli send 1.0 \
  --recipient Q0105... \
  --fee 100 \
  --otsindex <NEXT_UNUSED> \
  --wallet wallet.json \
  --mainnet
```

Key points from CLI documentation:

- **Fee** is in **shor** by default (`-f`).
- **OTS index** must be unused.
- **Offline flow**: `-T` saves signed tx to file; `-F` loads and broadcasts ([CLI send](https://docs.theqrl.org/build/qrl-cli/overview/)).
- Optional **80-byte message** on transfers via `-M`.

## Messages and notarization

- `qrl-cli send-message` sends up to **80 bytes** ([CLI](https://docs.theqrl.org/build/qrl-cli/overview/)).
- `qrl-cli notarize <SHA256>` posts a hash on-chain ([CLI](https://docs.theqrl.org/build/qrl-cli/overview/)).
- Structured payloads use **QIP002** encoding — see [Messages & Notarization](feature-messages.md).

## Multisig and tokens

- Multisig flows consume OTS from **each signer address** that votes; see [Multisignature](feature-multisig.md).
- Tokens use separate transaction types — see [Tokens](feature-tokens.md).

## Operational rules

- **Single writer** per signing key: no parallel processes sharing one `wallet.json` without coordination.
- **Backup after every sign** if your tool requires it to persist OTS state.
- **Never decrement** or reuse OTS indices.
- **Migrate before exhaustion**: send funds to a fresh address or install slave trees in advance.

## Ledger specifics

Ledger enforces a smaller maximum XMSS height (**8 → 256 OTS**) due to device memory limits ([OTS documentation](https://docs.theqrl.org/build/fundamentals/ots-keys)). Plan UTXO-style consolidation or slave delegation accordingly.
