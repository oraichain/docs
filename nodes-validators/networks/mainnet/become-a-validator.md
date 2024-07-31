# Become a Validator

## Requirement

#### 1. A synced Node

You can follow our tutorial to run a node on the Oraichain mainnet.

#### 2. A wallet

Your wallet must have some ORAI tokens to delegate and pay transaction fees.

Create a new wallet, change _**KEY\_NAME**_ as you like

```bash
oraid keys add KEY_NAME
```

_**Save your passphrase, address, mnemonic**_

List all your keys

```bash
oraid keys list
```

Get validator public key

```bash
oraid tendermint show-validator
```

## Create validator command

_**After you got wallet address, you must send some oraichain token to this wallet to active it.**_ Here is the empty command

```bash
oraid tx staking create-validator \
--from=[KEY_NAME] \
--amount=[staking_amount_uorai] \
--pubkey=$(oraid tendermint show-validator) \
--moniker="[moniker_id_of_your_node]" \
--chain-id="[chain-id]" \
--commission-rate="[commission_rate]" \
--commission-max-rate="[maximum_commission_rate]" \
--commission-max-change-rate="[maximum_rate_of_change_of_commission]" \
--min-self-delegation="[min_self_delegation_amount]" \
--gas="auto" \
--gas-prices="[gas_price]" \
```

Example

```bash
oraid tx staking create-validator \
--from=light1 \
--amount=1000000orai \
--pubkey=$(oraid tendermint show-validator) \
--moniker="OraichainValidator" \
--chain-id="Oraichain" \
--commission-rate="0.03" \
--commission-max-rate="0.1" \
--commission-max-change-rate="0.1" \
--min-self-delegation="1000000" \
--website="https://orai.io" \
--details="I'm trust and safe validator" \
--gas-prices=0.001orai
```

The _**from**_ flag is Name or address of private key with which to sign

The _**amount**_ flag is Amount of coins to bond. 1000000orai = 1 oraichain token

The _**pubkey**_ flag is The validator's Protobuf JSON encoded public key

The _**moniker**_ flag is The validator's name

The _**chain-id**_ flag is Oraichain (default value and can't change)

The _**commission-rate**_ is The initial commission rate percentage (in the example above, 3 percent)

The _**commission-max-rate**_ is The maximum commission rate percentage (in the example above, 10 percent)

The _**commission-max-change-rate**_ is The maximum commission change rate percentage (per day) (in the example above, 1 percent per day until reaching the max rate)

The _**min-self-delegatio**_n is The minimum self delegation required on the validator (in the example above, 1 orai)

The _**gas-prices**_ is Gas prices in decimal format to determine the transaction fee

The _**website**_ flag is The validator's (optional) website

The _**details**_ flag is The validator's (optional) detail

## Track validator

To see the current validator active set: (change OraichainValidator to your moniker name)

```bash
oraid query staking validators | grep OraichainValidator
```

To track your validator's signing history

```bash
oraid query slashing signing-info ${oraid tendermint show-validator}
```

Please join the [Oraichain validators group](https://t.me/joinchat/yH9nMLrokQRhZGY1) on Telegram to discuss ideas and problems!
