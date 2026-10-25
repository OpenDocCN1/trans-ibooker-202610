The remaining results of this section give the basic properties of differential maps.

# Theorem 14.4.3. Let M be a smooth manifold, let id_M : M ⟶ M be the identity map, and let p be a point in M. Then

d_p(id_M) = id_{T_p(M)}.

Proof. For a vector v in T_p(M) and a function f in C^∞(M), we have from (14.4.1) that

d_p(id_M)(v)(f) = v(f ∘ id_M) = v(f).

Since v and f were arbitrary, the result follows.

# Theorem 14.4.4. Let V and W be vector spaces, let A : V ⟶ W be a linear map, and let v be a vector in V. Suppose V and W have standard smooth structures, and make the identifications T_v(V) = V and T_{A(v)}(W) = W. Then

d_v(A) = A.

Proof. Straightforward.

# Theorem 14.4.5 (Chain Rule). Let M, N, and P be smooth manifolds, let F : M ⟶ N and G : N ⟶ P be smooth maps, and let p be a point in M. Then

d_p(G ∘ F) = d_{F(p)}(G) ∘ d_p(F).

Proof. For a vector v in T_p(M) and a function f in C^∞(P), we have

d_p(G ∘ F)(v)(f) = v(f ∘ G ∘ F) [(14.4.1)] = d_p(F)(v)(f ∘ G) [(14.4.1)] = d_{F(p)}(G)(d_p(F)(v)(f)) [(14.4.1)] = (d_{F(p)}(G) ∘ d_p(F))(v)(f).

Since v and f were arbitrary, the result follows.

The next result is a generalization of Theorem 14.3.5.

# Theorem 14.4.6 (Matrix of Differential Map). Let M be a smooth m-manifold, let N be a smooth n-manifold, and let F : M ⟶ N be a smooth map. Let p be a point in M, let (U, φ = (x^i)) and (V, (y^j)) be charts at p and F(p), respectively, and let X_p = ((∂/∂x^1)|_p, …, (∂/∂x^m)|_p) and Y_{F(p)} = ((∂/∂y^1)|_{F(p)}, …, (∂/∂y^n)|_{F(p)}) be the corresponding coordinate bases at p and F(p), respectively. Then:

(a) d_p(F)(∂/∂x^j|_p) = Σ_i ∂(y^i ∘ F)/∂x^j (p) ∂/∂y^i|_{F(p)} = Σ_i ∂(y^i ∘ F ∘ φ^{-1})/∂x^j (φ(p)) ∂/∂y^i|_{F(p)}.