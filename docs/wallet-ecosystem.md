# Wallet Ecosystem

QRL provides multiple wallet surfaces. All ultimately rely on **qrllib** for XMSS and address operations; the **web wallet** runs XMSS in **WebAssembly** compiled from qrllib so keys stay in the browser process ([wallet overview](https://docs.theqrl.org/use/wallet/overview)).

## Official wallet channels

| Channel | Entry | Documentation |
|---------|--------|---------------|
| Web | [wallet.theqrl.org](https://wallet.theqrl.org/) | [Web wallet docs](https://docs.theqrl.org/use/wallet/web/overview) |
| Desktop | Application release | [Desktop wallet](https://docs.theqrl.org/use/wallet/desktop) |
| Mobile | Application release | Mobile section under [use/wallet](https://docs.theqrl.org/use/wallet) |
| Ledger | Hardware device | [Ledger wallet](https://docs.theqrl.org/use/wallet/ledger) |
| CLI | `qrl-cli` / node CLI | [QRL CLI](https://docs.theqrl.org/build/qrl-cli/overview/), [node CLI](https://docs.theqrl.org/use/node/node-cli) |

## Key generation parameters

When creating an address you fix:

- **XMSS tree height** (`-h` on CLI, even values **4–18** per CLI docs) — determines OTS count ([CLI](https://docs.theqrl.org/build/qrl-cli/overview/)).
- **Hash function** — SHA2-256, SHAKE-128 (web default), or SHAKE-256 ([wallet overview](https://docs.theqrl.org/use/wallet/overview)).

## Public address example

Addresses are shared to receive funds; format is a **`Q` + hex** string ([wallet overview](https://docs.theqrl.org/use/wallet/overview)):

```text
Q010500cf8971ae2f24cecc4296a23c24277bd107dbbc630bc0799dca65f9c25449d781148b7a85
```

## Backups you must protect

- **Mnemonic** (34 words) or **hexseed** — full recovery of the master secret.
- **`wallet.json`** — often contains both; can be password-encrypted.
- **Ledger** — device PIN + **Ledger recovery seed** (BIP39-style backup for the device, not QRL-specific wording in QRL docs).
- **`slaves.json`** — required for spending via **slave** subtrees ([slave keys](feature-slave-keys.md)).

Never share private material. Verify any backup by **restoring into a throwaway wallet file** and comparing the **public address** before funding.

## Audits and source

The wallet overview references **third-party audits** and open-source repositories under [github.com/theQRL](https://github.com/theQRL) ([wallet overview](https://docs.theqrl.org/use/wallet/overview)).

## Explorer and notarization

The **block explorer** ([explorer.theqrl.org](https://explorer.theqrl.org/)) and wallet tools support **document notarization** flows tied to message transactions ([message encoding](https://docs.theqrl.org/build/messages/message-tx-encoding)).
