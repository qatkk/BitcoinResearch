# Coconut Adjusted for Fedimint
This document basically goes through the implementation of Coconut for Fedimint in this [repo](https://github.com/joschisan/ecash-ng-crypto/tree/master).

In the math throughout the document, we refer to the scalar values as small case letters and to the points on the curve with capital case.

### Setup

- What is the assumed setup in the code?

    I mean how many attributed do we have? What do each one of them correspond to if we were to match it the the current implementation?

#### Isuance Homomorphism

$$
    P_c = m_1 . G_p + r_p . H_p \\
    C_m = m_1 . H_{e_1} + m_2 . H_{e_2} + m_3 . H_{e_3} + r_m . G_{e_1} \\
    H = HashPointToCurve(C_m)\\
    C_1 = m_1 . H + r_1 . G_{e_1} \\
    C_2 = m_2 . H + r_2 . G_{e_2} \\
    C_3 = m_3 . H + r_3 . G_{e_3} \\
$$

#### Prepare Issuance

$$
    P_c = m_1 . G_p + r_p . H_p \\
    C_m = m_1 . H_{e_1} + m_2 . H_{e_2} + m_3 . H_{e_3} + r_m . G_{e_1} \\
    H = HashPointToCurve(C_m)\\
    C_1 = m_1 . H + r_1 . G_{e_1} \\
    C_2 = m_2 . H + r_2 . G_{e_2} \\
    C_3 = m_3 . H + r_3 . G_{e_3} \\
    \mathcal{Y} = \{P_c, C_m, C_1, C_2, C_3\} \\
    
$$

### Questions?
- The signatures are re-randomized at spent time, how are they keeping a list of revoked credentials? 
    I mean, we don't have anything in cleartext to store and be like we are not going to accept this nonce anymore? In comparison to the eCash KVC(not sure about the name) which has the nullifier, what would be the nullifier here?
    
    Note: Coconut itself doesn't have nullifiers, so we have to add it as a layer on top. How are we doing it here?
    Maybe doing it by deterministiclly deriving a value from an attribute in the credential?