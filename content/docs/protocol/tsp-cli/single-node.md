---
title: Single Node
linkTitle: Single Node
weight: 3
---

Following this page, you can run a single node local network manually or
by using the already prepared automated script. Running a single node setup is useful
for developers who want to test their applications and protocol features because of
its simplicity and speed. For more complex setups, please refer to the [Multi Node Setup](/docs/protocol/tsp-cli/multi-nodes) page.

## Prerequisite Readings

- [Install Binary](/docs/protocol/tsp-cli/)

## Automated Script

The simplest way to start a local TSP node is by using the provided helper script on the base level of the [TSP repository](https://github.com/tspnetwork/tspchain/blob/main/local_node.sh),
which will create a sensible default configuration for testing purposes:

```
$ local_node.sh...
```

{{< callout type="info" >}}
To avoid overwriting any data for a real node used in production, it was 
decided to store the automatically generated testing configuration at `~/.tmp-tspd` instead of the default `~/.tspd`.
{{< /callout >}}

When working with the `local_node.sh` script, it is necessary to extend all `tspd` commands, that target the local test node, with the `--home ~/.tmp-tspd` flag. This is mandatory, because the `home` directory cannot be stored in the `tspd` configuration, which can be seen in the output below. For ease of use, 
it might be sensible to export this directory path as an environment 
variable:

```
$ export TMP=$HOME/.tmp-tspd`
$ tspd config --home $TMP
{
"chain-id": "tsp_9000-1",
"keyring-backend": "test",
"output": "text",
"node": "tcp://localhost:26657",
"broadcast-mode": "sync"
}
```

You can customize the local node script by changing the configuration variables.
See the following excerpt from the script for ideas on what can be adjusted:

```
# Customize the name of your keys, the chain-id, moniker of the node, keyring backend, and more
KEYS[0]="dev0"
KEYS[1]="dev1"
KEYS[2]="dev2"
CHAINID="tsp_9000-1"
MONIKER="localtestnet"
# Remember to change to other types of keyring like 'file' in-case exposing to outside world,
# otherwise your balance will be wiped quickly
# The keyring test does not require private key to steal tokens from you
KEYRING="test"
KEYALGO="eth_secp256k1"
LOGLEVEL="info"
# Set dedicated home directory for the tspd instance
HOMEDIR="$HOME/.tmp-tspd"
# to trace evm
#TRACE="--trace"
TRACE=""

[...]

  # Adjust this set a different maximum gas limit
  jq '.consensus_params["block"]["max_gas"]="10000000"' "$GENESIS" >"$TMP_GENESIS" && mv "$TMP_GENESIS" "$GENESIS"

[...]

```

## Manual Deployment

This guide helps you create a single validator node that runs a network locally for testing and other development
related uses.

### Initialize the chain

Before actually running the node, we need to initialize the chain, and most importantly its genesis file. This is done
with the `init` subcommand:

```
$MONIKER=testing$KEY=dev0$CHAINID="tsp_9000-4"# The argument $MONIKER is the custom username of your node, it should be human-readable.tspd init $MONIKER --chain-id=$CHAINID
```

tip

You can [edit](/docs/protocol/tsp-cli/configuration#client-configuration) this `moniker` later by updating the `config.toml` file.

The command above creates all the configuration files needed for your node and validator to run, as well as a default
genesis file, which defines the initial state of the network. All these [configuration files](/docs/protocol/tsp-cli/configuration#client-configuration) are in `~/.tspd` by default, but you can overwrite the
location of this folder by passing the `--home` flag.

### Genesis Procedure

### Adding Genesis Accounts

Before starting the chain, you need to populate the state with at least one account using the [keyring](/docs/protocol/concepts/keyring#add-keys):

```
tspd keys add my_validator
```

Once you have created a local account, go ahead and grant it some `tsp` tokens in your chain's genesis file. Doing
so will also make sure your chain is aware of this account's existence:

```
tspd add-genesis-account my_validator 10000000000tsp
```

Now that your account has some tokens, you need to add a validator to your chain.

For this guide, you will add your local node (created via the `init` command above) as a validator of your chain.
Validators can be declared before a chain is first started via a special transaction included in the genesis
file called a `gentx`:

```
# Create a gentx# NOTE: this command lets you set the number of coins.# Make sure this account has some coins with the genesis.app_state.staking.params.bond_denom denomtspd add-genesis-account my_validator 1000000000stake,10000000000tsp
```

A `gentx` does three things:

1. Registers the `validator` account you created as a validator operator account (i.e. the account that controls the validator).
2. Self-delegates the provided `amount` of staking tokens.
3. Link the operator account with a Tendermint node pubkey that will be used for signing blocks. If no `--pubkey` flag
  is provided, it defaults to the local node pubkey created via the `tspd init` command above.

For more information on `gentx`, use the following command:

```
tspd gentx --help
```

### Collecting `gentx`

By default, the genesis file do not contain any `gentxs`. A `gentx` is a transaction that bonds
staking token present in the genesis file under `accounts` to a validator, essentially creating a
validator at genesis. The chain will start as soon as more than 2/3rds of the validators (weighted
by voting power) that are the recipient of a valid `gentx` come online after `genesis_time`.

A `gentx` can be added manually to the genesis file, or via the following command:

```
# Add the gentx to the genesis filetspd collect-gentxs
```

This command will add all the `gentxs` stored in `~/.tspd/config/gentx` to the genesis file.

### Run Single Node

Finally, check the correctness of the `genesis.json` file:

```
tspd validate-genesis
```

Now that everything is set up, you can finally start your node:

```
tspd start
```

tip

To check all the available customizable options when running the node, use the `--help` flag.

You should see blocks come in.

The previous command allow you to run a single node. This is enough for the next section on interacting with this node,
but you may wish to run multiple nodes at the same time, and see how consensus happens between them.

You can then stop the node using `Ctrl+C`.

## Further Configuration

### Key Management

To run a node with the same key every time: replace `tspd keys add $KEY` in `./local_node.sh` with:

```
echo "your mnemonic here" | tspd keys add $KEY --recover
```
{{< callout type="info" >}}
TSP currently only supports 24 word mnemonics.
{{< /callout >}}

You can generate a new key/mnemonic with:

```
tspd keys add $KEY
```

To export your TSP key as an Ethereum private key (for use with [Metamask](https://academy.evmos.org/articles/beginner/connect-your-wallet/metamask) for example):

```
tspd keys unsafe-export-eth-key $KEY
```
{{< callout type="info" >}}
For more about the available key commands, use the `--help` flag
{{< /callout >}}

```
tspd keys -h
```

### Keyring backend options

The instructions above include commands to use `test` as the `keyring-backend`. This is an unsecured keyring that doesn't require entering a password and should not be used in production. Otherwise, TSP supports using a file or OS keyring backend for key storage. To create and use a file
stored key instead of defaulting to the OS keyring, add the flag --keyring-backend file` to any relevant command and the password prompt will occur through the command line. This can also be saved as a CLI config option with:

```
tspd config keyring-backend file
```

{{< callout type="info" >}}
For more information about the Keyring and its backend options, click [here](/docs/protocol/concepts/keyring).
{{< /callout >}}

### Enable Tracing

To enable tracing when running the node, modify the last line of the `local_node.sh` script to be the following command,
where:

- `$TRACER` is the EVM tracer type to collect execution traces from the EVM transaction execution (eg. `json|struct|access_list|markdown`)
- `$TRACESTORE` is the output file which contains KVStore tracing (eg. `store.txt`)

```
tspd start --evm.tracer $TRACER --tracestore $TRACESTORE --pruning=nothing $TRACE --log_level $LOGLEVEL --minimum-gas-prices=0.0001tsp --json-rpc.api eth,txpool,personal,net,debug,web3
```

## Clearing data from chain

### Reset Data
1
Alternatively, you can **reset** the blockchain database, remove the node's address book files, and reset the `priv_validator.json` to the genesis state.

{{< callout type="warning" >}}
If you are running a **validator node**, always be careful when doing `tspd unsafe-reset-all`. You should never use this command if you are not switching `chain-id`.
{{< /callout >}}

{{< callout type="warning" >}}
**IMPORTANT**: Make sure that every node has a unique `priv_validator.json`. **Do not** copy the `priv_validator.json` from an old node to multiple new nodes. Running two nodes with the same `priv_validator.json` will cause you to double sign!
{{< /callout >}}

First, remove the outdated files and reset the data.

```
rm $HOME/.tspd/config/addrbook.json $HOME/.tspd/config/genesis.jsontspd tendermint unsafe-reset-all --home $HOME/.tspd
```

Your node is now in a pristine state while keeping the original `priv_validator.json` and `config.toml`. If you had any sentry nodes or full nodes setup before, your node will still try to connect to them, but may fail if they haven't also been upgraded.

### Delete Data

Data for the tspd binary should be stored at ~/.tspd, respectively by default. To **delete** the existing binaries and configuration, run:

```
rm -rf ~/.tspd
```

To clear all data except key storage (if keyring backend chosen) and then you can rerun the full node installation commands from above to start the node again.

## Recording Transactions Per Second (TPS)

In order to get a progressive value of the transactions per second, we use Prometheus to return the values. The Prometheus exporter runs at address [http://localhost:8877](http://localhost:8877) so please add this section to your [Prometheus installation](https://opencensus.io/codelabs/prometheus/#1) config.yaml file like this

```
global:
scrape_interval: 10s

external_labels:
  monitor: 'tsp'

scrape_configs:
- job_name: 'tsp'

  scrape_interval: 10s

  static_configs:
    - targets: ['localhost:8877']
```

and then run Prometheus like this

```
prometheus --config.file=prom_config.yaml
```

and then visit the Prometheus dashboard at [http://localhost:9090/](http://localhost:9090/) then navigate to the expression area and enter the
following expression

```
rate(tspd_transactions_processed[1m])
```

which will show the rate of transactions processed.

{{< callout type="info" >}}
TSP currently only supports 24 word mnemonics.
{{< /callout >}}