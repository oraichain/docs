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
The current mainnet version tag will be `v0.41.3` - i.e:

```bash
git checkout v0.41.3-statesync-script
```
{% endhint %}

Next, you should be able to build the binary file using the below command:

```bash
# in orai dir
go mod tidy
make install
```

To confirm that the installation has succeeded, you can run:

```bash
oraid version
```

The current binary version for Linux users is v0.41.3-statesync-script

Libwasmvm version: ```oraid query wasm libwasmvm-version```, which should give: 1.2.4

## Connecting to the network as a full-node operator

Statesync is the quickest way to let your node catch up with the network by starting at a specified trusted height and fetch snapshot data from other peers.

To use the statesync method, please run the following script:

```sh
./scripts/prod_statesync.sh
```

After running the script, your node will be ready to run as a full node!
