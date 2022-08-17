# OWallet Extension Cosmos-based APIs

Since OWallet Extension is based on [Keplr Wallet](https://github.com/chainapsis/keplr-wallet), its basic API & intergration method are similar to those of Keplr.

## How to detect OWallet Extension for Cosmos-based networks

The OWallet object is injected into the `window` object as window.owallet or window.keplr for backward compatibility with the Keplr Wallet. If it is **undefined**, then you will need to install it on the browser first. Similarly to the Keplr's documentation, there are a few ways to check the object's status:

```js
window.onload = async () => {
    if (!window.owallet) {
        alert("Please install owallet extension");
    } else {
        const chainId = "Oraichain";
        await window.owallet.enable(chainId);
    }
}
```

or you can check the browser document's state:

```js
function getOWallet(): Promise<OWallet | undefined> {
    if (window.owallet) {
        return window.owallet;
    }
    
    if (document.readyState === "complete") {
        return window.owallet;
    }
    
    return new Promise((resolve) => {
        const documentStateChange = (event: Event) => {
            if (
                event.target &&
                (event.target as Document).readyState === "complete"
            ) {
                resolve(window.owallet);
                document.removeEventListener("readystatechange", documentStateChange);
            }
        };
        
        document.addEventListener("readystatechange", documentStateChange);
    });
}
```

## Features

We share the same features as Keplr's when it comes to the Cosmos-based networks. Hence, the below documentation about the features are brought from the [Keplr Wallet documentation website](https://docs.keplr.app/api/) with slight modifications.

If you were able to connect Keplr with CosmJS, you may skip to the [Use Keplr with CosmJS](./cosmjs.md) section.

While Keplr supports an easy way to connect to CosmJS, there are additional functions specific to Keplr which provides additional features.

### Using with Typescript
**`window.d.ts`**
```javascript
import { Window as OWalletWindow } from "@owallet/types";

declare global {
  // eslint-disable-next-line @typescript-eslint/no-empty-interface
  interface Window extends OWalletWindow {}
}
```

The `@owallet/types` package has the type definition related to OWallet.  
If you're using TypeScript, run `npm install --save-dev @owallet/types` or `yarn add -D @owallet/types` to install `@owallet/types`.  
Then, you can add the `@owallet/types` window to a global window object and register the OWallet related types.

> Usage of any other packages besides @owallet/types is not recommended.
> - Any other packages besides @owallet/types are actively being developed, backward compatibility is not in the scope of support.
> - Since there are active changes being made, documentation is not being updated to the most recent version of the package as of right now. Documentations would be updated as packages get stable.

### Enable Connection

```javascript
enable(chainIds: string | string[]): Promise<void>
```

The `window.owallet.enable(chainIds)` method requests the extension to be unlocked if it's currently locked. If the user hasn't given permission to the webpage, it will ask the user to give permission for the webpage to access OWallet.

`enable` method can receive one or more chain-id as an array. When the array of chain-id is passed, you can request permissions for all chains that have not yet been authorized at once.

If the user cancels the unlock or rejects the permission, an error will be thrown.

### Get Address / Public Key

```javascript
getKey(chainId: string): Promise<{
    // Name of the selected key store.
    name: string;
    algo: string;
    pubKey: Uint8Array;
    address: Uint8Array;
    bech32Address: string;
}>
```

If the webpage has permission and OWallet is unlocked, this function will return the address and public key in the following format:

```javascript
{
    // Name of the selected key store.
    name: string;
    algo: string;
    pubKey: Uint8Array;
    address: Uint8Array;
    bech32Address: string;
    isNanoLedger: boolean;
}
```

It also returns the nickname for the key store currently selected, which should allow the webpage to display the current key store selected to the user in a more convenient mane.  
`isNanoLedger` field in the return type is used to indicate whether the selected account is from the Ledger Nano. Because current Cosmos app in the Ledger Nano doesn't support the direct (protobuf) format msgs, this field can be used to select the amino or direct signer. [Ref](./cosmjs.md#types-of-offline-signers)

### Sign Amino

```javascript
signAmino(chainId: string, signer: string, signDoc: StdSignDoc): Promise<AminoSignResponse>
```

Similar to CosmJS `OfflineSigner`'s `signAmino`, but OWallet's `signAmino` takes the chain-id as a required parameter. Signs Amino-encoded `StdSignDoc`.

### Sign Direct / Protobuf

```javascript
signDirect(chainId:string, signer:string, signDoc: {
    /** SignDoc bodyBytes */
    bodyBytes?: Uint8Array | null;

    /** SignDoc authInfoBytes */
    authInfoBytes?: Uint8Array | null;

    /** SignDoc chainId */
    chainId?: string | null;

    /** SignDoc accountNumber */
    accountNumber?: Long | null;
  }): Promise<DirectSignResponse>
```

Similar to CosmJS `OfflineDirectSigner`'s `signDirect`, but OWallet's `signDirect` takes the chain-id as a required parameter. Signs Proto-encoded `StdSignDoc`.

### Request Transaction Broadcasting

```javascript
sendTx(
    chainId: string,
    tx: Uint8Array,
    mode: BroadcastMode
): Promise<Uint8Array>;
```

This function requests OWallet to delegates the broadcasting of the transaction to OWallet's LCD endpoints (rather than the webpage broadcasting the transaction).
This method returns the transaction hash if it succeeds to broadcast, if else the method will throw an error.
When OWallet broadcasts the transaction, OWallet will send the notification on the transaction's progress.

### Request Signature for Arbitrary Message

```javascript
signArbitrary(
    chainId: string,
    signer: string,
    data: string | Uint8Array
): Promise<StdSignature>;
verifyArbitrary(
    chainId: string,
    signer: string,
    data: string | Uint8Array,
    signature: StdSignature
): Promise<boolean>;
```

This is an experimental implementation of [ADR-36](https://github.com/cosmos/cosmos-sdk/blob/master/docs/architecture/adr-036-arbitrary-signature.md). Use this feature at your own risk.  
  
It's main usage is to prove ownership of an account off-chain, requesting ADR-36 signature using the `signArbitrary` API.  
  
If requested sign doc with the `signAnimo` API with the ADR-36 that OWallet requires instead of using the `signArbitary` API, it would function as `signArbitary`  
- Only supports sign doc in the format of Amino. (in the case of protobuf, [ADR-36](https://github.com/cosmos/cosmos-sdk/blob/master/docs/architecture/adr-036-arbitrary-signature.md) requirements aren't fully specified for implementation)
- sign doc message should be single and the message type should be "sign/MsgSignData"
- sign doc "sign/MsgSignData" message should have "signer" and "data" as its value. "data" should be base64 encoded
- sign doc chain_id should be an empty string("")
- sign doc memo should be an empty string("")
- sign doc account_number should be "0"
- sign doc sequence should be "0"
- sign doc fee should be `{gas: "0", amount: []}`

When using the `signArbitrary` API, if the `data` parameter type is `string`, the signature page displays as plain text.  
  
Using `verifyArbitrary`, you can verify the results requested by `signArbitrary` API or `signAmino` API that has been requested with the ADR-36 spec standards.  
  
`verifyArbitrary` has been only implemented for simple usage. `verifyArbitrary` returns the result of the verification of the current selected account's sign doc. If the account is not the currently selected account, it would throw an error.  
  
It is recommended to use `verifyADR36Amino` function in the `@owallet/cosmos` package or your own implementation instead of using `verifyArbitrary` API.  
  
### Interaction Options

```javascript
export interface KeplrIntereactionOptions {
  readonly sign?: KeplrSignOptions;
}

export interface KeplrSignOptions {
  readonly preferNoSetFee?: boolean;
  readonly preferNoSetMemo?: boolean;
}
```
OWallet v0.8.11+ offers additional options to customize interactions between the frontend website and OWallet extension.

If `preferNoSetFee` is set to true, OWallet will prioritize the frontend-suggested fee rather than overriding the tx fee setting of the signing page.

If `preferNoSetMemo` is set to true, OWallet will not override the memo and set fix memo as the front-end set memo.

You can set the values as follows:
```javascript
window.owallet.defaultOptions = {
    sign: {
        preferNoSetFee: true,
        preferNoSetMemo: true,
    }
}
```

### Custom event

**Change Key Store Event**

```javascript
keplr_keystorechange
```

When the user switches their key store/account after the webpage has received the information on the key store/account the key that the webpage is aware of may not match the selected key in OWallet which may cause issues in the interactions.

To prevent this from happening, when the key store/account is changed, OWallet emits a `keplr_keystorechange` event to the webpage's window. You can request the new key/account based on this event listener.

```javascript
window.addEventListener("keplr_keystorechange", () => {
    console.log("Key store in OWallet is changed. You may need to refetch the account info.")
})
```

## CosmJS Integration

Similar to how Keplr [connects with CosmJS](https://docs.keplr.app/api/cosmjs.html), OWallet also uses `OfflineSigner` for Cosmos SDK Launchpad & `OfflineDirectSigner` for Cosmos SDK Stargate.

You can get the signer via:

```js

var offlineSigner = window.getOfflineSigner(chainId);
// or
var offlineSigner = await window.getOfflineSignerAuto(chainId);
// or
var offlineSigner = window.getOfflineSignerOnlyAmino(chainId);

```

then you can pass the `offlineSigner` variable into the `SigningCosmosClient`:

```js
// Initialize the gaia api with the offline signer that is injected by Keplr extension.
const cosmJS = new SigningCosmosClient(
    "https://rpc.orai.io",
    accounts[0].address,
    offlineSigner,
);
```

## Suggest chain

It works the same as the Keplr's [suggest chain feature](https://docs.keplr.app/api/suggest-chain.html), but with a slight change in the `ChainInfo` interface. We add several more fields to better filter networks within the Cosmos ecosystem.

dApps can request the wallet to add new Cosmos chains that are not supported by default. This allows the wallet to be fully decentralized & permissionless.

Below is the `ChainInfo` interface of the OWallet extension:

```js
interface ChainInfo {
    readonly rpc?: string;
    /**
     * evmRpc is only used for EVM-based networks, not Cosmos-based
     */
    readonly evmRpc?: string;
    readonly rpcConfig?: AxiosRequestConfig;
    readonly rest: string;
    readonly restConfig?: AxiosRequestConfig;
    readonly chainId: string;
    readonly chainName: string;
    readonly networkType: NetworkType;
    /**
     * This indicates the type of coin that can be used for stake.
     * You can get actual currency information from Currencies.
     */
    readonly stakeCurrency?: Currency;
    readonly bip44: BIP44;
    readonly alternativeBIP44s?: BIP44[];
    readonly bech32Config?: Bech32Config;
    readonly currencies: AppCurrency[];
    /**
     * This indicates which coin or token can be used for fee to send transaction.
     * You can get actual currency information from Currencies.
     */
    readonly feeCurrencies: Currency[];
    /**
     * This is the coin type in slip-044.
     * This is used for fetching address from ENS if this field is set.
     */
    readonly coinType?: number;
    /**
     * This is used to set the fee of the transaction.
     * If this field is empty, it just use the default gas price step (low: 0.01, average: 0.025, high: 0.04).
     * And, set field's type as primitive number because it is hard to restore the prototype after deserialzing if field's type is `Dec`.
     */
    readonly gasPriceStep?: {
        low: number;
        average: number;
        high: number;
    };
    /**
     * Indicate the features supported by this chain. Ex) cosmwasm, secretwasm ...
     */
    readonly features?: string[];
    /**
     * Shows whether the blockchain is in production phase or beta phase.
     * Major features such as staking and sending are supported on staging blockchains, but without guarantee.
     * If the blockchain is in an early stage, please set it as beta.
     */
    readonly beta?: boolean;
}
```

the suggest chain API is:

```js
experimentalSuggestChain(chainInfo: SuggestingChainInfo): Promise<void>
```

examples:

```js
await window.keplr.experimentalSuggestChain({
    chainId: "Oraichain",
    chainName: "Oraichain",
    rpc: "https://rpc.orai.io",
    rest: "https://lcd.orai.io",
    bip44: {
        coinType: 118,
    },
    bech32Config: {
        bech32PrefixAccAddr: "orai",
        bech32PrefixAccPub: "orai" + "pub",
        bech32PrefixValAddr: "orai" + "valoper",
        bech32PrefixValPub: "orai" + "valoperpub",
        bech32PrefixConsAddr: "orai" + "valcons",
        bech32PrefixConsPub: "orai" + "valconspub",
    },
    currencies: [ 
        { 
            coinDenom: "ORAI", 
            coinMinimalDenom: "orai", 
            coinDecimals: 6, 
            coinGeckoId: "orai", 
        }, 
    ],
    feeCurrencies: [
        {
            coinDenom: "ORAI",
            coinMinimalDenom: "orai",
            coinDecimals: 6,
            coinGeckoId: "orai",
        },
    ],
    stakeCurrency: {
        coinDenom: "ORAI",
        coinMinimalDenom: "orai",
        coinDecimals: 6,
        coinGeckoId: "orai",
    },
    gasPriceStep: {
        low: 0.01,
        average: 0.025,
        high: 0.03,
    },
    beta: true,
    features: [
        "stargate",
        "ibc-transfer",
        "cosmwasm"
    ],
});

```