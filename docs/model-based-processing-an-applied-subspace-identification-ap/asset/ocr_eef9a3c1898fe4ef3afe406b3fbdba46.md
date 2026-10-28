of the order N_x indicates the minimum number of terms required to exactly “match” the Markov sequence and extract the Markov parameters. Also the ρ(H_{K,K}) = N_x is the dimension of the minimal realization. If we did not know the dimension of the system, then we would let (in theory) K → ∞ and determine the rank of H_{∞,∞}. Therefore, the minimal dimension of an “unknown” system is the rank of the Hankel matrix.

In order for a system to be minimal, it must be completely controllable and completely observable [4]. This can be seen from the fact that the Hankel matrix factors as

```
H_{K,K} = [ CB ⋯ CA^{K-1}B ; ⋮ ⋮ ; CA^{K-1}B ⋯ CA^{2K-2}B ] = [ C ; ⋮ ; CA^{K-1} ] [ B | ⋯ | A^{K-1}B ] (3.54)
```

or simply

```
H_{K,K} = O_K × C_K (3.55)
```

From this factorization, it follows that the ρ(H_{K,K}) = min[ρ(O_K), ρ(C_K)] = N_x. Therefore, we see that the properties of controllability and observability are carefully woven into that of minimality and testing the rank of the Hankel matrix yields the dimensionality of the underlying dynamic system. This fact will prove crucial when we “identify” a system, Σ_{ABCD} = {A, B, C, D}, from noisy measurement data. We shall discuss realization theory in more detail in Chapter 6 using these results.

# 3.4.3 Equivalent Linear Systems

Equivalent systems are based on the concept that there are an infinite number of state-space systems that possess “identical” input/output responses. In the state-space, this is termed a set of coordinates; that is, we can change the state vectors describing a system by a similarity transformation such that the system matrices Σ_{ABCD} := {A, B, C, D} are transformed to a different set of coordinates by the transformation matrix T [1–5], that is,

```
Σ_{ABCD} --T--> Σ̅_{ABCD}
```

where we have

```
{A, B, C, D} → {Ā, B̄, C̄, D̄} := {TAT^{-1}, TB, CT^{-1}, D}
```

yields an “equivalent” system from an input/output perspective, that is, the transfer functions are identical

```
H(z) = C̄(zI - Ā)^{-1}B̄ + D̄ = CT^{-1} × (zI - TAT^{-1})^{-1} × TB + D = C(zI - A)^{-1}B + D
```