# Migrate one Oraichain node to another

Migrating your node to a new machine doesn't have to be complicated. Follow this structured guide to ensure a smooth transition with minimal downtime.

> \[!CAUTION] ⚠️ Do not put priv\_validator\_key.json on two running nodes simultaneously. Your validator will be jailed forever !!!

***

## **1. Back Up Your Current Node Information**

Before starting, **backup** your node's essential files to prevent any data loss:

```bash
cp -r ~/.oraid/config/priv_validator_key.json ~/.oraid/config/node_key.json ~/backup/
```

These files are crucial for maintaining your validator identity.

***

## **2. Set Up a New Node**

Initialize a new node to prepare for the migration. Follow the full node setup guide [here](https://docs.orai.io/developers/networks/mainnet/become-a-full-node-operator-from-source). You can reuse the same moniker as your existing node.

***

## **3. Sync the New Node with the Network**

Ensure your new node synchronizes with the network before migration to minimize downtime. Start your new node with a persistent connection to an existing network node.

```bash
oraid start --p2p.persistent_peers="<existing-node-id>@<ip>:<port>"
```

Let it fully sync before proceeding.

***

## **4. Shut Down the Old Node**

Once the new node is synced, gracefully stop the old node:

```bash
systemctl stop orai
```

This step prevents conflicts between the two nodes.

***

## **5. Transfer Node Data to the New Machine**

Copy the backed-up files to the new node:

```bash
scp ~/backup/priv_validator_key.json ~/backup/node_key.json <new-node-ip>:~/.oraid/config/
```

Ensure the new node retains the same identity by using the copied keys.

***

## **6. Restart the New Node**

Apply the changes by restarting the new node:

```bash
oraid start
```

Monitor logs to verify it is running correctly.

***

## **7. Verify Voting Power and Node ID**

Run the following command to check if your node has successfully migrated:

```bash
oraid status && oraid tendermint show-node-id
```

* If the **voting power** and **node ID** match your previous node, congratulations! 🎉 Your migration is complete.
* If not, double-check the copied files and restart the node.
* Common mistakes include not stopping the old node first or copying files before the new node has finished syncing.

***

## **8. Import Your Wallet (Optional)**

If you need to restore your wallet on the new node, use:

```bash
oraid keys add <wallet-name> --recover
```

Alternatively, use the **explorer** to manage your wallet effortlessly.

***

### **Final Thoughts**

Your old node remains unaffected throughout the process. If needed, you can always revert by restarting the old node.

🚀 **Happy validating!**
