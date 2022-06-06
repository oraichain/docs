# AI provider integration

This tutorial allows AI service providers to integrate their AI APIs onto the Oraichain's AI Oracle service. 

## 1. Write a deno script

The deno script will help you process the input & output of your service dynamically when being executed by AI executors. Since all services is different, the executors cannot run a program that contains a particular application's logic.

A simple example of a deno script that takes IPFS hashes as inputs, and call POST requests to a flower classification service:

```js
import _ from "https://deno.land/std@0.120.0/node/module.ts";

const httpPost = async (hash) => {
    const url = "https://100api.orai.dev/cv010";
    // Build formData object.
    let formData = new FormData();
    formData.append('input_source_hash', hash);

    const data = await fetch(url, {
        method: 'POST',
        body: formData
    }).then(data => data.json());
    return data;
}

const main = async (data) => {
    const params = JSON.parse(data);
    const responses = [];
    for (let i = 0; i < params.length; i++) {
        try {
            let result = await httpPost(params[i]);
            if (result.data && result.data.length > 0) {
                result.data = result.data.map(data => ({ ...data, score: Math.floor(data.score) }))
                responses.push(result);
            }
        } catch (error) {
            continue;
        }
    }
    console.log(JSON.stringify(responses))
};

main(...process.argv.slice(2))
```

You will see later that by creating a data source smart contract, you'll be able to pass an array of string parameters into this deno script.

After writing your deno script, please upload it to a public file hosting, which has a public URL that the AI executors can read from. In this case, we have uploaded it to a public github repository called [deno-scripts](https://github.com/oraichain/deno-scripts)

## 2. Deploy a data source contract

The data source contract is the trusted source for the executors to collect AI APIs & execute them. It stores the deno script's programming language, URL (which is the deno script's URL), and input parameters. These input parameters will be fed into your script as mentioned above and you can access them by using JSON parsing.

The simplest way to have a data source contract is to deploy an [empty contract](https://github.com/oraichain/oraiwasm/tree/master/package/aioracle/dsource_empty) (without custom business logic). Click [here](https://docs.orai.io/developers/cosmwasm-ide/tutorial-01) for more information on how to deploy a CosmWasm contract. You can also customize the data source to suit your need, but it must follow the same template as provided by the Oraichain. Otherwise, the executors won't be able to run your service.

## 3. Deploy an oracle script contract

The oracle script is used to aggregate your data source result after it is called multiple times by the AI executors.