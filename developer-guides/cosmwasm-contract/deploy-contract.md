# Deploy a Contract

## PreRequisite

Make sure you follow the [compile a contract](compile-contract.md) guide first for your project. Once you have a contract compiled in the artifacts/\*.wasm directory, you are ready for this guide.

This guide will show you how to deploy and interact with a contract using CLI. 


## Upload Contract

You need to upload the contract via the CLI to chain. To do this, perform the following

```bash
TXFLAGS="--chain-id Oraichain --gas auto --gas-adjustment 1.5 --fees 1000orai --node https://rpc.orai.io"

oraid tx wasm store artifacts/CONRTACT_NAME.wasm \
    --from <KEY> $TXFLAGS -y
```

This will then return a transaction hash. With this data, you need to query it to get the code id of the contract on chain

```bash
oraid q tx 38330E909CD219B80927009DA37FD69D334D19B2AD4EC47456A24E85034F0085 --output json --node https://rpc.orai.io
```

This will return the data about the transaction, and give us the code id of our contract

```json
{
  "height": "148",
  "txhash": "2A52A8B3201425A534052C1616C088271B90EBAFF3ECF4B9784420EF1D531B6D",
  "codespace": "",
  "code": 0,
  "data": "124E0A262F636F736D7761736D2E7761736D2E76312E4D736753746F7265436F6465526573706F6E7365122408011220795F644FE9F0CC5F5E2C48ADCE0F0C28B76AA8D3BF7DCF0104085A1B767248E1",
  "raw_log": "",
  "logs": [],
  "info": "",
  "gas_wanted": "1488045",
  "gas_used": "993511",
  "tx": {
    "@type": "/cosmos.tx.v1beta1.Tx",
    "body": {
      "messages": [
        {
          "@type": "/cosmwasm.wasm.v1.MsgStoreCode",
          "sender": "orai10vvuu8lgc70rrqdkr03jchtd4yk023gylsr84c",
          "wasm_byte_code": "FU4jZbtUYhyy2XsU2i1+2my2q3bepA0PSasjyNIKAftcBywqHisQMWnC1R8SlSl42jxxnwBcMu9N+a4jLIFkfkVc+RDglRa5LlTWs27naZIVsyRf4wdO1Pq2Kykjp0W6QCIcBRG0fKrPVFxAM3bCRXkOJiu1iodCkmMzuc1J5ESrRLsKQZOt3Oh17YhrQR5RyFB3inIym3OSoSLmZDqmZuu2Tw1Yphsdiw+cQILIJXdKMvwvCMjrXQBtlwcDEfkqGG9YARHySxEZIQ9cIy1xQe9aGEQxH4ReGcFhCcluiODtUPLOY2PL56HeI7k2QOSmkwQlWYKwHu3y0k7OpyPNnyOI/gUi2Y0gaqVvIURXsjEi/G+wuYAPonehX5bhTTJCMsT+TjebOzeSQnUQAQ90Q3hJE/LQso64JVqmB8LNtCIIUADFXC5pPgTQ3QCCkgRc/R4A18kXwwACCITQfOCS3F4v8nTIfyqBhNzrhfWwyQUbgAc2slJdWXiHB8A1MpA/QsBT9ICRyjs8XmgXv7fr/HsipoDXYwqYiilgaZ8CLuxT/k8AAAD//+oTrtn/FQIA",
          "instantiate_permission": null
        }
      ],
      "memo": "",
      "timeout_height": "0",
      "extension_options": [],
      "non_critical_extension_options": []
    },
    "auth_info": {
      "signer_infos": [
        {
          "public_key": {
            "@type": "/cosmos.crypto.secp256k1.PubKey",
            "key": "A6Le3gjpOFSjsX6mBy///SfMrmpKAYuCralBe30yc0jr"
          },
          "mode_info": { "single": { "mode": "SIGN_MODE_DIRECT" } },
          "sequence": "1"
        }
      ],
      "fee": {
        "amount": [{ "denom": "orai", "amount": "200" }],
        "gas_limit": "1488045",
        "payer": "",
        "granter": ""
      },
      "tip": null
    },
    "signatures": [
      "aU8Xty61MvidTNl5zbLY3r1882pAcvMnt4qYluZ3/zEhlHSvfELQszJcFdrNbsMU08IRwLMPQEPUM3aPs1pTUw=="
    ]
  },
  "timestamp": "2025-03-26T08:56:07Z",
  "events": [
    {
      "type": "coin_spent",
      "attributes": [
        {
          "key": "spender",
          "value": "orai10vvuu8lgc70rrqdkr03jchtd4yk023gylsr84c",
          "index": true
        },
        { "key": "amount", "value": "200orai", "index": true }
      ]
    },
    {
      "type": "coin_received",
      "attributes": [
        {
          "key": "receiver",
          "value": "orai17xpfvakm2amg962yls6f84z3kell8c5lr24r2w",
          "index": true
        },
        { "key": "amount", "value": "200orai", "index": true }
      ]
    },
    {
      "type": "transfer",
      "attributes": [
        {
          "key": "recipient",
          "value": "orai17xpfvakm2amg962yls6f84z3kell8c5lr24r2w",
          "index": true
        },
        {
          "key": "sender",
          "value": "orai10vvuu8lgc70rrqdkr03jchtd4yk023gylsr84c",
          "index": true
        },
        { "key": "amount", "value": "200orai", "index": true }
      ]
    },
    {
      "type": "message",
      "attributes": [
        {
          "key": "sender",
          "value": "orai10vvuu8lgc70rrqdkr03jchtd4yk023gylsr84c",
          "index": true
        }
      ]
    },
    {
      "type": "tx",
      "attributes": [{ "key": "fee", "value": "200orai", "index": true }]
    },
    {
      "type": "tx",
      "attributes": [
        {
          "key": "acc_seq",
          "value": "{\u0019�\u001f�Ǟ1��\u001b�,]m�,�E\u0004/1",
          "index": true
        }
      ]
    },
    {
      "type": "tx",
      "attributes": [
        {
          "key": "signature",
          "value": "aU8Xty61MvidTNl5zbLY3r1882pAcvMnt4qYluZ3/zEhlHSvfELQszJcFdrNbsMU08IRwLMPQEPUM3aPs1pTUw==",
          "index": true
        }
      ]
    },
    {
      "type": "message",
      "attributes": [
        {
          "key": "action",
          "value": "/cosmwasm.wasm.v1.MsgStoreCode",
          "index": true
        },
        {
          "key": "sender",
          "value": "orai10vvuu8lgc70rrqdkr03jchtd4yk023gylsr84c",
          "index": true
        },
        { "key": "module", "value": "wasm", "index": true },
        { "key": "msg_index", "value": "0", "index": true }
      ]
    },
    {
      "type": "store_code",
      "attributes": [
        {
          "key": "code_checksum",
          "value": "795f644fe9f0cc5f5e2c48adce0f0c28b76aa8d3bf7dcf0104085a1b767248e1",
          "index": true
        },
        { "key": "code_id", "value": "1", "index": true },
        { "key": "msg_index", "value": "0", "index": true }
      ]
    }
  ]
}

```

We can see events with type store\_code shows the code\_id being 1. If you wish the automate this return code in bash to a variable, you can&#x20;

```bash
# ensure jq is installed
UPLOAD_TX_HASH=38330E909CD219B80927009DA37FD69D334D19B2AD4EC47456A24E85034F0085
CODE_ID=$(oraid q tx $UPLOAD_TX_HASH --output json | jq -r '.events[] | select(.type == "store_code") | .attributes[] | select(.key == "code_id") | .value') && echo "Code Id: $CODE_ID"
```

## Instantiate

With the code now being up on chain, we can now run logic to setup our own copy of the contract which we control. This will then give us a unique contract address for others to interact with in accordance with the contract logic. This example is from the [cosmwasm/cw-template](https://github.com/CosmWasm/cw-template).

Ensure you change CODE\_ID to match your code id from the store code

<pre class="language-sh"><code class="lang-sh"># Manual
CODE_ID=1
oraid tx wasm instantiate $CODE_ID <contract_instantiate_msg> --label "some-contract" $FLAGS -y --admin &#x3C;your-address-here>
<strong># then query the tranasaction hash as we do above.
</strong><strong>
</strong># Automated return of the contract address
CODE_ID=1
TX_INIT=$(oraid tx wasm instantiate "$CODE_ID" '{"count":0}' --label "contract" $FLAGS -y --admin &#x3C;your-address-here> | jq -r '.txhash') &#x26;&#x26; echo $TX_INIT
CONTRACT_ADDR=$($BINARY query tx $TX_INIT --output json | jq -r '.events[] | select(.type == "instantiate") | .attributes[] | select(.key == "_contract_address") | .value') &#x26;&#x26; echo "CONTRACT_ADDR: $CONTRACT_ADDR"
</code></pre>
