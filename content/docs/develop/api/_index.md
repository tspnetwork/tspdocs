---
title: API
weight: 9
draft: false
---

The following API's are recommended for development purposes. For maximum control and reliability it's recommended to run your own node.

## Networks

Quickly connect your app or client to TSP mainnet and public testnets. Head over to [Networks](/docs/develop/api/networks) to find a list of publicly available endpoints that you can use to connect to the TSP.

## Clients

The TSP Network supports different clients in order to support Cosmos and  Ethereum transactions and queries. You can use Swagger as a REST interface for state queries and transactions:

|     | Description | Default Port | Swagger |
| --- | --- | --- | --- |
| **Cosmos [gRPC](/docs/develop/api/cosmos-grpc#cosmos-grpc)** | Query or send Evmos transactions using gRPC | `9090` |     |
| **Cosmos REST ([gRPC-Gateway](/docs/develop/api/cosmos-grpc#cosmos-http-rest-grpc-gateway))** | Query or send TSP transactions using an HTTP RESTful API | `9091` | [Testnet](https://api.tsp.network/) [Mainnet](https://api.tsp.network/) |
| **Ethereum [JSON-RPC](/docs/develop/api/ethereum-json-rpc)** | Query Ethereum-formatted transactions and blocks or send Ethereum txs using JSON-RPC | `8545` |     |
| **Ethereum [Websocket](/docs/develop/api/ethereum-json-rpc#ethereum-websocket)** | Subscribe to Ethereum logs and events emitted in smart contracts. | `8586` |     |
| **Tendermint [RPC](/docs/develop/api#tendermint-rpc)** | Query transactions, blocks, consensus state, broadcast transactions, etc. | `26657` | [Localhost](https://docs.tendermint.com/v0.34/rpc/) |
| **Tendermint [Websocket](/docs/develop/api#tendermint-websocket)** | Subscribe to Tendermint ABCI events | `26657` |     |
| **Command Line Interface ([CLI](/docs/protocol/tsp-cli))** | Query or send TSP transactions using your Terminal or Console. | N/A |     |