# Ethereum

## Smart contract

Contract address: [0x5D16E4Ca2339a7aeCb3Ac91Ec540e1EfE7c7EF89](https://etherscan.io/address/0x5D16E4Ca2339a7aeCb3Ac91Ec540e1EfE7c7EF89)

ORAI token: [0x4c11249814f11b9346808179Cf06e71ac328c1b5](https://etherscan.io/address/0x4c11249814f11b9346808179Cf06e71ac328c1b5)

Fee: 1 ORAI

## **Price Data Requests**

* A price feed request on the Ethereum network requires ERC20 ORAI for a fee.
* We support multi-currency pricing, each currency will map with the corresponding Id.

| Symbol | Id |
| ------ | -- |
| ORAI   | 0  |
| BTC    | 1  |
| ETH    | 2  |
| XRP    | 3  |
| DOGE   | 4  |
| USDT   | 5  |
| LINK   | 6  |
| UNI    | 7  |
| USDC   | 8  |
| BUSD   | 9  |
| SOL    | 10 |
| DOT    | 11 |
| LUNA   | 12 |
| XLM    | 13 |
| ATOM   | 14 |
| AAVE   | 15 |
| EOS    | 16 |
| AXS    | 17 |
| ALGO   | 18 |
| MKR    | 19 |

#### Example

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.5.16;

interface IERC20 {
    function approve(address spender, uint256 amount) external returns (bool);
}


interface IPriceFeedOracle {
    function priceRequest(uint256 _symbolId, bytes calldata _data) external returns (bytes32 reqId);

    function getFee() external returns (uint256);
}

contract PriceFeedConsumerExample {

    address public orai;
    address public oracle;
    uint256 public price;
    bytes32 public reqId;
    uint256 public decimal;

    constructor (address _oraiToken, address _oracle) public {
        orai = _oraiToken;
        oracle = _oracle;
    }

    function priceRequest(uint256 _symbolId) public {
        IERC20(orai).approve(oracle, IPriceFeedOracle(oracle).getFee());
        bytes memory data = abi.encode(address(this), this.fulfillPrice.selector);
        reqId = IPriceFeedOracle(oracle).priceRequest(_symbolId, data);
    }

    function fulfillPrice(bytes32 _reqId, uint256 _priceToUsd, uint256 _decimal) external {
        price = _priceToUsd;
        decimal = _decimal;
    }
}

```

### General guideline for smart contract users

To request price feed, your contract needs to inherit PriceFeedOracle and define 2 required functions:

1. **priceRequest**: initiate a request to get price
2. **fulfillPrice**: will perform receiving price and verifying data.

* :warning:Your contract needs to have enough ORAI or the needed token to make the request for a random number.
