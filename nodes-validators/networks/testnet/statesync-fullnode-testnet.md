# Oraid Installation and setup

You can see step of install and setup `oraid` [here](../testnet/become-full-node-testnet-from-source.md)


# Initialize Orai Testnet Node

Use oraid to initialize your node (replace the NODE_NAME with a name of your choosing):

```bash
oraid init NODE_NAME --home $ORAI_HOME/.oraid --chain-id Oraichain-fork
```

<!-- TODO: // need to export genesis.json file of testnet -->
Download and place the genesis file in the orai config folder:
```bash
sudo apt-get install wget liblz4-tool aria2 -y
wget -O $ORAI_HOME/.oraid/config/genesis.tar.lz4 https://orai.s3.us-east-2.amazonaws.com/testnet/genesis.tar.lz4
lz4 -c -d $ORAI_HOME/.oraid/config/genesis.tar.lz4 | tar -x -C $ORAI_HOME/.oraid/config/
rm -rf $ORAI_HOME/.oraid/config/genesis.tar.lz4 
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

# State-sync Oraichain testnet
* Create `state-sync.sh` file with the following code:
```bash
#!/bin/bash

APP_TOML_PATH=$ORAI_HOME/.oraid/config/app.toml
CONFIG_TOML_PATH=$ORAI_HOME/.oraid/config/config.toml

SYNC_RPC="https://testnet-v2.rpc.orai.io:443"
PERSISTENT_PEER_1="3aa2643144cc59e2d60a4b0c328223b0773e5d9e@134.209.164.196:26656"
PERSISTENT_PEER_2="fc7d01a6ffbbc097e60fcf7b5bb6970d693161c0@134.209.164.196:26666"

LATEST_HEIGHT=$(curl -s $SYNC_RPC/block | jq '.result.block.header.height | tonumber')
TRUST_HEIGHT=$((LATEST_HEIGHT - 5000))
TRUST_HASH=$(curl -s $SYNC_RPC/block?height=$TRUST_HEIGHT | jq .result.block_id.hash)

echo "height: $LATEST_HEIGHT"
echo "trust height: $TRUST_HEIGHT"
echo "hash: $TRUST_HASH"

sed -i -e "s%^snapshot-interval *=.*%snapshot-interval = 1200%; " $APP_TOML_PATH

sed -i -E 's|tcp://127.0.0.1:26657|tcp://0.0.0.0:26657|g' $CONFIG_TOML_PATH
sed -i -e "s%^enable *=.*%enable = true%; " $CONFIG_TOML_PATH
sed -i -e "s%^allow_duplicate_ip *=.*%allow_duplicate_ip = true%; " $CONFIG_TOML_PATH
sed -i -e "s%^addr_book_strict *=.*%addr_book_strict = false%; " $CONFIG_TOML_PATH
sed -i -e "s%^persistent_peers *=.*%persistent_peers = \"$PERSISTENT_PEER_1,$PERSISTENT_PEER_2\"%; " $CONFIG_TOML_PATH
sed -i -e "s%^max_num_outbound_peers *=.*%max_num_outbound_peers = 0%; " $CONFIG_TOML_PATH
sed -i -e "s%^rpc_servers *=.*%rpc_servers = \"$SYNC_RPC,$SYNC_RPC\"%; " $CONFIG_TOML_PATH
sed -i -e "s%^trust_height *=.*%trust_height = \"$TRUST_HEIGHT\"%; " $CONFIG_TOML_PATH
sed -i -e "s%^trust_hash *=.*%trust_hash = $TRUST_HASH%; " $CONFIG_TOML_PATH
```

* Grant privilege to execute script:
```bash
chmod 700 state-sync.sh
./state-sync.sh
```

* Start your node
```bash
oraid tendermint unsafe-reset-all --home $ORAI_HOME/.oraid
oraid start --home $ORAI_HOME/.oraid
```
{% hint style="warning" %}
Start node process may take many minutes!
{% endhint %}
