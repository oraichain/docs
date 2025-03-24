# Bank

[Bank ](https://github.com/oraidex/evm-entry-point/blob/main/packages/contracts/contracts/precompiles/IBank.sol)Precompiles contract is a gate-way that allowed Solidity contracts can interact with Cosmos SDK Bank module. This is convenient for developers as they don’t need to know the implementation details behind the `x/bank` module in the Cosmos SDK. Instead, they can interact with bank functions using the Ethereum interface they are familiar with.

## Solidity Interfaces

The Bank solidity interface includes the following transactions

* send

Send defined a method that perform sending logic to a specific address

```
function send(
        address toAddress,
        string memory denom,
        uint256 amount
    ) external returns (bool success);
```

* burn

Burn defined a method that perform burning amount of token from a specific address

```
function burn(
        address account,
        string memory denom,
        uint256 amount
    ) external returns (bool success);
```

* balance

Balance defined a query method that get a user balance of a specific denomination

```
function balance(
        address acc,
        string memory denom
    ) external view returns (uint256 amount);
```

* name

Get the Name that defined in Token Metadata

```
function name(
        string memory denom
    ) external view returns (string memory response);
```

* symbol

Get the Symbol that defined in Token Metadata

```
function symbol(
        string memory denom
    ) external view returns (string memory response);
```

* decimals

Get the token decimals that defined in Token Metadata

```
function decimals(
        string memory denom
    ) external view returns (uint8 response);
```

* supply

Get the total supply of the token

```
function supply(
        string memory denom
    ) external view returns (uint256 response);
```
