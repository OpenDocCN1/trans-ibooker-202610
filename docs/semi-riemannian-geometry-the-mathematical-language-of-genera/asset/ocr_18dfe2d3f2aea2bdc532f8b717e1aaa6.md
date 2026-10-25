Suppose $(V,\mathfrak{g})$ is in fact an inner product space. If $v$ is a vector in $V$ such that $\langle v,w\rangle = 0$ for all vectors $w$ in $V$, then $\langle v,v\rangle = 0$, hence $v = 0$. Thus, $\mathfrak{g}$ is nondegenerate on $V$, demonstrating that $(V,\mathfrak{g})$ is automatically a scalar product space. In the present context, the norm corresponding to $\mathfrak{g}$, given by (4.1.1), simplifies to the function

```
||·|| : V → ℝ
```

defined by

```
||v|| = √⟨v,v⟩ = √q(v)    (4.6.1)
```

for all vectors $v$ in $V$. Since an inner product space is a type of scalar product space, the definitions of orthogonal, orthogonal $k$-tuple, orthogonal basis, orthonormal, orthonormal $k$-tuple, and orthonormal basis given in Section 4.2 also apply here. Let $(e_1,...,e_m)$ be an orthonormal basis for $V$. Since $\mathfrak{g}$ is an inner product, $\langle e_i,e_i\rangle = 1$ for $i = 1,...,m$. Thus, ind$(\mathfrak{g}) = 0$ and the signature of $\mathfrak{g}$ is an $m$-tuple of $+1$s.

The fact that an inner product is positive definite distinguishes it from a scalar product space in ways that are fundamental. For example, in an inner product space, only the zero vector is orthogonal to itself; or equivalently, only the zero vector has zero norm. Perhaps most important of all, in an inner product space, the inner product is positive definite (hence nondegenerate) on every subspace. Consequently, pairing any subspace of an inner product with the corresponding restriction of the inner product yields an inner product space.

# Example 4.6.1 (ℝ₀ᵐ). Continuing with Example 4.1.1, let ν = 0. In this case, we denote 𝔰 by 𝔢. Then

```
𝔢 : ℝᵐ × ℝᵐ → ℝ
```

is given by

```
𝔢((x¹,...,xᵐ),(y¹,...,yᵐ)) = x¹y¹ + ⋯ + xᵐyᵐ.
```

We refer to

```
ℝ₀ᵐ = (ℝᵐ,𝔢)
```

as Euclidean m-space and to 𝔢 as the Euclidean inner product. It is easily shown that 𝔢 is in fact an inner product on ℝᵐ. Following Example 4.1.1, the standard basis for ℝ₀ᵐ is simply the standard basis for ℝᵐ. The norm corresponding to 𝔢 is given by

```
||(x¹,...,xᵐ)|| = √((x¹)² + ⋯ + (xᵐ)²)
```

and called the Euclidean norm. The reader is likely familiar with ||·|| as a measure of “length” in ℝᵐ, a notion to be explored further in Section 9.3. ◇

# Theorem 4.6.2. Let (V,𝔤) be a scalar product space of index ν. Then 𝔤 is an inner product if and only if ν = 0.