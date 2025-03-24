# Foundry

Install OraichainEVM by following the full instructions provided in the documentation.

### Create Foundry Project

Create a directory for your contracts and initialize it:

```bash
forge init oraichain-evm-foundry
cd oraichain-evm-foundry
```

For now, let’s check what the default template looks like:

```
tree . -d -L 1
.
├── lib
├── script
├── src
└── test

5 directories
```

Open `src/Counter.sol` with the following contract:

````solidity
```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

contract Counter {
    uint256 public number;

    function setNumber(uint256 newNumber) public {
        number = newNumber;
    }

    function increment() public {
        number++;
    }
}

```
````

Compile the contract:

```bash
forge build
```

### Anvil

Anvil is a fast local Ethereum development node.

Anvil is part of the Foundry suite and is installed alongside `forge`, `cast` and `chisel`. If you haven’t installed Foundry yet, see [Foundry installation](https://book.getfoundry.sh/getting-started/installation.html).

&#x20;To fork against a live OraichainEVM network run:

```
anvil --fork-url <RPC_URL>
```

### Deploy Contract

Migrate your contract in the foundry terminal:

```bash
forge create src/Counter.sol:Counter --rpc-url <RPC_URL> --private-key <PRIVATE_KEY> --broadcast
```

You should see deployment logs in the OraichainEVM terminal for each transaction.

### Run Foundry Tests

Run the tests using the OraichainEVM node:

```bash
forge test test/Counter.t.sol --rpc-url <RPC_URL>
```

This will confirm the contract's functionality.

### Run Foundry Script

To execute a script on OraichainEVM, run:

```bash
forge script script/Counter.s.sol --rpc-url <RPC_URL>
```

earched Pre-requisite reading installation run a node search.

For suggestions, contribute on GitHub!
