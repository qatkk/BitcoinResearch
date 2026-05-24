# Nym Technology's ZK Credentials

Nym technology uses a modified version of the coconut credentials which they claim to be more efficient and secure. In this file I'll explain the following:

- ZK credential's construction and math.
- The difference in the construction of the credential system in Nym's protocol and Coconut's.
- Some benchmarks regarding the size of the credentials usd in zk credentials.
- Issues that Nym technology addresses regarding using this protocol for ecash.

    - Offline payments
    - Nullifiers

## ZK Credential's Construction

### Entities and Architecture

We have $n$ **authorities**, any number of **users** and some **providers** that verify the credentials of the users and provide them the service upon verification.

### Construction

The construction of the credentials in zk credentials is pretty similar to Coconut's with a few changes addressing security and performance issues. The following steps show the construction of zk credentials and point out the differences it has with Coconut.

### Setup

- Fix the number of messages to be signed to $q$
- Fix the group parameters and the binding $e$ for $(p, \tilde{\mathbb{G}}, \mathbb{G}, \mathbb{G}_t, e, g, \tilde{g} )$
- Compute $q$ generators for each of the messages to be signed of $\mathbb{G}$ as $(h_1, h_2, h_3, \cdots, h_q)$

The result of running the setup algorithm are the parameters $params = (p, \tilde{\mathbb{G}}, \mathbb{G}, \mathbb{G}_t, e, g, \tilde{g}, h_1, \cdots, h_q)$.

### Generating the Keys

This phase can either be run by a trusted party or as a distributed key generation protocol. The output for this phase is basically the secret and public keys for all the $n$ authorities signing the messages as $(pk, sk_1, pk_1, sk_2, pk_2, \cdots, sk_n, pk_n)$ but for a threshold signature scheme, requiring at least $t$ out of $n$ signature shares for a valid signature to be verified with the scheme's public key, $pk$.

**Note**: More specifically speaking the public key for each authority $i$ in zk credentials is extended for security reasons as below:

$$pkᵢ = (\tilde{\alpha}_i, \tilde{β}_{i,1}, β_{i,1}, ..., \tilde{β}_{i,q}, β_{i,q}) =  (g̃^{x_i}, g̃^{y_{i, 1}}, g^{y_{i, 1}}, ..., g̃^{y_{i, q}}, g^{y_{i, q}})$$

The public key in Coconut credentials only contains the elements in group $\mathbb{G}$ as below:

$$pkᵢ = (αᵢ, β_{i,1}, β_{i,2}, ..., β_{i,q}) = (g̃^{x_i}, g̃^{y_{i,1}}, g̃^{y_{i,2}}, ..., g̃^{y_{i,q}}) $$

## Resources

- [Security Analysis of Coconut, an
Attribute-Based Credential Scheme with
Threshold Issuance](https://eprint.iacr.org/2022/011.pdf)
- [Implementation and Source Code](https://github.com/nymtech/nym/tree/develop)
- [Nym Technology's Documents on ZK Credentials](https://nym.com/docs/network/cryptography/zk-nym)
- [The Nym Network](https://nym.com/nym-whitepaper.pdf?_rsc=tfnoh&utm_source=chatgpt.com)