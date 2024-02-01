---
title: Chain IDs
weight: 2
draft: true
---

A chain ID is a unique identifier that represents a blockchain network. We use it to distinguish different blockchain
networks from each other and to ensure that transactions and messages are sent to the correct network. Evmos network
follows the format of `identifier_EIP155-version` format.

## Official Chain IDs

tip

**NOTE**:
 The latest Chain ID (i.e highest Version Number) is the latest version 
of the software and mainnet. Also note, that the following upgrades 
technically did not require a Chain ID change:

- `evmos_9001-1` -> `evmos_9001-2`
- `evmos_9000-3` -> `evmos_9000-4`

### Mainnet

| Name | Chain ID | Identifier | EIP155 Number | Version Number | Active |
| --- | --- | --- | --- | --- | --- |
| Evmos 2 | evmos_9001-2 | `evmos` | 9001 | 2   | ✅   |
| Evmos 1 | evmos_9001-1 | `evmos` | 9001 | `1` | 🚫  |

### Testnet

| Name | Chain ID | Identifier | EIP155 Number | Version Number | Active |
| --- | --- | --- | --- | --- | --- |
| Evmos Public Testnet | evmos_9000-4 | `evmos` | 9000 | 4   | ✅   |
| Evmos Public Testnet | evmos_9000-3 | `evmos` | 9000 | `3` | 🚫  |
| Olympus Mons Incentivized Testnet | evmos_9000-2 | `evmos` | 9000 | `2` | 🚫  |
| Arsia Mons Testnet | evmos_9000-1 | `evmos` | 9000 | `1` | 🚫  |

tip

You can also lookup the [EIP155](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-155.md) `Chain ID` by referring
to [chainlist.org](https://chainlist.org/).

![chainlistorg website](https://docs.evmos.org/assets/images/chainlist-44a2843bda9a28dd4d59686219e85272.png)

## The Chain Identifier

Every chain must have a unique identifier or `chain-id`. Tendermint requires each application to
define its own `chain-id` in the [genesis.json fields](https://docs.tendermint.com/master/spec/core/genesis.html#genesis-fields).
However, in order to comply with both EIP155 and Cosmos standard for chain upgrades, Evmos-compatible chains must implement
a special structure for their chain identifiers.

## Structure

The Evmos Chain ID contains 3 main components

- **Identifier**: Unstructured string that defines the name of the application.
- **EIP155 Number**: Immutable [EIP155](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-155.md) `CHAIN_ID` that
  defines the replay attack protection number.
- **Version Number**: Is the version number (always positive) that the chain is currently running.
  This number **MUST** be incremented every time the chain is upgraded or forked in order to avoid network or consensus errors.

### Format

The format for specifying and Evmos compatible chain-id in genesis is the following:

```
{identifier}_{EIP155}-{version}
```

The following table provides an example where the second row corresponds to an upgrade from the first one:

| ChainID | Identifier | EIP155 Number | Version Number |
| --- | --- | --- | --- |
| `evmos_9000-1` | evmos | 9000 | 1   |
| `evmos_9000-2` | evmos | 9000 | 2   |
| `...` | ... | ... | ... |
| `evmos_9000-N` | evmos | 9000 | N   |