---
title: Tokens
weight: 12
draft: false
---

## The TSP Token

The denomination used for staking, governance and gas consumption on the EVM is the TSP. The TSP provides the utility
of: securing the Proof-of-Stake chain, token used for governance proposals, distribution of fees to validator and users,
and as a mean of gas for running smart contracts on the EVM.

TSP uses [Atto](https://en.wikipedia.org/wiki/Atto-) TSP as the base denomination to maintain parity with Ethereum.
There are three types of assets:

- The native TSP token
- IBC Coins (via the IBC)
- Ethereum-typed tokens, e.g. ERC-20

`1 tsp = 10<sup>18</sup> tsp`

This matches Ethereum denomination of:

`1 ETH = 10<sup>18</sup> wei`

## Cosmos Coins

Accounts can own Cosmos coins in their balance, which are used for operations with other Cosmos and transactions. Examples
of these are using the coins for staking, IBC transfers, governance deposits and EVM.

## EVM Tokens

TSP is compatible with ERC20 tokens and other non-fungible token standards (EIP721, EIP1155) that are natively supported by the EVM.

## TSP Assets Page

Check out how we represent ERC-20 tokens and Cosmos IBC Coins through our _Single Token Representation_ feature on the TSP Dashboard. TSP enabled this feature to help with the user experiences by obfuscating the assets types away from the users and allow them to focus on the interaction. The protocol simplifies the process by handling the conversion and users are given the simplification of denomination and the amount of assets they hold. Tokens listed still require governance and registering the tokens ([Cosmos](https://academy.evmos.org/articles/advanced/cosmos-coin-registration) or [ERC-20](https://academy.evmos.org/articles/advanced/erc20-registration)).

For more information on how we handle token registration, head over [here](/docs/develop/mainnet#token-registration).