Elliptic curve cryptosystems (ECC) rely on the difficulty of the Elliptic Curve Discrete Logarithm Problem (ECDLP). This is a problem that cannot be solved in polynomial time on a classical computer. As you might have already surmised, this means that ECC is susceptible to attacks from quantum computers.

As you can see from the previous sections, elliptic curves form groups, and those groups can be used just as any other algebraic group. The practical implication of this is that one can adapt various algorithms to elliptic curve groups. There are many permutations of elliptic curve cryptography, including the following:

+   - Elliptic Curve Diffie-Hellman (used for key exchange)
+   - Elliptic Curve Digital Signature Algorithm (ECDSA)
+   - Elliptic Curve MQV key agreement protocol
+   - Elliptic Curve Integrated Encryption Scheme (ECIES)

In this section, we will take a closer look at two of these: ECC Diffie-Hellman and ECDSA.

# ## 11.3.1 ECC Diffie-Hellman

Diffie-Hellman, which we examined earlier in this chapter, is the oldest key exchange protocol. It is a natural to modify for elliptic curves. Elliptic curve Diffie-Hellman (ECDH) is a key exchange or key agreement protocol used to establish a shared secret over an insecure medium. That shared secret is then either used directly or as the basis to derive another key. In the case of ECDH, the public-private key pairs are based on elliptic curves.

+   - Public: Elliptic curve and point (x,y) on curve
+   - Secret: Alice's A and Bob's B
+     - Alice computes A(B(x,y)).
+     - Bob computes B(A(x,y)).
+     - These are the same since AB = BA.
+   - Public: Curve y² = x³ + 7x + b (mod 37) and point (2,5) ⇒ b = 3
+   - Alice's secret: A = 4
+   - Bob's secret: B = 7
+     - Alice sends Bob 4(2,5) = (7,32).
+     - Bob sends Alice 7(2,5) = (18,35).
+     - Alice computes 4(18,35) = (22,1).
+     - Bob computes 7(7,32) = (22,1).