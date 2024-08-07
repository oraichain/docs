# CosmWasm IDE

[**CosmWasm IDE**](https://github.com/oraichain/cw-ide-vscode) is a tool that simplifies CosmWasm smart contract development & deployment processes. It integrates with Gitpod & Keplr to create a simple yet powerful environment to build, deploy, and interact with CosmWasm smart contracts through default and custom networks using CosmWasm. With Gitpod, CosmWasm developers can develop smart contracts on the browser.&#x20;

CosmWasm IDE is currently maintained by Oraichain & CosmWasm.

## **Components**[**​**](https://docs.cosmwasm.com/docs/1.0/getting-started/installation/#components)

**CosmWasm IDE** consists of three sub-repositories:

* [CosmWasm Gitpod](https://github.com/oraichain/cosmwasm-gitpod) serves as a Gitpod builder which automatically builds a complete development environment including Rust installation, VS Code browser, crucial VS Code extensions, and is fully compatible with the Keplr wallet. With this repository, CosmWasm developers will not have to worry about spending hours installing tools & libraries, and they also feel secure when deploying contracts using Keplr.
* [CosmWasm IDE extension](https://github.com/oraichain/cw-vscode) is a VS Code extension which integrates all the important functionalities related to building & deploying CosmWasm smart contracts through simple button clicks.
* [CosmWasm IDE extension webview](https://github.com/oraichain/cw-ide-webview) is a React application that lies on top of the CosmWasm IDE Extension. It is responsible for connecting with the Keplr wallet and displaying inputs to deploy & interact with smart contracts. It also allows adding custom networks.

For more information, please visit the repositories on [GitHub](https://github.com/oraichain/cw-ide-vscode).
