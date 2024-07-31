---
description: >-
  This section showcases how to get a random value inside a smart contract from
  different blockchain networks like BSC and Fantom, etc.
---

# Get VRF Value from different networks

![How VRF values are generated from different netw](<../../../.gitbook/assets/Screen Shot 2022-04-12 at 14.40.14.png>)

![How VRF values are generated from a different network](<../../../.gitbook/assets/image (13).png>)

VRF execution flow from Binance Smart Chain (BSC), Avalanche (AVAX) and Fantom (FTM):

VRF execution flow from BNB Chain (formerly Binance Smart Chain), Avalanche (AVAX) and Fantom (FTM):

* First of all, smart contract users must pay fees (in OraiToken with BSC network and native tokens with AVAX and FTM) and make a request to generate random numbers with VRFOracleOraichain contract
* Next, the VRFOracleOraichain contract checks the request of the user and the fee, if everything is verified, the VRFOracleOraichain will generate a corresponding reqId and return it to the user.
* A keeper bridge will listen for events on the VRFOracleOraichain smart contract and make a random generated request to Oraichain network. The random value takes the input from the seed provided by the smart contract user.
* Oraichain Mainnet uses submitted information to generate random numbers and proofs. After generating a random number and proof, the Oraichain Mainnet sends that data back to the keeper bridge.
* The keeper bridge receives the information and fullfills the VRFOracleOraichain contract, then the random number will be sent back to the user via consumer address and reqId.
* Note: Only authorized keepers can fulfill the VRFOracleOraichain smart contract. The random number is guaranteed by the signatures of Oraichain mainnet's validators.
* First of all, smart contract users must pay fees (in OraiToken with BSC network and native tokens with AVAX and FTM) and make a request to generate random numbers with VRFOracleOraichain contract
* Next, the VRFOracleOraichain contract checks the request of the user and the fee, if everything is verified, the VRFOracleOraichain will generate a corresponding reqId and return it to the user.
* A keeper bridge will listen for events on the VRFOracleOraichain smart contract and make a random generated request to Oraichain network. The random value takes the input from the seed provided by the smart contract user.
* Oraichain Mainnet uses submitted information to generate random numbers and proofs. After generating a random number and proof, the Oraichain Mainnet sends that data back to the keeper bridge.
* The keeper bridge receives the information and fullfills the VRFOracleOraichain contract, then the random number will be sent back to the user via consumer address and reqId.
* Note: Only authorized keepers can fulfill the VRFOracleOraichain smart contract. The random number is guaranteed by the signatures of Oraichain mainnet's validators.

To generate a VRF value from your blockchain of choice, your contract needs to inherit VrfOracleOraichain and define 2 required functions:

To generate a VRF value from your blockchain of choice, your contract needs to inherit VrfOracleOraichain and define 2 required functions:

1. **randomnessRequest**: initiate a request to generate random numbers
2. **fulfillRandomness**: will perform receiving and verifying data.

* :warning:Your contract needs to have enough ORAI or the needed token to make the request for a random number.
* :warning:Some of the values in the example below need to be particularly correct for each network.

## Examples for best practices

### ORAI token based VRF Oracle (BNB Chain)

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.5.16;


interface IERC20 {

    function transfer(address recipient, uint256 amount) external returns (bool);

    function approve(address spender, uint256 amount) external returns (bool);
}


interface IVRFOracleOraichain {
    function randomnessRequest(uint256 _seed, bytes calldata _data) external returns (bytes32 reqId);

    function getFee() external returns (uint256);
}

contract VRFConsumerExample {

    address public orai;
    address public oracle;
    uint256 public random;
    bytes32 public reqId;

    constructor (address _oraiToken, address _oracle) public {
        orai = _oraiToken;
        oracle = _oracle;
    }

    function randomnessRequest(uint256 _seed) public {
        IERC20(orai).approve(oracle, IVRFOracleOraichain(oracle).getFee());
        bytes memory data = abi.encode(address(this), this.fulfillRandomness.selector);
        reqId = IVRFOracleOraichain(oracle).randomnessRequest(_seed, data);
    }

    function fulfillRandomness(bytes32 _reqId, uint256 _random) external {
        require(msg.sender == oracle, "Caller must is oracle");
        random = _random;
    }

    function setOracle(address _oracle) public {
        oracle = _oracle;
    }

    function clearERC20(IERC20 token, address to, uint256 amount) external {
        token.transfer(to, amount);
    }
}

```

### Native token based VRF Oracle (Avalanche, Fantom, etc.)

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.12;


interface IVRFOracleOraichain {
    function randomnessRequest(uint256 _seed, bytes calldata _data) external payable returns (bytes32 reqId);

    function getFee() external returns (uint256);
}

contract VRFConsumerExampleNativeFee {

    address public oracle;
    uint256 public random;
    bytes32 public reqId;

    constructor (address _oracle) public payable {
        oracle = _oracle;
    }

    fallback() external payable {}

    function randomnessRequest(uint256 _seed) public {
        uint256 fee = IVRFOracleOraichain(oracle).getFee();
        bytes memory data = abi.encode(address(this), this.fulfillRandomness.selector);
        reqId = IVRFOracleOraichain(oracle).randomnessRequest.value(fee)(_seed, data);
    }

    function fulfillRandomness(bytes32 _reqId, uint256 _random) external {
        require(msg.sender == oracle, "Caller must is oracle");
        random = _random;
    }

    function clearNativeCoin(address payable _to, uint256 amount) public payable {
        _to.transfer(amount);
    }

}

```

#### Here are more tested examples for [Fantom](https://ftmscan.com/address/0x943Df3CF0796A902ab37ceaA0de2ce694339EF5f#code) and [Avalanche](https://snowtrace.io/address/0x3c58947e167b87520c2e9210847939a4b9660f4d#code) developers.

### General guideline for smart contract users&#x20;

* There are currently two payment methods for Oraichain VRF service, the first of which requires Orai token for Binance Smart Chain; the latter requires native token for Avalanche and Fantom Opera. You must therefore use your wallet accordingly.
* Next, copy either of the example codes above (respective to your network of choice) into Remix IDE to compile and deploy your smart contract.&#x20;
* After the contract is deployed, you need to transfer an [amount of fee](contract-addresses-and-pricing.md) into it in order to request VRF value(s).
* You will find yourself using 2 important functions namely _randomnessRequest_ and _fulfillRandomness_
* Run _randomnessRequest_ function to request a random value. Each request is equivalent to a byte32 requestId retrieved from the oracle.
* _fulfillRandomness_ is a callback function, which serves as a VRF value receiver. **It is imperative that your smart contract has a **_**fulfillRandomness**_** function with the input that resembles the examples above**. Otherwise, the VRF value generating process shall be reverted.
* `require(msg.sender == oracle, "Caller must is oracle")`: your _fulfillRandomness_ function neecds to make sure your VRF caller is affiliated to Oraichain.
* [⚠️](https://emojipedia.org/warning/)Note: You may need to use the function _clearERC20_ or _clearNativeCoin_ to keep your asset from getting stuck &#x20;
* There are currently two payment methods for Oraichain VRF service, the first of which requires Orai token for Binance Smart Chain; the latter requires native token for Avalanche and Fantom Opera. You must therefore use your wallet accordingly.
* Next, copy either of the example codes above (respective to your network of choice) into Remix IDE to compile and deploy your smart contract.&#x20;
* After the contract is deployed, you need to transfer an [amount of fee](contract-addresses-and-pricing.md) into it in order to request VRF value(s).
* You will find yourself using 2 important functions namely _randomnessRequest_ and _fulfillRandomness_
* Run _randomnessRequest_ function to request a random value. Each request is equivalent to a byte32 requestId retrieved from the oracle.
* _fulfillRandomness_ is a callback function, which serves as a VRF value receiver. **It is imperative that your smart contract has a **_**fulfillRandomness**_** function with the input that resembles the examples above**. Otherwise, the VRF value generating process shall be reverted.
* `require(msg.sender == oracle, "Caller must is oracle")`: your _fulfillRandomness_ function neecds to make sure your VRF caller is affiliated to Oraichain.
* [⚠️](https://emojipedia.org/warning/)Note: You may need to use the function _clearERC20_ or _clearNativeCoin_ to keep your asset from getting stuck &#x20;

### Specific example

Say you have to pick 3 winners out of 1000 participants. First, number the 1000 participants (from 1 to 1000). Then use fuction randomPlayer like the example below after getting a VRF value from Oraichain.

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.5.16;


interface IVRFOracleOraichain {
    function randomnessRequest(uint256 _seed, bytes calldata _data) external payable returns (bytes32 reqId);

    function getFee() external returns (uint256);
}

contract VrfOracleOraichainExample {

    address public oracle;
    uint256 public random1;
    uint256 public random2;
    uint256 public random3;
    bytes32 public reqId;

    constructor (address _oracle) public {
        oracle = _oracle;
    }

    function randomnessRequest(uint256 _seed) public {
        uint256 fee = IVRFOracleOraichain(oracle).getFee();
        bytes memory data = abi.encode(address(this), this.fulfillRandomness.selector);
        reqId = IVRFOracleOraichain(oracle).randomnessRequest.value(fee)(_seed, data);
    }

    // get three randomnesses range(0:1000) from Oraichain randomness
    function fulfillRandomness(bytes32 _reqId, uint256 oraichainRandomness) public {
        random1 = random(oraichainRandomness, 1000);
        random2 = random(random1, 1000);
        random3 = random(random2, 1000);
    }

    function random(uint256 _oraiNumber, uint256 _weight) public returns (uint256){
        return uint256(keccak256(abi.encodePacked(_oraiNumber, block.difficulty, block.timestamp, block.coinbase, block.number, msg.sender))) % (_weight);
    }
} 
```

