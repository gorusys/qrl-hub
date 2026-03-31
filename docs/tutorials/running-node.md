# Tutorial: Running a Node

Goal: a **mainnet** QRL node that stays synced, exposes APIs safely, and restarts automatically.

## 1. Prepare the host

- Linux host meeting [requirements](https://docs.theqrl.org/use/node/requirements) (AES-NI, AVX2, disk, bandwidth)
- Open **P2P 19000** to the internet only if you want public peering; restrict everything else by default

## 2. Install software

Follow [installation](https://docs.theqrl.org/use/node/installation):

```bash
pip3 install -U qrl
```

## 3. First launch (foreground)

```bash
start_qrl
```

Confirm logs show P2P on **19000** and gRPC public service on **19009** ([installation example output](https://docs.theqrl.org/use/node/installation)).

## 4. Harden configuration

1. Copy the [example `config.yml`](https://docs.theqrl.org/use/node/config/).
2. Set `public_api_host` to `127.0.0.1` if only local services should query.
3. Keep `admin_api_enabled: False` unless you explicitly need it.
4. Restart the node after edits.

## 5. systemd service

Use the template from [installation docs](https://docs.theqrl.org/use/node/installation):

```bash
sudo systemctl enable qrl.service
sudo systemctl start qrl.service
sudo systemctl status qrl.service
```

Point `ExecStart` at `$(which start_qrl)` for the service user.

## 6. Validation checklist

- [ ] `GetHeight` / `qrl-cli status` matches public explorers within latency bounds
- [ ] Peer count stable
- [ ] Disk free space trending predictably
- [ ] No repeated OTS or signature errors in `qrl.log`

## 7. Next steps

- Read [API Reference](../api-reference.md) for automation
- Add monitoring alerts for height stalls and API failures
