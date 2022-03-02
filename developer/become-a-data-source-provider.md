# Become a data source provider

### A brief description of how to develop a data source for an AI service on Oraichain:[#](https://docs.orai.io/docs/Developers/become-a-ds-provider#a-brief-description-of-how-to-develop-a-data-source-for-an-ai-service-on-oraichain)

As Oraichain currently supports only shell script, a provider cannot use popular programming languages such as Python, Java to develop their services.

When developing the data source script, the job is fairly simple with no input arguments, as the providers only need to call their API using a cURL request. However, the output of a data source must be a string, not an object. A simple data source is given as follows:

```
#!/bin/bash
curl -s -X GET "https://min-api.cryptocompare.com/data/price?fsym=ETH&tsyms=USD" -H "accept: application/json" | jq -r ".USD"
```

The "jq" command is used to parse the JSON response of the request to collect only a string.[\
](https://docs.orai.io/docs/Developers/OraichainInstallation)
