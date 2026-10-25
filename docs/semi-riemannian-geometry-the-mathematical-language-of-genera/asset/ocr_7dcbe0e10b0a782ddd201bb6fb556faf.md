hence
\widetilde{\Upsilon}_p(cv + w) = \Upsilon(cX + Y)(p) \quad [(15.5.2)]
= c\Upsilon(X)(p) + \Upsilon(Y)(p)
= c\widetilde{\Upsilon}_p(v) + \widetilde{\Upsilon}_p(w), \quad [(15.5.2)]

so \widetilde{\Upsilon}_p is linear. Thus, \widetilde{\Upsilon}_p is a covector in T_p^*(M). Let us define a covector field \widetilde{\Upsilon} on M by the assignment p \longmapsto \widetilde{\Upsilon}_p. We claim that \widetilde{\Upsilon} is smooth; that is, the function \widetilde{\Upsilon}(Z) is smooth for all vector fields Z in \mathfrak{X}(M). From (15.4.1) and (15.5.2),

\widetilde{\Upsilon}(Z)(p) = \widetilde{\Upsilon}_p(Z_p) = \Upsilon(Z)(p)

for all p in M, hence
\widetilde{\Upsilon}(Z) = \Upsilon(Z). \quad (15.5.3)

Since \Upsilon is in Lin_{C^\infty(M)}(\mathfrak{X}(M), C^\infty(M)) and Z is in \mathfrak{X}(M), by definition, \Upsilon(Z) is a smooth function, and therefore, so is \widetilde{\Upsilon}(Z). This proves the claim. It follows from (15.5.1) and (15.5.3) that \mathfrak{C}(\widetilde{\Upsilon})(Z) = \Upsilon(Z). Since Z was arbitrary, \mathfrak{C}(\widetilde{\Upsilon}) = \Upsilon. Thus, \mathfrak{C} is surjective.

It is useful to isolate an aspect of the proof of Theorem 15.5.3. Let \Upsilon be a map in Lin_{C^\infty(M)}(\mathfrak{X}(M), C^\infty(M)). We showed that \mathfrak{C}^{-1}(\Upsilon) is the covector field in \mathfrak{X}^*(M) defined by
\mathfrak{C}^{-1}(\Upsilon)_p(v) = \Upsilon(X)(p) \quad (15.5.4)

for all points p in M and all vectors v in T_p(M), where X is any vector field in \mathfrak{X}(M) such that X_p = v, the existence of which is guaranteed by Theorem 15.1.2.

Now that we have Theorem 15.5.3, we usually (but not always) view \mathfrak{X}^*(M) as the vector space over \mathbb{R} and module over C^\infty(M) consisting of all C^\infty(M)-linear maps from \mathfrak{X}(M) to C^\infty(M). We will see a significant generalization of Theorem 15.5.3 in Section 15.7.

# 15.6 Tensor Fields

In this section, we generalize some of the material in Section 15.4.

Let M be a smooth m-manifold, and let r, s ≥ 0 be integers. An (r, s)-tensor field on M is a map \mathcal{A} that assigns to each point p in M an (r, s)-tensor \mathcal{A}_p in T_s^r(T_p(M)). We also refer to \mathcal{A} as an r-contravariant-s-covariant tensor field or simply a tensor field, and we define the rank of \mathcal{A} to be (r, s). When s = 0, \mathcal{A} is said to be an r-contravariant tensor field or just a contravariant tensor field; and when r = 0, \mathcal{A} is said to be an s-covariant tensor field or simply a covariant tensor field.

Let \omega^1, ..., \omega^r be covector fields in \mathfrak{X}^*(M), let X_1, ..., X_s be vector fields in \mathfrak{X}(M), and consider the function
\mathcal{A}(\omega^1, ..., \omega^r, X_1, ..., X_s) : M \longrightarrow \mathbb{R}