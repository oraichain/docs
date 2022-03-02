# System Overview

Oraichain is a public blockchain that allows users to create different data requests. Instead of users, smart contracts can also request data securely from AI APIs through Oraichain. The blockchain network is built based on [Cosmos SDK](https://cosmos.network) along with [Terdemint](https://tendermint.com)’s [Byzantine Fault Tolerance](https://en.wikipedia.org/wiki/Byzantine\_fault) consensus that helps speed up transactions’ confirmation time.

### Network Participants

The ORAI consensus protocol is similar to the delegated proof-of-stake (DPoS). Indeed, the network consists of numerous validators, each owning ORAI tokens while other ORAI token holders can be delegators staking their tokens to validators and get rewards for each newly created block.

The second task of the validators is to collect data from AI providers and validate that data before they are written to the blockchain. To validate an AI API, validators will do testing based on the test cases given by users, smart contracts, and test providers. If users do not know which test case is good, they can request some test cases from test providers.

### System Architecture

![](../.gitbook/assets/Oraichain\_request\_data\_flow-87cdf418be717d3ab68404be6b61bd46.jpg)

The flow of requesting an AI API is illustrated in the Oraichain’s System Overview figure. To perform a request, users or smart contracts need to call an oracle script that is available on the ORAI gateway or marketplace. In an oracle script, there are AI data sources (provided by AI providers), test cases, test source (optional), and transaction fees for each request. When a request comes, a random willing validator is chosen to perform this request. The chosen validator will fetch data from one or more AI providers on behalf of the user after executing the test scenarios, and if the AI provider fails in testing, the request is canceled.

A request is successful if its result is written to the Oraichain blockchain. The transaction result, which can be fetched from smart contracts and regular applications, is proof of execution, and fees are applied in this transaction. There is an overhead of reading results from Oraichain’s transactions, but it helps ensure that the AI API quality is good and there is no data tampering during the process of fetching data from AI providers.

Compared to Band Protocol and Chainlink, API testing based test cases is the unique functionality. Since Oraichain focuses on AI APIs, testing is very important to control the quality of AI providers. Besides, test providers can propose suitable test cases that users can choose to test an AI API. Test cases in the Oraichain marketplace can encourage AI providers to improve the accuracy of AI models.

Another interesting feature is that the Oraichain community has the power to rate the validators’ reputation for quality AI APIs improvement. If a validator has bad behavior, such as failing to perform test cases and validate AI providers, slow response time, and low availability, its holding token will be slashed.

Nevertheless, validators in Oraichain is responsible for performing many important tasks and could be a centralized point. Therefore, the number of chosen validators should be high in order to increase request performance, scalability, and high availability. Meanwhile, because we need many validators to participate in the Oraichain network and maintain their quality work, block reward and transaction fees must be applied for such validators to earn more ORAI tokens.

Specifically, there are two ways to incentivize validators as well as test case, data source providers.

#### First way

The validators that participate in the network can receive rewards after each block having transaction fees. Indeed, the validator proposing the block, which is called a proposer, collect a bonus reward for its helpful job of committing a block. The formulas are described below:

**(0) Propose multiplier = (base proposer reward + bonus proposer reward \* (sum of precommit voting power / total voting power))**

where **base proposer reward** is the base percentage of how much the proposer receives the reward; bonus proposer reward is the percentage for the amount of precommit voting power that the proposer has successfully collected when before committing a block. The current values are 1% and 4% respectively, so the maximum value of the propose multiplier is 5% or 0.05

**(1) Proposer reward (bonus) = Propose multiplier \* total fees within a block**

**(2) Remaining = Remaining - (1)**

**(3) vote multiplier = 1 - community tax - propose multiplier**

while the community tax is set to be 2% or 0.02

**(4) power fraction = voting power / total voting power**

**(5) a validator's reward = total fee **_**reward multiplier**_** power fraction**

**(6) remaining = remaining - reward for each validator**

the loop goes on until all validators have received their rewards.

**(7) The remaining reward is sent into the community pool, where it can be used to send to an abritary orai account.**

**Note**: The actual number of ORAI coins that a validator collect is the commission of the received reward. This figure is configured by each validator, and by default it is 0.1. The minimum gas price, on the other hand, must explicitly set in order to charge fees for each transaction. In addition, the tokens are allocated proportionally to the voting power. As a result, validators with more coins staked will receive more rewards.

#### Second way

The validators can actively participate in executing the oracle scripts to receive 20% of the rewards by publishing reports. A report contains information on the validator that created it, the data sources, test cases used, and the block containing that report transaction. Using this information, we can collect validators, test case owners, and data source owners involved in a specific block to reward them. Half of the total fees is reserved for rewarding data source and test case owners. Only those creating reports are able to receive the rewards. The formulas are as follows:

**Provider reward = total request fees within the previous block \* 0.5**

Here, we make sure that every Data Source and Test Case owner receives the correct amount that he requires after running the script. For example, an Oracle Script runs two AI Data Sources, 1 Test Case, each requires 0.05 ORAI to execute. As a result, the minimum fee needed is 1.5 ORAI, which is equal to total request fees within the previous block \* 0.5.

**Validator reward = Provider reward **_**0.4 or total request fees within the previous block**_** 0.2**

In here, validators that participate in the Oracle Script execution process will receive their rewards according to their voting powers within the **validator reward**.

The rest of the request fees (the remaining 30%) will be allocated to the validator that has successfully proposed a new block along with taxes that go into the community pool using the first way.
