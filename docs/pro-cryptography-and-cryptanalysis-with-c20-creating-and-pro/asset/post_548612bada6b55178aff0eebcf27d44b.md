
```cpp
cout<< endl << endl << "-------- Encrypting... --------";

cout<<endl<<"--Message--";
cout<<endl<<"m = "<<message;
cout<<endl<<"--Encryption of the message--";
cout<<endl<<"Enc(m) = (" << u << ", " << v << ")";

cout<< endl << endl << "-------- Decrypting... --------";
int result = (v - secretValue * u) % q;

int decryption;
if (result > q/2)
    decryption = 1;
else
    decryption  = 0;
cout<<endl<<"The message is: " << decryption;

return 0;
}
```

![](img/c3d9424f64735276d661e7aba725b2a2_304_0.png)

Figure 13-2. The result of running the program of the public-key LWE example

Listing 13-2 provides the example of public key encryption based on LWE, which was proposed in the work [5]. We first create a secret value `secretValue` which represents the private key. In the next step, we create the public key. The public key is formed by the values from a set of random numbers `values_A` and a set of values `values_B`, which are computed based on `values_A`, `secretValue`, and random errors `values_error`. This example is implemented for a single bit.

A simple workflow for this example is:

- Between 0 and q (in the example, q=97), we select a random set of 20 values `values_A` that represent one of the components of the public key.
- Further, we define the set `values_B` where every element is computed as `values_B[i]=values_A[i] x secretValue+values_error[i] mod q`, where `secretValue` is the secret key, and where `values_error` represents a list of small random values called *the errors values*.
- The sets `values_A`, `values_B` form the public key and `secretValue` represents the secret key. At this point, we can share `values_A` and `values_B` with anyone who wants to proceed with an encryption of a message (with the condition to keep `secretValue` secret). In the encryption process, we use samples from `values_A` and `values_B`. Moving forward, based on those samples we take a bit message and compute the following two values:
  - `u = ∑ (values_A_samples)(mod q)`
  - `v = ∑(values_B_samples) + q/2 × message (mod q)`
- At this point, we can say that the encrypted message is `(u, v)`. To proceed with the decryption, we need to compute:
  - `decryption = v − s × u (mod q)`
- If `decryption < q/2`, the message is equal with 0; otherwise 1.

The procedure described above is summarized from Oded Regev’s paper [5] in order to make it easy to follow and to give you a clear understanding of how you can transpose the complexity of LWE in reality.

## Conclusion

In this chapter, we discussed Ring Learning with Errors cryptography and we implemented two examples of encryption schemes using the C++ programming language as proposed in the works [4] and [5]. RLWE can be a space for many challenges for professionals and a starting point for significant contributions to this cryptographic primitive.

Through the chapter, you experienced an interesting journey with LWE from which you gained the following:

- A solid but short mathematical background of the main concepts and definitions on which RLWE is based and without which a practical implementation will have many gaps to fill
- Experience experimenting with the challenges brought by RLWE’s mathematical concepts and their transposition in practice
- The ability to implement simple examples of public-key encryption schemes based on LWE

## References

- [1] Oded Regev, The Learning with Errors Problem, https://cims.nyu.edu/~regev/papers/lwesurvey.pdf.
- [2] O. Regev, “On lattices, learning with errors, random linear codes, and cryptography” in *Journal of the ACM (JACM)*, 56(6) (pp. 1-40). 2009.
- [3] R. Lindner and C. Peikert, “Better key sizes (and attacks) for LWE-based encryption” in *Cryptographers’ Track at the RSA Conference* (pp. 319-339). Springer; Berlin, Heidelberg. February, 2011.
- [4] O. Regev, “The Learning with Errors Problem (Invited Survey)” in *2010 IEEE 25th Annual Conference on Computational Complexity* (pp. 191-204). Cambridge, MA. doi: 10.1109/CCC.2010.26. 2010.
- [5] D. Micciancio and O. Regev. “Lattice-based cryptography” in (eds. D. J. Bernstein and J. Buchmann) *Post-quantum Cryptography*. Springer. 2008.
- [6] C. Peikert, “Some recent progress in lattice-based cryptography” in Slides for invited tutorial at TCC ’09, 2009.
- [7] D. Micciancio, “Cryptographic functions from worst-case complexity assumptions” in (eds. P. Q. Nguyen and B. Vallée) *The LLL Algorithm: Survey and Applications, Information Security and Cryptography* (pp. 427–452). Springer. 2008. Prelim. version in LLL25, 2007.
- [8] O. Regev, “Lattice-based cryptography” in *CRYPTO* (pp. 131–141). 2006.
- [9] NewHope – Post-quantum Key Encapsulation. Available online: [https://newhopecrypto.org/](https://newhopecrypto.org/).
- [10] Jintai Ding, Xiang Xie, and Xiaodong Lin, “A Simple Provably Secure Key Exchange Scheme Based on the Learning with Errors Problem.” January 1, 2012. Available online: [https://eprint.iacr.org/2012/688](https://eprint.iacr.org/2012/688).
- [11] C. Peikert, “Lattice Cryptography for the Internet” in (ed. M. Mosca) *Post-Quantum Cryptography*. PQCrypto 2014. *Lecture Notes in Computer Science*, vol 8772. Springer, Cham. 2014.
- [12] Y. Desmedt, “Fiat–Shamir Identification Protocol and the Feige–Fiat–Shamir Signature Scheme” in (eds. H.C.A. van Tilborg and S. Jajodia) *Encyclopedia of Cryptography and Security*. Springer, Boston, MA. 2011.
- [13] V. Lyubashevsky “Lattice Signatures without Trapdoors” in (eds. D. Pointcheval and T. Johansson) *Advances in Cryptology – EUROCRYPT 2012. EUROCRYPT 2012. Lecture Notes in Computer Science*, vol 7237. Springer; Berlin, Heidelberg. 2012.
- [14] Tim Güneysu, Vadim Lyubashevsky, and Thomas Pöppelmann, “Practical Lattice-Based Cryptography: A Signature Scheme for Embedded Systems” in (eds. Emmanuel Prouff and Patrick Schaumont) *Lecture Notes in Computer Science* (pp. 530–547). Springer; Berlin, Heidelberg. doi:10.1007/978-3-642-33027-8_31. ISBN 978-3-642-33026-1. 2012.
- [15] Zvika Brakerski and Vinod Vaikuntanathan, “Fully Homomorphic Encryption from Ring-LWE and Security for Key Dependent Messages” in (ed. Phillip Rogaway) *Lecture Notes in Computer Science* (pp. 505–524). Springer; Berlin Heidelberg. doi:10.1007/978-3-642-22792-9_29. ISBN 978-3-642-22791-2. 2011.

## CHAPTER 14: Chaos-Based Cryptography

In chaos-based cryptography, the chaos theory and its mathematical background are applied for creating novel and unique cryptographic algorithms. The first attempt of using the chaos theory in cryptography was initiated by Robert Matthews in 1989 through the work [1], which attracted much interest.

In contrast to the regular cryptographic primitives used daily, the chaos theory and its system are used in an efficient way by implementing the chaotic maps towards confusion and diffusion. Through this chapter, the cryptographic algorithm is referred to as the chaotic system.

To understand the similarities and differences between chaotic systems and cryptographic algorithms, we present a set of correspondences in Table 14-1 introduced by L. Kocarev in [2].

Table 14-1. Similarities and Differences Between Chaotic Systems and Cryptographic Algorithms

| Chaotic System | Cryptographic Algorithm |
|---|---|
| Phase space: (sub) set of real numbers | Phase space: finite set of integers |
| Iterations | Rounds |
| Parameters | Key |
| Sensitivity to a change in initial conditions and parameters | Diffusion |
| ? | Security and Performance |

Further, we demonstrate the similarities and the differences from Table 14-1 using an example of a chaotic system, the shift map:

$$x(t+1)=ax(t)\pmod{1}$$

where the phase space x=[0,1] is the unit interval and a>1 is an integer value.

From the chaos theory perspective, cryptography can use different functions and discrete-time systems. By analyzing them, the phase space will become a finite set of integers and the parameters will be integer values. The version of the shift map that uses the discrete phase-space is one of the common examples:

$$p(t+1)=ap(t)\pmod{N}$$

where $a>1$, $N$ and $p$ are integer values, with the restrictions $p \in \{0,1,\ldots,N-1\}$, and $N$ is coprime to $a$. This representation of the shift map is invertible, which means that all trajectories placed within a dynamical system with a finite phase space are called *periodical*. This fact introduces a new concept, namely the period function $P_N$ that describes the least period of the map $F$, denoted $F^{P_N}$ as its identity, and $P_N$ is minimal as it is a function within a system of size $N$.

Another very important metric used in the practical chaotic systems is the *Lyapunov exponent* (LE), whose trivial value is 0. The reason for it is the case in which the orbit is periodic and it will reiterate itself.

With this information, below are presented two concepts of block diagrams (for text encryption and image encryption) that demonstrate an encryption scheme based on the chaos theory. Figures 14-1 and 14-2 show the encryption process and the decryption process, respectively, based on the logistic map. Figure 14-3 shows an example of image encryption and decryption.

Following the examples of the block diagrams, we can examine the former papers and the other papers listed in this chapter’s references to observe that the encryption models and the way in which they are built are different according to the chaotic map used. Before designing new cryptographic approaches and mechanisms based on the chaos theory, it is very important to comprehend the way in which different chaotic maps work.

A good starting point is to use the following block diagrams as a guide from theory to practice, because the models are created according to the similarities and differences from Table 14-1.
