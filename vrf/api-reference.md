# API Reference

## Index

#### Constructors

|              | Description               |
| ------------ | ------------------------- |
| Constructors | Oracle vrf initialization |



#### Functions

|                   | Description                                                   |
| ----------------- | ------------------------------------------------------------- |
| randomnessRequest | Make a request to generate random numbers                     |
| fulfillRandomness | VRF Oracle will call back when receiving valid VRF proof data |

## Constructors

```
constructor (address _oraiToken, address _oracle) public {
    fee = 1000000000000000000;
    orai = _oraiToken;
    oracle = _oracle;
}
```

##  Funtions

#### randomnessRequest

```
function randomnessRequest(uint256 _seed, bytes memory _data) public returns (bytes32 reqId);
```

* \_seed : a random number provided by the caller
* \_data : the result of callbackAddress and callbackFunc after encoding

```
bytes memory data = abi.encode(address(this), this.fulfillRandomness.selector);
```

* callbackAddress: smart contract address receive random number
* callbackFunct : hen a function is called, the first 4 bytes of `calldata` specifies which function to call.

#### fulfillRandomness



```
function fulfillRandomness(bytes32 reqId, bytes calldata _data, uint8 v, bytes32 r, bytes32 s) external;
```

* reqId: request id generated when calling function randomnessRequest
* \_data: includes callbackAddress and callbackFunc after encode
* v,r,s: proof vrf



