---
title: Drafting a Proposal
linkTitle: Drafting a Proposal
weight: 7
---

The `draft-proposal` command in the TSP CLI is part of the Cosmos-SDK
governance module and is used to generate a draft proposal JSON file.
This generated proposal JSON file contains a skeleton structure for a governance proposal.

## Command Syntax

```
tspd tx gov draft-proposal [flags]
```

## Usage

To create a draft proposal using the `tspd tx gov draft-proposal` command, follow these steps:

1. Run the command
  
  ```
  tspd tx gov draft-proposal
  ```
  

The command will present a list of proposal types for selection.
The available options typically include:

```
Use the arrow keys to navigate: ↓ ↑ → ←  ? Select proposal type:      text     community-pool-spend     software-upgrade     cancel-software-upgrade   ▸ other
```

In case you don't find the required proposal (e.g. update params),
choose the `other` option. It will show an extensive list of the supported messages:

```
✔ otherUse the arrow keys to navigate: ↓ ↑ → ← ? Select proposal message type:: ↑   /tsp.erc20.v1.MsgUpdateParams  ▸ /tsp.incentives.v1.MsgUpdateParams    /tsp.inflation.v1.MsgUpdateParams    /tsp.recovery.v1.MsgUpdateParams↓   /tsp.revenue.v1.MsgCancelRevenue
```

- Follow the on-screen instructions to complete the process.
  The command will generate a JSON file that you can use for your proposal.
  
- Once the JSON file is generated,
  you can make any necessary changes to the proposal information within the file.
  
- Finally, use the generated JSON file as input when submitting
  your proposal using the `tspd tx gov submit-proposal` command.
  
  ```
  tspd tx gov submit-proposal draft_proposal.json [flags]
  ```