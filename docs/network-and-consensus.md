# Network & Consensus

## Proof-of-Work: RandomX

The QRL mainnet uses **RandomX** (`rx/0`) for PoW validation. RandomX is a CPU-oriented, memory-hard PoW algorithm (see [RandomX project](https://github.com/tevador/RandomX)). QRL ships **QRandomX** / **pyqrandomx** as a Python-oriented interface to the hash and mining helpers ([QrandomX docs](https://docs.theqrl.org/build/mining/qrandomx)).

## What nodes do

From the [node overview](https://docs.theqrl.org/use/node/overview):

- Validate incoming transactions before they enter the mempool.
- Reject transactions that attempt to **reuse an OTS index** already seen on-chain for that address — this is a core integrity rule for XMSS.
- Propagate valid transactions and blocks to peers.
- Serve chain data via APIs to wallets and services.

## Peer-to-peer and sync

- Nodes connect over P2P (default listen port **19000** in [node configuration](https://docs.theqrl.org/use/node/config/)).
- Operators can tune `peer_list`, `enable_peer_discovery`, connection limits, bans, and NTP settings in `~/.qrl/config.yml`.
- Full sync replays history from **genesis**; ensure adequate **disk** and stable **network** ([requirements](https://docs.theqrl.org/use/node/requirements)).

## Hardware expectations for nodes

Official docs state:

- **Linux/Unix** is required for the reference node (Windows not fully supported for all dependencies).
- **AES-NI** and **AVX2** are expected for cryptographic and hashing paths; verify with `grep` / `lscpu` as documented.
- **Python 3.6+** minimum in published requirements (use a current supported Python in practice).

## Security-relevant behavior

- **OTS enforcement at the node**: even if a client is buggy, a correct node will refuse to relay transactions that violate OTS rules.
- **API exposure**: public gRPC is often enabled on **19009** with `public_api_host` defaulting to `0.0.0.0` in example configs — **restrict this in production** using firewalls or loopback binding.
- **Privileged APIs**: admin (**19008**), mining (**19007**), debug (**52134**), wallet daemon (**18091**), wallet API (**19010**) should not be internet-facing without strict controls.

## Further reading

- [Node configuration](https://docs.theqrl.org/use/node/config/)
- [Pool mining](https://docs.theqrl.org/use/mining/pool-mining/) (for PoW ecosystem context)
- [OTS keys](https://docs.theqrl.org/build/fundamentals/ots-keys)
