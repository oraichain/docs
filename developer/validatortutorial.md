# Validator Tutorial

### Join Oraichain Testnet Beta as a Validator[#](https://docs.orai.io/docs/Developers/OraichainInstallation#join-oraichain-testnet-beta-as-a-validator)

Step-by-step guide: How to become an Oraichain Testnet Validator Welcome SamOrais ! A new era has come for those who are holding ORAI tokens and investors wanting to earn more while still contributing greatly to the network. The Oraichain testnet beta version is coming with lots of new features, and we would love to introduce to you that anyone holding ORAI will have a chance to become a Validator. By joining as a Validator, you are helping the Oraichain ecosystem grow stronger and bigger than ever. Better yet, to encourage individuals and groups to join, those who have staked their tokens to be Validators during this period will participate in a new reward program. This article focuses mostly on how one can be a Validator with only a few simple steps. If you have any further questions or problems following this guideline, feel free to ask us on any social media platform, from Telegram to Facebook. We will try to answer and help you out as soon as possible. Please note that this guide assumes that the Oraichain testnet has already been in the running state, and those who follow this will not participate as genesis nodes, but rather full nodes only. Another point is that those who would like to be validators should prepare to learn some technical aspects relating to Oraichain. There will be times where you need to update your validator node, or there are bugs that cause you to re-run it. Long story short, you should not think that being a validator is a passive job. Delegators will be watching to see if you are a worthy validator or not, and earning more or fewer rewards depends heavily on it.

#### Prerequisites[#](https://docs.orai.io/docs/Developers/OraichainInstallation#prerequisites)

**Hardware requirement#**

Since we use Cosmos-SDK and Tendermint to build Oraichain, we decide to follow the hardware requirement recommendation from them which is specified below:

Minimum requirements:

* 1GB RAM
* 100GB of disk space
* 1.4 GHz CPU

Recommended:

* 2GB RAM
* 100GB SSD
* x64 2.0 GHz 2v CPU

{% hint style="info" %}
**NOTE**

For more information about the hardware requirement, please take a look at this [guide](https://docs.tendermint.com/master/tendermint-core/running-in-production.html#hardware) from Tendermint.
{% endhint %}

**Oraichain user account#**

You need an Oraichain account to officially be a Validator node, otherwise, it would only be a full node that receives blocks from others. Make sure you follow [this tutorial](https://www.youtube.com/watch?v=Nu2vapu3H9g\&feature=youtu.be\&ab\_channel=Oraichain) before going further.

### The complete guideline to become an Oraichain Validator node[#](https://docs.orai.io/docs/Developers/OraichainInstallation#the-complete-guideline-to-become-an-oraichain-validator-node)

**Step 1: Installing Docker and Docker-compose#**

Docker, according to the [Docker overview](https://docs.docker.com/get-started/overview/), is an open platform for building, developing, and running software applications within loosely isolated environment packages called containers. Docker-compose, on the other hand, is a tool that helps deploy Docker containers in a more convenient way. Since you will be using Docker-compose to run your Validator node, it is crucial that you install both before continuing further down the steps.

**Step 2: Download and run the setup script file#**

This file is used to prepare you with necessary files and configurations such as docker-compose file, environment file. Below are three commands to download, adjust the permission of the file, and run it:

**For Linux and MacOS users#**

```
curl -OL https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/setup.sh && chmod +x ./setup.sh && ./setup.sh
```

**For Windows users#**

It is a bit more complex for Windows users since the environment is completely different from Linux and macOS. First, open your Windows Powershell and run the following commands:

```
Remove-item alias:curl ; curl -OL https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/setup.ps1
```

On Windows, there may exist a similar native command Invoke-WebRequest which overrides the curl command that we want to use. That is why in the above command, we remove its alias and then download the script file. If you want to re-do the Remove-item command because you still need to use Invoke-WebRequest, type the following command to restore to its initial state:

```
Set-Alias curl Invoke-WebRequest
```

Before continuing, make sure that your Windows Powershell is in the same directory as the file. Next, run the below command:

```
.\setup.ps1
```

If you cannot execute the file, try to run the Windows Powershell as administrator.

After running the above commands, you should receive two files, one is a docker-compose file, and the other is an environment file, which is orai.env. Feel free to change its name as you prefer, but it must match the name written in the docker-compose file.

There are some parameters with example values and comments when you open the orai.env file. Some of them are optional, but others need to be filled with appropriate values in order to validate yourself to the network as a validator. Becoming a Validator node has many parameters that you can modify to suit your needs, and all of them have been specified in the environment orai.env file. Below is an example of the orai.env file, please read the comments carefully to understand the meaning of each key-value pair:

```
###################### basic information for a node
# account name, this should match your wallet name created on the testnet explorer website.
USER=dubedube
# the unencrypted mnemonic of your account which you generated on the testnet explorer website. Using this will automatically import your Oraichain account into your node.
MNEMONIC=disagree bicycle catch uphold license marriage client obvious bleak hobby garlic bachelor labor lift column vault kiwi same two gather tennis design weekend pull
# Passphrase for your keyring. Keep this safe and secure. This must have at least 8 characters. This passphrase is not related to the PIN on the testnet explorer website.
PASS=123456789
# the moniker of your node
MONIKER=dw
###################### basic information to become a validator
# the amount of tokens you want to stake into the validator
AMOUNT=1000000orai
# the amount of tokens you want to send to your reporter, which is used to send transactions to Oraichain after executing a request to store the result.
REPORTER_AMOUNT=100orai
# the commission rate between the delegators and the validator
COMMISSION_RATE=0.10
# the maximum comission rate allowed
COMMISSION_MAX_RATE=0.20
# the maximum changes in the commission rate
COMMISSION_MAX_CHANGE_RATE=0.01
# the minimum number of tokens you must delegate into your validator.
MIN_SELF_DELEGATION=100
# how gas is calculated, default is auto to reduce overhead
GAS=auto
# gas adjustment is the multiplier between the estimated gas and the gas specified. Example: gas = estimated gas * gas adjustment
GAS_ADJUSTMENT=1.15
# gas price for the fees you are willing to pay to be a validator.
GAS_PRICES=0.000000025orai
# your security contact (optional)
SECURITY_CONTRACT=efg
# your node identity (optional)
IDENTITY=node
# your website (optional)
WEBSITE=xyx.com
# details about your validator (optional)
DETAILS=helloworld
#################################### DO NOT TOUCH
GENESIS_URL=https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/genesis.json
PERSISTENT_PEERS=25e3dd0839fa44a89735b38b7b749acdfac8438e@164.90.180.95:26656,e07a89a185c538820258b977b01b44a806dfcece@157.230.22.169:26656,db13b4e2d1fd922640904590d6c9b5ae698de85c@165.232.118.44:26656,b46c45fdbb59ef0509d93e89e574b2080a146b14@178.128.61.252:26656,2a8c59cfdeccd2ed30471b90f626da09adcf3342@178.128.57.195:26656,b495da1980d3cd7c3686044e800412af53ae4be4@159.89.206.139:26656,addb91a1dbc48ffb7ddba30964ae649343179822@178.128.220.155:26656
```

This part is very important, because if you do not modify the environment file or you do it wrong, your validator node may not run properly, and you will need to use docker and docker-compose to rebuild and re-run the container.

**Step 3: Start your validator node#**

Before running your node, make sure that the docker image is the latest. You do not need to understand anything about the docker image. Just run:

```
docker-compose pull orai
```

and you will receive the latest version of the Oraichain network.

In order to run your node, you only need to run the following command:

```
docker-compose up -d --force-recreate
```

It will help you rebuild and rerun your container using the latest image you have just pulled. Your orai.env will also be updated respectively, so if you have already started a node, this command is helpful when you want to modify and update the orai.env file.

For those that have problems with the keyring error, you should remove all the runtime directories before starting, so your account from the orai.env file can be imported. Type the following command inside the container to clear them:

```
fn clear
```

**Monitoring your node#**

After starting, you can type:

```
docker-compose ps
```

to see your validator node running healthily. If you see something similar to the below snapshot, then congratulations, you have successfully run your own node!

```
Name                 Command               State                                     Ports                                   
-------------------------------------------------------------------------------------------------------------------------------
orai_node   /bin/sh -c fn init && fn start   Up      0.0.0.0:1317->1317/tcp, 0.0.0.0:26656->26656/tcp, 0.0.0.0:26657->26657/tcp
```

To see what your node is showing, type:

```
docker-compose logs -f orai
```

You should wait for your node to synchronize with the network before doing anything further. It is synchronized when you see a new block every approximately five seconds.

**Step 4: Broadcast yourself as a Validator#**

Once you are a full node, you can create a transaction that makes your node a Validator. To do so, it is necessary that you have an Oraichain account imported. In order to verify if you have an account in your node, type the following command inside your container. Before you do so, enter your container using the below command:

```
docker-compose exec orai bash
```

then type below to check your account:

```
oraicli keys list
```

You will be asked to type a passphrase. Use the one (PASS key)that you set in the environment file. If you see an empty list, then you should do the below steps to import your account. If it already shows an account, then you can skip.

Type:

```
oraicli keys add --recover <account-name>
```

where **account-name** is your wallet name, which is also the value for the USER key in the orai.env file. Next, the terminal may ask you to type the mnemonic for your account. Type the one that is in your orai.env file, which should be modified by now. Finally, it may ask you to type a passphrase. Type any you prefer, but it should be at least 8 characters.

After finishing, you can check again with the command oraicli keys list above to verify if you are successful or not.

**Creating validator transaction#**

Before moving on to this step, make sure that your account has some ORAI tokens, and it has already been broadcast to the Oraichain testnet. To verify, type:

```
oraicli query auth account <your-account-address>
```

If you receive something like this below, then you should wait for the development team to send you some tokens first.

```
unknown address: account <account-address> does not exist
```

Once your account is ready, type the command again, and it should look similar to this:

```
{
"type": "cosmos-sdk/Account",
"value": {
    "address": <account-address>,
    "coins": [
    {
        "denom": "orai",
        "amount": "10000000"
    }
    ],
    "public_key": null,
    "account_number": "5",
    "sequence": "0"
}
}
```

Next, broadcast your Validator transaction to become a part of the Oraichain Validator family by using the below command:

```
fn createValidator
```

Finally, after finishing the command, you need to restart your container to update your node state. Type exit then type:

**Step 5: Check your node if it is up and running successfully#**

To check if the process finishes correctly, type:

```
curl http://0.0.0.0:1317/websocket/health
```

If you receive a response similar to this:

```
{"alive": true}
```

then the process is finally complete.

To see if your validator is up and running successfully through the command line, type the following inside your container:

```
oraicli query staking validators | jq '.[] | "\(.description.moniker) \(.status) \(.tokens) \(.operator_address)"' | grep <your-moniker-value>
```

you will receive something similar to this:

```
"duc 2 1000000 oraivaloper1pu9qqke50krgj970hmwmzj6s6ke4gzw4tzzh5c"
```

duc is the moniker name, 2 is the status that you need to have to produce new blocks, 100000 is the amount of ORAI staked, and the last value is your operator address. If the status is 0, then it means you have not delegated enough tokens for your validator to be active.

If you receive it as:

```
{"alive": false}
```

or you cannot find your validator anywhere, then you may have done something not quite right during the process. If that happens, feel free to ask us. We would love to help and support you as much as possible. Currently, in order to produce new blocks, a Validator must have at least one voting power, which equals one million ORAI tokens. If your validator does not have enough tokens, you can delegate more using the following command:

```
oraicli tx staking delegate <oraivaloper operator address > <token amount like 500000000orai> --from <your-account-name>
```

**Final words…#**

After a few steps, you are now an Oraichain Validator node that can have a chance to propose new blocks by being in the top Validators!. Thank you for reading and following all the steps until here. We highly appreciate your effort, and it means a lot to us to deliver a quality product. By having more and more validators, more bugs may come out for us to fix before the mainnet launch. Meanwhile, the investors along with people in the community are able to know more about the system they are investing in. It is a win-win for everyone involved in this ecosystem, and we can, together, make a difference in this crypto world with the world’s first AI-powered data oracle.

### About Oraichain[#](https://docs.orai.io/docs/Developers/OraichainInstallation#about-oraichain)

Oraichain is a data oracle platform that aggregates and connects Artificial Intelligence APIs to smart contracts and regular applications. Oraichain is the world’s first AI-powered Oracle, it aims to revolutionize the AI, DeFi and Blockchain industries.
