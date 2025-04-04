---
description: Instructions to install the oraid binary and run as a service by systemd
---

# Oraid Installation and setup

## Preresquites

### Operating System

This tutorial assumes that your node is running Ubuntu LTS version (i.e: 18.04, 20.04 or 22.04). It does not work with Ubuntu 16.04 or older versions.

### Go version (required)

The Golang version should be from 1.22.11 and above
If you have not installed it yet, you can refer to [this document](https://github.com/oraichain/docs/blob/master/developer/tutorials/install-go.md).

Make sure that `$GOPATH` is in your `$PATH`. It's the crucial part of this tutorial.
### Make (required)

If your node does not have Make, install using: 
```bash
sudo apt update && sudo apt install make
```

### Gcc (required)

You need to install Gcc to build the binary. Type: 
```bash
sudo apt update && sudo apt install gcc
```

## Build the binary from source

Please define the `$ORAI_HOME` environment variable which will be used as the working directory, in this tutorial we will assume that your `$ORAI_HOME` is `root`. If you don't define it, all of the following installations will be using your `$HOME` folder as `$ORAI_HOME`, please replace `$ORAI_HOME` with `$HOME` in the corresponding commands (except export ORAI_HOME command).

Make sure your user has enough permissions to write data to the `$ORAI_HOME` folder.

```bash
# Export ORAI_HOME env variable
export ORAI_HOME="/root"
```

```bash
# clone the Oraichain network repository
cd $ORAI_HOME
git clone https://github.com/oraichain/wasmd.git

# enter the repo
cd wasmd

# checkout the latest tag
git checkout <tag>
```

The `<version-tag>` will need to be set to either a testnet or the latest mainnet version tag.

{% hint style="warning" %}
The current mainnet version tag will be `v0.50.9` - i.e:

```bash
git checkout v0.50.9
```
{% endhint %}

Next, you should be able to build the binary file using the below command:

```bash
# go to main folder ($ORAI_HOME/wasmd)
cd wasmd
make build
```
After running the above commands, your `oraid` binary can be found in `$GOPATH/bin`.
To confirm that the installation is succeeded, you can run (please make sure that `$GOPATH/bin` is in your `$PATH`):

```bash
oraid version
```

The current binary version for Linux users is v0.50.9

Libwasmvm version: ```oraid query wasm libwasmvm-version```, which should give: 2.1.3

# Initialize Orai Testnet Node

Use oraid to initialize your node (replace the NODE_NAME with a name of your choosing):

```bash
oraid init NODE_NAME --home $ORAI_HOME/.oraid --chain-id Oraichain-testnet
```

<!-- TODO: // need to export genesis.json file of testnet -->
Download and place the genesis file in the orai config folder:
```bash
sudo apt-get install wget -y
wget -O $ORAI_HOME/.oraid/config/genesis.json https://orai.s3.us-east-2.amazonaws.com/testnet/genesis.20240117.json
```

### Finally, your working directory should be like below:
```
$ORAI_HOME/.oraid/
├── config
│   ├── app.toml
│   ├── client.toml
│   ├── config.toml
│   ├── genesis.json
│   ├── node_key.json
│   └── priv_validator_key.json
└── data
    └── priv_validator_state.json
```
2 directories, 7 files

# Setup to run node

<!-- Download liblz4-tool to handle the compressed file, then Download & Decompress the snapshot:

```bash
cd $ORAI_HOME/.oraid
sudo apt-get install wget liblz4-tool aria2 -y
wget -O oraichain_latest.tar.lz4 https://orai.s3.us-east-2.amazonaws.com/snapshots/oraichain_latest.tar.lz4
lz4 -c -d oraichain_latest.tar.lz4 | tar -x -C $ORAI_HOME/.oraid
``` -->

## Edit config

```bash
vim $ORAI_HOME/.oraid/config/config.toml
```

Update seed and persistent_peers address
```bash
seeds = "e80c9d494188635284bb529308330cac10c326e9@143.198.28.190:26656, e80c9d494188635284bb529308330cac10c326e9@143.198.28.190:26656"
persistent_peers = "e80c9d494188635284bb529308330cac10c326e9@143.198.28.190:26656, e80c9d494188635284bb529308330cac10c326e9@143.198.28.190:26656"
```

Start your node
```bash
oraid start --home $ORAI_HOME/.oraid
```
