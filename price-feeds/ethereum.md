# Ethereum

## Smart contract

Contract address: [0x5D16E4Ca2339a7aeCb3Ac91Ec540e1EfE7c7EF89](https://etherscan.io/address/0x5D16E4Ca2339a7aeCb3Ac91Ec540e1EfE7c7EF89)

ORAI token: [0x4c11249814f11b9346808179Cf06e71ac328c1b5](https://etherscan.io/address/0x4c11249814f11b9346808179Cf06e71ac328c1b5)

Fee: 1 ORAI

## **Price Data Requests**

* A price feed request on the Ethereum network requires ERC20 ORAI for a fee.
* We support multi-currency pricing, each currency will map with the corresponding Id.

<table><thead><tr><th width="129">Symbol</th><th>Id</th></tr></thead><tbody><tr><td>ORAI</td><td>0</td></tr><tr><td>BTC</td><td>1</td></tr><tr><td>ETH</td><td>2</td></tr><tr><td>XRP</td><td>3</td></tr><tr><td>DOGE</td><td>4</td></tr><tr><td>USDT</td><td>5</td></tr><tr><td>LINK</td><td>6</td></tr><tr><td>UNI</td><td>7</td></tr><tr><td>USDC</td><td>8</td></tr><tr><td>BUSD</td><td>9</td></tr><tr><td>SOL</td><td>10</td></tr><tr><td>DOT</td><td>11</td></tr><tr><td>LUNA</td><td>12</td></tr><tr><td>XLM</td><td>13</td></tr><tr><td>ATOM</td><td>14</td></tr><tr><td>AAVE</td><td>15</td></tr><tr><td>EOS</td><td>16</td></tr><tr><td>AXS</td><td>17</td></tr><tr><td>ALGO</td><td>18</td></tr><tr><td>MKR</td><td>19</td></tr></tbody></table>

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
