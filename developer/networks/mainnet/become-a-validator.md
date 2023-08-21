# Become a Validator

## Become a validator

This tutorial helps validators and full nodes quickly synchronize with the Oraichain mainnet by downloading a storage snapshot prepared by the team. The downloading speed is much faster than synchronizing from the first block, which allows fast set up to join the network in no time!

If you want to synchronize your node using the traditional method, please follow [this tutorial](https://github.com/oraichain/oraichain-static-files/blob/master/Validator.md) instead.

### Hardware specifications for an Oraichain node

Please take a look [here](./#node-hardwarde-specification)

### Setup the validator node

#### 1. Download and run the setup file

```bash
curl -OL https://raw.githubusercontent.com/oraichain/orai/v0.41.3-statesync-script/docker-compose.prod.yml && curl -OL https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/orai.env
```

#### 2. Edit wallet name and moniker you prefer to create a new wallet and validator in the orai.env file you have just downloaded

#### 3. Build and enter the container

With docker, your validator can run on any platforms. As a result, it is a must to install and download Docker & docker-compose. Afterward, please type:

```bash
docker-compose pull && docker-compose up -d --force-recreate
```

#### 4. Statesync to synchronize your node

Statesync is the quickest way to let your node catch up with the network by starting at a specified trusted height and fetch snapshot data from other peers.

To use the statesync method, please run the following script:

```sh
wget https://raw.githubusercontent.com/oraichain/orai/v0.41.3-statesync-script/scripts/prod_statesync.sh -O prod_statesync.sh && chmod 755 prod_statesync.sh && ./prod_statesync.sh
```

After running the script, your node will be ready to run as a full node!

Please wait until your node is fully synchronized by typing: `oraid status &> status.json && cat status.json | jq '{catching_up: .SyncInfo.catching_up}'`. If the **catching up** status is **false**, you can continue.

#### 5. Start the node in background mode

Please stop the running node and follow the below steps to start the node in background mode (this step should only be run after the your node has fully synchronized with the network after step 5)

```bash
docker-compose restart orai && docker-compose exec -d orai bash -c 'oraivisor start --p2p.pex true'
```

If you do not specify the **--p2p.persistent\_peers** flags, you must add at least a persistent peer connection in the **.oraid/config/config.toml** file before running the below command, otherwise your node will not be able to connect to the Oraichain network.

The above commands run as the background process so when you turn off your Terminal, it is still running. You can always run them in the foreground process by removing the "-d" flag.

#### 6. Create validator transaction

You need to store two following files: .oraid/config/node\_key.json, .oraid/config/priv\_validator\_key.json. They contain your validator information for voting. Create backups for these files, otherwise you will lose your validator node if something wrong happens.

You can check your wallet information by typing: `oraid query auth account <your-validator-wallet-address>` inside of the container or through the explorer, where you import your wallet. To prevent spamming, your wallet is not activated by default. As a result, it needs to receive at least one MsgSend transaction from a different account. In other words, you should receive some ORAI tokens (minimum of 10^-6 ORAI) to continue. When your wallet has some tokens, please enter the container and type:

```bash
wget -O /usr/bin/fn https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/fn.sh && chmod +x /usr/bin/fn && fn createValidator
```

#### 7. Check your node status with voting power

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

**BACK UP TWO IMPORTANT FILES: .oraid/config/node_key.json & .oraid/config/priv_validator_key.json.**
**TO PREVENT DOUBLE SIGNING, BACK UP THE .oraid/data/priv_validator_state.json FILE BEFORE MIGRATING TO A DIFFERENT VALIDATOR NODE**

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
