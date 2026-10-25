Borrowing terminology from the special theory of relativity, we say that (with respect to b) a vector v in V is:

+   - **spacelike** if v = 0 or ⟨v, v⟩ > 0.
+   - **timelike** if ⟨v, v⟩ < 0.
+   - **lightlike** if v ≠ 0 and ⟨v, v⟩ = 0.

The zero vector is usually considered to be spacelike (as is the case here), but in some of the literature it is taken to be lightlike. Evidently, each vector is either spacelike, timelike, or lightlike, and this results in a corresponding partition of V. (There are several standard but differing uses of the term “partition” found at various places in this book. The present instance refers to the definition of partition given in Appendix A.)

Let b be a bilinear function on a vector space V, and let H = (h₁, …, hₘ) be a basis for V. The matrix of b with respect to H is denoted by b_H and defined by

```
b_H = [ b₁₁ … b₁ₘ ; ⋮ ⋱ ⋮ ; bₘ₁ … bₘₘ ]
```

where

```
b_ij = ⟨h_i, h_j⟩
```

for i, j = 1, …, m.

The next two results illustrate how matrix representations can be used to facilitate computations involving bilinear functions.

# Theorem 3.1.2. Let V be a vector space of dimension m, let b be a bilinear function on V, and let H be a basis for V. Then:

+   (a) For all vectors v, w in V,
+   ⟨v, w⟩ = ([v]_H)^T b_H [w]_H.
+   (b) For all integers 1 ≤ k ≤ m and all vectors v₁, …, v_k, w₁, …, w_k in V,
+   [ ⟨v₁,w₁⟩ … ⟨v₁,w_k⟩ ; ⋮ ⋱ ⋮ ; ⟨v_k,w₁⟩ … ⟨v_k,w_k⟩ ] = [ [v₁]_H … [v_k]_H ]^T b_H [ [w₁]_H … [w_k]_H ].

Proof. (a): Let H = (h₁, …, hₘ), and let v = Σ_i a^i h_i and w = Σ_j b^j h_j. Then

```
([v]_H)^T = [a¹ … a^m] and [w]_H = [b¹ … b^m]^T,
```

hence

```
⟨v, w⟩ = ⟨ Σ_i a^i h_i, Σ_j b^j h_j ⟩ = Σ_{ij} a^i b^j ⟨h_i, h_j⟩ = Σ_{ij} a^i b^j b_ij = ([v]_H)^T b_H [w]_H.
```