# Units (Quanta & Shor)

QRL amounts are expressed in two denominations:

- **Quanta** — human-oriented whole units (similar to “coins”).
- **Shor** — smallest on-chain unit used in protocol fields, APIs, and fee parameters.

The [QRL CLI](https://docs.theqrl.org/build/qrl-cli/overview/) exposes balance output in either form:

- `-q` / `--quanta` — display in Quanta
- `-s` / `--shor` — display in Shor

Always confirm which unit your script or wallet UI is using before interpreting balances or constructing transactions.

## Fees

- Explorer and tooling typically show **fees in Quanta** for transfers (example in [transaction lookup docs](https://docs.theqrl.org/use/tools/explorer/transaction-lookup)).
- CLI `send` examples use **fees in Shor** (default **100 shor** in documented `qrl-cli send` options).
- Node configuration defines a **minimum fee** the local node will accept into its mempool: `transaction_minimum_fee` defaults to **1000000000** in **shor** in the [example config](https://docs.theqrl.org/use/node/config/) (interpretation: this is a protocol-level floor expressed in shor — verify against your node version and network).

!!! warning
    Fee and minimum-fee behavior can change with software releases. For production, read the config template shipped with your exact `qrl` package and test on testnet first.

## Token creation fee

Token creation documentation references a default **100 shor** creation fee in the token overview ([QRL Token Overview](https://docs.theqrl.org/use/tools/tokens/overveiw)).

## Practical checklist

1. Decide whether your integration stores **integer shor** or **decimal quanta** internally.
2. Convert explicitly at boundaries (UI, API, database).
3. Log both **fee** and **unit** in operational audit trails.
