# Smart Contracts

Since the inception of Ethereum in 2015, the capability to govern digital assets through smart contracts has attracted a substantial community of developers, fostering the creation of decentralized applications (dApps) on the Ethereum Virtual Machine (EVM). This community continues to expand by developing comprehensive tools and establishing new standards, thereby accelerating the adoption of EVM-compatible technologies.

Whether you are developing novel use cases on Oraichain or migrating an existing dApp from another EVM-based blockchain, such as Ethereum, Oraichain provides a seamless environment to build and deploy EVM smart contracts to implement the core business logic of your dApp. Oraichain is fully compatible with the EVM, enabling the use of widely recognized tools, including Solidity, Remix, Oracles, and APIs like Ethereum JSON-RPC.

Furthermore, leveraging the interoperability of Cosmos-based blockchains, Oraichain facilitates the development of scalable cross-chain applications within the familiar EVM framework. The following section outlines the essential components involved in building and deploying EVM smart contracts on Oraichain.

### Build EVM smart contracts with Solidity <a href="#build-evm-smart-contracts-with-solidity" id="build-evm-smart-contracts-with-solidity"></a>

You can develop EVM smart contracts on Oraichain using [Solidity](https://github.com/ethereum/solidity). Solidity is also used to build smart contracts on Ethereum. So if you have deployed smart contracts on Ethereum (or any other EVM-compatible chain) you can use the same contracts on Oraichain.

Since it is the most widely used smart contract programming language in Blockchain, Solidity comes with well-documented and rich language support. Head over to our list of Tools and IDE Plugins to help you get started.

### EVM Precompiled contracts <a href="#evm-precompiled-contracts" id="evm-precompiled-contracts"></a>

EVM precompiled contracts that are built into the Ethereum Virtual Machine (EVM). Each offers specific functionality, that can be used by other smart contracts. Generally, they are used to perform operations that are either not possible or would be too expensive to perform with a regular smart contract implementation, such as hashing, elliptic curve cryptography, and modular exponentiation.

By adding custom EVM precompiled contracts to Ethereum's basic feature set, Oraichain allows developers to use previously unavailable functionality in smart contracts, like native Cosmos Bank operation. This will allow more complex smart contracts to be built on Oraichain and further improve the interoperability between Cosmos and Ethereum. It also is a key feature to achieve Oraichain' vision of being the definitive dApp chain, where any dApp can be deployed once and users can interact with a wide range of different blockchains natively.

To enable the described functionalities, Oraichain introduces so-called _stateful_ precompiled smart contracts, which can perform a state transition, as opposed to those offered by the standard Go-Ethereum implementation, which can only read state information. This is necessary because an operation like e.g. staking tokens will ultimately change the chain state. View a list of available precompiled contract [here](https://docs.evmos.org/develop/smart-contracts/list-evm-extensions).

### Deploy with Ethereum JSON-RPC <a href="#deploy-with-ethereum-json-rpc" id="deploy-with-ethereum-json-rpc"></a>

Oraichain is fully compatible with the Ethereum JSON-RPC APIs, allowing you to deploy and interact with smart contracts on Oraichain and connect with existing Ethereum-compatible web3 tooling. This gives you direct access to reading Ethereum-formatted transactions or sending them to the network.

You can connect to the Oraichain [Testnet](../guides/oraichain-evm-rpc.md) to deploy and test your smart contracts before moving to Mainnet.

**Block Explorers**[**​**](https://docs.evmos.org/develop/smart-contracts#block-explorers)**​**

You can use [block explorers](../guides/tools.md) to view and debug interactions with your smart contracts deployed on Oraichain. Block explorers index blocks and their transactions so that you can search for real-time and historical information about the blockchain, including data related to blocks, transactions, addresses, and more.
