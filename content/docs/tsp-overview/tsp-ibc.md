---
title: TSP Inter-Blockchain Communication
weight: 4
---

NOTE: For an in-depth explanation on fungible ibc token transfers, please reference

This page gives examples and an overview of `ibc` with `tsp`, to send assets between multiple `ibc`-compatible chains.

{{< callout type="info" >}}
NOTE: In order to send `ibc` transfers to another chain, there must be a `ibc` channel and relayers set up with that chain. To setup a relayer, reference
{{< /callout >}}

Run following in order to list out all available ibc channels

```
tspd q ibc channel channels
```
{{< callout type="info" >}}
NOTE: Beforehand, follow instructions to install the `seid` command line tool from the `tspchain` repo.
{{< /callout >}}

## IBC Transfers

First verify token balances:

```
tspd q bank balances $ACCOUNT_ADDRESS --chain-id=$CHAIN_ID
```

Submit a ibc token transfer:

```
tspd tx ibc-transfer transfer $SRC_PORT $SRC_CHANNEL $RECEIVER_ADDRESS $AMOUNT --chain-id=$CHAIN_ID --from=$ACCOUNT --fees=$FEE -y
```

{{< callout type="info" >}}
NOTE: the `tspd tx ibc-transfer transfer --help` contains useful information about the `cli` tool for submitting ibc transfers
{{< /callout >}}

Example: Sending 1000tsp from `atlantic-2` (sei incentivized testnet) to `coolweb` chain using the `transfer` source port, `channel-135` source channel

```
tspd tx ibc-transfer transfer transfer channel-135 $COOLWEB_WALLET 1000tsp --chain-id=atlantic-2 --from=$ACCOUNT --fees=300tsp -y
```


