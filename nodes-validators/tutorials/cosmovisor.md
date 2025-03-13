# **Cosmovisor: Automating Cosmos Node Upgrades**

`cosmovisor` is a lightweight process manager for Cosmos SDK application binaries that automates the upgrade process. It monitors the governance module for on-chain upgrade proposals and seamlessly handles binary upgrades by:

- Downloading and preparing the new binary
- Stopping the current binary
- Switching to the upgraded binary
- Restarting the node with the new binary

### **Why Use Cosmovisor?**
For validators, `cosmovisor` significantly reduces downtime during chain upgrades. Instead of manually handling upgrades, validators can pre-install new binaries, allowing `cosmovisor` to execute upgrades automatically when a proposal passes.

---
## **Configuration**

More details about `cosmovisor` settings can be found in the [official documentation](https://docs.cosmos.network/main/build/tooling/cosmovisor).

---
## **Installation**

### **1. Using Go Install**
Install the latest version of `cosmovisor` with:

```bash
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@latest
```

### **2. Manual Build**
Alternatively, build from source:

```bash
git clone https://github.com/cosmos/cosmos-sdk.git
cd cosmos-sdk
git checkout cosmovisor/vx.x.x
make cosmovisor
```

This builds `cosmovisor` in the `/cosmovisor` directory. To add it to your system’s PATH:

```bash
cp cosmovisor/cosmovisor ~/go/bin/cosmovisor
```

Verify the installation:

```bash
cosmovisor version
```

---
## **Directory Structure**

```
.
├── current -> genesis or upgrades/<name>
├── genesis
│   └── bin
│       └── $DAEMON_NAME
└── upgrades
    └── <name>
        ├── bin
        │   └── $DAEMON_NAME
        └── upgrade-info.json
```

---
## **Manual Software Upgrade Using Cosmovisor**

This tutorial assumes you have set up a validator node using [this guide](https://docs.orai.io/nodes-and-validators/networks/mainnet/become-a-full-node-operator-from-source).

### **Upgrade Process Overview**

#### **1. Foundation Team Publishes a New Release**
Example: [v0.50.8 Release](https://github.com/oraichain/wasmd/releases/tag/v0.50.8)

#### **2. Foundation Team Submits a Software Upgrade Proposal**
Example: [Proposal #310](https://scanium.io/Oraichain/gov/310)

- The most important detail is the **block height** at which the upgrade takes effect. In Proposal #244, the upgrade happens at block **`53006170`**.
- You can track this block height and estimate when the upgrade will occur.

#### **3. Validator's Steps: Prepare the Upgrade Binary**

1. Create the upgrade binary folder:
   ```bash
   mkdir -p $HOME/.oraid/cosmovisor/upgrades/v0.50.8/bin
   ```
2. Build the new binary:
   ```bash
   cd orai
   git pull
   git checkout v0.50.8
   make build
   ```
3. Copy the binary to the upgrade folder:
   ```bash
   cp $(which oraid) $HOME/.oraid/cosmovisor/upgrades/v0.50.8/bin
   ```
4. Verify the new binary version:
   ```bash
   oraid version
   $HOME/.oraid/cosmovisor/upgrades/v0.50.8/bin/oraid version
   ```

If both commands return `v0.50.8`, the setup is correct, and you are now ready for the upgrade.

#### **4. Automatic Upgrade at the Specified Block Height**

Once the network reaches block **`53006170`**, `cosmovisor` will automatically:
- Stop the current binary
- Switch to the new binary (`upgrades/v0.50.8/bin`)
- Restart the node

When **2/3 of validators approve the next block (`53006170`)**, the upgrade is considered **successful**.

---
### **Final Thoughts**
With `cosmovisor`, software upgrades are seamless and reduce validator downtime. Make sure to follow this guide for every upgrade and ensure your node is prepared in advance.

🚀 **Happy Validating!**

