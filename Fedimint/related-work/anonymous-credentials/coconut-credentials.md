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

Coconut has consists of a setup and key generation phase, issuance of the signature, and proving of signature possession explained in the following sections.

### Setup

- Fix the number of messages to be signed to $q$
- Fix the group parameters and the binding $e$ for $(p, \tilde{\mathbb{G}}, \mathbb{G}, \mathbb{G}_t, e, g, \tilde{g} )$
- Compute $q$ generators for each of the messages to be signed of $\mathbb{G}$ as $(h_1, h_2, h_3, \cdots, h_q)$

The result of running the setup algorithm are the parameters $params = (p, \tilde{\mathbb{G}}, \mathbb{G}, \mathbb{G}_t, e, g, \tilde{g}, h_1, \cdots, h_q)$.

### Generating the Keys

This phase can either be run by a trusted party or as a distributed key generation protocol. The output for this phase is basically the secret and public keys for all the $n$ authorities signing the messages as $(pk, sk_1, pk_1, sk_2, pk_2, \cdots, sk_n, pk_n)$ but for a threshold signature scheme, requiring at least $t$ out of $n$ signature shares for a valid signature to be verified with the scheme's public key, $pk$.

### Issuing the Signature

The issuance of the signature itself has three sub-phases. The user first needs to prepare the message to be signed by the authorities, which we will call **PrepareBlindSign()**. It will then send the blinded messages to the authorities to sign, which we will refer to as **BlindSign()**. The user will then gather $t$ shares signed by $t$ different authorities which needs to first unblind and then aggregate to create the signature, which we will refer to these phase as **Unblind()** and **AggSignature()** correspondingly.

#### PrepareBlindSign()

- Prepare ElGamal key pair $(d, \gamma)$ s.t $\gamma = g^d$.
- Users are creating commitments to the messages they are about to send to be signed. This is done as below:

    $$0 \gets ^r \mathbb{Z} \; \; c_m = g^o \Pi_{j=1} ^ q h_j ^ {m_i}$$

    The additive notation of it would be:

    $$ c_m = \Sigma_{i=1} ^ q o \cdot G + m_i \cdot H_i$$
- The user creates the generator $h$ used for the signature from the commitment $c_m$ in a deterministic manner (the authorities need to verify the correctness of the computation of the generator using $c_m$ which requires this to be deterministic).
- Other than committing to the messages, the users are also encrypting each of their messages using  ElGamal's encryption:

    $$c_j = (a_j, b_j) = (g^{k_j}, \gamma ^ {k_j} h ^ {m_j})$$
    which in additive notation would be:
    $$c_j = (a_j, b_j) = (k_j \cdot G , k_j \cdot \Gamma + m_j \cdot H)$$

- The user must as well verify that all these computations have been carried out correctly and that the same parameters have been used throughout the process (e.g. the same messages having been committed to are then later encrypted).

    $$
        \pi_s = \mathrm{NIZK}\left\{
        (d,m_1,\ldots,m_q,o,k_1,\ldots,k_q) :
        \begin{aligned}
        \gamma &= g^d \land \\[4pt]
        c_m &= g^o \prod_{j=1}^{q} h_j^{m_j} \land \\[4pt]
        a_j &= g^{k_j} \land b_j = \gamma^{k_j} h^{m_j},
        \quad \forall j \in [1,\ldots,q] \land \\[4pt]
        \phi(m_1,\ldots,m_q) &= 1
        \end{aligned}
        \right\}
    $$

The user sends the public parameter $(\gamma, c_m, c_1, \cdots, c_q, h)$ to the proof as well as the proof $\pi_s$ and the claim regarding the messages $\phi$ to the authorities to sign.

#### BlindSign()

- Check if the generator $h$ matches the commitment $c_m$.
- Verify the proof $\pi_s$ using the public parameters $(\gamma, c_m, c_1, \cdots, c_q, h)$.
- Each authority uses its shares of the secret key, $(x, y_i)$, to sign the encrypted messages.

    $$
        \tilde{c} = (\Pi_{j=1}^q a_j ^ {y_i} ,
                    h ^ x \Pi_{j=1} ^q b_j ^ {y_i})
    $$

The authority will then send its signed share as $(h, \tilde{c})$ to the user.

#### Unblind()

- The user will first check if the generator $h$ in the signed share is the same as its generator and will only continue if it is.
- It will then unblind the signature as bellow

    - Recall that $(a, b) = (\Pi_{j=1}^q g^{k_j}, \Pi_{j=1} ^q \gamma ^ {k_j} h ^ {m_j})$
    - Recall that $(\tilde{a}, \tilde{b}) = (\Pi_{j=1}^q a_j ^ {y_i} , h ^ x \Pi_{j=1} ^q b_j ^ {y_i})$
$$ \sigma_i = (h, s_i) = (h, \tilde{b} \; (\tilde{a})^ {-d}) $$
$$ s_i = \tilde{b} \; (\tilde{a})^ {-d} = h ^ x \Pi_{j=1} ^q b_j ^ {y_i} (\Pi_{j=1}^q a_j ^ {y_i})^{-d} =$$
$$ h ^ x \Pi_{j=1} ^q \gamma ^ {k_j y_i} h ^ {m_j y_i}(\Pi_{j=1}^q g ^ {k_j y_i})^{-d}  = $$
$$ h ^ x \Pi_{j=1} ^q g ^ {d k_j y_i} h ^ {m_j y_i}(\Pi_{j=1}^q g ^ {k_j y_i})^{-d} = $$
$$ h ^ x \Pi_{j=1} ^q g ^ {d k_j y_i} h ^ {m_j y_i} g ^ {- k_j y_i d} = $$
$$h ^ x \Pi_{j=1} ^q h ^ {m_j y_i} $$
$$\sigma_i = (h, s_i) = (h, h ^ x \Pi_{j=1} ^q h ^ {m_j y_i})$$

It's worth to mention tha the user only accepts the signature if it's verified against the authority's public key.

### Proving the Possession of the Signature

For the signatures to be unlinkable to the issuance, the user must reblind them before providing them for verification. Here we will see the **ReblindingProcess()** as well as the **Verification()** of the blinded signature.

#### ReblindingProcess()

- We have the aggregated signature as $\sigma = (h, s)$.
- We have the protocol's $pk$ as $(\alpha, \beta_1, \beta_2, \cdots, \beta_q)$.
- The user chooses two random values $r$ and $r'$ from the group's field.
- Computes $h'$ as $h^{r'}$ and $s'$ as $s^{r'}$.
- Computes $\kappa$ as $\alpha \Pi_{j=1} ^ q \beta_j ^ {m_j} \tilde{g} ^ r$.
- Computes $\nu$ as $h'^r$.
- Computes a NIZK proof $\pi_v$ proving the claim on the messafes, the correct computation of $\kappa$, and the correct computation of $\nu$.
- Sends out $(\nu, \kappa, h', s', \pi_v)$.

#### Verification()

- Verifies the proof with the public parameters.
- Checks if $h'$ is one and if so it rejects the signature.
- Checks the proof using the bilinear mapping $e$.

$$ e(h', \kappa) = e(s'\nu, \tilde{g})$$

## Resources

- [Coconut: Threshold Issuance Selective Disclosure Credentials with Applications to Distributed Ledgers](https://medium.com/chainspace/coconut-threshold-issuance-selective-disclosure-credentials-with-applications-to-distributed-f61d121c1903)
- [Author Presentation](https://www.youtube.com/watch?v=OI2iiBREDJ4)
- [White Paper](https://arxiv.org/pdf/1802.07344)
- [Code](https://coconut-lib.readthedocs.io/en/latest/)