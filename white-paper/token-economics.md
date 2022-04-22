# Token Economics

When we call **ORAI**, it means that it is native ORAI tokens created and contained in the Oraichain network. At the moment, there are three types of ORAI, including **ERC20 ORAI** on the Ethereum network, **BEP-20 ORAI** on Binance Smart Chain (BSC), and **Native ORAI** on the Oraichain Mainnet.

### Token Distribution Overview

After releasing **** all locked rewards (June 2021) and burn events, the circulating supply and new total supply are as follows:

* Circulating supply: **2,329,295 ORAI (\~11.77%)** (on April 22, 2022)
* New total supply (max supply): **19,779,272 ORAI**

**References:**&#x20;

* 1st token burn **(73% of initial total supply)** on December 22, 2020: [https://medium.com/oraichain/oraichain-tokenomics-v3-73-token-burn-and-key-updates-4e99d1972bcf](https://medium.com/oraichain/oraichain-tokenomics-v3-73-token-burn-and-key-updates-4e99d1972bcf)&#x20;
* 2nd token burn **(258,449.4 ORAI burnt from Team)** on March 28, 2021: [https://etherscan.io/tx/0xa66c06593dc8559f565e4a480c32c698ccbb128c49404f8a07a203a5f37902e1](https://etherscan.io/tx/0xa66c06593dc8559f565e4a480c32c698ccbb128c49404f8a07a203a5f37902e1)&#x20;
* 3rd token burn **(3,000,000 ORAI tokens from Team, Advisors, and Foundation)** on June 23, 2021: [https://etherscan.io/tx/0x953694d378f036ff0e136441e78875ae241d9f212b583007eb4814bab64cf7e3](https://etherscan.io/tx/0x953694d378f036ff0e136441e78875ae241d9f212b583007eb4814bab64cf7e3)&#x20;
* **ORAI tokenomics for sustainable development:** [https://blog.orai.io/updating-orai-tokenomics-for-sustainable-development-48536becdf49](https://blog.orai.io/updating-orai-tokenomics-for-sustainable-development-48536becdf49)&#x20;

The table below demonstrates specific percentage and amount for each allocation:

![](../.gitbook/assets/photo\_2022-03-10\_21-16-44.jpg)

**Planned token release schedule (%):**

![](../.gitbook/assets/token02.png)

![](../.gitbook/assets/token03.png)

### Token Utilities

Native ORAI tokens are required to secure and power the decentralized oracle network of validators. The native ORAI token is used in different scenarios below:

* **Staking for validators:** all validators are required to stake ORAI in order to be selected to create a block or fulfill data requests.
* **Transaction fee:** the ORAI token is required in order to run an AI request sent to the Oraichain network.
* **Participation in network governance:** the Oraichain network is organized in the DAO manner, all protocol upgrades and parameter changes must be voted by token holders.&#x20;

### About transaction fees on Oraichain Mainnet 2.0

The token plays a role as transaction fee that is paid for parties as follows:

* Request-executing validators
* AI-API providers
* Testcase providers
* Block-creating validators.

The transaction fee is different depending on the fee requirement of request-executing validators, AI-API providers, and test-case providers. The transaction fee should be explicitly defined in MsgSetAIRequest of a request. When a request comes, request-executing validators must decide if they want to execute it. After that, a random validator, which is responsible for proposing a block will execute the request before firing an event to those request-executing validators.

Afterwards, these validators interact with test cases and AI APIs to create a MsgResultReport in the end. A report contains the data sources, test cases used, the validator creating the report, and the block height of it. Using this information, we can collect validators, test case owners, and data source owners involved in a specific block to reward them. Only those creating reports are able to receive the rewards. If there is more than one validator asked in the MsgSetAIRequest (ValidatorCount), the transaction fee is divided proportionally to the voting power of each validator.

### Minting ORAI

There are two ways to mint ORAI tokens on the Oraichain Mainnet 2.0:&#x20;

* The first way is that the ORAI token is rewarded for each newly created block. In the Oraichain network, validators are responsible for creating new blocks and a random validator is chosen to do that. In order to become a validator, one needs enough ORAI tokens that are staked or delegated. Note that new ORAI tokens are only mined when a block contains one or more transactions with transaction fees. Such fees will be converted into tokens in the form of rewards for the validators.
* The second way is briefly described above, in which the request-executing validators can earn some extra ORAI tokens by executing test cases and data sources. Similar to the first way, 70% of the total transaction fee is extracted as a reward for the first three parties mentioned earlier. The remaining 30% is saved to reward validators for the newly committed block. Nevertheless, this second way only occurs when there is at least a report broadcast to the Oraichain network at the end of a block.

### Inflation, Staking, and Slashing

In order to keep the value of ORAI token, holders can stake their token to the Oraichain network and earn **an estimated APR of 29%**. The rewarding token is divided based on the number of tokens that a holder is staking to a validator.

[**Slashing**](https://blog.orai.io/updating-orai-tokenomics-for-sustainable-development-48536becdf49) is a mechanism to penalize misbehaviors of validators in aspects of AI API quality, response time, and availability.

Read more about incentives on Oraichain Mainnet 2.0 & rewards for delegators and validators [**here**](https://blog.orai.io/oraichain-mainnet-incentives-rewards-for-delegators-and-validators-958db9b4bb4b).
