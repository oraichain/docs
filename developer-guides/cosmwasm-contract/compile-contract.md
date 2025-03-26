# Compile a Contract

This guide will provide step-by-step instructions for compiling the cosmwasm smart contract. Example contracts for this can be found at [https://github.com/CosmWasm/cw-examples](https://github.com/CosmWasm/cw-examples).

{% hint style="info" %}
If you have ZERO experience with smart contracts, you need to check out [book.cosmwasm.com](https://book.cosmwasm.com) for setting up your environment, testnet, and the basics of CosmWasm.
{% endhint %}

## Install cosmwasm-tools

The easiest way is to simply use the [cosmwasm-tools](https://github.com/oraichain/cosmwasm-tools) to compile cosmwasm smart contract. Follow these steps to install cosmwasm-tools:
- Using npm:
```
npm install -g @oraichain/cwtools
```
- Or can use yarn:
```
yarn global add @oraichain/cwtools
```
After installed this package, to verify if installation process took place successfully, type to your terminal:
```
cwtools --version
```
For more information of cwtools command, type:
```
cwtools -h
```

## Compile cosmwasm contracts

To compile cosmwasm contract using cosmwasm-tools, use following command:
```
cwtools build path-to-contract
```
You can compile multiple cosmwasm contracts:
```
cwtools build path-to-contract_1 path-to-contract_2
```