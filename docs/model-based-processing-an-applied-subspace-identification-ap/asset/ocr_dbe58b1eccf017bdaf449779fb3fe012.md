where $N_a \ge N_b$ and $N_c$ and $\{e(t)\}$ is a zero-mean, white sequence with spectrum given by $R_{ee}$.

It is straightforward to show (see [5]) that the ARMAX model can be represented in *observer canonical form*:

```
x(t) = A₀ x(t − 1) + B₀ u(t − 1) + W₀ e(t − 1)
y(t) = C₀' x(t) + b₀ u(t) + c₀ e(t)  (3.80)
```

where $x$, $u$, $e$, and $y$ are the $N_a$-state vector, scalar input, noise, and output with

```
A₀ := [ 0 | −a_{N_a} ; --- | : ; I_{N_a−1} | −a₁ ]
B₀ := [ −a_{N_a} b₀ ; : ; −a_{N_b+1} b₀ ; --- ; b_{N_b} − a_{N_b} b₀ ; : ; b₁ − a₁ b₀ ]
W₀ := [ −a_{N_a} c₀ ; : ; −a_{N_c+1} c₀ ; --- ; c_{N_c} − a_{N_c} c₀ ; : ; c₁ − a₁ c₀ ]
C₀' := [0 ... 0 1]
```

Noting this structure we see that each of the matrix or vector elements $\{A_{i,N_a}, B_i, W_i, C_i\}$ $i = 1, ..., N_a$ can be determined from the relations

```
A_{i,N_a} = −a_i   i = 1, ..., N_a
B_i = b_i − a_i b₀
W_i = c_i − a_i c₀
C_i = δ(N_a − i)  (3.81)
```

where

```
b_i = 0      for  i > N_b
c_i = 0      for  i > N_c
δ(i − j)  is the Kronecker delta
```

Consider the following example to illustrate these relations.

# Example 3.3  Let $N_a = 3$, $N_b = 2$, and $N_c = 1$; then the corresponding ARMAX model is

```
y(t) = −a₁ y(t − 1) − a₂ y(t − 2) − a₃ y(t − 3) + b₀ u(t) + b₁ u(t − 1) + b₂ u(t − 2) + c₀ e(t) + c₁ e(t − 1)  (3.82)
```