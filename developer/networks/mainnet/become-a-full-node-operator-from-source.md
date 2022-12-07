---
description: Instruction to install the oraid binary in docker
---

# Oraid Installation and setup

## Preresquites

### Operating System

This tutorial assumes that your node is using Ubuntu 18.04 or 20.04. It does not work with Ubuntu 16.04

### Go version

The Golang version should be from 1.18 and above

### Make

If your node does not have Make, install using: `apt update && apt install make`

### Gcc

You need to install Gcc to build the binary. Type: `apt update && apt install gcc`

## Build the binary from source

```bash
# clone the Oraichain network repository
git clone https://github.com/oraichain/orai.git

# enter the repo
cd orai

# checkout the latest tag
git checkout <tag>
```

The `<version-tag>` will need to be set to either a testnet or the latest mainnet version tag.

{% hint style="warning" %}
The current mainnet version tag will be `v0.41.1` - i.e:

```bash
git checkout v0.41.1
```
{% endhint %}

Next, you should be able to build the binary file using the below command:

```bash
# in orai dir
go get ./...
make build VERSION=0.41.1
```

To confirm that the installation has succeeded, you can run:

```bash
oraid version
```

The current binary version for Linux users is v0.41.1, the checksum value may vary due to go.mod being updated after `go get ./...`

Libwasmvm version: `oraid query wasm libwasmvm-version`, which should give: 1.1.2

## Connecting to the network as a full-node operator

### Init config

First, we need to initiate config files for the node:

`oraid init foobar --chain-id Oraichain`

The default directory of .oraid is located in $PWD, not $HOME like other Cosmos networks.

### Update Genesis file

Next, replace the '.oraid/config/genesis.json' to the official genesis file of the Oraichain network: 

`wget -O $PWD/.oraid/config/genesis.json https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/genesis.json`

### Download the network's snapshot

`wget -O - <chain-dara-url> | tar -zxvf -`

where `<chain-data-url>` can be found in [Here](./README.md#chain-data-download-urls)

then move all data & wasm files into .oraid directory:

`mv data/* .oraid/data && mv wasm/ .oraid`

### Choose persistent peers

To connect to the Oraichain network you can either run your own node or connect to a public node's RPC endpoint. To find a public node to connect to consider looking in the official [chain registry](https://github.com/cosmos/chain-registry/blob/master/oraichain/chain.json) of Cosmos. Connecting to a public node requires the least configuration but you should be sure that you trust whatever node that you choose.

Below are also several options you can choose from to connect to the network:

```bash
4d0f2d042405abbcac5193206642e1456fe89963@3.134.19.98:26656,24631e98a167492fd4c92c582cee5fd6fcd8ad59@162.55.253.58:26656,bf083c57ed53a53ccd31dc160d69063c73b340e9@3.17.175.62:26656,35c1f999d67de56736b412a1325370a8e2fdb34a@5.189.169.99:26656,5ad3b29bf56b9ba95c67f282aa281b6f0903e921@64.225.53.108:26656,d091cabe3584cb32043cc0c9199b0c7a5b68ddcb@seed.orai.synergynodes.com:26656
```

### Start the node

oraid start --p2p.persistent_peers "<node-id1>@<pubclic-ip1>:26656,<node-id2>@<public-ip2>:26656"