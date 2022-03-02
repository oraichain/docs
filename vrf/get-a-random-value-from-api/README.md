---
description: >-
  This section showcases how to get a random value inside a smart contract using
  Oraichain VRF 2.0
---

# Get a Random Value from API

## Prerequisite

Before integrating Oraichain VRF 2.0 into your tech stack, you need to install **Oraichain cosmosjs library**. Kindly follow the steps as demonstrated [here](https://www.npmjs.com/package/@oraichain/cosmosjs).

## Integration steps

### For Back-end Usage

#### Step 1: **Import Oraichain cosmosjs library**

```smali
const Cosmos = require("@oraichain/cosmosjs").default;

const lcdUrl = "https://lcd.orai.io";
const chainId = "Oraichain";
const mnemonic = "foo bar";
const requestFees = “100”;

const message = Cosmos.message;
const cosmos = new Cosmos(lcdUrl, chainId);
const childKey = cosmos.getChildKey(mnemonic);
const sender = cosmos.getAddress(mnemonic);

cosmos.setBech32MainPrefix('orai');
```

#### **Step 2:** Request a random transaction

```
const input = Buffer.from(
       JSON.stringify({
           request_random: {
               input: btoa(userInput),
           },
       })
   );
 
const txBody = getHandleMessage(contract, input, sender, requestFees);
const response = await cosmos.submit(childKey, txBody, "BROADCAST_MODE_BLOCK", isNaN(fees) ? 0 : parseInt(fees), gas);
```

Enter a string value of your preference at _**userInput**_. You will see the fee for your request at _**requestFees**_. Also, maximum gas and tx fee need to be decided before each request.

:warning: In the unlikely case that your request takes too long to respond, try switching "**BROADCAST\_MODE\_BLOCK"** to _**"**_**BROADCAST\_MODE\_SYNC"**, and use tx hash to query until it is included on-chain.

Regarding how to query tx hash, please refer to [this example](https://lcd.orai.io/cosmos/tx/v1beta1/txs/%7Btx\_hash%7D) or [this](https://lcd.orai.io/cosmos/tx/v1beta1/txs/397FAFB6973CC0717773290E3F0A0FF2101EDD4689AD983052C60AA3CF8AA5C0).

#### Step 3: Get a random round number

```
const round = response.tx_response.logs[0].events[1].attributes[3].value;
```

#### Step 4: **Get the information from a round**

```
const queryRoundInput = JSON.stringify({
       get_round: {round: parseInt(round)},
   });
 
   const roundOutput = await cosmos.get(`/wasm/v1beta1/contract
/${contract}/smart/${Buffer.from(queryRoundInput).toString("base64")}`);
```

_**contract**_ is the contract address of the VRF issued by Oraichain and can be reviewed on Oraiscan any time**.**

If the round result has not displayed the randomness output, you just need to wait for some time before querying again.

### **For browser usage**

#### **Step 1: Import Oraichain cosmosjs library**

```
import Cosmos from "@oraichain/cosmosjs";
const lcdUrl = "https://lcd.orai.io";
const chainId = "Oraichain";
const requestFees = “100”;

const message = Cosmos.message;
const cosmos = new Cosmos(lcdUrl, chainId);
```

#### Step 2: Import keystation for wallet use by creating a new js file

```
// @ts-nocheck
/* eslint-disable eqeqeq */
// Find Left Boundry of the Screen/Monitor
function FindLeftScreenBoundry() {
   // Check if the window is off the primary monitor in a positive axis
   // X,Y                  X,Y                    S = Screen, W = Window
   // 0,0  ----------   1280,0  ----------
   //     |          |         |  ---     |
   //     |          |         | | W |    |
   //     |        S |         |  ---   S |
   //      ----------           ----------
   if (window.leftWindowBoundry() > window.screen.width) {
       return window.leftWindowBoundry() - (window.leftWindowBoundry() - window.screen.width);
   }
 
   // Check if the window is off the primary monitor in a negative axis
   // X,Y                  X,Y                    S = Screen, W = Window
   // 0,0  ----------  -1280,0  ----------
   //     |          |         |  ---     |
   //     |          |         | | W |    |
   //     |        S |         |  ---   S |
   //      ----------           ----------
   // This only works in Firefox at the moment due to a bug in Internet Explorer opening new windows into a negative axis
   // However, you can move opened windows into a negative axis as a workaround
   if (window.leftWindowBoundry() < 0 && window.leftWindowBoundry() > window.screen.width * -1) {
       return window.screen.width * -1;
   }
 
   // If neither of the above, the monitor is on the primary monitor whose's screen X should be 0
   return 0;
}
 
window.leftScreenBoundry = FindLeftScreenBoundry;
 
function PopupCenter(url, title, w, h) {
   const newWindow = window.open(
       url,
       title,
       "resizable=1, scrollbars=1, fullscreen=0, height=" +
       h +
       ", width=" +
       w +
       ", screenX=" +
       window.leftScreenBoundry +
       " , left=" +
       window.leftScreenBoundry +
       ", toolbar=0, menubar=0, status=1"
   );
   return newWindow;
}
 
function openWindowV1(type, payload, account = "", self) {
   console.log(`open ${self.client}`);
 
   // The account parameter is required for users having multiple keychain accounts.
   let apiUrl = "";
   switch (type) {
       case "signin":
           apiUrl = "signin";
           break;
       case "transaction":
           apiUrl = "tx";
           break;
       default:
           apiUrl = "signin";
           break;
   }
 
   return PopupCenter(
       self.keystationUrl +
       "/" +
       apiUrl +
       "?account=" +
       encodeURIComponent(account) +
       "&client=" +
       encodeURIComponent(self.client) +
       "&lcd=" +
       encodeURIComponent(self.lcd) +
       "&path=" +
       encodeURIComponent(self.path) +
       "&payload=" +
       encodeURIComponent(JSON.stringify(payload)),
       "",
       "470",
       "760"
   );
}
 
function openWindowV2(type, payload, account = "", self) {
   console.log(`open ${self.client}`);
 
   // The account parameter is required for users having multiple keychain accounts.
   let apiUrl = "";
   switch (type) {
       case "signin":
           apiUrl = "signin";
           break;
       case "transaction":
           apiUrl = "transaction";
           break;
       case "ai-request":
           apiUrl = "ai_request/set";
           break;
       default:
           apiUrl = "auth";
           break;
   }
 
   const network = "Oraichain";
   return PopupCenter(
       self.keystationUrl +
       "/" +
       apiUrl +
       "?lcd=" +
       encodeURIComponent(self.lcd) +
       "&raw_message=" +
       encodeURIComponent(JSON.stringify(payload)) +
       "&signInFromScan=true" +
       "&network=" +
       network,
       "",
       "470",
       "760"
   );
}
 
export default class Keystation {
   constructor(params) {
       if (!params) {
           return;
       }
       const { client, lcd, path, keystationUrl } = params;
       this.client = client;
       this.lcd = lcd;
       this.path = path;
 
       this.keystationUrl = keystationUrl;
   }
 
   openWindow(type, payload, account = "") {
       const self = this;
       return process.env.REACT_APP_WALLET_VERSION == 2 ? openWindowV2(type, payload, account, self) : openWindowV1(type, payload, account, self);
   }
}
 
export const myKeystation = new Keystation({
   client: "https://api.wallet.orai.io",
   lcd: "https://lcd.orai.io",
   path: "44/118/0/0/0",
   keystationUrl: "https://api.wallet.orai.io",
});

```

#### Step 3: **Create a randomness request**

```
const input = Buffer.from(
       JSON.stringify({
           request_random: {
               input: btoa(userInput),
           },
       })
   );
 
const {privateKey, chainCode, network} = await getChildKey();
const childKey = fromPrivateKey(Buffer.from(privateKey), Buffer.from(chainCode), network);
 
const sender = cosmos.getAddress(childKey);
 
 
const txBody = getHandleMessage(contract, input, sender, requestFees);
const response = await cosmos.submit(childKey, txBody, "BROADCAST_MODE_BLOCK", isNaN(fees) ? 0 : parseInt(fees), gas);
```

Function _**getChildKey()**_ is as follow:

```
   // const popup = window.open(`${config.walletapi}/auth?signInFromScan=true`, "", "resizable=1, scrollbars=1, fullscreen=0, width=470, height=760");
   const popup = myKeystation.openWindow("auth", "");
 
   return new Promise((resolve, reject) => {
       const loop = setInterval(function() {
           if (!popup) {
               clearInterval(loop);
               reject("window-blocked");
           } else if (popup.closed) {
               clearInterval(loop);
               reject("window-closed");
           }
       }, 500);
       const handler = e => {
           if (e.data.privateKey && e.data.chainCode && e.data.network) {
               clearInterval(loop);
               window.removeEventListener("message", handler);
               resolve(e.data);
           }
       };
       window.addEventListener("message", handler);
   });
};

```

#### **Step 4: Repeat the previously mentioned step 3 and 4 to get the random value and round information**
