# Become a test case provider

### A brief description of how to develop a test case for an AI service on Oraichain:[#](https://docs.orai.io/docs/Developers/become-a-tc-provider#a-brief-description-of-how-to-develop-a-test-case-for-an-ai-service-on-oraichain)

As Oraichain currently supports only shell script, a provider cannot use popular programming languages such as Python, hello world to develop their services.

When developing a test case, it must have at least three input arguments. The first one is used for the name of the data source that it needs to run, while the second and the third are the input and expected output respectively provided by the user. In some cases, the user's input may not be necessary, but we still need to reserve it for other cases. The expected output should be compared to the output of the data source, and if the output does not meet the requirement (of the test case) then we return _**null**_. This is very important, as currently, Oraichain only checks if the output of a test case is empty or equal to null. If it passes these conditions, the result from data source will be collected and considered valid. An example of a test case is:

```
#!/bin/bash

# $2 should be reserved for test case input if necessary

func_result="`/bin/bash .oraifiles/"$1"`"
# $3 is the expected output
expected_output=$(echo "$3" |base64 --decode)
diff=$(echo "scale=2; $func_result - $expected_output" |bc) # |bc allows adding two 
# allow compare integer with float
if awk 'BEGIN {exit !('$diff' < 10000)}';
then 
  echo $func_result
else
  echo null
fi
```
