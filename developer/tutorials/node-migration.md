# Tutorial to migrate a node to another node

## 1. Back up your current node information

You should back up all the node information before migrating to recover if something goes wrong. The two files you want to copy is: **.oraid/config/priv_validator_key.json and .oraid/config/node_key.json**

## 2. Create a new node

To start migrating, you can create a new node to initialize all the configuration files needed. For more information relating to creating a new node, you can follow the full node operator creation tutorial [here](https://docs.orai.io/developers/networks/mainnet/become-a-full-node-operator-from-source). You can use the same moniker as the current node's

## 3. Start your new node with appropriate persistent connection to an existing node in the network

Your newly created node should synchronize with the network before migrating to reduce the downtime

## 4. Shut the old node down

shut down your node by:

```bash
pkill oraid
```

## 5. Copy your current node information to the newly created node

You can edit freely while keeping your new node running.

## 6. Restart your new node to apply changes and finish the migration process

## 7. Check the new node's voting power to confirm the migration

```bash
oraid status && oraid tendermint show-node-id
```

if the voting power matches the node you have just migrated from, and the node id matches the current node, then congratuations, you have finished the migration process. If not, don't worry. Please check if you have copied everything right and restart again. A common mistake is to not stop the current node before restarting the new node. Another common mistake is to copy the files to the new node before syncing, as it may cause your new node unable to sync. It is because other nodes will reject connections coming from the same node id that they have been persistently connecting to.

Your current node will not be affected during the process. If you want, you can always stop the new node and then start the current node again without any interruptions.

You can import your wallet to the new node after the migration process has finished. Type:

```bash
oraid keys add <wallet name> --recover
```

Or you can just use the explorer for simplicity.

**Happy validating!**
