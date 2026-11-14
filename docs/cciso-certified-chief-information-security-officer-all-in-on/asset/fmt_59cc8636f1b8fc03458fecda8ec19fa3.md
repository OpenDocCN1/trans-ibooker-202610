
## Symmetric Encryption

Symmetric encryption’s strength lies in its speed. Symmetric key encryption is much faster and less computationally intensive than asymmetric encryption. The downsides to symmetric encryption are that it requires a secure method to transfer the private key out of band and it does not scale well because each pair of users must have a unique key. As the number of users increases, so does the number of keys, which can make key management difficult. The formula to calculate the number of keys needed for symmetric encryption based on users (denoted as `N`) is as follows:

![Images](Images/p219.jpg)

For example, if Alice, Bob, and Carole wanted to communicate with each other separately so as to not have anyone else read their messages, they would need three keys (one for Alice and Bob, one for Alice and Carole, and one for Bob and Carole). However, if the number of users grows, for example, to 100, that would require 4,950 keys as shown:

![Images](Images/p220.jpg)

Symmetric encryption provides confidentiality through encryption but does not provide authenticity or nonrepudiation. Asymmetric cryptography is needed to provide those services.

### Asymmetric Encryption

`Asymmetric encryption`, also known as public key cryptography, is characterized by the use of two mathematically related keys: a public key and a private key. The public key may be provided to anyone that the owner is interested in securely communicating with, but the private key must be known only to the owner. For example, in [Figure 4-11](#ch4.xhtml#fig4_11), Alice and Bob want to exchange confidential information. To send the message to Bob, Alice encrypts the plaintext message with Bob’s public key, which has been shared with her. Bob in turn must decrypt the message from ciphertext to plaintext using his private key known only to him. Likewise, if Bob wants to send a message to Alice, he encrypts the message with Alice’s public key and Alice decrypts the message with her private key. In terms of security services, asymmetric cryptography provides confidentiality through encryption and provides authenticity and nonrepudiation by utilizing digital signatures (discussed later in this section).

![Images](Images/F04-11.jpg)

`Figure 4-11` Asymmetric encryption example

Common asymmetric algorithm implementations include the following:

- `Diffie-Hellman` Key agreement protocol that allows two entities to generate and agree upon a symmetric key (session key) value that is then used by a separate symmetric algorithm to perform encryption and decryption. It provides key agreement functionality but not encryption or digital signature functionality.
- `Rivest-Shamir-Adleman (RSA)` RSA was developed by Ron Rivest, Adi Shamir, and Leonard Adleman. The algorithm provides digital signature, key distribution, and encryption services. The public and private keys are mathematically related and the algorithm is based on the mathematics involved in the difficulty of factoring the product of large prime numbers.
- `Elliptic curve cryptography (ECC)` ECC is another asymmetric implementation that provides digital signature, key distribution, and encryption services when combined with other encryption schemes. The differing factor is its efficiency and how keys are created. ECC is much more efficient than other asymmetric algorithms, allowing for faster encryption/decryption and smaller key sizes while maintaining the same security level. In addition, keys are generated based on properties of elliptic curves instead of factoring the product of large prime numbers.

The main benefits of asymmetric cryptography are scalability and key distribution. Asymmetric cryptography is easier to scale as each user needs only two keys (one public and one private). The formula to calculate the number of keys needed for asymmetric encryption based on users (denoted as `N`) is shown here:

```
2 × N = number of unique keys needed
```

In addition, since the public keys can be freely shared, key distribution is made easier; the public key does not require protection since it is shared with everyone. The downside to asymmetric encryption is that it is slower and more computationally intensive than symmetric encryption. Symmetric key encryption is much faster and less computationally intensive than asymmetric encryption. This is why symmetric encryption is often used for encrypting data (content) in conjunction with asymmetric encryption for exchanging cryptographic keys. This is why symmetric encryption is often used for encrypting data (content) whereas asymmetric encryption is most often used for encrypting keys.

### Hybrid

As discussed, symmetric and asymmetric cryptography have their own strengths and weaknesses. Modern cryptosystems typically utilize a hybrid model that makes use of both. The use of these two techniques is often referred to as `hybrid cryptography` or as a `digital envelope`. [Figure 4-12](#ch4.xhtml#fig4_12) provides an illustration for how common hybrid cryptosystems operate. The steps are as follows:

![Images](Images/F04-12.jpg)

`Figure 4-12` Hybrid cryptosystem example

1. Alice shares her public key with Bob.
2. Bob generates a session key and encrypts it with Alice’s public key to create a digital envelope.
3. Bob sends the digital envelope to Alice.
4. Alice decrypts the digital envelope with her private key to access the session key.
5. Alice and Bob both have access to the session key (symmetric key) and can use this to encrypt and decrypt messages from one another.

The hybrid approach provides the best of both worlds as it allows for the message data to be encrypted quickly with symmetric encryption and for ease of key exchange of the symmetric key using asymmetric encryption. Protocols that utilize a hybrid approach include:

- SSL/TLS
- SSH
- OpenPGP

For simplicity, this example uses two users, Alice and Bob. However, in practice, this is often a user communicating with a web server to share files (for example, SFTP or FTPS) or to access a site securely (for example, HTTPS).

### Cryptosystems Summary

Table 4-3 provides a summary and comparison of the fundamental differences between symmetric and asymmetric cryptographic functionality.

![Images](Images/p222.jpg)

`Table 4-3` Comparison of Symmetric and Asymmetric Cryptography

## Hash Algorithms

Hashing was briefly introduced in the “Cryptographic Definition” section. To recap, a hash algorithm is a one-way function that maps information to a fixed-length string of data, referred to as a hash value, fingerprint, or message digest. It is referred to as a one-way function because the original message cannot be reproduced from the hash value, unlike encryption where the ciphertext can be converted back to plaintext using the decryption operation. In addition, there is no key involved when using hash algorithms. For example, consider the `MD5` hash values for the text strings “my-secret” and “My-secret” illustrated in [Figure 4-13](#ch4.xhtml#fig4_13).

![Images](Images/F04-13.jpg)

`Figure 4-13` MD5 hash example

If one single bit of the message changes (such as a capital M instead of a lowercase m), the hash value dramatically changes, as shown in the output of [Figure 4-13](#ch4.xhtml#fig4_13). The hash value can be used for integrity checking to ensure that a message has not been modified. Common hash algorithms include the following:

- `MD5` Message-digest algorithm version 5, produces a 128-bit hash value
- `SHA-1` Secure Hash Algorithm 1, produces a 160-bit hash value
- `SHA-2` Secure Hash Algorithm 2, includes a family of hash functions that produce 224, 256, 384, or 512-bit hash values: SHA-224, SHA-256, SHA-384, SHA-512, SHA-512/224, SHA-512/256
- `SHA-3` Secure Hash Algorithm 3, includes a family of hash functions: SHA3-224, SHA3-256, SHA3-384, SHA3-512, SHAKE128, and SHAKE256
