# Become a Full Node Operator

This tutorial helps full node operators quickly synchronize with the Oraichain mainnet by downloading a storage snapshot prepared by the team. The downloading speed is much faster than synchronizing from the first block, which allows fast set up to join the network in no time!

I assume you have installed Docker on the server; if not, you may refer to the following [instructions](https://github.com/oraichain/docs/blob/master/developer/tutorials/install-docker.md) before getting started.

You can choose one of the two methods below.

## Hardware specifications for an Oraichain node:

Please take a look [here](./#node-hardwarde-specification)

## Setup the node using snapshot (recomend using)

First, you need to create a directory and navigate into it. Eg: Eg: *** mkdir oraichain && cd oraichain ***

### 1. Download and run the setup file

```bash
curl -o docker-compose.yml https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/docker-compose.init.yml && curl -o setup.sh https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/setup.init.sh && chmod +x setup.sh && curl -OL https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/orai.env
```

### 2. Edit your moniker in the orai.env file you have just downloaded

Please review all the values in the orai.env file once. If you have experience running a validator, adjust the parameters according to your experience. Otherwise, you just need to change the ***MONIKER*** value.
In your `orai.env` file, it's a good idea to set the `LOG_LEVEL` parameter to `"info"` if you want to see all the container logs.

Also, please review the port configuration in the docker-compose.yml file. You can remove any ports you do not want to expose to the internet.

### 3. Init orai node

```bash
docker-compose up -d && docker-compose exec orai bash -c "sh setup.sh"
```

Finally, your working directory should be like below

```
├── .oraid
│   ├── config
│   │   ├── app.toml
│   │   ├── client.toml
│   │   ├── config.toml
│   │   ├── genesis.json
│   │   ├── node_key.json
│   │   └── priv_validator_key.json
│   └── data
│       └── priv_validator_state.json
├── docker-compose.yml
├── orai.env
└── setup.sh
```
3 directories, 10 files

### 4. Download snapshot

```bash
wget -O oraichain_latest.tar.lz4 https://orai.s3.us-east-2.amazonaws.com/snapshots/oraichain_latest.tar.lz4
rm -rf data
lz4 -c -d oraichain_latest.tar.lz4 | tar -x -C ./.oraid/
```

### 5. Init orai node and wait for syncing to latest block

```bash
docker-compose exec -d orai bash -c "oraivisor start"
```

Check container log

```bash
docker-compose logs orai -f --tail 0
```

Check your node status

```bash
curl -s localhost:26657/status | grep "catching_up"
```

If the catching_up status is false, your node finished syncing process.
Finally, you can delete snapshot file and backup your config folder.
The snapshot file may be outdated; you can reach out to our community for it.

## Setup the node using statesync (rarely using)

***!!! This method requires the RAM configuration to be more than 8GB !!!***

First, you need to create a directory and navigate into it. Eg: *** mkdir oraichain && cd oraichain ***

### 1. Download and run the setup file

```bash
curl -o docker-compose.yml https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/docker-compose.latest.yml && curl -o setup.sh https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/setup.latest.sh && chmod +x setup.sh && curl -OL https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/orai.env
```

### 2. Edit your moniker in the orai.env file you have just downloaded

In your `orai.env` file, it's a good idea to set the `LOG_LEVEL` parameter to `"info"` if you want to see all the container logs.

Also, please review the port configuration in the docker-compose.yml file. You can remove any ports you do not want to expose to the internet.

### 3. Build and enter the container

With docker, your node can run on any platforms. As a result, it is a must to install and download Docker & docker-compose. Afterward, please type:

```bash
docker-compose pull && docker-compose up -d --force-recreate
```

This command starts setting up a container for your Oraichain node. It runs several commands to get a statesync node ready. You can see these steps in the docker-compose file. For more details, the setup.sh file has many Bash scripts that handle different tasks automatically. Also, it makes a special folder called .oraid, which keeps all important settings and data for your node.

### 4. Statesync to synchronize your node

Statesync is the quickest way to let your node catch up with the network by starting at a specified trusted height and fetch snapshot data from other peers.

When you execute the aforementioned command, it initiates statesync in your container. This involves retrieving and applying a snapshot. Following this, your node will begin synchronizing new blocks, a process that typically takes around **30 minutes ~ 1 hour**. You can monitor the progress of this operation by viewing the container log using the command provided below:

```bash
docker-compose logs -f orai --tail 0
```

After running the script, your node will be ready to run as a full node! Please wait until your node is fully synchronized by typing (from your container):&#x20;

```bash
oraid status &> status.json && cat status.json | jq '{catching_up: .SyncInfo.catching_up}'
```

If the **catching up** status is **false**, your node finished syncing process.

Please join the [Oraichain validators group](https://t.me/joinchat/yH9nMLrokQRhZGY1) on Telegram to discuss ideas and problems!