# Local Testnet Chain

This guide will provide step-by-step how to run a local testnet chain

### **Step 1: Access the Oraichain repository**

The first thing you need to do is head over to the Oraichain repository. This repository is designed to help you launch and interact with local testnet chain. You can find this repo [here](https://github.com/oraichain/orai.git)

### **Step 2: Clone the repository**

After find Oraichain repo, clone it to your desire folder
```bash
git clone https://github.com/oraichain/orai.git
```

### **Step 3: Set up**

To set up and run chain locally, you have set up:
- Install Golang: [Please visit the official Golang website to download & install Go](https://go.dev/doc/install)
- Install make: Normally, for Linux-based machines, you already have Make installed by default
- Install libwasmvm: The wasmd module of CosmWasm uses a wasm vm library, which should be included when building the chain binary. Hence, we need to download and place it in a specific location. For Linux based machines, please run the following command:
```bash
sudo wget https://github.com/CosmWasm/wasmvm/releases/download/v0.13.0/libwasmvm_muslc.a -O /lib/libwasmvm_muslc.a
```
- After set up, move to your folder that clone repo, move to `orai` folder
```bash
cd your_clone_folder
cd orai/orai
```
- Download Go dependencies:
```bash
go get ./...
```
- Build the binary
```bash
make build
```
- Check result
```bash
oraid version
```

### **Step 4: Write sh script file**

To run locally testnet chain, create a `.sh` script file as follow:
- Create `local_validator.sh` script in folder `./scripts/`
```bash
nano ./scripts/local_validator.sh
```
- Fill in script as follow:
```sh
#!/bin/bash
set -ux

# always returns true so set -e doesn't exit if it is not running.
killall oraid || true
rm -rf $HOME/.oraid/
killall screen

# make four orai directories
mkdir $HOME/.oraid
mkdir $HOME/.oraid/validator1
mkdir $HOME/.oraid/validator2

# init all two validators
oraid init --chain-id=testing validator1 --home=$HOME/.oraid/validator1
oraid init --chain-id=testing validator2 --home=$HOME/.oraid/validator2

# create keys for all two validators
oraid keys add validator1 --keyring-backend=test --home=$HOME/.oraid/validator1
oraid keys add validator2 --keyring-backend=test --home=$HOME/.oraid/validator2

update_genesis () {    
    cat $HOME/.oraid/validator1/config/genesis.json | jq "$1" > $HOME/.oraid/validator1/config/tmp_genesis.json && mv $HOME/.oraid/validator1/config/tmp_genesis.json $HOME/.oraid/validator1/config/genesis.json
}

# change staking denom to orai
update_genesis '.app_state["staking"]["params"]["bond_denom"]="orai"'

# create validator node 1
oraid add-genesis-account $(oraid keys show validator1 -a --keyring-backend=test --home=$HOME/.oraid/validator1) 1000000000000orai,1000000000000stake --home=$HOME/.oraid/validator1
oraid gentx validator1 500000000orai --keyring-backend=test --home=$HOME/.oraid/validator1 --chain-id=testing
oraid collect-gentxs --home=$HOME/.oraid/validator1
oraid validate-genesis --home=$HOME/.oraid/validator1

# update staking genesis
update_genesis '.app_state["staking"]["params"]["unbonding_time"]="240s"'
# update crisis variable to orai
update_genesis '.app_state["crisis"]["constant_fee"]["denom"]="orai"'
# udpate gov genesis
update_genesis '.app_state["gov"]["deposit_params"]["min_deposit"][0]["denom"]="orai"'
# update mint genesis
update_genesis '.app_state["mint"]["params"]["mint_denom"]="orai"'
update_genesis '.app_state["gov"]["voting_params"]["voting_period"]="5s"'
# port key (validator1 uses default ports)
# validator1 1317, 9090, 9091, 26658, 26657, 26656, 6060
# validator2 1316, 9088, 9089, 26655, 26654, 26653, 6061

# change app.toml values
VALIDATOR1_APP_TOML=$HOME/.oraid/validator1/config/app.toml
VALIDATOR2_APP_TOML=$HOME/.oraid/validator2/config/app.toml

# change config.toml values
VALIDATOR1_CONFIG=$HOME/.oraid/validator1/config/config.toml
VALIDATOR2_CONFIG=$HOME/.oraid/validator2/config/config.toml

# validator2
sed -i -E 's|tcp://0.0.0.0:1317|tcp://0.0.0.0:1316|g' $VALIDATOR2_APP_TOML
sed -i -E 's|0.0.0.0:9090|0.0.0.0:9088|g' $VALIDATOR2_APP_TOML
sed -i -E 's|0.0.0.0:9091|0.0.0.0:9089|g' $VALIDATOR2_APP_TOML
sed -i -E 's|0.0.0.0:8545|0.0.0.0:7545|g' $VALIDATOR2_APP_TOML

# Pruning - comment this configuration if you want to run upgrade script
pruning="custom"
pruning_keep_recent="5"
pruning_keep_every="10"
pruning_interval="10000"

sed -i -e "s%^pruning *=.*%pruning = \"$pruning\"%; " $VALIDATOR1_APP_TOML
sed -i -e "s%^pruning-keep-recent *=.*%pruning-keep-recent = \"$pruning_keep_recent\"%; " $VALIDATOR1_APP_TOML
sed -i -e "s%^pruning-keep-every *=.*%pruning-keep-every = \"$pruning_keep_every\"%; " $VALIDATOR1_APP_TOML
sed -i -e "s%^pruning-interval *=.*%pruning-interval = \"$pruning_interval\"%; " $VALIDATOR1_APP_TOML

sed -i -e "s%^pruning *=.*%pruning = \"$pruning\"%; " $VALIDATOR2_APP_TOML
sed -i -e "s%^pruning-keep-recent *=.*%pruning-keep-recent = \"$pruning_keep_recent\"%; " $VALIDATOR2_APP_TOML
sed -i -e "s%^pruning-keep-every *=.*%pruning-keep-every = \"$pruning_keep_every\"%; " $VALIDATOR2_APP_TOML
sed -i -e "s%^pruning-interval *=.*%pruning-interval = \"$pruning_interval\"%; " $VALIDATOR2_APP_TOML

# state sync  - comment this configuration if you want to run upgrade script
snapshot_interval="10"
snapshot_keep_recent="2"

sed -i -e "s%^snapshot-interval *=.*%snapshot-interval = \"$snapshot_interval\"%; " $VALIDATOR1_APP_TOML
sed -i -e "s%^snapshot-keep-recent *=.*%snapshot-keep-recent = \"$snapshot_keep_recent\"%; " $VALIDATOR1_APP_TOML

sed -i -e "s%^snapshot-interval *=.*%snapshot-interval = \"$snapshot_interval\"%; " $VALIDATOR2_APP_TOML
sed -i -e "s%^snapshot-keep-recent *=.*%snapshot-keep-recent = \"$snapshot_keep_recent\"%; " $VALIDATOR2_APP_TOML

# validator1
sed -i -E 's|allow_duplicate_ip = false|allow_duplicate_ip = true|g' $VALIDATOR1_CONFIG

# validator2
sed -i -E 's|tcp://127.0.0.1:26658|tcp://0.0.0.0:26655|g' $VALIDATOR2_CONFIG
sed -i -E 's|tcp://127.0.0.1:26657|tcp://0.0.0.0:26654|g' $VALIDATOR2_CONFIG
sed -i -E 's|tcp://0.0.0.0:26656|tcp://0.0.0.0:26653|g' $VALIDATOR2_CONFIG
sed -i -E 's|allow_duplicate_ip = false|allow_duplicate_ip = true|g' $VALIDATOR2_CONFIG

# copy validator1 genesis file to validator2
cp $HOME/.oraid/validator1/config/genesis.json $HOME/.oraid/validator2/config/genesis.json

# copy tendermint node id of validator1 to persistent peers of validator2-3
sed -i -E "s|persistent_peers = \"\"|persistent_peers = \"$(oraid tendermint show-node-id --home=$HOME/.oraid/validator1)@localhost:26656\"|g" $VALIDATOR2_CONFIG

# start all two validators
screen -S validator1 -d -m oraid start --home=$HOME/.oraid/validator1 --minimum-gas-prices=0.00001orai

# send orai from first validator to second validator
echo "Waiting 7 seconds to send funds to validators 2..."
sleep 7

oraid tx send $(oraid keys show validator1 -a --keyring-backend=test --home=$HOME/.oraid/validator1) $(oraid keys show validator2 -a --keyring-backend=test --home=$HOME/.oraid/validator2) 5000000000orai --keyring-backend=test --home=$HOME/.oraid/validator1 --chain-id=testing --broadcast-mode block --gas 200000 --fees 2orai --node http://localhost:26657 --yes

# create second validator
oraid tx staking create-validator --amount=500000000orai --from=validator2 --pubkey=$(oraid tendermint show-validator --home=$HOME/.oraid/validator2) --moniker="validator2" --chain-id="testing" --commission-rate="0.1" --commission-max-rate="0.2" --commission-max-change-rate="0.05" --min-self-delegation="500000000" --keyring-backend=test --home=$HOME/.oraid/validator2 --broadcast-mode block --gas 200000 --fees 2orai --node http://localhost:26657 --yes

screen -S validator2 -d -m oraid start --home=$HOME/.oraid/validator2 --minimum-gas-prices=0.00001orai

echo "All 2 Validators are up and running!"

```
- After create file, give the script permission with 
```bash 
chmod +x ./scripts/local_validator.sh
```

### **Step 5: Run local testnet chain**

After set up, create script file, you can run local testnet chain
- Run script file:
```bash
./scripts/local_validator.sh
```
- Log local node:
```bash
Validator1: screen -r validator1
Validator2: screen -r validator2
```
- View validator status:
```bash
Validator2: oraid status --node "tcp://localhost:26654"
Validator3: oraid status --node "tcp://localhost:26651"
or
Validator1: oraid keys list --keyring-backend test --home $HOME/.oraid/validator1
Validator2: oraid keys list --keyring-backend test --home $HOME/.oraid/validator2
```
