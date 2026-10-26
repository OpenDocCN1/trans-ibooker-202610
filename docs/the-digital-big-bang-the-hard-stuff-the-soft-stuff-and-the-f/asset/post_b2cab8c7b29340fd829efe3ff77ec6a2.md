
## THE DIGITAL BIG BANG

In 1998, Daniel Bleichenbacher showed that this widely used standard is not chosen ciphertext secure and presented a practical attack. To see how this attack works, suppose that the attacker has intercepted a ciphertext ct encrypted under Alice's public key. The attacker wants to decrypt this ciphertext, but he does not have Alice's secret key. As it turns out, it is quite easy for the attacker to create a new ciphertext ct' where the RSA inversion of ct' is twice the RSA inversion of ct. The attacker can now send this new ciphertext ct' to Alice and observe her response. If Alice sends back a message saying that the ciphertext was malformed, the attacker learns that the RSA inversion of ct' does not begin with the characters "02". If she sends back any other error message, then the attacker learns that the RSA inversion of ct' does indeed begin with "02".

How is this useful? This tiny information leak seems insignificant, and yet it can be used to completely decrypt the ciphertext ct. To do so, the attacker creates a sequence of ciphertexts, each one related to ct, but where the RSA inversion is some known multiple of the RSA inversion of ct. It sends each of these ciphertexts to Alice to decrypt and observes her response. After about 4 million queries, the attacker learns the entire decryption of ct. Four million queries may seem like a large number, but if Alice is a web server then 4 million queries can be processed in just a few minutes, as was demonstrated in practice. This is a wonderful example of how a seemingly insignificant information leak can completely compromise a system.

There are a number of ways to address this vulnerability. The recommended approach is to move to an encryption scheme that has been shown to be chosen ciphertext secure. These days systems that need public key encryption use a variant of the ElGamal public key encryption scheme that is known to be chosen ciphertext secure under a standard computational assumption.

## IMPLEMENTING CRYPTOGRAPHY

Modeling and proving cryptographic security as discussed in the previous section is a necessary first step, but it is far from sufficient for ensuring security. Even when the cryptosystem or protocol is well designed, it is still possible that implementation bugs will cause the system to be insecure. The following are a few examples that capture common bugs that occur in practice:

**Weak Sources of Randomness.** Much of cryptography depends on locally generated randomness that is unknown to an attacker. For example, when parties need to generate keys, it is important that those keys are chosen at random from a strong entropy source that the adversary cannot predict. This is much harder than it looks and is often the source of vulnerabilities. Consider a cheap IoT device that needs to communicate securely with a cloud server. When the device is first turned on, it needs to generate a key pair for use in a public key encryption scheme. The trouble is that at that point in time the device does not have a strong pool of entropy. Recent works beautifully show that such devices often end up generating the same key or closely related keys. Because the entropy pool is so small, anyone can recover the private keys of many devices from their public keys.

The only way to defend against this problem is to ensure that devices have sufficient entropy when they need it for cryptographic applications. Fortunately, several modern processors include a hardware random generator that can provide entropy on demand, and this entropy can be used as one of several sources of entropy for cryptographic operations. This is still not available on all low-end processors, but we can hope that in time it will be.

**Missed Checks.** To defend against data coming from a potentially malicious source, cryptographic systems often include a number of checks that must be applied in order to reject malicious inputs. For example, when decrypting a given ciphertext or verifying a digital signature, one rejects the data if it fails to pass a number of checks (such as checking for “02”, as discussed in the previous section). It is a fairly common mistake that some of these checks are dropped or implemented incorrectly, thereby opening up the system to attack. Some examples include the aforementioned “goto error” bug where a number of certificate checks were accidentally skipped, an attack on the RSA-PKCS1 signature scheme where a critical step in signature validation was skipped, and an attack on validating certificate chains where an important step was not implemented. In all these cases, the system operates fine but is completely vulnerable to attack.

**Fault Attacks.** Suppose a software library implementing integer multiplication has a hidden bug: The library computes the correct product for all inputs, except for a single pair of numbers where it obtains the wrong product. Bugs like this are difficult to detect by testing because the offending input will likely never be encountered during testing. Nevertheless, such a bug can result in a complete exposure of a secret key. Consider a device holding a secret key. Several works show that an attacker can send malicious input to the device to trigger the bug. As a result, the device computes the wrong result, and this wrong result can make it possible for the attacker to extract the secret key from the device.

## A Bad Optimization.

Developers love to write optimized code, and rightfully so. Optimizing cryptography code, however, can be quite dangerous because the optimizations can lead to vulnerabilities. A well-known example, called the ROCA attack, is a devastating vulnerability in RSA-PKCS1 key generation code that was a direct result of an optimization to the key generation algorithm. It made key generation faster, but the resulting keys were insecure: The private key could be computed given only the public key.

While these bugs make it seem as if the situation is bleak, that is not quite the case. Impressive advances in software verification make it possible to prove that a software implementation matches the cryptographic specification. These verification tools hold the promise of eliminating some of the bugs discussed here, except for the question of weak randomness. The weak randomness problem is much harder to address, although there is progress there, too, thanks to the introduction of new sources of entropy from the embedded hardware.

## SIDE-CHANNEL ATTACKS

Even when the cryptography specification is sound and the implementation is bug-free, the system can still be insecure due to a clever type of attack called a side-channel attack. Side channels refer to information leaked by a working system as it executes a cryptographic algorithm. Some example side-channels attacks include, among others:

- Timing Attacks. Measure the time that the system takes to run the cryptographic algorithm
- Power Attacks. Measure the amount of power used by the system as it executes a cryptographic algorithm
- E/M Attacks. Measure the electromagnetic radiation generated by the processor as it executes a crypto algorithm
- Log Analysis. Examine the error messages written to a log file

These attacks are extremely effective. For example, in some cases, a timing attack can extract a server’s secret key, even if the measurement is not very precise. Similarly, by measuring the system’s power consumption, an attacker can sometimes read the bits of the secret key directly from the power chart. A more subtle use of the measured information, called differential power analysis, can be used to extract the key bits even when the measurements are highly noisy.

Defending against side-channel attacks is quite difficult. Many cryptography libraries attempt to defend against timing attacks by making sure that every operation takes a fixed amount of time. Defending against power and E/M attacks is even harder. Many of the defenses either make the device more expensive because of additional hardware requirements or make the system slower because of extra complexity in the software.

It is fair to say that understanding the full power of side-channel attacks, and designing cost-effective defenses, is still a very active area of research.

## ADVANCED CRYPTOGRAPHY

So far, we mostly focused on the core of cryptography: Data confidentiality, data integrity, and user authentication. But cryptography can do so much more. This last section briefly discusses more advanced capabilities, some of which can seem like magic. Some are ready for use and are being deployed, whereas others are further out.

### Zero-Knowledge Proofs.
Let one party, Alice, prove that a certain fact is true without revealing anything else about that fact. Anyone can verify the proof and be convinced. As a simple example, suppose Alice encrypts a number between zero and five under Bob’s public key. Let *ct* be the resulting ciphertext. She wants to prove to Claire that *ct* is indeed an encryption of a number between zero and five but without telling Claire what the number is. She can do so by giving Claire a zero-knowledge proof that confirms this fact. Claire can verify the proof and be convinced, without learning anything
