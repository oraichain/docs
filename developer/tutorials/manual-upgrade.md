# Tutorial to manually upgrade a node

This tutorial assumes that an Oraichain network has already experienced multiple upgrades with many binaries. If the automatic upgrade fails, please follow the below steps to fix the problem and get your node updated. Make sure to check the required binary version of your node first before continuing. To check, enter your node's container and type:

```bash
pkill oraid && oraid version
```

The commands kill the current running process of your node and view the current oraid binary version

## 1. Back up your current node information

You should always back up all the node information before upgrading in case something goes wrong. The two files you want to copy is: **.oraid/config/priv_validator_key.json and .oraid/config/node_key.json**

## 2. Create a directory for the latest binary version

You should check the [explorer's latest software upgrade proposal](https://scan.orai.io/proposals?type=SoftwareUpgradeProposal) to collect the binary's version as well as its URL for downloading. To find the exact binary URL, please view the details of the exact upgrade version and enter the URL in the ***infor*** section. After entering, there should be another URL in the form of:

```js
{
  "binaries": {
  "linux/amd64": "<download-url>"
  }
}
```

Please collect the URL and type:

```bash
apk add curl && rm -rf /root/oraivisor/upgrades/<binary-version> && mkdir -p /root/oraivisor/upgrades/<binary-version>/bin && curl <binary-url> > /root/oraivisor/upgrades/<binary-version>/bin/oraid && chmod +x /root/oraivisor/upgrades/<binary-version>/bin/oraid
```

The above commands remove the old error upgrade directory & creates a new one before downloading the actual binary

## 4. Link the current binary version to the upgraded one

```bash
unlink /root/oraivisor/current && ln -s /root/oraivisor/upgrades/<binary-version> /root/oraivisor/current
```

## 5. Check the binary version to finish the manual upgrade process

Type:

```bash
cd /workspace/ && oraid version
```

If it matches your node's required version, then the manual upgrade process has succeeded.

## 6. Restart your node to continue the validating journey

Use your favorite command to restart your node, and you are good to go!

**Happy validating!**