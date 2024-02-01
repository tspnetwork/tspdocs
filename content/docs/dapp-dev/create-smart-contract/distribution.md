---
title:  Distribution
linkTitle: Distribution
weight: 4
---

## Solidity Interface & ABI

`Distribution.sol` is an interface through which Solidity contracts can interact with Cosmos SDK distribution.
This is convenient for developers as they don’t need to know the implementation details behind
the `x/distribution` module in the Cosmos SDK. Instead,
they can interact with distribution functions using the Ethereum interface they are familiar with.

### Interface `Distribution.sol`

Find the [Solidity interface in the `tspchain/extensions` repo](https://github.com/tspnetwork/tspchain/extensions/blob/main/precompiles/stateful/Distribution.sol).

### ABI
`
Find the [ABI in the `tspchain/extensions` repo](https://github.com/tspnetwork/tspchain/extensions/blob/main/precompiles/abi/distribution.json).

## Transactions

- `setWithdrawAddress`
  
  ```
    /// @dev Withdraw the rewards of a delegator from a validator
    /// @param delegatorAddress The address of the delegator
    /// @param validatorAddress The address of the validator
    /// @return amount The amount of Coin withdrawn
    function withdrawDelegatorRewards(
        address delegatorAddress,
        string memory validatorAddress
    )
    external
    returns (
        Coin[] calldata amount
    );
  ```
  
- `withdrawDelegatorRewards`
  
  ```
    /// @dev Withdraw the rewards of a delegator from a validator
    /// @param delegatorAddress The address of the delegator
    /// @param validatorAddress The address of the validator
    /// @return amount The amount of Coin withdrawn
    function withdrawDelegatorRewards(
        address delegatorAddress,
        string memory validatorAddress
    )
    external
    returns (
        Coin[] calldata amount
    );
  ```
  
- `withdrawValidatorCommission`
  
  ```
    /// @dev Withdraws the rewards commission of a validator.
    /// @param validatorAddress The address of the validator
    /// @return amount The amount of Coin withdrawn
    function withdrawValidatorCommission(
        string memory validatorAddress
    )
    external
    returns (
        Coin[] calldata amount
    );
  ```
  

## Queries

- `validatorDistribution`
  
  ```
    /// @dev Queries validator commission and self-delegation rewards for validator.
    /// @param validatorAddress The address of the validator
    /// @return distributionInfo The validator's distribution info
    function validatorDistributionInfo(
        string memory validatorAddress
    )
    external
    view
    returns (
        ValidatorDistributionInfo calldata distributionInfo
    );
  ```
  
- `validatorOutstandingRewards`
  
  ```
    /// @dev Queries the outstanding rewards of a validator address.
    /// @param validatorAddress The address of the validator
    /// @return rewards The validator's outstanding rewards
    function validatorOutstandingRewards(
        string memory validatorAddress
    )
    external
    view
    returns (
        DecCoin[] calldata rewards
    );
  ```
  
- `validatorCommission`
  
  ```
    /// @dev Queries the accumulated commission for a validator.
    /// @param validatorAddress The address of the validator
    /// @return commission The validator's commission
    function validatorCommission(
        string memory validatorAddress
    )
    external
    view
    returns (
        DecCoin[] calldata commission
    );
  ```
  

`validatorSlashes`

```
    /// @dev Queries the slashing events for a validator in a given height interval
    /// defined by the starting and ending height.
    /// @param validatorAddress The address of the validator
    /// @param startingHeight The starting height
    /// @param endingHeight The ending height
    /// @return slashes The validator's slash events
    /// @return pageResponse The pagination response for the query
    function validatorSlashes(
        string memory validatorAddress,
        uint64 startingHeight,
        uint64 endingHeight
    )
    external
    view
    returns (
        ValidatorSlashEvent[] calldata slashes,
        PageResponse calldata pageResponse
    );
```

`delegationRewards`

```
/// @dev Queries the total rewards accrued by a delegation from a specific address to a given validator./// @param delegatorAddress The address of the delegator/// @param validatorAddress The address of the validator/// @return rewards The total rewards accrued by a delegation.function delegationRewards(    address delegatorAddress,    string memory validatorAddress)externalviewreturns (    DecCoin[] calldata rewards);
```

`delegationTotalRewards`

```
    /// @dev Queries the total rewards accrued by a delegation from a specific address to a given validator.
    /// @param delegatorAddress The address of the delegator
    /// @param validatorAddress The address of the validator
    /// @return rewards The total rewards accrued by a delegation.
    function delegationRewards(
        address delegatorAddress,
        string memory validatorAddress
    )
    external
    view
    returns (
        DecCoin[] calldata rewards
    );
```

`delegatorValidators`

```
    /// @dev Queries all validators, that a given address has delegated to.
    /// @param delegatorAddress The address of the delegator
    /// @return validators The addresses of all validators, that were delegated to by the given address.
    function delegatorValidators(
        address delegatorAddress
    ) external view returns (string[] calldata validators);
```

`delegatorWithdrawAddress`

`delegatorWithdrawAddress` queries withdraw address of a delegator

```
    /// @dev Queries the address capable of withdrawing rewards for a given delegator.
    /// @param delegatorAddress The address of the delegator
    /// @return withdrawAddress The address capable of withdrawing rewards for the delegator.
    function delegatorWithdrawAddress(
        address delegatorAddress
    ) external view returns (string memory withdrawAddress);
```

## Events

Each of the transactions emits its corresponding event. These are:

- `SetWithdrawerAddress`
  
  ```
    /// @dev SetWithdrawerAddress defines an Event emitted when a new withdrawer address is being set
    /// @param caller the caller of the transaction
    /// @param withdrawerAddress the newly set withdrawer address
    event SetWithdrawerAddress(
        address indexed caller,
        string withdrawerAddress
    );
  ```
  

`WithdrawDelegatorRewards`

```
    /// @dev WithdrawDelegatorRewards defines an Event emitted when rewards from a delegation are withdrawn
    /// @param delegatorAddress the address of the delegator
    /// @param validatorAddress the address of the validator
    /// @param amount the amount being withdrawn from the delegation
    event WithdrawDelegatorRewards(
        address indexed delegatorAddress,
        string indexed validatorAddress,
        uint256 amount
    );
```

`WithdrawValidatorCommission`

```
    /// @dev WithdrawValidatorCommission defines an Event emitted when validator commissions are being withdrawn
    /// @param validatorAddress is the address of the validator
    /// @param commission is the total commission earned by the validator
    event WithdrawValidatorCommission(
        string indexed validatorAddress,
        uint256 commission
    );
```

## Interact with the Solidity Interface

Below are some examples of how to interact with this Solidity interface from your smart contracts.

Make sure to import the precompiled interface, e.g.:

```
import "https://github.com/tspnetwork/tspchain/extensions/blob/main/precompiles/stateful/Distribution.sol";
```

### Set withdraw address

The `changeWithdrawAddress` function allows a user to set a new withdraw address in the Cosmos `x/distribution` module.
For this transaction to be successful, make sure the user had already approved the `MSG_SET_WITHDRAWER_ADDRESS` message.

```
    function changeWithdrawAddress(
        string memory _withdrawAddr
    ) public returns (bool) {
        return
            distribution.DISTRIBUTION_CONTRACT.setWithdrawAddress(
                msg.sender,
                _withdrawAddr
            );
    }
```

### Withdraw staking rewards

The `withdrawStakingRewards` function allows a user to withdraw his/her rewards corresponding to a specified validator.
For this transaction to be successful, make sure the user had already approved the `MSG_WITHDRAW_DELEGATOR_REWARD` message.

```
    function withdrawStakingRewards(
        string memory _valAddr
    ) public returns (types.Coin[] memory) {
        return
            distribution.DISTRIBUTION_CONTRACT.withdrawDelegatorRewards(
                msg.sender,
                _valAddr
            );
    }
```

### Withdraw validator commission

If the user is running a validator, he/she could withdraw the corresponding commission using a smart contract.
The user could use a function similar to `withdrawCommission`.
For this transaction to be successful,
make sure the user had already approved the `MSG_WITHDRAW_VALIDATOR_COMMISSION` message.

```
function withdrawCommission(
    string memory _valAddr
) public returns (types.Coin[] memory) {
    return
        distribution.DISTRIBUTION_CONTRACT.withdrawValidatorCommission(
            _valAddr
        );
}
```

### Queries

Similarly to transactions, smart contracts can use query methods. These are read-only methods.
Examples of this are the `getDelegationRewards` and `getValidatorCommision` functions that return the information for the specified validator address.

```
getDelegationRewards(
    string memory _valAddr
) public view returns (types.DecCoin[] memory) {
    return
        distribution.DISTRIBUTION_CONTRACT.delegationRewards(
            msg.sender,
            _valAddr
        );
}

function getValidatorCommission(
    string memory _valAddr
) public view returns (types.DecCoin[] memory) {
    return distribution.DISTRIBUTION_CONTRACT.validatorCommission(_valAddr);
}
```