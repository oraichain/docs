# Become a Validator

## Requirements

### 1. A Synced Node
You need to have a fully synced node running on the Oraichain mainnet. Follow our [tutorial](./become-a-full-node-operator-from-source.md) to set up a node.

### 2. A Wallet
Your wallet must have some ORAI tokens to delegate and pay transaction fees.

#### Create a New Wallet
Replace `KEY_NAME` with your preferred wallet name:
```bash
oraid keys add KEY_NAME
```
**Important:** Save your passphrase, address, and mnemonic securely.

#### List All Keys
```bash
oraid keys list
```

#### Get Validator Public Key
```bash
oraid tendermint show-validator
```

---

## Create `validator.json` File
As of version **v0.50**, creating a `validator.json` file is required.

### Sample `validator.json`
```json
{
    "pubkey": {"@type":"/cosmos.crypto.ed25519.PubKey","key":"XXXXXXXX"},
    "amount": "1000000orai",
    "moniker": "myvalidator",
    "identity": "optional identity signature (ex. UPort or Keybase)",
    "website": "validator's (optional) website",
    "security": "validator's (optional) security contact email",
    "details": "validator's (optional) details",
    "commission-rate": "0.1",
    "commission-max-rate": "0.2",
    "commission-max-change-rate": "0.01",
    "min-self-delegation": "1"
}
```
Edit the file with your actual validator details:

- **`pubkey`**: The validator's Protobuf JSON encoded public key (retrieved from the previous command).
- **`amount`**: Amount of coins to bond (e.g., `1000000orai = 1 ORAI` token).
- **`moniker`**: Your validator's name.
- **`commission-rate`**: Initial commission rate (e.g., `0.1` for 10%).
- **`commission-max-rate`**: Maximum commission rate (e.g., `0.2` for 20%).
- **`commission-max-change-rate`**: Maximum daily commission rate change (e.g., `0.01` for 1% per day).
- **`min-self-delegation`**: Minimum self-delegation (e.g., `1` ORAI).
- **`website`**: (Optional) Your validator's website.
- **`details`**: (Optional) Additional validator information.

---

## Create Validator Command
Before creating your validator, **fund your wallet with some ORAI tokens** to activate it.

#### Command Format
```bash
oraid tx staking create-validator path/to/validator.json \
--from [KEY_NAME] \
--chain-id="Oraichain" \
--gas="auto" \
--gas-prices="[gas_price]"
```

#### Example
```bash
oraid tx staking create-validator /root/validator.json \
--from=light1 \
--chain-id="Oraichain" \
--gas="auto" \
--gas-prices=0.001orai
```

### Explanation of Flags
- **`--from`**: The name or address of the private key used to sign.
- **`--chain-id`**: The chain ID (fixed as `Oraichain`).
- **`--gas-prices`**: Gas prices in decimal format to determine the transaction fee.

---

## Track Your Validator

### Check Active Validator Set
Replace `OraichainValidator` with your moniker name:
```bash
oraid query staking validators | grep OraichainValidator
```

### Track Signing History
```bash
oraid query slashing signing-info $(oraid tendermint show-validator)
```

---

## Join the Validator Community
Join the [Oraichain validators group](https://t.me/joinchat/yH9nMLrokQRhZGY1) on Telegram to discuss ideas and troubleshoot issues!

