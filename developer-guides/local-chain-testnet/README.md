# Local testnet chain

This guide will provide step-by-step how to run a local testnet chain

### **Step 1: Access the Oraichain repository**

The first thing you need to do is head over to the Oraichain repository. This repository is designed to help you launch and interact with local testnet chain. You can find this repo [here](https://github.com/oraichain/wasmd.git)

### **Step 2: Clone the repository**

After find Oraichain repo, clone it to your desire folder

```bash
git clone https://github.com/oraichain/wasmd.git && cd wasmd
```

### **Step 3: Set up**

* Build the binary

```bash
make build
```

* Check result

```bash
oraid version
```

### **Step 4: Start the local network**

```bash
./scripts/multinode-local-testnet.sh
```

This command essentially creates a local network of three validators running in the background.

## Logs

Validator1: screen -r validator1

Validator2: screen -r validator2

Validator3: screen -r validator3

CTRL + A + D to detach

## Directories

Validator1: `.oraid/validator1`

Validator2: `.oraid/validator2`

Validator3: `.oraid/validator3`

## Ports

"x, x, x, x, rpc, p2p, x"

Validator1: `1317, 9090, 9091, 26658, 26657, 26656, 6060`

Validator2: `1316, 9088, 9089, 26655, 26654, 26653, 6061`

Validator3: `1315, 9086, 9087, 26652, 26651, 26650, 6062`

Ensure to include the `--home` flag or `--node` flag when using a particular node.

## Examples

Validator2: `oraid status --node "tcp://localhost:26654"`

Validator3: `oraid status --node "tcp://localhost:26651"`

or

Validator1: `oraid keys list --keyring-backend test --home $HOME/.oraid/validator1`

Validator2: `oraid keys list --keyring-backend test --home $HOME/.oraid/validator2`
