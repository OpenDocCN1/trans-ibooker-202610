
## IDS/IPS Tuning and Risk Management

The key to using `IDS/IPS` solutions wisely comes down to `tuning`. Tuning involves choosing the sensitivity level of the detection, reporting, alerting, and prevention functions. If security operations staff set the threshold level very low, they can detect all potentially malicious events; if they set it very high, they may miss some events but will be sure to catch the most important ones. However, setting the level too low may result in the organization and the security operations staff being overwhelmed by too many messages and alerts that do not indicate intrusive activity (false positives). Likewise, setting the threshold too high could allow serious security breaches to go undetected (false negatives). The challenge for every security organization and the `CISO` is settling on the right level of `IDS/IPS` operation.

So, how should the `CISO` decide what the right level is? Like all security decisions, it comes down to risk management. How valuable are the assets being monitored? And what is the cost of detection? Setting the levels low and capturing more events than are necessary increases operation costs. It may be unwise to spend $2M per year to operate an `IDS` capability to monitor a $1M network. The `CISO` and the security organization should create models to predict the equipment and operational level of effort required to deploy and maintain an `IDS/IPS` at various detection thresholds as compared with the organization’s appetite for risk.

An `IDS/IPS` is essentially just another security control and its implementation should be considered alongside the other candidate controls based on risk.

## Cryptography

`Cryptography` is one of the most fundamental information security practices, and possibly the oldest. Modern `cryptography` is the practice of using mathematics to secure information by rendering it unintelligible to unauthorized parties. This section presents the following cryptographic topics and concepts:

- `Cryptographic definitions`
- `Cryptographic services`
- `Symmetric, asymmetric, and hybrid cryptosystems`
- `Hash algorithms`
- `Method authentication codes`
- `Digital signatures`
- `Public key infrastructure`

### Steganography

`Steganography` is a technique used to hide secret information in plain sight, typically in innocuous looking digital files. This is accomplished through the use of special software that embeds messages inside images, videos, audio, text, or other files.

`Steganography` and `cryptography` are two sides of the same coin. Although both are used to communicate secret information, the method for doing so varies. `Steganography` hides the fact that the communication has even taken place, as an unintended recipient of a file is unaware that the file contains a hidden message. `Cryptography` does not hide the communication but hides the data itself through encryption. If an unintended recipient receives an encrypted message, they know the message is there but they cannot read it. `Steganography` does not encrypt the message; it simply hides the message in another file.

### Cryptographic Definitions

Some fundamental definitions must be established to facilitate the discussion of cryptography. The foundational components of cryptography include the following:

- `Cryptographic key` A string of values used in conjunction with cryptographic algorithms for operations such as encryption and decryption. In general, the longer the key size the more security that is provided; however, this also depends on the algorithm and the implementation used.
- `Cryptographic algorithm` A mathematical equation that can be used for encryption, decryption, or hashing.
- `Plaintext` Information in a readable format that has not been encrypted or has been decrypted.
- `Ciphertext` Information in an unreadable format that has been encrypted.
- `Encryption` The process of transforming plaintext to ciphertext using cryptographic keys and algorithms.
- `Decryption` The process of transforming ciphertext to plaintext using cryptographic keys and algorithms.
- `Hashing` A one-way function that uses algorithms to transform information to a string of data, often used for integrity checking.
- `Cryptosystem` Includes all the necessary system components for encryption and decryption such as software, protocols, algorithms, keys, key management, and so on.

The cryptographic key values are used by the cryptographic algorithms to indicate which equations to use, in what order, and with what values. Together, keys and algorithms allow for encryption and decryption operations to take place to transfer plaintext to ciphertext and vice versa. The encryption and decryption operations are illustrated in [Figure 4-9](#ch4.xhtml#fig4_9).

![Images](Images/F04-09.jpg)

**Figure 4-9** Cryptographic operations

### Cryptographic Services

One of the beautiful aspects of cryptography is the range of security services that can be provided, including

- `Confidentiality` Ensures data is only accessible to authorized parties and remains unintelligible to unauthorized entities (for example, encryption)
- `Integrity` Ensures data has not been altered by unauthorized parties (for example, digital signatures or message authentication codes)
- `Authentication` Verifies the identity of a system or user that created a resource
- `Nonrepudiation` Prevents a sender from legitimately denying that they sent a message (for example, digital signatures)

These services are discussed throughout this section as we explore various algorithms and cryptographic implementations.

### Symmetric, Asymmetric, and Hybrid Cryptosystems

The two types of cryptographic algorithms in use today for encryption and decryption are symmetric and asymmetric. In addition, there are hybrid approaches that utilize the strengths of both.

![Images](Images/exam.jpg)

**EXAM TIP** `CCISO` candidates should be aware of the differences between symmetric and asymmetric cryptography.

#### Symmetric Encryption

`Symmetric encryption`, also known as symmetric key cryptography, is characterized by the use of a single key for encryption and decryption. This means that the sender and receiver of an encrypted message must both have a copy of the same key. Because anyone with access to the key will be able to decrypt the message, the key must be kept private. This is why it is referred to as a `private key`. For example, in [Figure 4-10](#ch4.xhtml#fig4_10), Alice and Bob want to exchange confidential information. To send the message to Bob, Alice encrypts the message with the shared private key to convert the message from plaintext to ciphertext. Bob in turn must have a copy of the same private key to decrypt the message from ciphertext to plaintext. Likewise, if Bob wants to send a message to Alice, he encrypts the message with the shared private key and Alice decrypts the message with the same private key.

![Images](Images/F04-10.jpg)

**Figure 4-10** Symmetric encryption example

Examples of symmetric algorithms include the following:

- `Data Encryption Standard (DES)` The algorithm uses a 56-bit key and is considered insecure due to the short key length, which is susceptible to brute-force attacks. Consequently, `DES` was replaced with `3DES` and `AES`.
- `Triple-DES (3DES)` `3DES` was released as an interim replacement for `DES` while `AES` was still being created. `3DES` applies `DES` three times to each block of data during the encryption process. Modern implementations of `3DES` provide 168 bits of security.
- `Advanced Encryption Standard (AES)` `AES` was built as a replacement for `DES`. `AES` comes in key sizes of 128, 192, and 256 bits. `AES` is a mature, internationally used standard for symmetric encryption.
