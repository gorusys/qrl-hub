# Mining (RandomX)

QRL mainnet PoW uses **RandomX** (`rx/0`). Pool mining doc: [Pool Mining QRL](https://docs.theqrl.org/use/mining/pool-mining/).

## Algorithm reference

- RandomX repository: [github.com/tevador/RandomX](https://github.com/tevador/RandomX)
- QRL wrapper library: [QrandomX / pyqrandomx](https://docs.theqrl.org/build/mining/qrandomx)

## Pool mining

- You need a **QRL payout address**, **RandomX-capable mining software**, and a pool’s stratum connection details.
- **There is no official QRL pool**; the project may run **testnet** pools only for testing ([pool mining doc](https://docs.theqrl.org/use/mining/pool-mining/)).
- Pools charge **fees**; payout thresholds can change — read pool rules.
- **Do not leave large balances on a pool**; withdraw to your wallet at minimum payout.

## Mining software

Documented example: **xmrig** (CPU/GPU, TLS, JSON API) — any RandomX-compatible miner should work in principle ([pool mining doc](https://docs.theqrl.org/use/mining/pool-mining/)).

## Solo mining

Solo mining is configured via the node (`mining_enabled`, `mining_address`, `mining_thread_count` in [config](https://docs.theqrl.org/use/node/config/) and `start_qrl` flags). Ensure your node is **synced** and hardware meets **AES-NI/AVX2** expectations.

## Operational checklist

- [ ] Dedicated **cold or hardware-secured** payout address
- [ ] Monitor **hashrate**, **stale shares**, **payout history**
- [ ] Track **pool operator reputation** (no official operator)

## Related QIP

RandomX adoption is documented historically as [QIP009](https://theqrl.org/qips/qip009) (mining algorithm change proposal on theqrl.org).
