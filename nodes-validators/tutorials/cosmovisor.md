# Cosmovisor

`cosmovisor` is a small process manager for Cosmos SDK application binaries that monitors the governance module for incoming chain upgrade proposals. If it sees a proposal that gets approved, `cosmovisor` can automatically download the new binary, stop the current binary, switch from the old binary to the new one, and finally restart the node with the new binary.

We recommend validators to use `cosmovisor` to run their nodes. This will make low-downtime upgrades smoother, as validators don’t have to manually upgrade binaries during the upgrade. Instead, they can pre-install new binaries, and `cosmovisor` will automatically update them based on the onchain software upgrade proposals.

## Configuration

More information about Cosmovisor settings can be found in the [Cosmovisor documentation](https://docs.cosmos.network/main/build/tooling/cosmovisor).

## Installation

#### Using go install

To install the latest version of `cosmovisor`, run the following command:

```bash
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@latest
```

#### Manual Build

You can also install from source by pulling the cosmos-sdk repository and switching to the correct version and building as follows:

```bash
git clone https://github.com/cosmos/cosmos-sdk.git
cd cosmos-sdk
git checkout cosmovisor/vx.x.x
make cosmovisor
```

This will build Cosmovisor in `/cosmovisor`
 directory. Afterwards you may want to put it into your machine's PATH like as follows:

```bash
cp cosmovisor/cosmovisor ~/go/bin/cosmovisor
```

To check your Cosmovisor version, run

```bash
cosmovisor version
```

## Directory structure

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

## Manual Software Update using Cosmovisor

In this tutorial, I assume that you have built your own validator node using our guide in [this link](https://docs.orai.io/nodes-and-validators/networks/mainnet/become-a-full-node-operator-from-source).

When a software upgrade proposal passes, we need to prepare the binary file for the upgrade period. Below is the lifecycle of a software update:

1. **Foundation Team Publishes a Release Version**  
   Example: [v0.42.4 release](https://github.com/oraichain/orai/releases/tag/v0.42.4).

2. **Foundation Team Submits a Software Upgrade Proposal**  
   Example: [Proposal #244](https://scan.orai.io/proposals/244).  
   In this example, you can see all the proposal details. The most important piece of information is the block height at which the upgrade will begin. In this case, Proposal #244 specifies the upgrade will occur at block height `34717352`. When you search for this block height, it will typically be in the future, and the estimated time until the upgrade is provided.

3. **Validator Owner's Step: Prepare the Upgrade Binary**  
   First, create the upgrade binary folder:

   ```bash
   mkdir -p $HOME/.oraid/cosmovisor/upgrades/v0.42.4/bin
   ```

   You need to prepare the binary file by building it from the source code. Checkout the release tag mentioned in the proposal, run the `make build` command, and the binary file will be located in `$HOME/go/bin`. Copy this binary to the upgrade folder created earlier:

   ```bash
   cd orai
   git pull
   git checkout v0.42.4
   make install
   cp $(which oraid) $HOME/.oraid/cosmovisor/upgrades/v0.42.4/bin
   ```

   Verify the version of the binary; it should be `v0.42.4` for both of the commands below:

   ```bash
   oraid version
   $HOME/.oraid/cosmovisor/upgrades/v0.42.4/bin/oraid version
   ```

   Now you can rest easy.

4. **Upgrade at the Specified Block Height**  
   When the network reaches block height `34717352`, Cosmovisor will automatically switch to the new binary folder (`upgrades/v0.42.4/bin`) as mentioned in the proposal and restart the main process. Once 2/3 of validators approve the new block (`34717353`), the upgrade will be successful.
