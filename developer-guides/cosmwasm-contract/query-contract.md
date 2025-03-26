# Query A Contract

## Command Line Interface

The most common way to query a cosmwasm smart contract is within the oraid wasm smart query command. This follows the following format where query is a JSON string with no spaces. By default, the least amount of data this can be is an empty JSON payload `'{}'.`

```
oraid query wasm contract-state smart [contract_bech32] [query] [flags]
```

For this example, we will use a example scenario of NFT contract. This will show you how to brute force query a contract if you have no idea what the query schema of the contract is. At this time, there is no way to query the format of a contract's requests, but this is something many are actively working on.

Now we attempt to query this contract address and extract some data from it and get which queries are allowed. As you can see, we pass through a random payload for abcde so that the contract will return actual valid query requests

**NOTE**: A Query can never be empty such as `'{}'` given you need to specify the path of data you want to reach.

<pre class="language-sh"><code class="lang-sh">CONTRACT=orai1ase8wkkhczqdda83f0cd9lnuyvf47465j70hyk
oraid q wasm contract-state smart $CONTRACT '{"abcde":{}}' --node https://rpc.orai.io

<strong># Error parsing into type 
</strong><strong>#    Error parsing into type oraichain_nft::msg::QueryMsg: 
</strong>#    unknown variant `abcde`, 
#    expected one of `owner_of`, `approved_for_all`, `num_tokens`, `contract_info`, 
#    `nft_info`, `all_nft_info`, `tokens`, `all_tokens`, `minter` 
</code></pre>

{% hint style="info" %}
The query shows CW721 Base is this contracts name. As this is a standard contract, all messages can be found in the CosmWasm/cw-nfts repository on github\
\
[https://github.com/CosmWasm/cw-nfts/blob/main/contracts/cw721-base/src/msg.rs](https://github.com/CosmWasm/cw-nfts/blob/main/contracts/cw721-base/src/msg.rs)
{% endhint %}

From this, we now know all of the query endpoints and can requests something more specific from the contract for our usage. Let's get

```sh
CONTRACT=orai1ase8wkkhczqdda83f0cd9lnuyvf47465j70hyk
oraid q wasm contract-state smart $CONTRACT '{"all_tokens":{}}' --node https://rpc.orai.io

data:
  tokens:
  - "10"
  - "100"
  - "1000"
  - "10005"
  - "10006"
  - "10009"
  - "1001"
  - "10010"
  - "10011"
  - "10023"
  
# You can use --output json to read it via JSON form
# oraid q wasm contract-state smart $CONTRACT '{"all_tokens":{}}' --output json | jq .data
```

Here we can see there are 8 tokens in this set. Lets query one of the NFTs information

```bash
CONTRACT=orai1ase8wkkhczqdda83f0cd9lnuyvf47465j70hyk
oraid q wasm contract-state smart $CONTRACT '{"nft_info":{}}' --node https://rpc.orai.io

# missing field `token_id`: query wasm contract failed
```

Just like the first query, we can see that the payload needs more information. It returned an error that we need to specify the token\_id we want the nft\_info for. Note, Uint128 sized numbers are read as a string

```bash
CONTRACT=orai1ase8wkkhczqdda83f0cd9lnuyvf47465j70hyk
oraid q wasm contract-state smart $CONTRACT '{"nft_info":{"token_id":"10"}}' --node https://rpc.orai.io

# data:
#   description: Thay pagoda, Hoai Duc, Ha Noi
#   image: https://gateway.ipfs.airight.io/ipfs/QmPhRnGtGWEd1He6zegcKZK86X5uRg2r17AZDNoCXoFqRL
#   name: Thay pagoda
```
