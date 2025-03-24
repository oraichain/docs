# Token Economics

<mark style="background-color:red;">**FAQs (08/2023):**</mark> [<mark style="background-color:red;">**https://oraichain.notion.site/ORAI-Tokenomics-Transformation-FAQs-7c2bc5c4dc4a4fc8ba61dc9eb8478160**</mark>](https://oraichain.notion.site/ORAI-Tokenomics-Transformation-FAQs-7c2bc5c4dc4a4fc8ba61dc9eb8478160)

When we call **ORAI**, it means that it is native ORAI tokens created and contained in the Oraichain network. At the moment, there are three types of ORAI, including **ERC20 ORAI** on the Ethereum network, **BEP20 ORAI** on the BNB Chain, and **Native ORAI** on the Oraichain Mainnet.

**ERC20 ORAI contract address:** 0x4c11249814f11b9346808179cf06e71ac328c1b5

**BEP20 ORAI contract address:** 0xa325ad6d9c92b55a3fc5ad7e412b1518f96441c0

### Token Distribution Overview

Summary of Circulating Supply (as of August 2023)

* Ecosystem: 5,538,595 ORAI
* DAO Treasury: 1,700,000 ORAI
* Foundation: 6,062,294 ORAI (3,229,850 ORAI delegated in FDC Program)

**Total CS (8/2023): 15,078,971 ORAI -** [**Real-time circulating supply**](https://api.token.orai.io/v1/cs)

**Total supply (8/2023): 16,779,272 ORAI -**[ **Real-time total supply**](https://api.token.orai.io/v1/total-supply)

**Max supply: 19,779,272 ORAI**



**References:**&#x20;

* 1st token burn **(73% of initial total supply)** on December 22, 2020: [https://medium.com/oraichain/oraichain-tokenomics-v3-73-token-burn-and-key-updates-4e99d1972bcf](https://medium.com/oraichain/oraichain-tokenomics-v3-73-token-burn-and-key-updates-4e99d1972bcf)&#x20;
* 2nd token burn **(258,449.4 ORAI burnt from Team)** on March 28, 2021: [https://etherscan.io/tx/0xa66c06593dc8559f565e4a480c32c698ccbb128c49404f8a07a203a5f37902e1](https://etherscan.io/tx/0xa66c06593dc8559f565e4a480c32c698ccbb128c49404f8a07a203a5f37902e1)&#x20;
* 3rd token burn **(3,000,000 ORAI tokens from Team, Advisors, and Foundation)** on June 23, 2021: [https://etherscan.io/tx/0x953694d378f036ff0e136441e78875ae241d9f212b583007eb4814bab64cf7e3](https://etherscan.io/tx/0x953694d378f036ff0e136441e78875ae241d9f212b583007eb4814bab64cf7e3)&#x20;
* **ORAI tokenomics for sustainable development:** [https://blog.orai.io/updating-orai-tokenomics-for-sustainable-development-48536becdf49](https://blog.orai.io/updating-orai-tokenomics-for-sustainable-development-48536becdf49)&#x20;

The table below demonstrates the specific percentage and amount for each allocation:

**Planned token release schedule from August 2023(%):**

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

### **How will each allocation be used?**&#x20;

As the Oraichain Foundation commits to providing deeper transparency for all current and future DAO members, it is helpful to understand the specific purpose and appropriate use of each allocation.

**Ecosystem**: Primarily used for Block Rewards, but can also be allocated for new liquidity pools and liquidity mining incentives.

**DAO Treasury**: Our expectation is that the Oraichain DAO will continue to organize, and formalize standard procedures for community-led initiatives, including developer incentives programming, marketing actions, and educational content creation. The Oraichain Foundation will continue to provide resources to meet the unique needs and assist the DAO as needed.

**Foundation**: Primarily used to support research and development for Oraichain’s open-source core protocols, provide funding for Foundation hosted hackathons, and continue to support the community-inspired Foundation Delegation Campaign (FDC), providing delegations to support our validators.

### Token Utilities&#x20;

Native ORAI tokens are required to secure and power the decentralized blockchain oracle network of validators. The native ORAI token is used in different scenarios below:

* **Transaction fee:** the ORAI token is required in order to run an AI request sent to the Oraichain network and run any transaction.
* **Staking for validators:** all validators are required to stake ORAI in order to be selected to create a block or fulfill data requests.
* **Participation in network governance:** the Oraichain network is organized in the DAO manner, all protocol upgrades and parameter changes must be voted by token holders.&#x20;

### About transaction fees on Oraichain Mainnet 2.0

The token plays a role as a transaction fee that is paid for parties as follows:

* Request-executing validators
* AI-API providers
* Testcase providers
* Block-creating validators.

The transaction fee is different depending on the fee requirement of request-executing validators, AI-API providers, and test-case providers. The transaction fee should be explicitly defined in MsgSetAIRequest of a request. When a request comes, request-executing validators must decide if they want to execute it. After that, a random validator, which is responsible for proposing a block will execute the request before firing an event to those request-executing validators.

Afterward, these validators interact with test cases and AI APIs to create a MsgResultReport in the end. A report contains the data sources, test cases used, the validator creating the report, and the block height of it. Using this information, we can collect validators, test case owners, and data source owners involved in a specific block to reward them. Only those creating reports are able to receive the rewards. If there is more than one validator asked in the MsgSetAIRequest (ValidatorCount), the transaction fee is divided proportionally to the voting power of each validator.

### Minting ORAI

There are two ways to mint ORAI tokens on the Oraichain Mainnet 2.0:&#x20;

* The first way is that the ORAI token is rewarded for each newly created block. In the Oraichain network, validators are responsible for creating new blocks, and a random validator is chosen to do that. To become a validator, one needs enough ORAI tokens that are staked or delegated. Note that new ORAI tokens are only mined when a block contains one or more transactions with transaction fees. Such fees will be converted into tokens in the form of rewards for the validators.
* The second way is briefly described above, in which the request-executing validators can earn some extra ORAI tokens by executing test cases and data sources. Similar to the first way, 70% of the total transaction fee is extracted as a reward for the first three parties mentioned earlier. The remaining 30% is saved to reward validators for the newly committed block. Nevertheless, this second way only occurs when there is at least a report broadcast to the Oraichain network at the end of a block.

### Inflation, Staking, and Slashing

To keep the value of the ORAI token, holders can stake their token to the Oraichain network. The rewarding token is divided based on the number of tokens that a holder is staking to a validator.

[**Slashing**](https://blog.orai.io/updating-orai-tokenomics-for-sustainable-development-48536becdf49) is a mechanism to penalize misbehaviors of validators in aspects of AI API quality, response time, and availability.

Read more about incentives on Oraichain Mainnet 2.0 & rewards for delegators and validators [**here**](https://blog.orai.io/oraichain-mainnet-incentives-rewards-for-delegators-and-validators-958db9b4bb4b).
