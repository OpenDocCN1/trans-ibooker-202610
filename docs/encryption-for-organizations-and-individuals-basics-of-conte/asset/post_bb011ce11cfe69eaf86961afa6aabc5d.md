
In the next chapter we’ll examine many other exciting technologies the very near future holds for quantum-based computing and cryptography.

## References

- Elliott, C. The DARPA Quantum Network. 2004. Retrieved from arXiv:quant-ph/0412029
- Peev, M., Pacher, C., Alleaume, R. et al. The SECOQC quantum key distribution network in Vienna. New Journal of Physics, Volume 11, 2009.
- D. Stucki, M. Legre, F. Buntschu et al. Long term performance of the SwissQuantum quantum key distribution network in a field environment. New Journal of Physics, 13 123001, 2011.
- Kramer, H. QUESS (Quantum Experiments at Space Scale) / Micius. Retrieved from: https://directory.eoportal.org/web/eoportal/satellite-missions/q/quess
- Sasaki, M., Fujiwara, M., Ishizuka, H. et al. Field test of quantum key distribution in the Tokyo QKD Network. 2011. Retrieved from https://arxiv.org/abs/1103.3566

## CHAPTER 14: Post-Quantum Cryptography

For the last few chapters, we have kept a focus on quantum key distribution, or QKD. This maturing technology is still going to be relevant for a long time. What’s going to come with it is known as *post-quantum cryptography*, which will be the main topic for this chapter. We’ll explore an overview of this topic, going through the basics of the most relevant approaches to encryption schemes in the post-quantum realm.

### Post-Quantum Cryptography

To clarify, *post-quantum cryptography (PQC)* is a field of science in which new quantum-resistant (mostly) public-key algorithmic solutions are researched for a full variety of devices and scenarios. *Quantum cryptography*, on the other hand, often refers to the use of quantum key distribution (QKD) alongside contemporary encryption techniques, as explained previously in the book.

Now, many classical cryptographic algorithms rely either on integer factorization (e.g., RSA) or discrete logarithms (e.g., ElGamal). While offering robust security within the current computing paradigm, the near future may not be as bright for these approaches. Algorithms hazardous to contemporary encryption schemes, like *Shor’s algorithm*, have been discovered after all. As powerful quantum computers are emerging, some effective defensive measures need to be implemented. The first line of defense is to simply increase key sizes used in contemporary encryption algorithms from, say, 128 to 256 bits (and beyond). What follows is the transition to full-scale encryption which is intrinsically quantum-resistant. Again, cryptographic schemes in this new era are referred to as *post-quantum algorithms*.

One of the biggest challenges in the near future comes in the form of integrating quantum resistance into existing devices. The potential solution of add-on hardware cryptoprocessors is in development by big business, including Microsoft and Google.

Many of today’s hashing functions are largely considered immune to quantum-based hacking. However, a technique known as *Grover’s algorithm* is potentially able to defeat some of these hash functions. When computing this algorithm, one is basically testing every input value simultaneously to see which is the correct one, taking advantage of quantum superposition in doing so.

Most proposed PQC protocols use an asymmetric approach. There are several major implementations of PQC being currently researched. We’ll explore some of them next.

### Hash-Based Cryptography

Cryptographic primitives based on the security of hash functions are generically known as *hash-based cryptography*. This variety of cryptography is currently limited to digital signature schemes. The US National Institute of Standards and Technology (NIST) announced in 2019 its intention to publish standards for stateful hash-based cryptography based on *Leighton-Micali Signatures (LMS)* and *eXtended Merkle Signature Scheme (XMSS)*. We will look at both of these schemes soon.

A state in the context of hashing schemes refers to recorded information, often an input password that is not to be reused. Stateless signature schemes do not request information/passwords after each signature is calculated.

- **Merkle signature scheme (MSS).** Although this approach dates back to the late 1970s, it’s still considered to be both computationally fast and resistant to quantum computers. The Merkle scheme can be used to sign an arbitrary number of messages (to the power of two) using one public key. Merkle trees are created by repeatedly hashing pairs of nodes until there is only one hash left, which is known as the Merkle Root (see Figure 14-1).

  ![](img/df9b6c40c665f58cab0c435444990c63_270_0.png)

  Figure 14-1. The basic layout of a Merkle tree. Concatenation in this context refers to the process of combining hex characters. By modifying information in data nodes A, B, C, or D, the above nodes are also altered, leading to a different Merkle Root.

  This root node becomes a public key; it can be then used to authenticate all other nodes. Message signing aside, a number of cryptocurrencies, including *Bitcoin*, use the Merkle tree approach for its quick and efficient data verification capabilities.

- **eXtended Merkle signature scheme (XMSS, XMSS MT).** Introduced in 2011 and not surprisingly sharing a lot with the Merkle signature scheme, XMSS is a more secure version of the venerable scheme, often producing smaller signature sizes as well. *XMSS MT* is a more powerful, multitree variant of this scheme and can in theory sign an infinite number of messages; compared to XMSS, this variant typically creates signatures faster at the cost of bigger file size (Hülsing & Gazdag).

- **Leighton-Micali Signatures (LMS).** Taking the Merkle scheme and improving on it, the basic component of the full LMS scheme is a *one-time (OT) signature scheme*. It consists of OT key generation, signing, and verifying algorithms. In the full scheme, we then combine this one-time scheme as a subroutine with a Merkle tree construction to have the full stateful LMS signature scheme.

- **SPHINCS/SPHINCS+.** Introduced in 2015, *SPHINCS* is a stateless signature scheme. A minor downside to SPHINCS is in the rather large signature size of 41 kilobytes. The scheme offers robust security against quantum computers. SPHINCS+ is an update on this approach which, for one, addresses the signature size. Even the highest security implementation of SPHINCS+ results in signatures of 30 kilobytes in size; the scheme can go as low as eight kilobytes for its lowest security level (Bernstein & Hopwood et al., 2015).

### Code-Based Cryptography

This branch of quantum-resistant cryptography relies on error correction codes (ECC). Most basic cryptographic functions like encryption and signing can be implemented using code theoretic concepts. From a consumer’s point of view, optimized code-based cryptography can be exceptionally fast during both data encryption and decryption, making it ideal for mobile devices with limited battery life (Engelbert, Overbeck, & Schmidt, 2007).

- The asymmetric *McEliece cryptosystem* from 1978 was the first implementation of the code-based approach. This scheme uses a specific error correction code, the *binary Goppa*. A Goppa code is based on modular arithmetic, in which numbers reaching a specific target value (i.e., the *modulus*) revert back to zero again. The most common real-life example of this is the twelve-hour clock. Such a device is said to use *arithmetic modulo 12*. The McEliece cryptosystem aside, numerous cryptographic algorithms, including RSA and AES, leverage modular arithmetic in various ways (Engelbert, Overbeck, & Schmidt, 2007).

- As robust as the Goppa codes are, they tend to need rather large public keys. This facet of the McEliece scheme might require some further development. Other, more lightweight codes have been proposed to replace the trapdoor functions provided by the Goppa codes, but none have proven to be as resilient to cryptanalysis (Au, Eubanks-Turner, & Everson, 2013).
