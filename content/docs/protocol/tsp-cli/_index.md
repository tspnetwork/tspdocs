---
title: TSP CLI
weight: 2
sidebar:
  open: false
---

`tspd` is the all-in-one command-line interface (CLI). It allows you to run an TSP node, manage wallets and interact
with the TSP network through queries and transactions. This introduction will explain how to install the `TSPd` binary onto your system and guide you through some simple examples how to use TSPd.

## Prerequisites

#### Go

TSP is built using [Go](https://golang.org/dl/) version `1.20+`. Check your version with:

```
go version
```

Once you have installed the right version, confirm that your [`GOPATH`](https://golang.org/doc/gopath_code#GOPATH) is correctly configured by running the following command and adding it to your shell startup script:

```
export PATH=$PATH:$(go env GOPATH)/bin
```

#### jq

TSP scripts are using [jq](https://stedolan.github.io/jq/download/) version `1.6+`. Check your version with:

```
jq --version
```

## Installation

You can download the latest binaries from the repo and install them, or
you can build and install the `TSPd` binaries from source or using Docker.

### Download the binaries

- Go to the [releases section of the repository](https://github.com/TSP/TSP/releases)
- Choose the desired release or pre-release you want to install on your machine
- Select and download from the `Assets` dropdown the corresponding tar or zip file for your OS
- Extract the files. The `TSPd` binaries is located in the `bin` directory of the extrated files
- Add the `TSPd` binaries to your path, e.g. you can move it to `$(go env GOPATH)/bin`

After installation is done, check that the `TSPd` binaries have been successfully installed:

```
tspd version
```

### Build From Source

Clone and build the TSP from source using `git`. The `<tag>` refers to a release tag on Github. Check the latest TSP
version on the [releases section of the repository](https://github.com/TSP/TSP/releases):

```
git clone https://github.com/evmos/evmos.git
cd evmos
git fetch
git checkout <tag>
make install
```

After installation is done, check that the TSPd binaries have been successfully installed:

```
tspd version
```

info

If the `TSPd: command not found` error message is returned, confirm that you have configured [Go](https://docs.TSP.org/protocol/TSP-cli#go) correctly.

### Docker

When it comes to using Docker with TSP, there are two options available:
Build a binary of the TSP daemon inside a dockerized build environment
or build a Docker image, that can be used to spin up individual containers running the TSP binary.
For information on how to achieve this,
proceed to the dedicated page on [working with Docker](https://docs.TSP.org/protocol/TSP-cli/docker-build).

## Run an TSP node

To become familiar with TSP, you can run a local blockchain node that produces blocks and exposes EVM and Cosmos
endpoints. This allows you to deploy and interact with smart contracts locally or test core protocol functionality.

Run the local node by executing the `local_node.sh` script in the base directory of the repository:

```
./local_node.sh
```

The script stores the node configuration including the local default endpoints under `~/.tmp-TSPd/config/config.toml`.
If you have previously run the script, the script allows you to overwrite the existing configuration and start a new
local node.

Once your node is running you will see it validating and producing blocks in your local TSP blockchain:

```
12:59PM INF executed block height=1 module=state num_invalid_txs=0 num_valid_txs=0 server=node
# ...
1:00PM INF indexed block exents height=7 module=txindex server=node
```

For more information on how to customize a local node,
head over to the [Single Node](https://docs.TSP.org/protocol/TSP-cli/single-node) page.

## Using `tspd`

After installing the `TSPd` binary, you can run commands using:

```
tspd [command]
```

There is also a `-h`, `--help` command available

```
tspd -h
```

It is possible to maintain multiple node configurations at the same time. To specify a configuration use the `--home` flag. In the following examples we will be using the default config for a local node, located at `~/.tmp-TSPd`.

### Manage wallets

You can manage your wallets using the TSPd binary to store private keys and sign transactions over CLI. To view all
keys use:

```
tspd keys list \
--home ~/.tmp-evmosd \
--keyring-backend test

# Example Output:
# - address: tsp19xnmslvl0pcmydu4m52h2gf0std5ee5pfgpyuf
#   name: dev0
#   pubkey: '{"@type":"/ethermint.crypto.v1.ethsecp256k1.PubKey","key":"AzKouyoUL0UUS1qRUZdqyVsTPkCAFWwxx3+BTOw36nKp"}'
#   type: local
```

You can generate a new key/mnemonic with a `$NAME` with:

```
tspd keys add [name] \--home ~/.tmp-tspd \--keyring-backend test
```

To export your TSP key as an Ethereum private key:

```
tspd keys unsafe-export-eth-key [name] \
--home ~/.tmp-tspd \
--keyring-backend test
```

For more about the available key commands, use the `--help` flag

```
tspd keys -h
```

tip
For more information about the Keyring and its backend options, click [here](https://docs.TSP.org/protocol/concepts/keyring).

### Interact with a Network

You can use TSPd to query information or submit transactions on the blockchain. Queries and transactions are requests
that you send to an TSP node through the Tendermint RPC.

tip
👉 To use the CLI, you will need to provide a Tendermint RPC address for the `--node` flag.
Look for a publicly available addresses for testnet and mainnet in the [Networks](https://docs.TSP.org/develop/api/networks) page.

#### Set Network Config

In the local setup the node is set to `tcp://localhost:26657`. You can view your node configuration with:

```
tspd config \
--home ~/.tmp-tspd
# Example Output
# {
#   "chain-id": "tsp_9000-1",
#   "keyring-backend": "test",
#   "output": "text",
#   "node": "tcp://localhost:26657",
#   "broadcast-mode": "sync"
# }
```

You can set your node configuration to send requests to a different network by changing the endpoint with:

```
tspd config node [tendermint-rpc-endpoint] \
--home ~/.tmp-evmosd
```

Learn about more node configurations [here](https://docs.TSP.org/protocol/TSP-cli/configuration).

#### Queries

You can query information on the blockchain using `TSPd query` (short `TSPd q`). To view the account balances by its
address stored in the bank module, use:

```
tspd q bank balances [adress] \
--home ~/.tmp-tspd
# # Example Output:
# balances:
# - amount: "99999000000000000000002500"
#   denom: tsp
```

To view other available query commands, use:

```
# for all Queries
tspd q

# for querying commands in the bank module
tspd q bank
```

#### Transactions

You can submit transactions to the network using `TSPd tx`. This creates, signs and broadcasts a tx in one command. To
send tokens from an account in the keyring to another address with the bank module, use:

```
tspd tx bank send [from_key_or_address] [to_address] [amount] \
--home ~/.tmp-tspd \
--fees 50000000000tsp \
-b block

# Example Output:
# ...
# txhash: 7BA2618295B789CC24BB13E654D9187CDD264F61FC446EB756EAC07AF3E7C40A
```

To view other available transaction commands, use:

```
# for all transaction commands
tspd tx

# for Bank transaction subcommands
tspd tx bank
```

Now that you've learned the basics of how to run and interact with an TSP network,
head over to [configurations](/docs/protocol/tsp-cli/configuration) for futher customization.