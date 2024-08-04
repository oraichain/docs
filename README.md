# Oraichain

As the **world’s first AI-powered oracle and ecosystem for blockchains**, **Oraichain** now transforms itself to become an advanced **AI Layer 1 for Data Economy and Oracle services** that serves as a foundational layer for the creation of a new generation of smart contracts and Dapps and create a complete AI Ecosystem on blockchain.&#x20;

This document describes the Oraichain's system overview, use cases, and token economics.&#x20;

# Introduction

### Why conventional blockchain doesn’t include AI models?[#](https://docs.orai.io/docs/WhitePaper/Introduction#why-conventional-blockchain-doesnt-include-ai-models)

Current smart contracts cannot run AI models inside, and it is almost impossible to integrate an AI model into a smart contract. AI models should be complex approaches, such as SVM, neural network, and clustering. The reasons come from three characteristics of smart contracts as follows:

* Strictness: smart contracts always follow strict rules in which the inputs must be 100% accurate (e.g. signature) to generate an output. However, AI models can hardly give such accuracy (e.g. face recognition). As a result, Oraichain is there to reduce some aspects of strictness to obtain better functionality and user experience.
* Environment: smart contracts are mostly written in high-level programming languages such as Solidity and Rust that provide stricter syntax and better security. Nevertheless, the AI models are is typically written in Python or Java.
* Data size: On one hand, smart contracts often have relatively small storage since it helps reduce transaction fees in some blockchain networks such as Ethereum. On the other hand, the size of an AI model is much bigger.

There are below scenarios where AI is very useful and necessary.

* Identification based on biometric methods, such as face and fingerprint
* Automated trading based on price prediction and AI strategies
* Lending based on credit scores
* Comparison of two products from their photos
* Evaluating the price of game items

### Oraichain's Solutions

The proposed Oraichain could be a bridge to bring AI to smart contracts. The Oraichain mechanism seems similar to Band Protocol and Chainlink, but it focuses more on AI APIs and the quality of the provided AI models. In each user request, test cases are attached, and the providers’ API must pass a certain number of test cases to receive payment. The validators manage the features of test cases and AI model quality, and that makes Oraichain unique and different.

* AI Oracles: Oraichain enables smart contracts to securely access external AI APIs. Artificial Intelligence helps enhance smart contracts.
* AI Marketplace: Have access to, look up, and choose an ever-increasing AI algorithms and models from different providers around the world. Develop your applications along with the integration of AI services to increase your application functionalities as well as values.
* AI Provider: AI developers, individuals or companies, can have a chance to publish, edit, and manage their work and earn rewards from the users on a global scale. Hence, you can improve your models, even more, collect data and continue to provide exceptional AI services.
* Staking & Earning: Staking means earning. By staking your ORAI tokens into one or more validators, you will have a chance to participate in the system as a governor while earning more. Moreover, by doing so, your contributions also have a positive impact on our operations of the network. It is a win-win for all sides.
* Users & AI requests: Users can make different requests to AI services that are not easy to find elsewhere, as the service providers may not have enough resources to provide a stand-alone AI service application. If the services are great, sending funds to these services can be worth considering to allow them to grow more and more.
* ORAI DAO: you are the governor of the product serving you. Oraichain is of, for, and by the community. Oraichain team just helps initialize the project and when the mainnet is started, any changes of Oraichain should be reviewed by validators and stakeholders.

### Oracle data on Oraichain

Smart contracts are a self-operating computer program that automatically executes when specific conditions are met:

> A smart contract is a computer program or a transaction protocol which is intended to automatically execute, control or document legally relevant events and actions according to the terms of a contract or an agreement. The objectives of smart contracts are the reduction of need in trusted intermediators, arbitrations and enforcement costs, fraud losses, as well as the reduction of malicious and accidental exceptions. (From [Ethereum whitepaper](https://ethereum.org/en/whitepaper) and [Wikipedia](https://en.wikipedia.org/wiki/Smart\_contract))

The common use case of smart contracts is only tokenization, and it has not been adopted in the industry yet. The primary reason is that smart contracts are not connected to off-chain data that is not stored on the blockchain. Due to interacting with off-chain data that can lead to multiple states of the blockchain, it is not allowed to interact with off-chain data. A consensus protocol on the blockchain ensures a single state of the blockchain ledger.

The single state of blockchain ledger makes all operations deterministic. It means that the same operation performed on different nodes should give the same result. Note that the difference in results will lead to failed consensus among blockchain nodes. The typical example of non-deterministic operations is calling an external API from outside since each API call could return different values.

Oracle, in terms of definition, is a person who can provide useful information. We also have Oracle as a name of a company, but this is not that Oracle. Oracle in the blockchain is a type of middleware that connects blockchain with the resources outside. In the past, blockchain could only have access to its internal data and resources, which is very limited, whereas other systems were free to do so with simple REST APIs. Now things have changed, oracle has come to provide blockchain with more functionalities by allowing it to call external APIs. In a blockchain, an oracle can be a node that collects data from other sources of data. It also can be a script file that calls a CURL request and receive the request's response before writing that response onto the blockchain.

Oraichain provides an oracle that helps fetch external data securely and its architecture is designed to focus more on AI data sources as follows:

* Request format including input parameters and test cases
* API testing based test cases that are the unique functionality.
* Test providers proposing suitable test cases that users can choose to test an AI API.
* API marketplace and test-case marketplace
* High scalability and performance
* Cross-chain compatibility that is able to serve data to most publicly available blockchains. The target blockchains can verify data on Oraichain efficiently.
