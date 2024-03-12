# Reduce the block time to 1 second

In summary, the goal of reducing block time in the Oraichain network ecosystem is to improve transaction speed, network throughput, and overall performance, making the blockchain more suitable for a wide range of applications, particularly those requiring fast and efficient transaction processing.

## When to reduce the block time

We will reduce our network block time starting from block height 16200000. To make sure every node stops at the same block height as others, please do the following step ASAP:

### Non-docker users

For non-docker users, please restart your node with the --halt-height flag: `oraid start --halt-height 16200000`

### Docker users

- For those whose docker-compose.yml file contains the line: `command: sh setup.sh`, if the file has this line: `oraivisor start --home=.oraid --minimum-gas-prices=0.001orai`, then you can:

  - Edit the line to: `oraivisor start --home=.oraid --minimum-gas-prices=0.001orai --halt-height 16200000`. Please don't forget to add additional flags if you need to based on your node setup. The provided command is only an example for you to add the `--halt-height` flag
  - Restart the container to apply the change: `docker-compose restart orai`

- For those whose docker-compose.yml does not have similar lines like: `command: sh setup.sh`, then you can:
  - Restart the container: `docker-compose restart orai`
  - Run the node with the following command: `docker-compose exec -d orai bash -c "oraivisor start --halt-height 16200000"`. Please don't forget to add additional flags if you need to based on your node setup. The provided command is only an example for you to add the `--halt-height` flag

## What to do at block height 16200000

After your node has automatically & gracefully stopped at height 16200000, you now can edit the config.toml file to reduce block time.

Note that if the majority of validators have changed their block time to 1s, and you haven't after the block 16200000, your node won't be able to produce new blocks resulting in reward losses.

### Edit config.toml

The current value of **_timeout_commit_** is now 5s; let's change it to 500ms.

```bash
[consensus]
timeout_commit = "500ms"
```

### Start your node to apply the change

#### Non-docker users

For non-docker users, please start your node by removing the `--halt-height` flag: `oraid start`

#### Docker users

- For those whose docker-compose.yml file contains the line: `command: sh setup.sh`:

  - `oraivisor start --home=.oraid --minimum-gas-prices=0.001orai`

- For those whose docker-compose.yml does not have similar lines like: `command: sh setup.sh`:
  - `docker-compose exec -d orai bash -c "oraivisor start`

### Verification

We expect that our network will halt at block 16200000 since all validator nodes will stop & edit their files. Once most validators become online again (2/3 voting power), the network should start again and produce blocks at a faster rate. Those who configure correctly will eventually produce new blocks.
