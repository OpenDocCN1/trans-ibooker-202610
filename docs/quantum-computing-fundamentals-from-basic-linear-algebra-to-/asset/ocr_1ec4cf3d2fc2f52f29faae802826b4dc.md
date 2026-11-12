Just to ensure this is clear to you, let us look at a few of the entries. Consider x = 4 and a = 2:

```
fa, N(x) = a^x mod N
```

or

```
fa, N(x) = 2^4 mod 15
```

which leads to

```
= 16 mod 15
```

```
= 1
```

Let us consider one more example, where x = 5 and a = 4:

```
fa, N(x) = a^x mod N
```

or

```
fa, N(x) = 4^5 mod 15
```

which leads to

```
= 1024 mod 15
```

```
= 4
```

This part requires the second phase of the algorithm. This is the quantum period-finding algorithm, which is described in the next subsection.

Returning from the period function, we continue the factoring process. If the value we find for r is odd, then we return to step 1, selecting a different pseudo-random number a. Also, if

```
a r / 2 == -1
```

we return to step 1 and select a different a.

However, if we have succeeded, then gcd (a^(r/2) + 1, N) and we have found the factors of N.

# 10.6.1 The Quantum Period-Finding Function

This section is rather technical. It is not critical that all readers master this section. If you understand the general outline of Shor’s algorithm, that should be adequate. You may choose to treat this portion as a black box. However, for those who wish to understand more, details are provided.

The heart of the quantum portion of Shor’s algorithm is that each choice of N and each choice of a used in f(x) = a^x mod N have their own quantum circuit. Given some N (the integer we wish to factor), first find Q = 2q such that N^2 < Q < 2N^2. Q is the number of states. This, in turn, implies that