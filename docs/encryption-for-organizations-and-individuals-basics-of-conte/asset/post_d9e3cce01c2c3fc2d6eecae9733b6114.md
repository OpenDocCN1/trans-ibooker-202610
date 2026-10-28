
+   - The *Niederreiter cryptosystem* is a more recent take on the McEliece cryptosystem. Developed in 1986 by *Harald Niederreiter*, the approach is considered equal to its predecessor when it comes to the overall level of security. However, Niederreiter is faster than the McEliece cryptosystem when encrypting data (Kapshikar & Mahalanobis, 2020).

## Multivariate Cryptography

*Multivariate cryptography* is a concept referring to asymmetric cryptographic primitives based on multivariate polynomials. It represents a very robust take on post-quantum cryptography. However, as most of this technology hasn’t fully matured yet as of 2020, it’s feasible that undiscovered attack vectors become an issue.

+   - Cryptologist *Jacques Patarin* presented a public-key cryptosystem called *Hidden Field Equations (HFE)* in 1996. This was perhaps the first example of a highly secure form of *multivariate cryptography* and it remains popular today. HFE is built on the difficulty of the problems of solving a system of quadratic equations. HFE is based on polynomials over finite fields of different sizes to mask the connection between private and public keys.
+   - *Unbalanced oil and vinegar (UOV)* is another signature scheme by Patarin based on his earlier work. UOV depends on the difficulty of distinguishing between two types of variables it uses, referred to as “oil” and “vinegar.” The term unbalanced comes from this scheme implementing a differing amount of the two aforementioned variables. In order to create and receive signatures with UOV, only a minimal quadratic equation system is to be solved. This scheme is considered quantum-resistant. UOV is also highly implementable in the most basic of hardware devices as it’s mostly based on multiplication and addition. The key lengths in this scheme tend to be rather large, resulting in somewhat hefty key file sizes.

+   An earlier version of UOV, known as *balanced oil and vinegar*, was broken in 1998, calling for this updated version (Kipnis & Shamir, 1998).

+   **Rainbow** by *Jintai Ding* and *Dieter Schmidt* is a signature scheme based on UOV. Published in 2005, it can be described as a multilayer implementation of the aforementioned approach. While being very speedy, Rainbow shares the issue of large key and signature sizes of its predecessor(s), although to a smaller degree (Ferozpuri & Gaj, 2018).

## Lattice-Based Cryptography

A *lattice* is a mathematical concept in group theory referring to repeating sets of points. Lattices are coordinate vectors in an n-dimensional space (see Figure 14-2). Perhaps the best real-life example of lattices in nature comes in the form of crystals. The term *n-dimensional space* refers to a vector space with an arbitrary number of vectors.

![](img/df9b6c40c665f58cab0c435444990c63_275_0.png)

Figure 14-2. A two-dimensional lattice (left) next to a three-dimensional lattice. The latter is 1 of 14 of the so-called Bravais lattices, representing one of their primitive types

Lattice-based cryptography is considered extremely robust as it represents the learning with errors (LWE) concept. In LWE a small, controlled error is introduced to datasets to make them impossible to decrypt (at least within a reasonable time frame). This technique is a major part of machine learning. The LWE problem was introduced in 2005 by theoretical computer scientist Oded Regev who went on to win the 2018 Gödel Prize.

Decryption in LWE is done by knowing the secret error distribution which will allow the recipient to receive the actual message. Although lattice-based cryptosystems are very fast, they need large key sizes. Next, we’ll review some signature schemes based on the aforementioned approach.

> Lattice problems refer to a class of optimization problems which are extremely difficult to solve. These are used to create the robust cryptographic properties lattices have to offer. For one, highly collision-resistant hash functions can be built based on the difficulty of finding an approximate shortest vector in a lattice.

+   - **Bimodal Lattice Signature Scheme (BLISS).**
+   Compared to many other approaches, this scheme offers smaller signature size by the use of *Huffman encoding*, which is a type of lossless compression. BLISS is also claimed to be computationally effective and to offer robust protection against cryptographic attacks (Ducas & Durmus et al., 2013).
+   - **Goldreich–Goldwasser–Halevi (GGH) scheme.** The GGH is a public-key cryptosystem which utilizes the *closest vector problem (CVP)*. In this scheme both the public and private keys are representations of the same lattice. GGH offers good performance due to its effective leveraging of simple matrix operations.
+   - **GGH signature scheme.** Like the other GGH, this scheme uses the CVP problem for solid security, in this case for the purposes of digital signatures. Introduced in 1997, the GGH signature scheme uses an approach where messages need to be hashed into a mathematical space spanned by a lattice. The signature for a specific hash in this space is the closest lattice point. It’s worth noting the most basic variant of the GGH signature scheme was demonstrated to be broken (Nguyen, 1999).
+   - **NewHope.** This extremely secure key exchange scheme is based on a computational problem known as *Ring learning with errors (RLWE)*. This provides exceptional performance and is simple to implement in software. However, it’s been postulated that the standard learning with errors (LWE) approach might actually be more robust as it’s been studied more thoroughly for threat resistance over the years (Alkim & Avanzi et. al., 2020).

+   - **NTRU** is a formerly proprietary cryptographic software consisting of two components: *NTRUencrypt*, a robust encryption solution, and *NTRUsign*, a signature scheme. NTRU was created in 1996 by mathematicians *Jeffrey Hoffstein, Jill Pipher, and Joseph H. Silverman*. As of 2017, NTRU has been made available in the public domain. The system is known to be both very speedy (being hailed as the fastest lattice-based system available) and quantum-resistant. A derivative of the NTRU algorithm, the *Stehle-Steinfeld*, is not considered as efficient. Another take on NTRU, *NTRU Prime*, has made it into the second round of NIST’s *Post-Quantum Cryptography Standardization project*.

## Homomorphic Encryption

With *homomorphic encryption (HE)*, decryption is not actually necessary to access any parts of encrypted datasets. This approach works well for health and finance sectors as at no point is decryption necessary in the traditional sense when operating on encrypted information. Critical cloud-based storage is just one example of a scenario in which (fully) homomorphic cryptography becomes highly useful. One notable feature of lattice-based cryptography is known to facilitate *fully homomorphic encryption (FHE)*, the most robust form of homomorphism, particularly well. However, homomorphic encryption can be implemented with numerous public-key algorithms such as *ElGamal* and some of its variations (Parmar, Padhar, & Patel et al., 2014).

The somewhat elderly RSA encryption method, mentioned in the previous chapters, actually represents *partially homomorphic encryption (PHE)*.

Now, there are basically three degrees to homomorphism in cryptography, each with different pros, cons, and usage scenarios. These are summarized in Table 14-1.

## Table 14-1. The three main degrees of homomorphic encryption

| Classification | Features | Potential Usage Scenarios |
|---|---|---|
| Partially homomorphic (PHE) | Permits either addition or multiplication on encrypted data but not both, fast computational speed | Statistical analysis |
| Somewhat homomorphic (SHE) | Supports more than a single type of operation, limits total number of operations | Secure databases |
| Fully homomorphic (FHE) | Arbitrary computation on encrypted data, slowest rate of computation | Cloud-based storage, electronic voting, managing highly classified data |
