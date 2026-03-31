# Private Networks & Docker

For integration testing without mainnet funds, QRL supports **isolated private chains** and **containerized** node images.

## Private QRL network

Official guide: [QRL Private Network](https://docs.theqrl.org/build/private-network).

Minimum `~/.qrl/config.yml` excerpt (from upstream docs):

```yaml
genesis_difficulty: 500
mining_enabled: True
peer_list: []
```

- **`peer_list: []`** overrides default public peers so the node does not join mainnet.
- **`genesis_difficulty`** controls how hard the first blocks are to mine (lower = easier).
- If this host previously synced **mainnet**, delete **`~/.qrl/data/`** before starting the private chain ([private network](https://docs.theqrl.org/build/private-network)).

Example launch with simplified mining measurement (from upstream docs):

```bash
start_qrl --miningAddress Q0108... --mockGetMeasurement 1000000000
```

`--mockGetMeasurement` is documented as a **test/integration** aid to keep difficulty tractable on weak hardware ([private network](https://docs.theqrl.org/build/private-network)).

!!! warning
    `mockGetMeasurement` is not a mainnet security setting. Use only in controlled lab networks.

## Docker image

Official overview: [QRL Docker Overview](https://docs.theqrl.org/build/docker/overview).

```bash
docker pull qrledger/qrl-docker:jammy
docker run -d --name=qrl-node qrledger/qrl-docker:jammy
docker start qrl-node
docker logs qrl-node
```

The Docker page also documents running wallet generation inside the container (`docker exec -i -t qrl-node ...`). Command names may vary by image tag — follow the exact version you pull.

## When to use which

- **Private network** on bare metal or VM: fastest iteration for protocol integrators who need custom `config.yml` and persistent volumes.
- **Docker**: reproducible environments for CI or onboarding; pin image digests in production-like test pipelines.

## See also

- [Node Installation & Config](node-setup.md)
- [Developer Guide](developer-guide.md)
- Reference links under **Build** in [Reference Catalog](reference-catalog.md)
