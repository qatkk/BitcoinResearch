# Fedimint Protocol and Specifics
Note that these notes will be focused more on the cryptographic aspects of the protocol(due to my personal interest).
#### BFT 
- What is it?
- Why is this needed? 
- Which one do they use? 

#### Minting Side (Mint Module)
- What does each member do as a mint? 

#### Bitcoin Side (Wallet Module)
- What does each memeber's wallet do? 

#### How are the BFT, the Minting, and the Wallet Side Connected?


#### What extra cases must be considered in the federated case in comparison to the centralized eCash scenario? 
- (To be explained!) The whole transaction (containing the inputs and outputs)updating the state of the coins must be signed using the public keys for the inputs to bind the information to the transaction. This only means that the finalized transaction is verified (or agreed on) by at least `t` out of `n` entities in the federation (aka signers?).[^1]

#### How does the protocol transfer eCash from one user to another using fix denominations(how does it handle the case in which the sender doesn't have tokens that add up to the specific payment amount)?
<!-- To do
page 40 of the paper  -->

### Cryptography 
#### Signature Schemes 
According to the whitepaper, the protocol uses three different signature schemes; Multisig, BLS, and Musig. 
- Where eache of these schemes are being used? 

### Questions 
- Does the current version of Fediming achieve offline transferability?



[^1]: The description for this is provided in the whitepaper page 29.
## Resources 
[Fedimint White Paper by Eric Sirion](https://sirion.io/p/federated_e-cash_nopub.pdf)