# Become a Full Node Operator

This tutorial helps full node operators quickly synchronize with the Oraichain mainnet by downloading a storage snapshot prepared by the team. The downloading speed is much faster than synchronizing from the first block, which allows fast set up to join the network in no time!

## Hardware specifications for an Oraichain node:

Please take a look [here](./#node-hardwarde-specification)

## Setup the node

### 1. Download and run the setup file

```bash
curl -o docker-compose.yml https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/docker-compose.latest.yml && curl -o setup.sh https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/setup.latest.sh && chmod +x setup.sh && curl -OL https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/orai.env
```

### 2. Edit your moniker in the orai.env file you have just downloaded

In your `orai.env` file, it's a good idea to set the `LOG_LEVEL` parameter to `"info"` if you want to see all the container logs.

### 3. Build and enter the container

With docker, your node can run on any platforms. As a result, it is a must to install and download Docker & docker-compose. Afterward, please type:

```bash
docker-compose pull && docker-compose up -d --force-recreate
```

This command starts setting up a container for your Oraichain node. It runs several commands to get a statesync node ready. You can see these steps in the docker-compose file. For more details, the setup.sh file has many Bash scripts that handle different tasks automatically. Also, it makes a special folder called .oraid, which keeps all important settings and data for your node.

### 4. Statesync to synchronize your node

Statesync is the quickest way to let your node catch up with the network by starting at a specified trusted height and fetch snapshot data from other peers.

When you execute the aforementioned command, it initiates statesync in your container. This involves retrieving and applying a snapshot. Following this, your node will begin synchronizing new blocks, a process that typically takes around **30 minutes**. You can monitor the progress of this operation by viewing the container log using the command provided below:

```bash
docker-compose logs -f orai
```

After running the script, your node will be ready to run as a full node! Please wait until your node is fully synchronized by typing (from your container):&#x20;

```bash
oraid status &> status.json && cat status.json | jq '{catching_up: .SyncInfo.catching_up}'
```

If the **catching up** status is **false**, your node finished syncing process.
