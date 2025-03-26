# Send Tokens to a Contract

## Command Line Interface

When you execute a message, a user can also pass through a flag which sends funds from their account to the contract to do logic. You can check if a user sends any funds in your contract's execute endpoint with the `info.funds` array of Coins sent by the user. These funds then get added to the contracts balance just like any other account. So it is up to you as the developer to ensure to save how many funds each user has sent via a BTreeMap or other object storage in state (if they can redeem funds back at a later time).

To send funds to a contract with some arbitrary endpoint, you use the `--amount` flag.

```sh
oraid tx wasm execute CONTRACT '{"some_endpoint":{}}' --amount 1000000orai
```

{% hint style="info" %}
If the "some_endpoint" execute errors on the contract, the funds will remain in the users account.
{% endhint %}

## Typescript

```typescript
import { coin, makeCosmoshubPath } from "@cosmjs/amino";
import { GasPrice } from "@cosmjs/stargate";
import { DirectSecp256k1HdWallet } from "@cosmjs/proto-signing";
import { SigningCosmWasmClient } from "@cosmjs/cosmwasm-stargate";
import "dotenv/config";
import { MsgExec } from "cosmjs-types/cosmos/authz/v1beta1/tx";
import { MsgSend } from "cosmjs-types/cosmos/bank/v1beta1/tx";

const main = async () => {
  const mnemonic = "YOUR_MNEMONIC_HERE";
  const chainInfo = {
    chainId: "Oraichain",
    rpcEndpoint: "https://rpc.orai.io",
    prefix: "orai",
    gasPrice: GasPrice.fromString("0.002orai"),
    feeToken: "orai",
  };
  const hdPath = makeCosmoshubPath(0);

  // Setup signer
  const offlineSigner = await DirectSecp256k1HdWallet.fromMnemonic(mnemonic, {
    prefix: chainInfo.prefix,
    hdPaths: [hdPath],
  });

  const { address } = (await offlineSigner.getAccounts())[0];
  console.log(`Connected to ${address}`);

  // Init SigningCosmWasmClient client
  const client = await SigningCosmWasmClient.connectWithSigner(
    chainInfo.rpcEndpoint,
    offlineSigner,
    {
      gasPrice: chainInfo.gasPrice,
    }
  );

  const balance = await client.getBalance(address, chainInfo.feeToken);
  console.log("balance: ", balance);

  const sendCoin = coin(1000, "orai");
  const tx = await client.execute(
    address,
    "CONTRACT_ADDRESS",
    "CONTRACT_MSG",
    "auto",
    "memo",
    [sendCoin]
  );

  console.log("txhash: ", tx.transactionHash);
};

main();
```
