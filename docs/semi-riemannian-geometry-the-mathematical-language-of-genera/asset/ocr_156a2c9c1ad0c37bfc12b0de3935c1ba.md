When m = n, the determinant det(J_F(p)) is called the Jacobian determinant of F at p. Let 1 ≤ i1 < ⋯ < is ≤ m and 1 ≤ j1 < ⋯ < js ≤ n be integers, where 1 ≤ s ≤ min(m, n). Using multi-index notation, the submatrix of J_F(p) consisting of the intersection of rows i1, …, is and columns j1, …, js (in that order) is

```
J_F(p)^{(i1,...,is)}_{(j1,...,js)} = [ ∂F^{i1}/∂x^{j1}(p) ... ∂F^{i1}/∂x^{js}(p) ; : ⋱ : ; ∂F^{is}/∂x^{j1}(p) ... ∂F^{is}/∂x^{js}(p) ]
```

In the literature, the above matrix is commonly denoted by ∂(F^{i1}, …, F^{is})/∂(x^{j1}, …, x^{js})(p).

# Theorem 10.1.10 (Classical Chain Rule). Let U and V be open sets in ℝ^m and ℝ^n, respectively, let F : U ⟶ ℝ^n and G : V ⟶ ℝ^k be maps such that F(U) ⊆ V, and let p be a point in U. If F is differentiable at p and G is differentiable at F(p), then G ∘ F is differentiable at p and J_{G∘F}(p) = J_G(F(p)) J_F(p).

Equivalently, let F = (F^1, …, F^n) and G = (G^1, …, G^k), and let (x^1, …, x^m) and (y^1, …, y^n) be coordinates on ℝ^m and ℝ^n, respectively. Then

```
∂(G^i ∘ F)/∂x^j (p) = Σ_{l=1}^n ∂G^i/∂y^l (F(p)) ∂F^l/∂x^j (p)  (10.1.4)
```

for i = 1, …, k and j = 1, …, m.

Proof. Let ℰ, ℱ, and 𝒢 be the standard bases for ℝ^m, ℝ^n, and ℝ^k, respectively. The Jacobian matrix of G at F(p) is the k × n matrix

```
J_G(F(p)) = [ ∂G^1/∂y^1(F(p)) ... ∂G^1/∂y^n(F(p)) ; : ⋱ : ; ∂G^k/∂y^1(F(p)) ... ∂G^k/∂y^n(F(p)) ]  (10.1.5)
```

We have [J_{G∘F}(p)]^𝒢_ℰ = [d_p(G ∘ F)]^𝒢_ℰ [(10.1.3)] = [d_{F(p)}(G) ∘ d_p(F)]^𝒢_ℰ [Th 10.1.9] = [d_{F(p)}(G)]^𝒢_ℱ [d_p(F)]^ℱ_ℰ [Th 2.2.2(c)] = J_G(F(p)) J_F(p) [(10.1.3)] = [ Σ_l ∂G^i/∂y^l(F(p)) ∂F^l/∂x^j(p) ]. [(10.1.2), (10.1.5)]