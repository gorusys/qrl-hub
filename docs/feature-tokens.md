# Tokens (Colored Coins)

QRL supports **on-chain tokens** distinct from native **Quanta** balances. Creation and transfers use dedicated transaction types ([Token overview](https://docs.theqrl.org/use/tools/tokens/overveiw)).

## Concepts

- Tokens are identified by their **creation transaction hash** on-chain.
- Holders and balances are tracked in ledger state; transfers pay **network fees** in the usual fee denomination (see [Units](units-and-fees.md)).
- Token logic is **not** general-purpose smart contracts on the legacy mainnet; it is ledger-enforced token state.

## Creating tokens

GUI paths: [web](https://docs.theqrl.org/use/wallet/web) and [desktop](https://docs.theqrl.org/use/wallet/desktop) wallets, plus APIs/CLI for automation ([token overview](https://docs.theqrl.org/use/tools/tokens/overveiw)).

Documented creation fields:

| Field | Notes |
|-------|--------|
| Owner address | Token owner metadata (distinct from initial holder list) |
| Symbol | Short symbol (doc table shows max **10** bytes; follow current wallet UI limits) |
| Name | Longer name (doc table shows max **30** bytes) |
| Decimals | Up to **99** per docs (validate against wallet version) |
| Initial holders | Up to **100** QRL addresses in the holder/balance array |
| Creation fee | Documented default **100 shor** |
| OTS | Consumes one OTS from the creator address |

Detailed walkthrough: [Create tokens](https://docs.theqrl.org/use/tools/tokens/create).

## Sending tokens

Tokens move between addresses by reference to the **token creation tx hash**; transfers are **irreversible** once confirmed ([send tokens](https://docs.theqrl.org/use/tools/tokens/send)).

## API hooks

Wallet API methods include **`RelayTokenTxn`** and **`RelayTransferTokenTxn`** ([wallet API index](https://docs.theqrl.org/api/wallet-api)). Use these when automating beyond GUI workflows.

## Explorer

Token metadata and balances can be inspected in the explorer ([token lookup docs](https://docs.theqrl.org/use/tools/explorer/token-lookup)).
