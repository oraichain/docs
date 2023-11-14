# Become a Validator

## Become a validator

This tutorial helps validators and full nodes quickly synchronize with the Oraichain mainnet by downloading a storage snapshot prepared by the team. The downloading speed is much faster than synchronizing from the first block, which allows fast set up to join the network in no time!

If you want to synchronize your node using the traditional method, please follow [this tutorial](https://github.com/oraichain/oraichain-static-files/blob/master/Validator.md) instead.

### Hardware specifications for an Oraichain node

Please take a look [here](./#node-hardwarde-specification)

### Setup the validator node

#### 1. Download and run the setup file

```bash
curl -o docker-compose.yml https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/docker-compose.0.41.4.yml && curl -o setup.sh https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/setup.0.41.4.sh && chmod +x setup.sh && curl -OL https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/orai.env
```

#### 2. Edit wallet name and moniker you prefer to create a new wallet and validator in the orai.env file you have just downloaded

In your `orai.env` file, it's a good idea to set the `LOG_LEVEL` parameter to `"info"` if you want to see all the container logs.

#### 3. Build and enter the container

With docker, your validator can run on any platforms. As a result, it is a must to install and download Docker & docker-compose. Afterward, please type:

```bash
docker-compose pull && docker-compose up -d --force-recreate
```

This command starts setting up a container for your Oraichain node. It runs several commands to get a statesync node ready. You can see these steps in the docker-compose file. For more details, the setup.sh file has many Bash scripts that handle different tasks automatically. Also, it makes a special folder called .oraid, which keeps all important settings and data for your node.

#### 4. Statesync to synchronize your node

Statesync is the quickest way to let your node catch up with the network by starting at a specified trusted height and fetch snapshot data from other peers.

When you execute the aforementioned command, it initiates statesync in your container. This involves retrieving and applying a snapshot. Following this, your node will begin synchronizing new blocks, a process that typically takes around **30 minutes**. You can monitor the progress of this operation by viewing the container log using the command provided below:

```bash
docker-compose logs -f orai
```

After running the script, your node will be ready to run as a full node!

Please wait until your node is fully synchronized by typing:&#x20;

```bash
oraid status &> status.json && cat status.json | jq '{catching_up: .SyncInfo.catching_up}'
```

If the **catching up** status is **false**, you can continue.

#### 5. Create validator transaction

Ensure the preservation of two crucial files:

* &#x20;`.oraid/config/node_key.json` and&#x20;
* `.oraid/config/priv_validator_key.json`,&#x20;

which house essential validator information for voting. To safeguard against potential losses of your validator node, create backups for these files in case of unforeseen issues.

Access your container, a necessary step, by executing the following command:

```bash
docker-compose exec orai bash
```

Before proceeding, it is imperative to have a wallet containing ORAI tokens, generated using owallet ([https://chrome.google.com/webstore/detail/owallet/hhejbopdnpbjgomhpmegemnjogflenga](https://chrome.google.com/webstore/detail/owallet/hhejbopdnpbjgomhpmegemnjogflenga)).&#x20;

Upon creation, obtain a mnemonic and add the wallet to your node using the command below (replace `<your wallet name>` with your preferred name). Follow the system instructions as prompted:

```bash
oraid keys add <your wallet name> --recover
```

Optionally, validate your key with the following command:

```bash
oraid keys list
```

To initiate the creation of your validator, execute the subsequent command (ensure you possess sufficient ORAI for transaction fees):

```
oraid tx staking create-validator \
  --amount 2000orai \
  --commission-max-change-rate "0.2" \
  --commission-max-rate "0.2" \
  --commission-rate "0.03" \
  --min-self-delegation "1" \
  --pubkey  $(oraid tendermint show-validator) \
  --moniker $MONIKER \
  --chain-id $CHAIN_ID \
  --fees 1000orai \
  --from <your wallet name>
```

#### 6. Check your node status with voting power

Type the following command to check your voting power:

```bash
oraid status &> status.json && cat status.json | jq '{catching_up: .SyncInfo.catching_up, voting_power: .ValidatorInfo.VotingPower}'
```

If you see that your VotingPower is greater than 0, and the catching\_up is false, then congratulations, you are a validator now!

### Tips

You should monitor your nodes frequently. Make sure it has the correct tendermint public key to vote. To check, you should take a look at your **tendermint public key** in the **ValidatorInfo** attribute after typing:

```bash
oraid status
```

Please compare the **tendermint public key** to the one when you type:

```bash
oraid query staking validator <operator address>
```

If they match, then your node is still running fine. If not, then you should remove the **.oraid/config/node\_key.json, .oraid/config/priv\_validator\_key.json** files, replace them with your backup files and restart the node.

## IMPORTANT NOTE

**YOU MUST NOT RUN TWO VALIDATOR NODES WITH THE SAME NODE KEY AND VALIDATOR KEY AT THE SAME TIME. OTHERWISE, YOUR VALIDATOR WILL BE TOMBSTONED BECAUSE OF DOUBLE SIGNING, AND IT WILL NEVER BE ABLE TO JOIN THE VALIDATORSET EVER AGAIN.**

**BACK UP TWO IMPORTANT FILES: .oraid/config/node\_key.json & .oraid/config/priv\_validator\_key.json.** **TO PREVENT DOUBLE SIGNING, BACK UP THE .oraid/data/priv\_validator\_state.json FILE BEFORE MIGRATING TO A DIFFERENT VALIDATOR NODE**

### Setup your sentry nodes (optional)

This section is optional if you want to follow the sentry architecture. For more information about the sentry architecture, please click [here](https://docs.tendermint.com/master/nodes/validators.html).

Setting up a fast sync sentry node is the same to that of a validator node but without the steps [4](become-a-validator.md#4-create-validator-transaction) and [5](become-a-validator.md#5-check-your-node-status-with-voting-power)

### Setup the sentry architecture (optional)

This section is optional if you want to follow the sentry architecture. You can set the following configurations in the file **.oraid/config/config.toml** directly. Some pairs can be configured through the start command. This architecture will help you connect your genesis nodes with your sentry nodes, and your sentry nodes are responsible for connecting to other nodes within the network. To start using flags, please type:

```bash
oraivisor start --help
```

You also need to prepare your own VPC network beforehand.

#### 1. Validator node configuration

```bash
pex = false
persistent_peers = <list of sentry nodes with node id, private ips, port 26656>
addr_book_strict = false
unconditional_peer_ids (optional) = <list of sentry node ids>
```

To get a node id, type:

```bash
oraid tendermint show-node-id
```

some configuration values can only be changed in the **.oraid/config/config.toml** file, like **addr\_book\_strict**

Example:

```bash
pex = false
persistent_peers = "014b6fa1fd8d14fa7e08c353497baa1f5581a089@1.2.3.4:26656,bc806159212529879b42c737c2338042e396b1dd@2.3.4.5:26656"
addr_book_strict = false
unconditional_peer_ids (optional) = "014b6fa1fd8d14fa7e08c353497baa1f5581a089,bc806159212529879b42c737c2338042e396b1dd"
```

#### 2. Sentry node configuration

```bash
pex = true
unconditional_peer_ids = <validator node id>
persistent_peers = <validator nodes, optionally other sentry nodes>
private_peer_ids = <validator node ids>
addr_book_strict = false
external_address = <your-public-address. Eg: tcp://1.2.3.4:26656>
when starting, set flag --rpc.laddr tcp://0.0.0.0:26657
```

Example:

```bash
pex = true
unconditional_peer_ids = "cc433de0f3d7e8e125ca40396e7cedb12a5d68bc"
persistent_peers = "cc433de0f3d7e8e125ca40396e7cedb12a5d68bc@5.6.7.8:26656"
private_peer_ids = "cc433de0f3d7e8e125ca40396e7cedb12a5d68bc"
addr_book_strict = false
external_address = "tcp://1.2.3.4:26656"
when starting: oraivisor start --rpc.laddr tcp://0.0.0.0:26657
```

You should also set up firewalls for your genesis nodes.

Please join the [Oraichain validators group](https://t.me/joinchat/yH9nMLrokQRhZGY1) on Telegram to discuss ideas and problems!
