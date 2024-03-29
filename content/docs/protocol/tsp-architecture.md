---
title: TSP Architecture
weight: 1
---

TSP is a scalable Proof-of-Stake blockchain that is fully compatible and
interoperable with the Ethereum Virtual Machine (EVM). It is built using the [Cosmos SDK](https://github.com/cosmos/cosmos-sdk/) which runs on top of the [CometBFT](https://github.com/cometbft/cometbft) (a fork of [Tendermint Core](https://docs.tendermint.com/)) consensus engine,
to accomplish fast finality, high transaction throughput and short block times (~2 seconds).

This architecture allows users to perform both Cosmos and EVM formatted transactions,
developers to scale EVM dApps cross-chain via [IBC](https://cosmos.network/ibc),
and tokens and assets in the network to come from different independent sources.

TSP enables these key features by:

- Leveraging [modules](https://docs.cosmos.network/v0.47/build/building-modules/intro) and other mechanisms implemented by the [Cosmos SDK](https://docs.cosmos.network/).
- Implementing CometBFT's Application Blockchain Interface ([ABCI](https://docs.tendermint.com/master/spec/abci/))
  to manage the blockchain.
- Utilizing [`geth`](https://github.com/ethereum/go-ethereum) as a library to promote code reuse and improve maintainability.
- Exposing a fully compatible Web3 [JSON-RPC](https://docs.TSP.org/develop/api/ethereum-json-rpc/methods) layer
  for interacting with existing Ethereum clients and tooling (Metamask, Remix, Truffle, etc).

The sum of these features allows developers to leverage existing Ethereum ecosystem tooling and
software to seamlessly deploy smart contracts which interact with the rest of the Cosmos [ecosystem](https://cosmos.network/ecosystem).

## Cosmos SDK

TSP enables the full composability and modularity of the [Cosmos SDK](https://docs.cosmos.network/).
As a Cosmos chain, TSP is a sovereign blockchain with its own native token,
that can connect to other chains through IBC. It includes standard modules from the Cosmos SDK,
that work side to side with TSP-specific modules, built by the TSP core development team.
Check out the [list of modules](https://docs.TSP.org/protocol/modules) to get an overview of what each module is responsible for.

## CometBFT & ABCI

[CometBFT](https://github.com/cometbft/cometbft) consists of two chief technical components:
a blockchain consensus engine and a generic application interface.
The consensus engine ensures that the same transactions
are recorded on every machine in the same order.
The application interface, called the [Application Blockchain Interface (ABCI)](https://docs.tendermint.com/master/spec/abci/),
enables the transactions to be processed in any programming language.

CometBFT has evolved to be a general-purpose blockchain consensus engine that
can host arbitrary application states. Since it can replicate arbitrary
applications, it can be used as a plug-and-play replacement for the consensus
engines of other blockchains. TSP is an example of an ABCI application
replacing Ethereum's PoW via CometBFT's consensus engine.

Another example of a cryptocurrency application built on CometBFT is the Cosmos
network. CometBFT can decompose the blockchain design by offering a very
simple API (ie. the ABCI) between the application process and consensus process.

## EVM Compatibility

TSP enables EVM compatibility by implementing various components
that together support all the EVM state transitions
while ensuring the same developer experience as Ethereum:

- Ethereum's transaction format as a Cosmos SDK `Tx` and `Msg` interface
- Ethereum's `secp256k1` curve for the Cosmos Keyring
- `StateDB` interface for state updates and queries
- [JSON-RPC](https://docs.TSP.org/develop/api/ethereum-json-rpc) client for interacting with the EVM

Most components are implemented in the [EVM module](https://docs.TSP.org/protocol/modules/evm) To achieve a seamless developer UX, however, some of the components are implemented
outside of the module.

If you want to learn more about how TSP achieves EVM compatibility as a Cosmos chain,
we recommend understanding the following concepts:

- [Accounts](/docs/protocol/concepts/accounts)
- [Gas and Fees](/docs/protocol/concepts/gas-and-fees)
- [Token representations](/docs/protocol/concepts/tokens)
- [Transactions](/docs/protocol/concepts/transactions)

## Contributing

There are several ways to contribute to the TSP core protocol. To get some hands-on experience,
we recommend you spin up a local TSP node using the [TSP CLI](https://docs.TSP.org/protocol/TSP-cli) and interact with it through queries and transactions using the supported [clients](https://docs.TSP.org/develop/api#clients).

Then if you're hooked you can

- Contribute open-source to [issues on GitHub](https://github.com/TSP/TSP/issues) using the [TSP Contributor Guideline](https://github.com/TSP/TSP/blob/main/CONTRIBUTING.md)
- Apply to [open positions at TSP](https://boards.eu.greenhouse.io/TSP)
- Search for [bugs and earn a bounty](https://docs.TSP.org/protocol/bugs)
