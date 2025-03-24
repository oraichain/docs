# Wasm

Oraichain not only provides Ethereum Virtual Machine but also has CosmWasm for developing dApps. With the Wasm precompiled contract, the EVM contract can now interact with the CosmWasm smart contract.

## Solidity Interfaces

The Wasm solidity interfaces include the following transactions:

* instantiate

Instantiate method allows caller to instantiate a wasm contract

```
function instantiate(
        uint64 codeID,
        string memory admin,
        bytes memory payload,
        string memory label,
        bytes memory coins
    ) external returns (string memory contractAddr, bytes memory data);
```

* execute

Execute method allows caller to execute a wasm contract

```
function execute(
        string memory contractAddress,
        bytes memory payload,
        bytes memory coins
    ) external returns (bytes memory response);
```

* query

Query method allows caller to query wasm smart contract state

```
function query(
        string memory contractAddress,
        bytes memory req
    ) external view returns (bytes memory response);
```
