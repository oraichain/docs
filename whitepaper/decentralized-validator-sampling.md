# Decentralized Validator Sampling

### Requirements of the algorithm

The validator selection algorithm should have three main characteristics stated below in order to provide a fair and fast system for all validators participating in the network:

1. All validators should have a chance to execute an AI request regardless of their ranks. This is the most important requirement, as we do not want big validator companies with high stakes to entirely dominate the network.
2. The percentage of being picked for executing an AI request should be based on the amount of ORAI tokens staked (voting power). This allows validators to dedicate themselves to execute the request correctly to earn more stakes and trust, which help them climb the ranks. The delegators also know which validator has more potentials to deliver rewards through the voting powers.
3. The algorithm should not be too complicated. This is also quite crucial because it is run everytime there is a request from the users. If it takes too many resources or time to sample a list of validators, the traffic may increase, which reduces the number of requests per second as well as the QoS.

### Validator duplication

The validators are chosen randomly based on their voting powers. Indeed, all the bonded validators are sorted and put in a `list`, where the first element is the highest-staked validator, while the last element contains the lowest-staked one. In each AI request, the system updates this `list` since there may be an update in the validator set, or the voting powers have been changed. Within the `list`, each validator is duplicated n - i - 1 times, with i is the index of the `list`'s element, and n is the length of the list. This helps increase the chances of getting chosen for those validators with high voting powers, while validators in the lower ranks also can participate in the AI request flow. Note that if the voting powers of two validators are equal, both will be duplicated with the same number.

#### **Initial bonded validator list diagram**

![Initial bonded validator list diagram](https://docs.orai.io/assets/images/sorted\_val\_list-fe39cf49cde2b9675302445c7a6d8d0f.png)

The diagram shows an example of a bonded validator `list` with each index value is the validator's voting power. In the system, the value is actually a Validator struct to hold more information needed.

For the first index, the validator is duplicated 10 - 0 - 1 = 9 times since it is in the top rank. It is similar for the 2nd and 3rd validator. However, there are three validators having the same voting power value, which makes them equal in ranks despite having different indexes. Consequently, these validators will be duplicated based on the first encounter of this voting power. In this example, the number of duplication is 10 - 2 - 1 = 7 times which applies to three validators in the index 2, 3, 4 respectively.

The algorithm continues with the 6th validator being copied 10 - 6 - 1 = 3 times and so on until we reach the final index of the `list`.

#### **Validator duplication diagram**

![Validator duplication diagram](https://docs.orai.io/assets/images/validator\_duplication-577740af94ac2cebc8ebc346c8d59124.png)

The diagram demonstrates the list after the duplication step, where three dots means that the list can be longer than that.

### Validator sampling

#### **1) Random generator tool:**

Oraichain makes uses of an existing tool from [Oasis Protocol Foundation](https://oasisprotocol.org). It has implemented a [HMAC-DRBG algorithm](https://github.com/oasisprotocol/oasis-core/blob/master/go/common/crypto/drbg/hmac\_drbg.go) published in the [NIST SP 800-90A](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90Ar1.pdf) to generate a random number given a seed, a nonce, and a personalizationString. This helps generate a random figure that is consistent for every node based on a set of number parameters above, whereas the random function of Golang cannot provide the similar behaviour. There are a number of HMAC-DRBG implementations for Go, but Oasis is growing fast, and the foundation focuses on security and privacy the most. As a result, we choose it as a package to generate a random number.

#### **2) HMAC-DRBG parameters:**

The seeding parameter is generated every new block, while the nonce is taken from the request ID. The personalizationString, on the other hand, is the chain ID of the system, which is Oraichain, to personalize the random generator.

#### **3) Process the random value:**

After receiving the random number. We make many modulo calculations, with first the dividend being the random number, while the divisor is the total voting power of all validators at the calculation time. The quotient then becomes the next divisor, while the divisor becomes to next dividend. This loop continues until we collect a value between the range of the `list` before using it as an index to choose the next validator. Finally, the chosen validator is removed from the chosen list, and the process starts again with a new random value. It stops when we have selected enough validators to execute the oracle scripts. As a result, we can collect a random validator that is based on its voting power. Oraichain does not store this `list`, so it does not consume any additional gas.

For example, we need two validators to execute the oracle scripts. Firstly, we get a random number which is 18090388581894877127 from the random generator. Assume that the total voting power is 300, and the list size is 12 like in the diagram above, we then get 227 as the quotient. Next, we calculate 300 % 227, which is equal to 73. It goes on until we collect a number within the range of \[0, 11], which is 8 in this case. So the validator at the 8th index is chosen. The process repeats again with the new random value.

This allows the validator sampling process be random at each step, which makes sure that potentially malicious validators cannot fully control the seed nor the chosen indexes.
