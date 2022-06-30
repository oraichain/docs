# Introduction to Oraichain VRF 2.0

#### Currently available on Oraichain Mainnet 2.0, supporting most prominent blockchains: BNB Chain, Ethereum, Polygon, Solana, Cosmos, Fantom, Polkadot and Avalanche.

**Please** [**contact us** ](https://landing.mailerlite.com/webforms/landing/t0c1f1)**for more information.**

## **The need for provably-fair randomness in the crypto world**

Random number generators (RNG) are utilized across a multitude of fields including academia, investment back-testing, and gaming. In the crypto space, the need for randomness is also apparent, as astute users and investors demand a bias-free distribution of opportunity. This is where **VRF** **(Verifiable Random Function)** comes into play. It serves as an oracle for smart contracts by which applications can generate unpredictable values with on-chain proof ensuring none of the involved parties tampered with the outcomes.

As a result, **VRF** plays an important role in the transparency and fairness of decentralized applications as well as normal applications. Utilizations include such as:

* NFT/ game item creation with varied rarities
* Winning odds for gambling/ casino-like projects
* Selecting the winner(s) for airdrop or whitelisting
* Selecting a representative sample concerning the mechanism of consensus
* Randomly assigning duties and resources
* And many more!

## **Current shortcomings of VRF**

VRF is widely trusted because, at its core, this solution solves the transparency problem by decentralizing the validation process of randomness generation.

![How current VRF works](<../.gitbook/assets/Screen Shot 2 (1).png>)

Each validator holds his or her own public key with which the collective group signature is generated. Similarly, a group signature is formed to validate the creation of a random value, or a set of it. It would appear that this model ensures tamper-proof unpredictability by delegating the verification process to a neutral party i.e. the validators. However, it is certain that the creation of the random value(s) and the verification of group signature still happens off-chain, which may hinder true decentralization and transparency.

Additionally, there is also a possibility that malicious validators can purposely send invalid data to undermine the randomness generation process, thereby delaying it.

![How a VRF can be abused](<../.gitbook/assets/Screen Shot 20.png>)

This may not seem problematic at first glance, but considering the characteristics of a project’s underlying blockchain consensus mechanism, validators (or miners) can leverage the delay to perform rewrite attacks, making a new VRF output that may work in their favor. By the same token, new random outputs can also be forcefully rerolled or manipulated via chain reorganization attacks, which can take full advantage of the said delay and pose a threat to VRF users.

In the worst-case scenario, a scheming VRF provider can also connive with its validators to agree upon a predetermined value under the facade of transparency and decentralization.

![Current VRF has the ability to skew random results in its own favor](<../.gitbook/assets/Screen Shot 202 (1).png>)

Although there have not been any formal records of a VRF provider and its validators conspiring against end-users leading to substantial financial loss and volatility, this risk is indeed palpable considering the current randomness generation process and group signature verification still function off-chain.

## Oraichain VRF 2.0

As a devoted supporter of decentralization, Oraichain has undergone extensive research and development to overcome the previously mentioned shortcomings of VRF with Oraichain VRF 2.0.

First, to verify validators’ signatures and create a group signature on-chain, we bind a pairing-based threshold cryptosystem library to our VRF smart contracts. When a request for randomness comes, each validator of the Oraichain VRF subnetwork will create a signature locally with the given seeding input number and the current round number. Next, the validators submit the signatures to the smart contract, at which the signatures will be validated against the input and the public keys of these validators.

After collecting enough signatures (based on a predefined threshold), the smart contract will automatically recover the final group signature, which will also be verified against the group public key. If it is valid, then the contract will apply a simple SHA256 hash function to the signature to produce the randomness value.

![How all shared parts of Oraichain VRF 2.0 function on-chain](../.gitbook/assets/pagezzz.png)

Each signature contribution must be verified on-chain before storing. Anyone can also validate the group signature by making a simple query to the Oraichain’s smart contract, and it will return either true or false for the requested round.

**With this framework, Oraichain VRF 2.0 is the first-ever subnetwork that provides unpredictable, publicly verifiable, and bias-resistant random values in a fully decentralized manner.**
