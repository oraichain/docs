# Hardhat

### Setting Up a Hardhat OraichainEVM Local Development Environment

Hardhat is a comprehensive Ethereum development environment that enables you to compile, deploy, test, and debug your Solidity code, also supporting EVM-compatible blockchains.

#### Install Dependencies

To get started, ensure you have Node.js installed, then initialize a new project and install Hardhat:

```shell
mkdir oraichain-evm-hardhat
cd oraichain-evm-hardhat
npm init -y
npm install --save-dev hardhat
```

#### Initialize Hardhat

Create a basic Hardhat project setup:

```
npx hardhat
```

Choose "Create a basic sample project" and proceed with the prompts.

For now, let’s check what the default template looks like:

```
tree . -d -L 1
.
├── lib
├── script
├── src
└── test
​
5 directories
```

Open `contracts/Lock.sol` with the following contract:

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.28;
​
// Uncomment this line to use console.log
// import "hardhat/console.sol";
​
contract Lock {
    uint public unlockTime;
    address payable public owner;
​
    event Withdrawal(uint amount, uint when);
​
    constructor(uint _unlockTime) payable {
        require(
            block.timestamp < _unlockTime,
            "Unlock time should be in the future"
        );
​
        unlockTime = _unlockTime;
        owner = payable(msg.sender);
    }
​
    function withdraw() public {
        // Uncomment this line, and the import of "hardhat/console.sol", to print a log in your terminal
        // console.log("Unlock time is %o and block timestamp is %o", unlockTime, block.timestamp);
​
        require(block.timestamp >= unlockTime, "You can't withdraw yet");
        require(msg.sender == owner, "You aren't the owner");
​
        emit Withdrawal(address(this).balance, block.timestamp);
​
        owner.transfer(address(this).balance);
    }
}
```

#### Configure the OraichainEVM Network

Modify `hardhat.config.js` to add the OraichainEVM network configuration:

```javascript
require("@nomiclabs/hardhat-ethers");
​
module.exports = {
  solidity: "0.8.13",
  networks: {
    oraichain: {
      url: "<RPC_URL>",
      accounts: [`0x${<PRIVATE_KEY>}`]
    }
  }
};
```

#### Compile Contracts

Compile your Solidity contracts using:

```
npx hardhat compile
```

#### Deploy Contracts

Deploy your contract with a Hardhat script. Create `scripts/deploy.js`:

```typescript
async function main() {
  const Lock = await ethers.getContractFactory("Lock");
  const lock = await Lock.deploy();
  console.log("Lock deployed to:", lock.address);
}
​
main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

Run the deployment script:

```
npx hardhat run scripts/deploy.js --network oraichain
```

#### Run Tests

Run tests using Hardhat's testing environment:

```
npx hardhat test
```

#### Execute Scripts

You can further execute custom scripts for interaction with your contracts similarly by specifying the network:

```
npx hardhat run scripts/interact.js --network oraichain
```

Hardhat provides flexibility and robust tooling for your Solidity dApp development on EVM-compatible chains like OraichainEVM.
