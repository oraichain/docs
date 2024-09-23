---
description: Instructions to install the oraid binary and run as a service by systemd
---

# Oraid Installation and setup

## Preresquites

### Operating System

This tutorial assumes that your node is running Ubuntu LTS version (i.e: 18.04, 20.04 or 22.04). It does not work with Ubuntu 16.04 or older versions.

### Go version (required)

The Golang version should be from 1.18 and above
If you have not installed it yet, you can refer to [this document](https://github.com/oraichain/docs/blob/master/nodes-validators/tutorials/install-go.md).

Make sure that `$GOPATH` is in your `$PATH`. It's the crucial part of this tutorial.
### Make (required)

If your node does not have Make, install using: `sudo apt update && sudo apt install make`

### Gcc (required)

You need to install Gcc to build the binary. Type: `sudo apt update && sudo apt install gcc`

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
git clone https://github.com/oraichain/orai.git

# enter the repo
cd orai

# checkout the latest tag
git checkout <tag>
```

The `<version-tag>` will need to be set to either a testnet or the latest mainnet version tag.

{% hint style="warning" %}
The current mainnet version tag will be `v0.42.4` - i.e:

```bash
git checkout v0.42.4
```
{% endhint %}

Next, you should be able to build the binary file using the below command:

```bash
# go to main folder ($ORAI_HOME/orai)
go mod tidy
make install
```
After running the above commands, your `oraid` binary can be found in `$GOPATH/bin`.
To confirm that the installation is succeeded, you can run (please make sure that `$GOPATH/bin` is in your `$PATH`):

```bash
oraid version
```

The current binary version for Linux users is v0.42.4

Libwasmvm version: ```oraid query wasm libwasmvm-version```, which should give: 1.5.2

## Initialize Orai Node

Use oraid to initialize your node (replace the NODE_NAME with a name of your choosing):

```bash
oraid init NODE_NAME --home $ORAI_HOME/.oraid --chain-id Oraichain
```

Download and place the genesis file in the orai config folder:

```bash
wget -O $ORAI_HOME/.oraid/config/genesis.json https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/genesis.json
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

## Set Up Cosmovisor

(You may also refer to the Cosmovisor [installation instructions](https://github.com/cosmos/cosmos-sdk/tree/main/tools/cosmovisor#installation).)
Set up cosmovisor to ensure any future upgrades happen flawlessly. To install Cosmovisor: 

```bash
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@latest
```

Create the required directories, and Copy the current oraid binary into the cosmovisor/genesis folder:

```bash
mkdir -p $ORAI_HOME/.oraid/cosmovisor
mkdir -p $ORAI_HOME/.oraid/cosmovisor/genesis
mkdir -p $ORAI_HOME/.oraid/cosmovisor/genesis/bin
mkdir -p $ORAI_HOME/.oraid/cosmovisor/upgrades
cp $GOPATH/bin/oraid $ORAI_HOME/.oraid/cosmovisor/genesis/bin
```

Set the environment variables

```bash
echo "# Setup Cosmovisor" >> ${HOME}/.profile
echo "export ORAI_HOME=/root" >> ${HOME}/.profile
echo "export DAEMON_NAME=oraid" >> ${HOME}/.profile
echo "export DAEMON_HOME=$HOME/.oraid" >> ${HOME}/.profile
echo "export DAEMON_ALLOW_DOWNLOAD_BINARIES=false" >> ${HOME}/.profile
echo "export DAEMON_LOG_BUFFER_SIZE=512" >> ${HOME}/.profile
echo "export DAEMON_RESTART_AFTER_UPGRADE=true" >> ${HOME}/.profile
echo "export UNSAFE_SKIP_BACKUP=true" >> ${HOME}/.profile
source ~/.profile
```

To check your work, ensure the version of cosmovisor and oraid are the same:

```bash
cosmovisor version
oraid version
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
├── cosmovisor
│   ├── current -> /root/.oraid/cosmovisor/genesis
│   ├── genesis
│   │   └── bin
│   │       └── oraid
│   └── upgrades
└── data
    └── priv_validator_state.json
```
7 directories, 8 files

## Download Chain Data

Download liblz4-tool to handle the compressed file, then Download & Decompress the snapshot:

```bash
# install lib
cd $ORAI_HOME/.oraid
sudo apt-get install wget liblz4-tool aria2 -y
```

We provide a snapshot file every hour, available at https://snapshot.orai.io/. Please change [SNAPSHOT_URL] to the link provided.

```bash
wget -O oraichain_latest.tar.lz4 [SNAPSHOT_URL]
lz4 -c -d oraichain_latest.tar.lz4 | tar -x -C $ORAI_HOME/.oraid
```

## Edit config

```bash
vim $ORAI_HOME/.oraid/config/config.toml
```

Update seed address
```bash
seeds = "e18f82a6da3a9842fa55769955d694f62f7f48bd@seed1.orai.zone:26656,defeea41a01b5afdb79ef2af155866e122797a9c@seed4.orai.zone:26656"
```

Some seeds address from Oraichain validators group:
```
f223f1be06ef35a6dfe54995f05daeb1897d94d7@seed-node.mms.team:42656
8542cd7e6bf9d260fef543bc49e59be5a3fa9074@seed.publicnode.com:26656
fe0a0d46eb5436905bf8465f83d2da5a503bf4eb@mainnet-seed.konsortech.xyz:33165
ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@seeds.polkachu.com:23356
5f5cfac5c38506fbb4275c19e87c4107ec48808d@seeds.nodex.one:11210
49165f4ef94395897d435f144964bdd14413ea28@seed.orai.synergynodes.com:26656
```

## Set Up Orai Service

Set up a service to allow cosmovisor to run in the background as well as restart automatically if it runs into any problems:

```bash
cat > /tmp/orai.service <<EOF
[Unit]
Description=Cosmovisor daemon
After=network-online.target
[Service]
Environment="ORAI_HOME=/root"
Environment="DAEMON_NAME=oraid"
Environment="DAEMON_HOME=${ORAI_HOME}/.oraid"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_LOG_BUFFER_SIZE=512"
Environment="UNSAFE_SKIP_BACKUP=true"
User=$USER
ExecStart=$GOPATH/bin/cosmovisor run start --home ${ORAI_HOME}/.oraid --minimum-gas-prices=0.001orai
Restart=always
RestartSec=3
LimitNOFILE=infinity
LimitNPROC=infinity
[Install]
WantedBy=multi-user.target
EOF
```

Move this new file to the systemd directory:

```bash
sudo mv /tmp/orai.service /etc/systemd/system/orai.service
```

Reload and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl restart systemd-journald
sudo systemctl start orai
sudo systemctl enable orai
```

Check the status of the service:

```bash
sudo systemctl status orai
```

To see live logs of the service:

```bash
journalctl -u orai -f
```

Check your node status

```bash
oraid status
# OR
oraid status 2>&1 | jq .SyncInfo
# OR
curl -s localhost:26657/status | grep "catching_up"
# OR
oraid status 2>&1 | jq .SyncInfo.check_status
```

If the catching_up status is false, your node finishes syncing process.
Finally, you can delete the snapshot file and backup your config folder.
The snapshot file may be outdated; you can reach out to our community for it.

Please join the [Oraichain validators group](https://t.me/joinchat/yH9nMLrokQRhZGY1) on Telegram to discuss ideas and problems!
