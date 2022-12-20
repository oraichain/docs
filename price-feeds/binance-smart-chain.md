# BNB Chain

## Smart contract

Contract address: [0x48D21bb7DbC0b6214018766C8415DB508147CC38](https://testnet.bscscan.com/address/0x48D21bb7DbC0b6214018766C8415DB508147CC38#readContract)

## **Price Data Requests**

1. Basically, there are two types of getting price data for developers: getPrice() and getPriceBulk(). Via smart contract 'OraiOraclerPriceDataProxy', developers can use NodeJs or smart contract to request price date on blockchain networks like Oraichain Network, Binance Chain, Ethereum.
2. At the moment, Oraichain Oracle supports 3 networks (Oraichain, Binance Chain, Ethereum) with corresponding addresses for Oraichain Oracle Price.

**getPrice():** Input includes symbols of two assets and output results in the ratio between them and the latest timestamp of the update.

**getPriceBulk():** This allows us to get as much information as possible on the price of 2 asset pairs at once. Input requires 2 asset symbols output results in the ratio between them and the latest timestamp of the update.]

```
vd : getPrice("BNB","ETH")
```

1.  getPrice()

    ```
    vd : getPriceBulk(["BNB","ETH"],["BTC","BNB"])
    ```
2.  getPriceBulk()

    ```
    function getPrice(string memory _base, string memory _quote) public override view returns (ResponsePriceData memory){
         return oracle.getPrice(_base, _quote);
     }
    function getPrice(string memory _base)public override view returns (PriceData memory){
         return oracle.getPrice(_base);
     }
    ```

```
function getPriceBulk(string[] memory _bases, string[] memory _quotes)
 public
 override
 view
 returns (ResponsePriceData[] memory)
 {
     require(_bases.length == _quotes.length, "BAD_INPUT_LENGTH");
     uint256 len = _bases.length;
     ResponsePriceData[] memory results = new ResponsePriceData[](len);
     for (uint256 idx = 0; idx < len; idx++) {
         results[idx] = getPrice(_bases[idx], _quotes[idx]);
     }
     return results;
 }
```

## Example

#### Solidity Smart Contract <a href="#solidity-smart-contract" id="solidity-smart-contract"></a>

```
pragma solidity 0.6.11;
pragma experimental ABIEncoderV2;


interface IOraiOraclePriceDataProxy {
    struct ResponsePriceData {
        uint128 rate; // base/quote exchange rate, multiplied by 1e18.
        uint64 lastUpdatedBase;
        uint64 lastUpdatedQuote; // UNIX epoch of the last time when quote price gets updated.
    }

    struct PriceData {
        uint128 rate; // USD-rate, multiplied by 1e18.
        uint64 resolveTime; // UNIX epoch when data is last resolved.
    }

    function getPrice(string memory _base, string memory _quote)
    external
    view
    returns (ResponsePriceData memory);

    function getPrice(string memory _base)
    external
    view
    returns (PriceData memory);

    function getPriceBulk(string[] memory _bases)
    external
    view
    returns (PriceData[] memory);

    function getPriceBulk(string[] memory _bases, string[] memory _quotes)
    external
    view
    returns (ResponsePriceData[] memory);

}


contract Example {
    IOraiOraclePriceDataProxy public oracle;

    constructor(IOraiOraclePriceDataProxy _oracle) public {
        oracle = _oracle;
    }

    function getPriceExample(string memory _base, string memory _quote) public view returns (uint128, uint64, uint64) {
        IOraiOraclePriceDataProxy.ResponsePriceData memory data = oracle.getPrice(_base, _quote);
        return (data.rate, data.lastUpdatedBase, data.lastUpdatedQuote);
    }

}
```

#### Integrate with Node.js

```
const Web3 = require("web3");
const web3 = new Web3("END_POINT_NETWORK_BLOCKCHAIN");
const oraiOracleAbi = [{"inputs":[{"internalType":"contract IOraiBase","name":"_oracle","type":"address"}],"stateMutability":"nonpayable","type":"constructor"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"previousOwner","type":"address"},{"indexed":true,"internalType":"address","name":"newOwner","type":"address"}],"name":"OwnershipTransferred","type":"event"},{"inputs":[],"name":"renounceOwnership","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"contract IOraiBase","name":"_oracle","type":"address"}],"name":"setOracle","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"address","name":"newOwner","type":"address"}],"name":"transferOwnership","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"string[]","name":"_bases","type":"string[]"}],"name":"getPriceBulk","outputs":[{"components":[{"internalType":"uint128","name":"rate","type":"uint128"},{"internalType":"uint64","name":"resolveTime","type":"uint64"}],"internalType":"struct IOraiBase.PriceData[]","name":"","type":"tuple[]"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"oracle","outputs":[{"internalType":"contract IOraiBase","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"owner","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"}]
const addr = addressOraiOraclePriceDataProxy ;
const oraiPriceFeed = new web3.eth.Contract(oraiOracleAbi, addr);

oraiPriceFeed.methods.getPrice("BNB","USDT").call()
    .then((result) => {
        // Do something with price
        console.log("result", result);
    })
```
