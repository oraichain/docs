# Become a full node operator

This tutorial helps full node operators quickly synchronize with the Oraichain mainnet by downloading a storage snapshot prepared by the team. The downloading speed is much faster than synchronizing from the first block, which allows fast set up to join the network in no time!

## Hardware specifications for an Oraichain node:

Please take a look [here](./README.md#node-hardwarde-specification)

## Setup the node

### 1. Download and run the setup file

```bash
curl -OL https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/docker-compose.yml && curl -OL https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/orai.env
```

### 2. Edit your moniker in the orai.env file you have just downloaded

### 3. Build and enter the container

With docker, your node can run on any platforms. As a result, it is a must to install and download Docker & docker-compose. Afterward, please type:

```bash
docker-compose pull && docker-compose up -d --force-recreate
```

### 4. Initiate the node

Please enter the container and type:

```bash
oraid init "$MONIKER" --home /workspace/.oraid
```

then, you need to download the network's genesis file by using the following command inside your container:

```bash
wget -O /workspace/.oraid/config/genesis.json https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/genesis.json

```

### 5. Download Chain Data

Download the latest chain data from a snapshot provider. Select the tab to the desired node type (Default or Pruned). A Pruned node will have the smallest disk size possible, but it will only keep the latest network state, which is recommended for sentry, seed nodes & light clients. Meanwhile, a Default node will store more network state history, but it will have larger size, which is suitable for validators.

Click [Here](./README.md#chain-data-download-urls) to view the chain data download URLs from different regions.

- Command:

``` bash
docker-compose exec orai bash -c 'wget -O - <chain-dara-url> | tar -zxvf -'
```

After extracting the chain data, you need to move such data into the .oraid/ directory:

```bash
docker-compose exec orai bash -c 'mv /workspace/data/* /workspace/.oraid/data && mv /workspace/wasm/ /workspace/.oraid && rmdir /workspace/data'
```

### 6. Start the node

Please exit the container and follow the below steps to start the nodes

#### List of seed & sentry nodes that you can connect to

```bash
4d0f2d042405abbcac5193206642e1456fe89963@3.134.19.98:26656,
24631e98a167492fd4c92c582cee5fd6fcd8ad59@162.55.253.58:26656,
bf083c57ed53a53ccd31dc160d69063c73b340e9@3.17.175.62:26656,
35c1f999d67de56736b412a1325370a8e2fdb34a@5.189.169.99:26656,
d091cabe3584cb32043cc0c9199b0c7a5b68ddcb@seed.orai.synergynodes.com:26656
```


```bash
docker-compose restart orai && docker-compose exec -d orai bash -c 'oraivisor start --p2p.pex false --p2p.persistent_peers "<node-id1>@<pubclic-ip1>:26656,<node-id2>@<public-ip2>:26656"'
```

If you do not specify the **--p2p.persistent_peers** flags, you must add at least a persistent peer connection in the **.oraid/config/config.toml** file before running the below command, otherwise your node will not be able to connect to the Oraichain network.

The above commands run as the background process so when you turn off your Terminal, it is still running. You can always run them in the foreground process by removing the "-d" flag.