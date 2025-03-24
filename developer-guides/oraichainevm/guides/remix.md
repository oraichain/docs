---
description: Set up a Remix Ethermint testnet development environment.
---

# Remix

[Remix](http://remix.ethereum.org/) is an in-browser IDE for [Solidity](https://github.com/ethereum/solidity) smart contracts. In this guide, we will learn how to deploy a contract to a running Ethermint network through Remix and interact with it.&#x20;

### Connect Oraichain Account to Remix

First, follow the steps in the Metamask guide to import your Oraichain private key into Metamask. Start the Oraichain daemon and rest server.Once that is complete, go to [Remix](http://remix.ethereum.org/). There are some contracts in the File Explorer. Select any of these contracts. In this example, we use `Counter.sol` from the Foundry guide. On the left-most bar, select the Solidity Compiler and compile the contract.CommentNext, select the `Deploy and Run` option. Select `injected provider - Metamask` as the environment. This will open a metamask popup for you to confirm connecting your Metamask to Remix. Hit confirm.CommentYou should see your account show up in the left-hand panel.Comment

<figure><img src="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVpLm1Fn91YqukBDhp9gm%2Fuploads%2FahqsVzmCFb5PGmt0QUzp%2Fimage.png?alt=media&#x26;token=0e1fe1f2-6d0d-4e82-b350-5152acfa8f5b" alt=""><figcaption></figcaption></figure>

### Deploy and Interact with the Contract <a href="#deploy-and-interact-with-the-contract" id="deploy-and-interact-with-the-contract"></a>

Now that your account is connected, you can deploy the contract. Press the `Deploy` button. A metamask pop-up will appear, asking you to confirm. Confirm the transaction. You should see a log for the deployment transaction in the oraichain logslogs.

1. **Interact with Contract**:
   *   The deployed contract will be visible under Deployed Contracts in Remix.

       * Use the Remix interface to interact. For `Counter.sol`, click `deploy`. Confirm the transaction in Metamask.



       <figure><img src="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVpLm1Fn91YqukBDhp9gm%2Fuploads%2F61AAFwfAsYfDOGoSz1sQ%2Fimage.png?alt=media&#x26;token=9c6f8f9d-25e2-47ee-80c3-8b23d094ffc3" alt=""><figcaption></figcaption></figure>

       * Retrieve the counter value by clicking `getCounter`; it should return 1.

       <figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
