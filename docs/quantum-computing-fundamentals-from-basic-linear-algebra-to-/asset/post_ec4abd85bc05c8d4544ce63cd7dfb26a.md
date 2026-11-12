
To add a point P to itself, a tangent line to the curve is drawn at the point P. If y_P is not 0, then the tangent line intersects the elliptic curve at exactly one other point, –R, and –R is reflected in the x-axis to R. This operation is called “doubling the point P”, as illustrated by Figure 11.4.

![](img/49061be7fe11d9f7f3e56114ab15acdc_240_0.png)

FIGURE 11.4 Doubling the point P

The tangent from P is always vertical if y_P = 0. If a point P is such that y_P = 0, then the tangent line to the elliptic curve at P is vertical and does not intersect the elliptic curve at any other point. By definition, 2P = O for such a point P.

Recall that the field Fp uses the numbers from 0 to p – 1, and computations end by taking the remainder on division by p (i.e., the modulus operations). For example, in F23, the field is composed of integers from 0 to 22, and any operation within this field will result in an integer also between 0 and 22.

An elliptic curve with the underlying field of Fp can be formed by choosing the variables a and b within the field of Fp. The elliptic curve includes all points (x,y) that satisfy the elliptic curve equation modulo p (where x and y are numbers in Fp). For example, y^2 mod p = x^3 + ax + b mod p has an Fp's underlying field if a and b are in Fp.

If x^3 + ax + b contains no repeating factors, then the elliptic curve can be used to form a group. An elliptic curve group over Fp consists of the points on the corresponding elliptic curve, together with a special point O called the point at infinity. There are finitely many points on such an elliptic curve.

At the foundation of every asymmetric cryptosystem is a hard mathematical problem that is computationally infeasible to solve. The discrete logarithm problem is the basis for the security of many cryptosystems, including the Elliptic Curve Cryptosystem. More specifically, the ECC relies upon the difficulty of the Elliptic Curve Discrete Logarithm Problem (ECDLP).

Recall that we examined two geometrically defined operations over certain elliptic curve groups. These two operations were point addition and point doubling. By selecting a point in an elliptic curve group, one can double it to obtain the point 2P. After that, one can add the point P to the point 2P to obtain the point 3P. The determination of a point nP in this manner is referred to as scalar multiplication of a point. The ECDLP is based upon the intractability of scalar multiplication products.

In the multiplicative group Zp*, the discrete logarithm problem is as follows: given elements r and q of the group, and a prime p, find a number k such that r = q^k mod p. If the elliptic curve groups are described using multiplicative notation, then the elliptic curve discrete logarithm problem is as follows: given points P and Q in the group, find a number such that P^k = Q, where k is called the discrete logarithm of Q to the base P. When the elliptic curve group is described using additive notation, the elliptic curve discrete logarithm problem is the following: given points P and Q in the group, find a number k such that kP = Q.

The following is a common example, used in many textbooks, papers, and web pages. It uses rather small numbers that you can easily work with but still makes the general point of how elliptic curve cryptography works.

In the elliptic curve group defined by

```
y^2 = x^3 + 9x + 17 over F23
```

what is the discrete logarithm k of Q = (4,5) to the base P = (16,5)?

One way to find k is to compute multiples of P until Q is found. The first few multiples of P are shown here:

- P = (16,5) 2P = (20,20) 3P = (14,14) 4P = (19,20) 5P = (13,10)
- 6P = (7,3) 7P = (8,7) 8P = (12,17) 9P = (4,5)

Because 9P = (4,5) = Q, the discrete logarithm of Q to the base P is k = 9.

In a real application, k would be large enough such that it would be infeasible to determine k in this manner. This is the essence of elliptic curve cryptography; obviously, we use much larger fields. For more information, an excellent online tutorial can be found at http://arstechnica.com/security/2013/10/a-relatively-easy-to-understand-primer-on-elliptic-curve-cryptography/.

Elliptic curve cryptosystems (ECC) rely on the difficulty of the Elliptic Curve Discrete Logarithm Problem (ECDLP). This is a problem that cannot be solved in polynomial time on a classical computer. As you might have already surmised, this means that ECC is susceptible to attacks from quantum computers.

As you can see from the previous sections, elliptic curves form groups, and those groups can be used just as any other algebraic group. The practical implication of this is that one can adapt various algorithms to elliptic curve groups. There are many permutations of elliptic curve cryptography, including the following:

- Elliptic Curve Diffie-Hellman (used for key exchange)
- Elliptic Curve Digital Signature Algorithm (ECDSA)
- Elliptic Curve MQV key agreement protocol
- Elliptic Curve Integrated Encryption Scheme (ECIES)

In this section, we will take a closer look at two of these: ECC Diffie-Hellman and ECDSA.

## 11.3.1 ECC Diffie-Hellman

Diffie-Hellman, which we examined earlier in this chapter, is the oldest key exchange protocol. It is a natural to modify for elliptic curves. Elliptic curve Diffie-Hellman (ECDH) is a key exchange or key agreement protocol used to establish a shared secret over an insecure medium. That shared secret is then either used directly or as the basis to derive another key. In the case of ECDH, the public-private key pairs are based on elliptic curves.

- Public: Elliptic curve and point (x,y) on curve
- Secret: Alice's A and Bob's B
  - Alice computes A(B(x,y)).
  - Bob computes B(A(x,y)).
  - These are the same since AB = BA.
- Public: Curve y² = x³ + 7x + b (mod 37) and point (2,5) ⇒ b = 3
- Alice's secret: A = 4
- Bob's secret: B = 7
  - Alice sends Bob 4(2,5) = (7,32).
  - Bob sends Alice 7(2,5) = (18,35).
  - Alice computes 4(18,35) = (22,1).
  - Bob computes 7(7,32) = (22,1).

For more details, consult NIST Special Publication 800-56A, Revision 2, at http://csrc.nist.gov/publications/nistpubs/800-56A/SP800-56A_Revision1_Mar08-2007.pdf.

## 11.3.2 ECDSA

The Digital Signature Algorithm was invented specifically for digitally signing messages. Of course, one can utilize any asymmetric algorithm to sign a message, but the Digital Signature Algorithm was designed for that purpose. As you might expect, there is an elliptic curve variant on this algorithm.

To illustrate how this works, we will consider the fictitious characters Bob and Alice once again. First, the two parties must agree on some parameters: the curve, denoted as E, the base point/generator of the elliptic curve, denoted as G, and the order of G (an integer), denoted by n. Now to sign a message, Alice takes the following steps:

- **Step 1.** Select a random integer k that is less than n (i.e., 0 < k < n).
- **Step 2.** Compute kG = (x1,y1) and r = x1 mod n. If r = 0, then go to step 1.
- **Step 3.** Compute k^-1 mod n.
- **Step 4.** Compute e = SHA-1(m). Most digital signature algorithms use a hash; in this case, the hash is usually SHA-1. Therefore, this is stating that Alice computes the SHA-1 hash of the message.
- **Step 5.** Compute s = k^-1{e + d_A . r} mod n.
- **Step 6.** If s = 0, then go to step 1. In other words, Alice keeps repeating the process until s != 0. This is not usually time consuming and could happen on the first attempt.
- **Step 7.** Alice’s signature for the message m is (r, s).

In order for Bob to verify Alice’s signature (r,s), he will execute the following steps:

- **Step 1.** Verify that r and s are integers in [1, n-1].
- **Step 2.** Compute e = SHA-1(m).
- **Step 3.** Compute w = s^-1 mod n.
- **Step 4.** Compute u1 = ew mod n and u2 = rw mod n.
- **Step 5.** Compute (x1, y1) = u1G + u2 Q_A.
- **Step 6.** Compute v = x1 mod n.
- **Step 7.** Accept the signature if and only if v = r.

Figure 11.5 summarizes the process.
