# Manage Contract Pattern

## Prerequisites

To follow this pattern, you need to install our [CosmWasm tools](./compile-contract.md#install-cosmwasm-tools) first.

## The pattern

Even though you can upload, deploy and interact with CosmWasm contracts any way you want, it is recommended to organize your code in a pattern that is scalable and portable to different dApps.

In Oraichain Labs, we use a `Nx` mono repo that consists of at least three packages below:

```
tonbridge-sdk/
├── packages/
│   ├── contracts-build/
│   └── contracts-sdk/
|   └── contracts-demo/
├── .gitignore
├── README.md
├── lerna.json
├── nx.json
├── package.json
├── tsconfig.json
└── yarn.lock
```

- **contracts-build**:

  - Contains .wasm files that are related to the service you're trying to develop. They are also useful when you want to write simulate tests in Node.js resembling the actual production code.
  - For example, Our [Ton Bridge SDK's contracts build](https://github.com/oraichain/tonbridge-sdk/tree/main/packages/contracts-build/data) contains 3 .wasm files: a bridge, a light client, and a cw20 token. These are up-to-date smart contract builds that are used for [simulation](./cosmwasm-simulate.md) and deployment to the Oraichain network. Meanwhile, the src/index.ts has two main functions: read the .wasm files and deploy those onto the Oraichain network.

  ```
  contracts-build/
  ├── data/
  │   ├── cw-tonbridge-bridge.wasm
  │   └── cw-tonbridge-validator.wasm
  |   └── oraiswap-token.wasm
  ├── src/
  |   ├── index.ts
  ```

- **contracts-sdk**:

  - Contains TypeScript code generated from the contract schemas. You can also generate types for multiple contracts and put them in the contracts-sdk. The types usually come from the contracts used in contracts-build for consistency. However, there are some common already been published on NPM that you can reuse. For example, the CW20 types are in the `@oraichain/oraidex-contracts-sdk`.
  - For example, [Ton Bridge SDK's contracts sdk](https://github.com/oraichain/tonbridge-sdk/tree/main/packages/contracts-sdk) contains types of the TON CW contracts.

  ```
  contracts-sdk/
  ├── src/
  |   ├── index.ts
  |   ├── TonbridgeBridge.client.ts
  |   ├── TonbridgeBridge.types.ts
  |   ├── TonbridgeValidator.client.ts
  |   ├── TonbrideValidator.types.ts
  |   ├── types.ts
  ```

  - These files are generate by using [CosmWasm tools](./compile-contract.md#install-cosmwasm-tools). You will learn to build contracts and generate types in the [below section](#how-to-setup-the-pattern).

- **contracts-demo**:

  - The name speaks for itself. This package is for writing example codes to interact with the contracts you are developing.
  - Since intergrating the contract logic into an dApp can be confusing, having examples of how to use the contract end-to-end can be useful for other developers and for your future-self.
  - You can write anything in this package, and since it is just a demo package, you don't need to publish it onto NPM.
  - You can have a tests/ directory for contract simulation testing in this package.

  ```
  contracts-demo/
  ├── tests/
  |   ├── abcd.test.ts
  ```

## Benefits

You may ask, what's the benefits of following this coding pattern?. Say no more!

1. **Increase portability**

- You can publish the `contracts-build` and `contracts-sdk` packages onto NPM and use them in various dApps.
- The `contracts-build` are for end-to-end dapp testing simulation, while the `contracts-sdk` are for easy contracts interaction.
- Whenever there are contract schema or code changes, dApps only need to pump the dependency versions and modify their code accordingly to the new types & .wasm.

2. **Reduce Development time**

- By using a `Nx` mono repo, the devs don't need to publish any packages before making sure everything is correct.
- For example, after testing, they see that the contract schemas need to be updated to meet the user requirements. With a mono repo, they only need to update the contract code, re-generate the schemas and rebuild the mono repo if needed.
- The service is also loosely-coupled. For example, if there's a new feature changing the contract logic, but not the schemas, then we only need to update the `contracts-build` package and the dApp logic.

## How to setup the pattern

Assuming your contracts are located at the same level as your root project directory like below:

```
  oraichain-labs/
  ├── tonbridge-sdk/
  ├── tonbridge-cw-contracts/
  |   ├── contracts/
```

then you can run the following commands to build & generate types:

```bash
# build code. -o here is the output directory. -w is watch for hot-reloading when there's a change in the contract code
cwtools build ../tonbridge-cw-contracts/contracts/* -o packages/contracts-build/data -w
# build schema. -s is for schema
cwtools build ../tonbridge-cw-contracts/contracts/* -s
# gen typescript codes. -o here is the output directory
cwtools gents ../tonbridge-cw-contracts/contracts/* -o packages/contracts-sdk/src
```

You can also build multiple contracts at different directories at once. The same goes for `gents`:

```bash
cwtools build ../tonbridge-cw-contracts/contracts/* ../oraiswap/contracts/* -o packages/contracts-build/data
```

