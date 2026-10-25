# 22.3 Einstein Tensor

Let $(M,\mathfrak{g})$ be a semi-Riemannian manifold, and let $\mathcal{R}$ be the Riemann curvature tensor. The Ricci curvature tensor (field) in $\mathcal{T}_2^0(M)$ is defined by

```
R = C_{14}(\mathcal{R})
```

(22.3.1)

the scalar curvature (function) in $C^{\infty}(M)$ is defined by

```
S = C_{12}(R)
```

(22.3.2)

and the Einstein tensor (field) in $\mathcal{T}_2^0(M)$ is defined by

```
G = R - \frac{1}{2} S\mathfrak{g}.
```

In physics, the terms “Ricci curvature tensor”, “scalar curvature”, and “Einstein tensor” are typically used only when $M$ is a Lorentz 4-manifold, but we will continue with the more general context.

# Theorem 22.3.1 (Local Coordinate Expression for Ricci Curvature).

If $(M,\mathfrak{g})$ is a semi-Riemannian manifold, then, in local coordinates:

(a)

```
R_{ij} = \sum_{k} R_{kij}^{k}
```

```
= \sum_{k} \left( \frac{\partial \Gamma_{ij}^{k}}{\partial x^{k}} - \frac{\partial \Gamma_{jk}^{k}}{\partial x^{i}} \right) + \sum_{kl} (\Gamma_{kl}^{k} \Gamma_{ij}^{l} - \Gamma_{il}^{k} \Gamma_{jk}^{l}).
```

(22.3.3)

(b) R is symmetric; that is, $R_{ij} = R_{ji}$ for $i,j = 1,\dots,m$.

Proof. A preliminary result is

```
R_{ij} = C_{14}(\mathcal{R})_{ij} \quad [(22.3.1)]
```

```
= C_{1} \circ S_{4}^{1}(\mathcal{R})_{ij} \quad [(19.6.2)]
```

```
= \sum_{kl} \mathfrak{g}^{kl} \mathcal{R}_{kijl}. \quad [mv Th 6.3.5(a)]
```

(22.3.4)

(a): We have

```
R_{ij} = \sum_{kl} \mathfrak{g}^{kl} \mathcal{R}_{kijl} \quad [(22.3.4)]
```

```
= \sum_{kl} \mathfrak{g}^{kl} \left( \sum_{n} \mathfrak{g}_{ln} R_{kij}^{n} \right) \quad [Th 19.8.2(a)]
```

```
= \sum_{kn} \left( \sum_{l} \mathfrak{g}^{kl} \mathfrak{g}_{ln} \right) R_{kij}^{n} = \sum_{kn} \delta_{n}^{k} R_{kij}^{n} = \sum_{k} R_{kij}^{k},
```

which proves the first equality. The second equality follows from Theorem 18.7.2.