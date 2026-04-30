# Coconut Credentials

## Introduction

- Why are we talking about credential systems in eCash setting? (This is quoted from Wikipedia and it kind of makes sense)

    "The shared characteristic of being tied to an individual (credentials) forms the basis for the numerous similarities between digital cash and digital credentials. This commonality explains why these two concepts often exhibit overlapping features. In fact, it is worth noting that a significant majority of implementations of anonymous digital credentials also incorporate elements of digital cash systems."
- What are selective disclosure credentials?
    The credential scheme allows disclosure of a number of attributes without invalidating the signature.
    A really simple case could be just hiding the attributes of a credential system with hashes ( hashing them when providing it) and later provide the plaintext to the verifier. The can see that the plaintext is valid for the hashed attribute in the credential and they can validate the signature to validate the whole credential.

## Setting and properties?

not sure if the naming represents what I'm going to write tbh, but for now we will continue with this.

- What is the setting in which Coconut is issued?
    1. Distributed issuance of credentials on Blockchain.
    2. Signing keys and attributes should remain secret although being issued publicly.
    3. Everyone can see the issuance results, but we don't want anyone to be able to <u>link</u> the later usage of the credential with its issuance.
- What are the properties we want?
    1. Blindness: Credentials can be issued on blinded values. This makes the issuer itself not to be able to link the credentials.
    2. Unlinkability: Same credential being used multiple times must be distinct-looking for everyone.
    3. Threshold Authority: Multiple issuers instead of trusting a central one.
    4. Non-interactivity
    5. Efficient: The verification must be efficient enough if we want a huge crowd to do it.

## Scheme and Flow

- Coconut is a mix of BLS signatures and PS signatures.
- The size of the credentials is two group elements independent of the number of attributes and authorities. ( when proving knowledge of the credential, depending on the chosen scheme the cost may scale with respect to the number of attributes )

## To Do

- Put the math from the paper

    - Normal version with attribute blinding
    - Threshold version with attribute blinding
    - Multi-attribute threshold version

- Put the details from the code

## Resources

- [Coconut: Threshold Issuance Selective Disclosure Credentials with Applications to Distributed Ledgers](https://medium.com/chainspace/coconut-threshold-issuance-selective-disclosure-credentials-with-applications-to-distributed-f61d121c1903)
- [Author Presentation](https://www.youtube.com/watch?v=OI2iiBREDJ4)
- [White Paper](https://arxiv.org/pdf/1802.07344)
- [Code](https://coconut-lib.readthedocs.io/en/latest/)