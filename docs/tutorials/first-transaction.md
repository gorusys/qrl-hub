# Tutorial: First Transaction

Goal: confirm a **mainnet** (or **testnet**) spend using a correct **unused OTS index** and a fee compatible with your node’s minimum.

## Prerequisites

- A funded QRL address with remaining OTS capacity ([OTS keys](https://docs.theqrl.org/build/fundamentals/ots-keys))
- [`qrl-cli`](https://docs.theqrl.org/build/qrl-cli/overview/) installed (`npm install -g @theqrl/cli`)
- Network connectivity to a **synced** node (public `-g host:port` or local `127.0.0.1:19009`)

## 1. Verify node status

```bash
qrl-cli status --mainnet
```

On a local node you can also tail `~/.qrl/qrl.log` ([installation docs](https://docs.theqrl.org/use/node/installation)).

## 2. Inspect balance and next OTS

```bash
qrl-cli balance Q0105... --mainnet -q
qrl-cli ots Q0105... --mainnet
```

Record the **next unused** OTS index reported by `ots`.

## 3. Send Quanta

```bash
qrl-cli send 0.1 \
  --recipient Q0105... \
  --fee 100 \
  --otsindex <NEXT_UNUSED> \
  --wallet wallet.json \
  --mainnet
```

- `--fee` is in **shor** unless you adapt per CLI help.
- Add `-p` if `wallet.json` is encrypted.

## 4. Confirm on-chain

```bash
qrl-cli search <TX_HASH> --mainnet
```

Or use the explorer ([transaction lookup](https://docs.theqrl.org/use/tools/explorer/transaction-lookup)).

## 5. Re-check OTS

```bash
qrl-cli ots Q0105... --mainnet
```

The next unused index should have incremented.

## Troubleshooting

| Symptom | Likely cause |
|---------|----------------|
| OTS rejection | Wrong index or already used on-chain |
| Fee rejection | Below node `transaction_minimum_fee` ([config](https://docs.theqrl.org/use/node/config/)) |
| Timeout after signing | Check mempool / rebroadcast carefully without reusing OTS |

## Next steps

- Practice **offline signing** (`-T` / `-F` flags in [CLI send](https://docs.theqrl.org/build/qrl-cli/overview/))
- Read [Slave Keys](../feature-slave-keys.md) before high-frequency sending from one address
