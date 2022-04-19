# Executor incentive mechanism

There are two ways for executors to receive executor rewards. One is constantly keeping the program online, while the other is through executing the requests.

## 1. Passive income

When the executor program is running, it will periodically invoke a ping transaction onto the ping smart contract, which will increment the number of times the executor pings. The ping number is used as proof that the executor is online. It is also a value to calculate the total reward that an executor will receive.

An executor can claim the reward anytime by invoking the claim reward transaction function. However, the transaction will fail if the contract does not have enough funds, and the Oraichain team will have to top it up.

One note is that, at the moment, there is a maximum reward parameter for executors which is used as an upper bound for the reward one can receive. If an executor receives, for example, 5 ORAI in total due to not claiming the rewards for a long time while the maximum reward value is 4 ORAI, then that executor can only receive 4 ORAI. The purpose of the parameter is to limit the executor rewards within the first months. When the AI Oracle becomes more stable, we will remove this parameter.

Below is the basic formula for the passive incentive mechanism:

```js
reward_amount = executor_total_ping + base_reward_unit
if (reward_amount < maximum_reward_can_claim) reward_amount = maximum_reward_can_claim
```

where the ```base_reward_unit``` is a contract parameter set by the contract owner, which is currently the Oraichain team. In the future, this parameter will be only adjusted through DAO.

After sending the reward successfully, the executor's total ping will reset to 0.

## 2. Rewards from executing AI Oracle requests

### 2.1 Accumulating the reward pool

AI Executors can also earn active rewards by executing AI Oracle requests. Each request will accumulate to the total reward that an executor will receive. Each executor has a reward pool to measure such rewards. Below is the equation to calculate the executor's reward when executing an AI Oracle request:

```
new_total_reward = current_total_reward + (minimum value between preference_requester_fee & preference_executor_fee)
```

where ```preference_requester_fee``` & ```preference_executor_fee``` are set by the requester and executor, respectively. We choose the minimum value between the two to prevent a specific case, where an executor sets a low preference fee at first, then immediately increases it after finishing a request to try getting more rewards. Typically, executors with higher fee preferences will not execute requests that have lower fee preferences.

The reward pool is accumulated automatically through the AI Oracle smart contract. Nevertheless, the pool's rewards can be slashed for a certain amount if an executor acts maliciously when executing a request. Currently, the process of slashing is being designed & implemented at the moment.

### 2.2 Claiming the reward

In order to claim the rewards, executors must trigger a ```claim reward``` transaction either on Oraiscan or through CLI, which will move such rewards from a normal to an unbonding state. During the unbonding period, the slashing mechanism still applies to the unbonding amount, which prevents executors from executing requests poorly that yet can still claim the rewards before getting slashed.

After the unbonding period ends, the executors can trigger the ```claim reward``` transaction again to finally receive the rewards.