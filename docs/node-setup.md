# Node Installation & Configuration

This page condenses the official [installation](https://docs.theqrl.org/use/node/installation), [configuration](https://docs.theqrl.org/use/node/config/), [requirements](https://docs.theqrl.org/use/node/requirements), and [overview](https://docs.theqrl.org/use/node/overview) documentation.

## Requirements summary

- **OS**: Linux/Unix for reference node software (Windows not fully supported for all components).
- **CPU**: 64-bit with **AES-NI** and **AVX2** (verify via `/proc/cpuinfo` or `lscpu` per docs).
- **Python**: **3.6+** stated in docs; use a current Python in production.
- **Disk / network**: sufficient for full history from **genesis (June 2018)** and steady peer I/O.

## Install (Ubuntu example)

From [installation docs](https://docs.theqrl.org/use/node/installation):

```bash
sudo apt update && sudo apt upgrade -y
sudo apt-get -y install swig3.0 python3-dev python3-pip build-essential pkg-config \
  libssl-dev libffi-dev libhwloc-dev libboost-dev cmake libleveldb-dev
pip3 install setuptools==65.7.0
pip3 install service-identity==21.1.0
pip3 install -U qrl
```

## Run the node

```bash
start_qrl
```

Creates data under **`~/.qrl`** by default and begins P2P sync. Useful flags ([installation docs](https://docs.theqrl.org/use/node/installation)):

| Flag | Purpose |
|------|---------|
| `--network-type {mainnet,testnet}` | Separate data dir (`~/.qrl-testnet` for testnet) |
| `--qrldir PATH` | Custom root for data/config |
| `--mining_thread_count N` | Mining threads |
| `--miningAddress Q...` | Mining reward address |
| `--loglevel LEVEL` | Logging verbosity |

## Configuration file

Primary path: **`~/.qrl/config.yml`**. Download the official [example config](https://docs.theqrl.org/use/node/config/) and uncomment only what you need.

**Restart the node** after changes.

### Default ports (from example config)

| Service | Default port |
|---------|----------------|
| P2P | 19000 |
| Public gRPC API | 19009 |
| Admin API | 19008 |
| Mining API | 19007 |
| Debug API | 52134 |
| gRPC proxy | 18090 |
| Wallet daemon | 18091 |
| Wallet API | 19010 |

### Mining block in config

```yaml
mining_enabled: False
mining_address: 'Q0105...'
mining_thread_count: 0
```

### P2P and mempool (excerpt)

Important keys include `peer_list`, `enable_peer_discovery`, `max_peers_limit`, `transaction_pool_size`, `pending_transaction_pool_size`, `transaction_minimum_fee`, `stale_transaction_threshold` ([full table](https://docs.theqrl.org/use/node/config/)).

### Ephemeral messaging (experimental)

The example configuration includes **ephemeral** traffic toggles such as `accept_ephemeral` and `outgoing_message_expiry` (seconds). Official docs mark this area as **still in development** and subject to change ([node configuration](https://docs.theqrl.org/use/node/config/)). Treat it as non-production unless you explicitly depend on the feature and track upstream releases.

## Production deployment patterns

### systemd

Official docs provide a `qrl.service` template pointing `ExecStart` at `$(which start_qrl)` ([installation docs](https://docs.theqrl.org/use/node/installation)).

### screen / tmux

For lab setups, `screen -dmS QRL start_qrl` keeps the process attached to a persistent session.

## Observability

- Log file: `~/.qrl/qrl.log` (tail with `tail -f`).
- Process check: `ps aux | rg qrl` or `systemctl status qrl.service`.

## Security baseline

- Bind **admin/debug/mining/wallet** APIs to **loopback** unless you have explicit network controls.
- Firewall **19000** only to trusted peers if you operate a closed topology.
- Monitor peer count, block height, disk usage, and API error rates.

## Helpers

- **Node helpers (npm)**: [@theqrl/node-helpers](https://docs.theqrl.org/build/helpers/node-helpers) for gRPC integration from Node.js.
