---
Learn how to install the `oraid` binary, set up your node, and run it as a service using `systemd`.
---

# 🚀 Oraichain Node Setup Guide

Everything you need to go from zero to running a fully synced Oraichain node — the right way.

---

## 🧰 Prerequisites

Before diving in, make sure your system meets the following requirements.

### ✅ OS Compatibility

- Ubuntu LTS versions only: **22.04**
- ❌ Not supported: Ubuntu **16.04** or older

### ✅ Go (Golang)

- Required version: **1.22.6+**
- Follow the [Go installation guide](https://github.com/oraichain/docs/blob/master/nodes-validators/tutorials/install-go.md) if not already installed.
- Ensure `$GOPATH/bin` is included in your `$PATH`.

### ✅ Essential Packages

Install these if not already available:

```bash
sudo apt update && sudo apt install make gcc -y
```

---

## 🏗️ Build `oraid` from Source

Let’s compile the node binary from scratch.

### 1. Set your ORAI working directory

```bash
export ORAI_HOME="/root"
```

> 📝 If you're not using `/root`, replace all occurrences of `$ORAI_HOME` with your desired path.

### 2. Clone and build

```bash
cd $ORAI_HOME
git clone https://github.com/oraichain/wasmd
cd wasmd
git checkout v0.50.9  # or latest tag
make build
```

> ⚠️ Confirm the binary version:
```bash
oraid version
```

Expected output: `v0.50.9`

---

## ⚙️ Initialize Your Node

```bash
oraid init NODE_NAME --home $ORAI_HOME/.oraid --chain-id Oraichain
```

(*replace the NODE_NAME with a name of your choosing*)

Download the latest genesis file:

```bash
wget -O $ORAI_HOME/.oraid/config/genesis.json https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/genesis.json
```

✅ **Resulting directory structure:**

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

---

## 🧠 Setup Cosmovisor (Optional but Recommended)

Cosmovisor makes future upgrades smooth and stress-free.

### 1. Install Cosmovisor

```bash
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@latest
```

### 2. Set up folders

```bash
mkdir -p $ORAI_HOME/.oraid/cosmovisor/genesis/bin
mkdir -p $ORAI_HOME/.oraid/cosmovisor/upgrades
cp $GOPATH/bin/oraid $ORAI_HOME/.oraid/cosmovisor/genesis/bin
```

### 3. Add environment variables

```bash
echo "# Cosmovisor Setup" >> ~/.profile
echo "export ORAI_HOME=/root" >> ~/.profile
echo "export DAEMON_NAME=oraid" >> ~/.profile
echo "export DAEMON_HOME=$ORAI_HOME/.oraid" >> ~/.profile
echo "export DAEMON_ALLOW_DOWNLOAD_BINARIES=false" >> ~/.profile
echo "export DAEMON_LOG_BUFFER_SIZE=512" >> ~/.profile
echo "export DAEMON_RESTART_AFTER_UPGRADE=true" >> ~/.profile
echo "export UNSAFE_SKIP_BACKUP=true" >> ~/.profile
source ~/.profile
```

Check installation:

```bash
cosmovisor version
oraid version
```

---

## ⛓️ Sync Chain Data

Download and extract the latest snapshot:

```bash
sudo apt install wget liblz4-tool aria2 -y
cd $ORAI_HOME/.oraid
wget -O oraichain_latest.tar.lz4 [SNAPSHOT_URL]
lz4 -c -d oraichain_latest.tar.lz4 | tar -x -C $ORAI_HOME/.oraid
```

We provide a snapshot file every hour, available at https://snapshot.orai.io/. Please change [SNAPSHOT_URL] to the provided link.

---

## 🛠️ Update Node Config

Edit config:

```bash
vim $ORAI_HOME/.oraid/config/config.toml
```

Update seeds:

```toml
seeds = "e18f82a6da3a9842fa55769955d694f62f7f48bd@seed1.orai.zone:26656,defeea41a01b5afdb79ef2af155866e122797a9c@seed4.orai.zone:26656"
```

> You may also add more from this list:
```
f223f1be06ef35a6dfe54995f05daeb1897d94d7@seed-node.mms.team:42656
8542cd7e6bf9d260fef543bc49e59be5a3fa9074@seed.publicnode.com:26656
fe0a0d46eb5436905bf8465f83d2da5a503bf4eb@mainnet-seed.konsortech.xyz:33165
ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@seeds.polkachu.com:23356
5f5cfac5c38506fbb4275c19e87c4107ec48808d@seeds.nodex.one:11210
49165f4ef94395897d435f144964bdd14413ea28@seed.orai.synergynodes.com:26656
...
```

---

## 🔧 Run as a Systemd Service

Create a service file:

```bash
cat > /tmp/orai.service <<EOF
[Unit]
Description=Oraichain Cosmovisor Node
After=network-online.target

[Service]
User=$USER
Environment="ORAI_HOME=/root"
Environment="DAEMON_NAME=oraid"
Environment="DAEMON_HOME=${ORAI_HOME}/.oraid"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_LOG_BUFFER_SIZE=512"
Environment="UNSAFE_SKIP_BACKUP=true"
ExecStart=$GOPATH/bin/cosmovisor run start --home ${ORAI_HOME}/.oraid --minimum-gas-prices=0.001orai
Restart=always
RestartSec=3
LimitNOFILE=infinity
LimitNPROC=infinity

[Install]
WantedBy=multi-user.target
EOF

sudo mv /tmp/orai.service /etc/systemd/system/orai.service
```

Enable and start:

```bash
sudo systemctl daemon-reload
sudo systemctl start orai
sudo systemctl enable orai
```

---

## 🩺 Monitor Your Node

### Check service status

```bash
sudo systemctl status orai
```

### View logs live

```bash
journalctl -u orai -f
```

### Sync status

```bash
oraid status | jq .SyncInfo
# OR
curl -s localhost:26657/status | grep "catching_up"
```

✅ If `catching_up` is `false`, you're fully synced!

---

## 👥 Join the Community

Join our Telegram group to get help, share ideas, and stay up to date:

👉 [Oraichain Validators Group](https://t.me/joinchat/yH9nMLrokQRhZGY1)

Some useful resource from validator community:

+ [https://polkachu.com/networks/orai](https://polkachu.com/networks/orai)
+ [https://itrocket.net/services/mainnet/oraichain](https://itrocket.net/services/mainnet/oraichain/)
+ [https://www.synergynodes.com/service/oraichain](https://www.synergynodes.com/service/oraichain/)
+ [https://ccvalidators.com/oraichain](https://ccvalidators.com/oraichain/)
+ [https://cosmos.directory/oraichain](https://cosmos.directory/oraichain/)
