property: for every number n between 1 and p–1 inclusive, there is a power k of g such that n = gk mod p. Let us continue the standard cryptographic practice of using Alice and Bob to illustrate this.

Alice generates a random private value a and Bob generates a random private value b. Both a and b are drawn from the set of integers.

They derive their public values using parameters p and g and their private values. Alice's public value is g^a mod p and Bob's public value is g^b mod p.

They exchange their public values.

Alice computes g^ab = (g^b)^a mod p, and Bob computes g^ba = (g^a)^b mod p.

Because g^ab = g^ba = k, Alice and Bob now have a shared secret key, k.

Figure 11.1 illustrates this process.

![](bbox=[0.112, 0.385, 0.88, 0.65])

FIGURE 11.1 Diffie-Hellman

If you examine this process, you should note that the security of Diffie-Hellman is predicated on the difficulty of solving the discrete logarithm problem. Shor's algorithm can also be applied to discrete logarithms. This means that given a quantum computer running Shor's algorithm, Diffie-Hellman can be broken in a reasonable time period.

# 11.2.1 Elgamal

Elgamal is one of many improvements made to Diffie-Hellman. Taher Elgamal first described it in 1984. Elgamal is based on the Diffie-Hellman key exchange algorithm.