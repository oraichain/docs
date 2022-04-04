---
description: >-
  This section showcases how to get a random value inside a smart contract from
  different blockchain networks like BSC and Fantom, etc.
---

# Get VRF Value from different networks

To generate a VRF value from your blockchain of choice, your contract needs to inherit VrfOracleOraichain and define 2 required functions:

1. **randomnessRequest**: initiate a request to generate random numbers
2. **fulfillRandomness**: will perform receiving and verifying data.

* :warning:Your contract needs to have enough ORAI or the needed token to make the request for a random number.
* :warning:Some of the values in the example below need to be particularly correct for each network.

```
Fee :fee request
ORAI token : address token ORAI
VrfOracleOraichain: address oracle
```

## Examples for best practices

### ORAI token based VRF Oracle (BSC)

```
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/utils/Address.sol";
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/utils/math/SafeMath.sol";
import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

interface IVRFOracleOraichain {
    function randomnessRequest(uint256 _seed, bytes calldata _data) external returns (bytes32 reqId);

    function getFee() external returns (uint256);
}

contract VrfOracleOraichainExample {

    using SafeERC20 for IERC20;

    uint256 public  fee;

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
        random = _random;
    }
    
    function claim(IERC20 token, address to, uint256 amount) external {
        token.safeTransfer(to, amount);
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
        random = _random;
    }

    function clearNativeCoin(address payable _to, uint256 amount) public payable {
        _to.transfer(amount);
    }

}
```

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

