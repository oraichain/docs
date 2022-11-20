# Tutorial to manually recover your node using Docker

Before following the below steps, please back up your necessary information in two files: node_key.json and priv_validator_key.json. You should store your wallet's mnemonic too.

Also, this tutorial is for those forgetting to take snapshots of your local node, and the only option is to reset and synchronize with the network from the start. It also does not handle binary upgrades, so if your node also lost upgraded binaries (please located in /root/oraivisor/upgrades/. If the directory does not exist, it means that your node has also lost the binaries, and you should not follow this tutorial). We provide a snapshot, which is updated periodically, from our node through a docker image. Please note that once you use our snapshot, it is automatic that your node will have the same blocks and transactions as ours, so think thoroughly before doing so. We recommend you take snapshots (copying the .oraid/ directory) frequently instead.

However, if your node needs to recover fast because it is a validator node, and it is getting slashed due to downtime, then the following quick steps may be your savior.

## 0. Stop your current running node

Enter your container

```bash
docker-compose exec orai bash
```

Kill the oraid process

```bash
pkill oraid
```

## 1. Download the docker-compose file

```bash
mkdir $PWD/snapshot && cd snapshot && wget -O ./docker-compose.yml https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/docs/manual_snapshot/docker-compose.yml
```

## 2. Copy the snapshot directory from the container to the host

Enter the container using the following command:

```bash
sudo docker-compose up -d --force-recreate && sudo docker exec -it orai_snapshot sh
```

Next, copy the snapshot directory:

```bash
cp -r /root/snapshot/ /snapshot
```

## 3. Replace your .oraid/ directory with the snapshot you just copied

Please exit the container:

```bash
exit
```

then:

```bash
sudo rm -rf <path-to-.oraid-dir>/.oraid && sudo cp -r $PWD/snapshot/.oraid <path-to-.oraid-dir>/.oraid
```

## 4. Replace your node information back to the newly updated .oraid/ directory

This step requires you to update the content of the two files: node_key.json & priv_validator_key.json (these two files must be added in the .oraid/config/ directory). You also need to import your wallet by typing the following inside your container:

```bash
oraid keys add <wallet-name> --recover
```

## 5. Restart your node

Please use your favorite command to restart your node, and observe the result. At this step, your node should run and synchronize the remaining blocks starting from the snapshot height to the latest one.

Remember to verify your node information by typing the following command inside your container:

```bash
oraid status
```

If it is correct, then congratulations, you have successfully recovered your node!

### BONUS

To fully reset your node and start over with automatic binary upgrade handling, please type the following command:

```bash
oraid unsafe-reset-all && pkill oraid && unlink /root/oraivisor/current && ln -s /root/oraivisor/genesis /root/oraivisor/current && rm -rf /root/oraivisor/upgrades && rm .oraid/wasm/wasm/wasm/* .oraid/wasm/wasm/modules/v1/* && rm .oraid/config/write-*
```

The following command will remove all the necessary files to allow your node to synchronize with the network from the beginning without any errors. This is the best way to do if you have time.

Cheers, and happy validating!