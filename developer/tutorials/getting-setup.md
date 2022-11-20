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
docker run -v $(pwd):/workspace -it oraichain/foundation-orai:0.41.0-alpine-dev

# clone the Oraichain network repository
git clone https://github.com/oraichain/orai.git

# enter the repo
cd orai

# checkout the latest tag
git checkout <tag>
```

The `<version-tag>` will need to be set to either a testnet or the latest mainnet version tag.

{% hint style="warning" %}
For genesis, the current mainnet version tag will be `v0.41.0` - i.e:

```bash
git checkout v0.41.0
```
{% endhint %}

Next, you should verify the /lib/libwasmvm_muslc.a file located in the container to see if it matches the one that is published on the [Oraichain mainnet release](https://github.com/oraichain/orai/releases/download/v0.41.0/libwasmvm_muslc.a) using its checksum:

```md5sum /lib/libwasmvm_muslc.a  | cut -d ' ' -f 1```

The current checksum of the file should be: ```767ec1695ea6566524e5b3010d1df833```

Next, you should be able to build the binary file using the below command:

```bash
# in orai dir
go get ./...
make build LEDGER_ENABLED=false BUILD_TAGS=muslc GOMOD_FLAGS= VERSION=0.41.0
```

To confirm that the installation has succeeded, you can run:

```bash
oraid version
```

The current binary version is v0.41.0, which has the checksum value:

- Uncompressed: ```58bac3b5cbcddbf7714cfd16d70b52e3```
- Compressed: ```a59cf862cb3dc2d8c522f7f8aafd0374``` (UPX 3.96, Markus Oberhumer, Laszlo Molnar & John Reiser, Jan 23rd 2020) with command: ```upx --best --lzma /workspace/build/oraid```

## Connecting to the network

To connect to the Oraichain network you can either run your own node or connect to a public node's RPC endpoint. To find a public node to connect to consider looking in the official [chain registry](https://github.com/cosmos/chain-registry/blob/master/oraichain/chain.json) of Cosmos. Connecting to a public node requires the least configuration but you should be sure that you trust whatever node that you choose.

This tutorial allows you to build the orai binary from source. Nonetheless, we also have [another tutorial](https://docs.orai.io/developers/networks/mainnet/become-a-full-node-operator) that already automates this process, which is suitable for those who want to setup a quick node and connect to the Oraichain network.