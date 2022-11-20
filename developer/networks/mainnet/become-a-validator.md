# Become a Validator

## Become a validator

This tutorial helps validators and full nodes quickly synchronize with the Oraichain mainnet by downloading a storage snapshot prepared by the team. The downloading speed is much faster than synchronizing from the first block, which allows fast set up to join the network in no time!

If you want to synchronize your node using the traditional method, please follow [this tutorial](https://github.com/oraichain/oraichain-static-files/blob/master/Validator.md) instead.

### Hardware specifications for an Oraichain node

Please take a look [here](./#node-hardwarde-specification)

### Setup the validator node

#### 1. Download and run the setup file

```bash
curl -OL https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/docker-compose.yml && curl -OL https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/orai.env
```

#### 2. Edit wallet name and moniker you prefer to create a new wallet and validator in the orai.env file you have just downloaded

#### 3. Build and enter the container

With docker, your validator can run on any platforms. As a result, it is a must to install and download Docker & docker-compose. Afterward, please type:

```bash
docker-compose pull && docker-compose up -d --force-recreate
```

#### 4. Initiate the node

Please enter the container and type:

```bash
oraid init "$MONIKER" --home /workspace/.oraid
```

then, you need to download the network's genesis file by using the following command inside your container:

```bash
wget -O /workspace/.oraid/config/genesis.json https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/genesis.json
```

#### 5. Download Chain Data

Download the latest chain data from a snapshot provider. Select the tab to the desired node type (Default or Pruned). A Pruned node will have the smallest disk size possible, but it will only keep the latest network state, which is recommended for sentry, seed nodes & light clients. Meanwhile, a Default node will store more network state history, but it will have larger size, which is suitable for validators.

Click [Here](./#chain-data-download-urls) to view the chain data download URLs from different regions.

* Command:

```bash
docker-compose exec orai bash -c 'wget -O - <chain-dara-url> | tar -zxvf -'
```

After extracting the chain data, you need to move such data into the .oraid/ directory:

```bash
docker-compose exec orai bash -c 'mv /workspace/data/* /workspace/.oraid/data && mv /workspace/wasm/ /workspace/.oraid && rmdir /workspace/data'
```

#### 6. Initiate your validator wallet

Please enter your container and type:

```
oraid keys add $USER 2>&1 | tee account.txt && exit
```

After running, there will be an account.txt file generated, which stores your account information as well as its mnemonic. Please keep it safe, and remove the file when you finish storing your account information.

### Start the network

Please exit the container and follow the below steps to start the nodes

### List of seed & sentry nodes that you can connect to

```bash
d263c8c8fdd279d33de643c3baffbe053d0a8a73@18.223.242.70:26656
9749da4a81526266d7b8fe9a03d260cd3db241ad@18.116.209.76:26656
8a8c58e5514c86051940b3bbe39db2817de62288@34.79.231.38:26656
59d49e39d507bb190e746bcf5590d65879c132e2@13.79.247.74:26656
35c1f999d67de56736b412a1325370a8e2fdb34a@5.189.169.99:26656
5ad3b29bf56b9ba95c67f282aa281b6f0903e921@64.225.53.108:26656
d091cabe3584cb32043cc0c9199b0c7a5b68ddcb@seed.orai.synergynodes.com:26656
```

#### 1. Start the node

```bash
docker-compose restart orai && docker-compose exec -d orai bash -c 'oraivisor start --p2p.pex false --p2p.persistent_peers "<node-id1>@<pubclic-ip1>:26656,<node-id2>@<public-ip2>:26656"'
```

If you do not specify the **--p2p.persistent\_peers** flags, you must add at least a persistent peer connection in the **.oraid/config/config.toml** file before running the below command, otherwise your node will not be able to connect to the Oraichain network.

The above commands run as the background process so when you turn off your Terminal, it is still running. You can always run them in the foreground process by removing the "-d" flag.

#### 3. Wait until your node is synchronized

Please wait until your node is fully synchronized by typing: `oraid status &> status.json && cat status.json | jq '{catching_up: .SyncInfo.catching_up}'`. If the **catching up** status is **false**, you can continue.

#### 4. Create validator transaction

You need to store two following files: .oraid/config/node\_key.json, .oraid/config/priv\_validator\_key.json. They contain your validator information for voting. Create backups for these files, otherwise you will lose your validator node if something wrong happens.

You can check your wallet information by typing: `oraid query auth account <your-validator-wallet-address>` inside of the container or through the explorer, where you import your wallet. To prevent spamming, your wallet is not activated by default. As a result, it needs to receive at least one MsgSend transaction from a different account. In other words, you should receive some ORAI tokens (minimum of 10^-6 ORAI) to continue. When your wallet has some tokens, please enter the container and type:

```bash
wget -O /usr/bin/fn https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/fn.sh && chmod +x /usr/bin/fn && fn createValidator
```

#### 5. Check your node status with voting power

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
