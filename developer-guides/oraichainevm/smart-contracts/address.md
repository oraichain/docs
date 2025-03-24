# Address

Oraichain supports two types of addresses: the EVM address (0x...) for the Ethereum Virtual Machine (EVM) side and the Bech32 address (orai...) for the Cosmos side. The [Address](https://github.com/oraidex/evm-entry-point/blob/main/packages/contracts/contracts/precompiles/IAddr.sol) precompiled contract enables users to retrieve the association between these two address formats.&#x20;

## Solidity Interfaces

* getCosmosAddr

Get the associated Cosmos Address (orai...) by providing EVM address (0x...)

```
function getCosmosAddr(
        address addr
    ) external view returns (string memory response);
```

* getEvmAddr

Get the associated EVM address (0x...) by providing Cosmos Address (orai...)

```
function getEvmAddr(
        string memory addr
    ) external view returns (address response);
```

* associate

Get the associated EVM address and Cosmos Address by message signature components

```
function associate(
        string memory v,
        string memory r,
        string memory s,
        string memory customMessage
    ) external returns (string memory cosmosAddr, address evmAddr);
```

* associatePubKey

Get the associated EVM address and Cosmos Address by pubkey

```
function associatePubKey(
        string memory pubKeyHex
    ) external returns (string memory cosmosAddr, address evmAddr);
```
