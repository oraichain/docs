# Joining Oraichain mainnet

## Basic Oraichain mainnet metadata & properties:

**Chain ID:** Oraichain

**Coin minimal denom:** orai

**Coin decimal:** 6

**Bip44 Coin type:** 118

**Light Client Daemon (LCD):** https://lcd.orai.io

**Remote Procedural Call (RPC):** https://rpc.orai.io

**Google Remote Procedural Call (gRPC):** http://18.223.242.70:9090, http://18.116.209.76:9090

**Cosmos SDK version:** v0.42.11

**Tendermint version:** v0.34.14

**Cosmwasm-std version:** v0.13.2 (with no CustomQuery feature). Documentation: https://oraiwasm.web.app

### Node hardwarde specification:

A dedicated node that can run Docker amd64.

**Minimum requirements**

```
The number of CPUs: 2vCPUs
RAM: 4GB
Storage: 200GB SSD
```

**Recommended requirements**

```
The number of CPUs: 4vCPUs
RAM: 4GB
Storage: 500GB SSD
```

**Estimated cost for a node with minimum requirements using Digital Ocean: $25/month**

![droplet](https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/droplet.jpg)

![disk](https://raw.githubusercontent.com/oraichain/oraichain-static-files/master/disk.jpg)

### Chain Data Download URLs:

- 1st option:

| Region       | Default                                                                                | Pruned                                                            |
| :---         | :---                                                                                   | :----                                                         |
| Stockholm    |                                                                                 | https://orai-stockholm.s3.eu-north-1.amazonaws.com/mainnet_statesync_data_9077207_wasm_at_9149946_v0410.tar.gz                                                             |
| Singapore    |                                                                               | https://orai-singapore.s3.ap-southeast-1.amazonaws.com/mainnet_statesync_data_9077207_wasm_at_9149946_v0410.tar.gz                                                              |
| US-East      | https://orai.s3.us-east-2.amazonaws.com/mainnet_default_bk.tar.gz                      | https://orai.s3.us-east-2.amazonaws.com/mainnet_statesync_data_9077207_wasm_at_9149946_v0410.tar.gz  |

- 2nd option:

| Region       | Pruned                                                            |
| :---         | :----                                                         |
| Stockholm    | https://orai-stockholm.s3.eu-north-1.amazonaws.com/mainnet_statesync_data_9149946_wasm_at_9149946_v0410.tar.gz                                                             |
| Singapore    | https://orai-singapore.s3.ap-southeast-1.amazonaws.com/mainnet_statesync_data_9149946_wasm_at_9149946_v0410.tar.gz                                                              |
| US-East      | https://orai.s3.us-east-2.amazonaws.com/mainnet_statesync_data_9149946_wasm_at_9149946_v0410.tar.gz  |
