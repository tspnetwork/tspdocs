---
title: Snapshots & Archive Nodes
weight: 5
draft: false
---

Quickly sync your node with TSP using a snapshot or serve queries for prev versions using archive nodes

## List of Snapshots and Archives

Below is a list of publicly available snapshots that you can use to sync with the TSP mainnet and
archived [9001-1 mainnet](https://github.com/tspnetwork/tspchain/mainnet/tree/main/tsp_9001-1):

### Snapshots

| Name | URL |
| --- | --- |
| `Staketab` | [github.com/staketab/nginx-cosmos-snap](https://github.com/staketab/nginx-cosmos-snap/blob/main/docs/tsp.md) |
| `Polkachu` | [polkachu.com](https://www.polkachu.com/tendermint_snapshots/tsp) |
| `Notional` | [mainnet/pruned/tsp_9001-2(pebbledb)](https://snapshot.notional.ventures/tsp/) <br> [mainnet/archive/tsp_9001-2(pebbledb)](https://snapshot.notional.ventures/tsp-archive/) <br> [testnet/archive/tsp_9000-4(pebbledb)](https://snapshot.notional.ventures/tsp-testnet-archive/) |
| `Windpowerstake` | [mainnet/archive/tsp_9001-2(goleveldb)](http://backup03.windpowerstake.com/) |
| `GalaxyStaking` | [galaxystaking.space](https://tools.galaxystaking.space/tsp/) |

### Archives

| Name | URL |
| --- | --- |
| `Polkachu` | [polkachu.com/tendermint_snapshots/tsp](https://www.polkachu.com/tendermint_snapshots/tsp) |

### PebbleDB

To use PebbleDB instead of GoLevelDB when using snapshots from Notional:

Build:

```
go mod edit -replace github.com/tendermint/tm-db=github.com/baabeetaa/tm-db@pebble
go mod tidy
go install -tags pebbledb -ldflags "-w -s -X github.com/cosmos/cosmos-sdk/types.DBBackend=pebbledb" ./...
```

Download snapshot:

```
cd $HOME/.tspd/
URL_SNAPSHOT="https://snapshot.notional.ventures/tsp/data_20221024_193254.tar.gz"
wget -O - "$URL_SNAPSHOT" |tar -xzf -
```

Start:

Set `db_backend = "pebbledb"` in `config.toml` or start with `--db_backend=pebbledb`

```
tspd start --db_backend=pebbledb
```

**Note**: use this [workaround](https://github.com/notional-labs/cosmosia/blob/main/docs/pebbledb.md) when upgrading a node running PebbleDB.