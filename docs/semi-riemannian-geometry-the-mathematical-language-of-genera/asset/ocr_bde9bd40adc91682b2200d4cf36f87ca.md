and

C_n^k ∘ F_n^{k+1}(𝒜)^{i_1...î_k...i_{r-1}}_{j_1...ĵ_n...j_{s+1}} = Σ_p F_n^{k+1}(𝒜)^{i_1...i_{k-1} p i_{k+1}...i_{r-1}}_{j_1...j_n-1 p j_{n+1}...j_{s+1}} [Th 5.4.5] = Σ_p ( Σ_q 𝔤_{pq} 𝒜^{i_1...i_{k-1} p q i_{k+1}...i_{r-1}}_{j_1...j_n...j_{s+1}} ) [(6.2.3)] = Σ_{pq} 𝔤_{pq} 𝒜^{i_1...i_{k-1} p q i_{k+1}...i_{r-1}}_{j_1...j_n...j_{s+1}},

which gives the first equality. Relabeling indices (i_{k+1},...,i_{r-1}) as (i_k,...,i_{r-2}), and (j_{n+1},...,j_{s+1}) as (j_m,...,j_s), gives the second equality.

We observe that p and q in (6.2.1) are in the k th and l th contravariant positions of 𝒜, respectively, and that p and q in (6.2.2) are in the k th and (k+1)th contravariant positions of 𝒜, respectively. We also note that the right-hand side of (6.2.1) is independent of n. This makes sense because F_n^l lowers the l th contravariant index to the n th covariant position, and then C_n^k contracts over the k th contravariant index and the n th covariant index. In a manner of speaking, the n th covariant position is simply a temporary location for the lowered l th contravariant index to reside prior to its being contracted with the k th contravariant index.

# Theorem 6.2.6. Let (V,𝔤) be a scalar product space, let 𝒜 be a tensor in 𝒯_s^r(V), and let 1 ≤ k ≤ r be an integer. Then

F_1^k(𝒜) = C_1^k(𝔤 ⊗ 𝒜).

Proof. All the components to follow are computed with respect to a basis ℋ for V. By Theorem 6.2.4, the tensor F_1^k(𝒜) in 𝒯_{s+1}^{r-1}(V) has the components

F_1^k(𝒜)^{i_1...i_{r-1}}_{j_1...j_{s+1}} = Σ_p 𝔤_{j_1 p} 𝒜^{i_1...i_{k-1} p i_k...i_{r-1}}_{j_2...j_{s+1}}. (6.2.4)

It follows from Theorem 5.1.5 that the tensor 𝔤 ⊗ 𝒜 in 𝒯_{s+2}^r(V) has the components

(𝔤 ⊗ 𝒜)^{i_1...i_r}_{j_1...j_{s+2}} = 𝔤_{j_1 j_2} 𝒜^{i_1...i_r}_{j_3...j_{s+1}},

and then from Theorem 5.4.5 that the tensor C_1^k(𝔤 ⊗ 𝒜) in 𝒯_{s+1}^{r-1}(V) has the components

C_1^k(𝔤 ⊗ 𝒜)^{i_1...î_k...i_r}_{j_2...j_{s+2}} = Σ_p 𝔤_{p j_2} 𝒜^{i_1...i_{k-1} p i_{k+1}...i_r}_{j_3...j_{s+2}}.

Relabeling (i_{k+1},...,i_r) as (i_k,...,i_{r-1}), and (j_2,...,j_{s+2}) as (j_1,...,j_{s+1}), gives

C_1^k(𝔤 ⊗ 𝒜)^{i_1...i_{r-1}}_{j_1...j_{s+1}} = Σ_p 𝔤_{p j_1} 𝒜^{i_1...i_{k-1} p i_k...i_{r-1}}_{j_2...j_{s+1}}. (6.2.5)

The result now follows from (6.2.4) and (6.2.5).