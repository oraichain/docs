# Become a Full Node Operator

This tutorial helps full node operators quickly synchronize with the Oraichain mainnet by downloading a storage snapshot prepared by the team. The downloading speed is much faster than synchronizing from the first block, which allows fast set up to join the network in no time!

## Hardware specifications for an Oraichain node:

Please take a look [here](./#node-hardwarde-specification)

## Setup the node

### 1. Download and run the setup file

```bash
curl -OL https://raw.githubusercontent.com/oraichain/orai/v0.41.3-statesync-script/docker-compose.prod.yml && curl -OL https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/orai.env
```

### 2. Edit your moniker in the orai.env file you have just downloaded

### 3. Build and enter the container

With docker, your node can run on any platforms. As a result, it is a must to install and download Docker & docker-compose. Afterward, please type:

```bash
docker-compose pull && docker-compose up -d --force-recreate
```

### 4. Statesync to synchronize your node

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
