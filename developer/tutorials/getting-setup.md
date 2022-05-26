---
description: Instruction to install the oraid binary
---

# Oraid Installation and setup

## Choose an Operating System

The operating system you use for your node is entirely your personal preference. You will be able to compile the `oraid` daemon on most modern linux distributions and recent versions of macOS.

{% hint style="info" %}
For the tutorial, it is assumed that you are using an Ubuntu LTS release.

If you have chosen a different operating system, you will need to modify your commands to suit your operating system.
{% endhint %}

## Install Docker

By installing docker, you will not have to install Go & pre-requisites. Follow the instructions [here](https://docs.docker.com/engine/install/ubuntu/) to install Docker.

## Build Orai from source

```bash
# run the docker container with pre-built image from Oraichain
docker run -v $(pwd):/workspace -it orai/orai:0.40.4-ibc-sbuild

# clone the Oraichain network repository
git clone https://github.com/oraichain/orai.git

# enter the repo
cd orai

# checkout the latest tag
git checkout <tag>
```

The `<version-tag>` will need to be set to either a testnet or the latest mainnet version tag.

{% hint style="warning" %}
For genesis, the current mainnet version tag will be `v0.40.4-ibc-beta` - i.e:

```bash
git checkout v0.40.4-ibc-beta
```
{% endhint %}

Once you're on the correct tag, you can build:

```bash
# in orai dir
go get ./...
make build
```

To confirm that the installation has succeeded, you can run:

```bash
oraid version
```

## Connecting to the network

To connect to the Oraichain network you can either run your own node or connect to a public node's RPC endpoint. To find a public node to connect to consider looking in the official [chain registry](https://github.com/cosmos/chain-registry/blob/master/oraichain/chain.json) of Cosmos. Connecting to a public node requires the least configuration but you should be sure that you trust whatever node that you choose.

This tutorial allows you to build the orai binary from source. Nonetheless, we also have [another tutorial](./become-a-validator.md) that already automates this process, which is suitable for those who want to setup a quick node and connect to the Oraichain network.