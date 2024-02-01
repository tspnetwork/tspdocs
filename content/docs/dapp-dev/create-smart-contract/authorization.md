---
title: Authorization
linkTitle: Authorization
weight: 1
---
The user should grant authorization to allow smart contracts
to send messages on behalf of a user account.
This is achieved by the `Authorization.sol` that provides the necessary functions to grant approvals and allowances.
The precompiled contracts use the `AuthorizationI` interface,
to allow users to approve the corresponding messages and amounts.

## Solidity Interfaces

### `Authorization.sol`

Find the [Solidity interface in the `tspchain/extensions` repo](https://github.com/tspnetwork/tspchain/extensions/blob/main/precompiles/common/Authorization.sol).

## Transactions

- `approve`
  
  Approves a list of Cosmos or IBC transactions with a specific amount of tokens
  
  ```
  function approve(
          address spender,
          uint256 amount,
          string[] calldata methods
      ) external returns (bool approved);
  ```

- `revoke`
  
  Revokes authorizations of Cosmos transactions.
  
  ```
  function revoke(
      address spender,
      string[] calldata methods
  ) external returns (bool revoked);
  ```

- `increaseAllowance`
  
  Increase the allowance of a given spender by a specific amount of tokens for IBC transfer methods or staking
  
  ```
  function increaseAllowance(
          address spender,
          uint256 amount,
          string[] calldata methods
      ) external returns (bool approved);
  ```

- `decreaseAllowance`
  
  Decreases the allowance of a given spender by a specific amount of tokens for IBC transfer methods or staking
  
  ```
  function decreaseAllowance(
          address spender,
          uint256 amount,
          string[] calldata methods
      ) external returns (bool approved);
  ```

## Queries

- `allowance`
  
  Returns the remaining number of tokens that the spender will be allowed to
  spend on behalf of the owner through IBC transfer methods or staking.
  This is zero by default
  
  ```
  function allowance(
          address owner,
          address spender,
          string calldata method
  ) external view returns (uint256 remaining);
  ```

## Events

- `Approval`
  
  This event is emitted when the allowance of a spender is set by a call to the `approve` method.
  The `value` field specifies the new allowance and the `methods` field holds the information for which methods the approval was set.
  
  ```
  event Approval(
          address indexed owner,
          address indexed spender,
          string[] methods,
          uint256 value
      );
  ```

- `Revocation`
  
  This event is emitted when an owner revokes a spender's allowance.
  
  ```
  event Revocation(
      address indexed owner,
      address indexed spender,
      string[] methods
  );
  ```

`AllowanceChange`

This event is emitted when the allowance of a spender is changed by a call to the decrease or increase allowance method.
The `values` field specifies the new allowances and the `methods` field holds the information for which methods the approval was set.

```
event AllowanceChange(
        address indexed owner,
        address indexed spender,
        string[] methods,
        uint256[] values
    );
```
