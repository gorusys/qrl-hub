# Slave Keys (Hypertrees)

**Slave keys** extend how many transactions a **master** QRL address can authorize by attaching additional XMSS subtrees signed into the chain ([slave keys](https://docs.theqrl.org/build/address/slave-keys)).

## Mechanism

- The master address generates one or more **slave** XMSS trees (CLI or API).
- The master broadcasts **`RelaySlaveTxn`**, registering the slaves’ **public** identities on-chain.
- After confirmation, those slaves may **sign spends on behalf of the master** without moving Quanta to separate top-level addresses first ([slave keys](https://docs.theqrl.org/build/address/slave-keys)).

Funds remain associated with the **master** address; slaves are signing delegates.

## Limits and files

- Up to **100** slave addresses can be generated in one tooling flow ([node CLI slave doc reference](https://docs.theqrl.org/use/node/node-cli/node-cli-slave-xmss)).
- Private slave material is stored in **`slaves.json`** (or payment slave variants) — **as critical as the master seed** ([slave keys](https://docs.theqrl.org/build/address/slave-keys)).

## Wallet daemon automation

The [walletd-rest-proxy](https://docs.theqrl.org/api/walletd-rest-proxy) documents **automatic slave transactions**, caching slave metadata in **`~/.qrl/walletd.json`** by default ([slave keys](https://docs.theqrl.org/build/address/slave-keys)).

## When to use slaves

- High-frequency signing from one logical account.
- Services that cannot rotate user-facing addresses often but must avoid OTS exhaustion.
- Any architecture that would otherwise hit the **tree height** cap.

!!! danger
    Loss of `slaves.json` without backup can lock spending capacity even if the master seed exists. Treat slave files as **tier-0 secrets**.

## Documentation entry points

- [Slave keys (build)](https://docs.theqrl.org/build/address/slave-keys)
- [Node CLI — slave XMSS](https://docs.theqrl.org/use/node/node-cli/node-cli-slave-xmss)
- [Wallet API — RelaySlaveTxn](https://docs.theqrl.org/api/wallet-api#relayslavetxn)
