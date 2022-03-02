# Become an oracle script developer

### A brief description of how to develop an oracle script on Oraichain:[#](https://docs.orai.io/docs/Developers/become-an-oracle-script-dev#a-brief-description-of-how-to-develop-an-oracle-script-on-oraichain)

Oraichain currently only supports oracle scripts written in shell script

For an oracle script, the number of input arguments must be at least two. The first one: _**$1**_ is reserved for execution type of the script. For now, the validators only accept three execution types: _**aiDataSource**_, _**testcase**_, and _**aggregation**_. Other types will not be read, and unexpected behaviours may happen. The first type collects all the AI data source names that the oracle wants to execute, while the second type gets all the test cases it prefers. The last type, on the other hand, is the business logic of the oracle script, which is responsible for processing all the results retrieved from the data sources. Indeed, all the results are put in a string passed to the second argument _**$2**_ with a delimiter, which is _**-**_, and it is the oracle script's job to process these values to come up with a final result to be returned back to Oraichain. An example of an oracle script that is syntaxtically correct is below:

```
#!/bin/bash

# the number of parameters are fixed depending on the total data sources run
route() {
  if [[ $1 = "aiDataSource" ]]
  then 
    echo "crypto_compare_btc coindesk_btc coingecko_btc" # return names of the data sources
  elif [[ $1 = "testcase" ]] # return names of the test cases
  then
    echo "testcase_price"
  elif [[ $1 = "aggregation" ]] # $2 is true output, $3 is expected output
  then
    # collect input string with a delimiter of each data source value separated with a '-' delimiter
    IFS='-' read -ra array <<< "$2"
    aggregation_result=0
    size=0
    # here is the algorithm for each oracle script. This should be different based on the oscript
    for i in "${array[@]}"; do
      let "size+=1"
      # process "$i"
      aggregation_result=`echo $aggregation_result + $i | bc`
      #$aggregation_result = $aggregation_result + $i
    done
    temp=$aggregation_result
    # scale=2 allows division with floating points (two decimals)
    aggregation_result=$(echo "scale=2; ($temp) / $size" |bc) # |bc allows adding two floating point numbers
    echo "$aggregation_result"
  else
    echo 0
  fi
}

route_name="$(route $1 $2 $3 $4)"
echo $route_name
```

#### Note:[#](https://docs.orai.io/docs/Developers/become-an-oracle-script-dev#note)

**+)**: In order to deploy an oracle script on Oraichain, all sufficient data sources and test cases provided in the script need to be on the network already.

**+)**: If a data source or a test case change its name, the oracle script deployer must manually edit his script. Otherwise, the script will not run properly because it cannot find the correct data source or test case.
