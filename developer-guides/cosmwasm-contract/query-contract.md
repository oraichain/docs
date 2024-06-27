# Query A Contract

## Command Line Interface

The most common way to query a cosmwasm smart contract is within the oraid wasm smart query command. This follows the following format where query is a JSON string with no spaces. By default, the least amount of data this can be is an empty JSON payload `'{}'.`

```
oraid query wasm contract-state smart [contract_bech32] [query] [flags]
```

For this example, we will use a example scenario of NFT contract. This will show you how to brute force query a contract if you have no idea what the query schema of the contract is. At this time, there is no way to query the format of a contract's requests, but this is something many are actively working on.

Now we attempt to query this contract address and extract some data from it and get which queries are allowed. As you can see, we pass through a random payload for abcde so that the contract will return actual valid query requests

**NOTE**: A Query can never be empty such as `'{}'` given you need to specify the path of data you want to reach.

<pre class="language-sh"><code class="lang-sh">CONTRACT=orai1anh4pf98fe8uh64uuhaasqdmg89qe6kk5xsklxuvtjmu6rhpg53sj9uejj
oraid q wasm contract-state smart $CONTRACT '{"abcde":{}}'

<strong># Error parsing into type 
</strong><strong>#    cw721_base::msg::QueryMsg&#x3C;cosmwasm_std::results::empty::Empty>
</strong>#    unknown variant `abcde`, 
#    expected one of `owner_of`, `approval`, `approvals`, `all_operators`, 
#    `num_tokens`, `contract_info`, `nft_info`, `all_nft_info`, `tokens`, 
#    `all_tokens`, `minter`, `extension`
</code></pre>

{% hint style="info" %}
The query shows CW721 Base is this contracts name. As this is a standard contract, all messages can be found in the CosmWasm/cw-nfts repository on github\
\
[https://github.com/CosmWasm/cw-nfts/blob/main/contracts/cw721-base/src/msg.rs](https://github.com/CosmWasm/cw-nfts/blob/main/contracts/cw721-base/src/msg.rs)
{% endhint %}

From this, we now know all of the query endpoints and can requests something more specific from the contract for our usage. Let's get

```sh
CONTRACT=orai1anh4pf98fe8uh64uuhaasqdmg89qe6kk5xsklxuvtjmu6rhpg53sj9uejj
oraid q wasm contract-state smart $CONTRACT '{"all_tokens":{}}'

data:
  tokens:
  - "0"
  - "1"
  - "2"
  - "3"
  - "4"
  - "5"
  - "6"
  - "7"
  - "8"
  
# You can use --output=json to read it via JSON form
# oraid q wasm contract-state smart $CONTRACT '{"all_tokens":{}}' --output=json | jq .data
```

Here we can see there are 8 tokens in this set. Lets query one of the NFTs information

```bash
CONTRACT=orai1anh4pf98fe8uh64uuhaasqdmg89qe6kk5xsklxuvtjmu6rhpg53sj9uejj
oraid q wasm contract-state smart $CONTRACT '{"nft_info":{}}'

# missing field `token_id`: query wasm contract failed
```

Just like the first query, we can see that the payload needs more information. It returned an error that we need to specify the token\_id we want the nft\_info for. Note, Uint128 sized numbers are read as a string

```bash
CONTRACT=orai1anh4pf98fe8uh64uuhaasqdmg89qe6kk5xsklxuvtjmu6rhpg53sj9uejj
oraid q wasm contract-state smart $CONTRACT '{"nft_info":{"token_id":"8"}}'

# data:
#   extension: null
#   token_uri: ipfs://bafyreib42csdu7426ki7mxk6firvbz4uk3fo4dxpjy2kkskzdhtgj3rriq/metadata.json
```
