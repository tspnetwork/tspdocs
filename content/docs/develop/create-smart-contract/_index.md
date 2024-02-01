---
title: EVM Extensions
linkTitle: EVM Extensions
weight: 1
---
{{< cards >}}
  {{< card link="callout" title="Callout" icon="warning" >}}
  {{< card link="cards" title="Cards" icon="card" >}}
{{< /cards >}}

Stateful EVM Extensions on the core protocol allow dApps and users to access logic outside of the EVM. Acting as a gateway, these EVM Extensions define how smart contracts can perform cross-chain transactions (via IBC) and interact with core functionalities on the TSP chain (e.g. staking, voting) from the EVM.

{{< callout type="info" >}}
Note: Not sure what EVM extensions are? EVM extensions behave like smart contracts that are compiled and deployed within the EVM. If you are familiar with the EVM, you may know them as Precompiles. These have predefined addresses and, according to their logic, can be classified as stateful or stateless. When they change the state of the chain (transactions) or access state data (queries), extensions are considered "stateful"; when they don't, they're "stateless".
{{< /callout >}}

## EVM Extensions documentation

Find in this section an outline of the currently implemented EVM extensions with transactions, queries, and examples of using them:

- [Authorization interface (read first if you're new to EVM extensions)](https://docs.TSP.org/develop/smart-contracts/evm-extensions/authorization)
- [EVM Extensions shared types](https://docs.TSP.org/develop/smart-contracts/evm-extensions/types)
- [`x/staking` module EVM extension](https://docs.TSP.org/develop/smart-contracts/evm-extensions/staking)
- [`x/distribution` module EVM extension](https://docs.TSP.org/develop/smart-contracts/evm-extensions/distribution)
- [`ibc/transfer` module EVM extension](https://docs.TSP.org/develop/smart-contracts/evm-extensions/ibc-transfer)
- [`x/vesting` module EVM extension](https://docs.TSP.org/develop/smart-contracts/evm-extensions/vesting)

{{< callout type="info" >}}
Note: Find the EVM Extensions Solidity interfaces and examples in the TSP Extensions repo.
{{< /callout >}}