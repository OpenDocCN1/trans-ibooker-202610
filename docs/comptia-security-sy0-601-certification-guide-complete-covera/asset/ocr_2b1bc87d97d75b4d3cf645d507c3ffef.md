# Asymmetric Algorithms

Asymmetric algorithms use a PKI environment as they use two keys: a private key and a public key. Let's now look at different asymmetric techniques.

**Diffie Hellman (DH)** does not encrypt data. Its main purpose is to create a secure session so that symmetric data can travel down it. The DH handshake is shown in the following diagram:

![](bbox=[0.175, 0.26, 0.78, 0.495])

Figure 2.11 – DH handshake

DH creates the keys used in the **Internet Key Exchange (IKE)**; it uses UDP port 500 to set up the secure session for the L2TP/IPSec VPN. Once the secure tunnel has been created, then the symmetrically encrypted data flows down the tunnel.

+   - **Rivest, Shamir, and Adelman (RSA)**: RSA is named after the three people who invented the algorithm. The keys were the first private and public key pairs, and they start at 1,024, 2,048, 3,072, and 4,096 bits. They are used for encryption and digital signatures.

+   - **Digital Signature Algorithm (DSA)**: DSA keys are used for digital signatures; they start at 512 bits, but their 1024-bit and 2048-bit keys are faster than RSA for digital signatures.

+   - **Elliptic Curve Cryptography (ECC)**: ECC is a small, fast key that is used for encryption in small mobile devices. However, AES-256 is used in military mobile cell phones. It uses less processing than other encryptions.