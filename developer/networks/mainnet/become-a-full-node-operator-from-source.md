---
description: Instruction to install the oraid binary and run as service by systemd
---

# Oraid Installation and setup

## Preresquites

### Operating System

This tutorial assumes that your node is using Ubuntu 18.04 or 20.04. It does not work with Ubuntu 16.04

### Go version

The Golang version should be from 1.18 and above
If you have not installed it yet, you can refer to this document: https://github.com/oraichain/docs/blob/master/developer/tutorials/install-go.md

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
The current mainnet version tag will be `v0.41.6` - i.e:

```bash
git checkout v0.41.6
```
{% endhint %}

Next, you should be able to build the binary file using the below command:

```bash
# go to main folder (/root/orai/orai)
cd orai
go mod tidy
make install
```

To confirm that the installation has succeeded, you can run:

```bash
oraid version
```

The current binary version for Linux users is v0.41.6

Libwasmvm version: ```oraid query wasm libwasmvm-version```, which should give: 1.5.2

## Initialize Orai Node

Use oraid to initialize your node (replace the NODE_NAME with a name of your choosing):

```bash
oraid init NODE_NAME --home $HOME/.oraid
```

Download and place the genesis file in the orai config folder:

```bash
wget -O $HOME/.oraid/config/genesis.json https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/genesis.json
```

### Finally, your working directory should be like below:
```
/root/.oraid/
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
mkdir -p ~/.oraid/cosmovisor
mkdir -p ~/.oraid/cosmovisor/genesis
mkdir -p ~/.oraid/cosmovisor/genesis/bin
mkdir -p ~/.oraid/cosmovisor/upgrades
cp $GOPATH/bin/oraid ~/.oraid/cosmovisor/genesis/bin
```

Set the environment variables

```bash
echo "# Setup Cosmovisor" >> ~/.profile
echo "export DAEMON_NAME=oraid" >> ~/.profile
echo "export DAEMON_HOME=$HOME/.oraid" >> ~/.profile
echo "export DAEMON_ALLOW_DOWNLOAD_BINARIES=false" >> ~/.profile
echo "export DAEMON_LOG_BUFFER_SIZE=512" >> ~/.profile
echo "export DAEMON_RESTART_AFTER_UPGRADE=true" >> ~/.profile
echo "export UNSAFE_SKIP_BACKUP=true" >> ~/.profile
source ~/.profile
```

To check your work, ensure the version of cosmovisor and oraid are the same:

```bash
cosmovisor version
oraid version
```

### Finally, your working directory should be like below:
```
/root/.oraid/
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
cd $HOME/.oraid
sudo apt-get install wget liblz4-tool aria2 -y
wget -O oraichain_latest.tar.lz4 https://orai.s3.us-east-2.amazonaws.com/snapshots/oraichain_latest.tar.lz4
lz4 -c -d oraichain_latest.tar.lz4 | tar -x -C $HOME/.oraid
```

### Optional (problem with binnary version v0.41.6)

In the data folder, if the file upgrade-info.json does not exist, you can ignore this section. Otherwise, open the upgrade-info.json file and check the version.

```bash
cat $HOME/.oraid/data/upgrade-info.json
```

If version is not "v0.41.6" then delete this file

```bash
rm $HOME/.oraid/data/upgrade-info.json
```

## Edit config

```bash
vim $HOM/.oraid/config/config.toml
```

Update seed address
```bash
seeds = "e18f82a6da3a9842fa55769955d694f62f7f48bd@seed1.orai.zone:26656,893f246ffdffae0a9ef127941379303531f50d5c@seed2.orai.zone:26656,4fa7895fc43f618b53cd314585b421ee47b75639@seed3.orai.zone:26656,defeea41a01b5afdb79ef2af155866e122797a9c@seed4.orai.zone:26656"```
```

## Set Up Orai Service

Set up a service to allow cosmovisor to run in the background as well as restart automatically if it runs into any problems:

```bash
echo "[Unit]
Description=Cosmovisor daemon
After=network-online.target
[Service]
Environment="DAEMON_NAME=oraid"
Environment="DAEMON_HOME=${HOME}/.oraid"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_LOG_BUFFER_SIZE=512"
Environment="UNSAFE_SKIP_BACKUP=true"
User=$USER
ExecStart=${HOME}/go/bin/cosmovisor run start --home ${HOME}/.oraid --minimum-gas-prices=0.001orai
Restart=always
RestartSec=3
LimitNOFILE=infinity
LimitNPROC=infinity
[Install]
WantedBy=multi-user.target
" >orai.service
```

Move this new file to the systemd directory:

```bash
sudo mv orai.service /etc/systemd/system/orai.service
```

Reload and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl restart systemd-journald
sudo systemctl start orai
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
curl -s localhost:26657/status | grep "catching_up"
```

If the catching_up status is false, your node finished syncing process.
Finally, you can delete snapshot file and backup your config folder.
The snapshot file may be outdated; you can reach out to our community for it.
Please join the [Oraichain validators group](https://t.me/joinchat/yH9nMLrokQRhZGY1) on Telegram to discuss ideas and problems!