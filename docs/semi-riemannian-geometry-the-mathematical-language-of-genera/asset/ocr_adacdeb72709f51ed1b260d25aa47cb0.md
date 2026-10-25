Then

ℒ_X(d(x^i)) = d(α^i)

for i = 1, …, m.

Proof. It follows from (16.4.1) that

ℒ_X(x^i) = X(x^i) = (∑_j α^j ∂/∂x^j)(x^i) = ∑_j α^j ∂x^i/∂x^j = α^i,

and then from Theorem 16.4.6 that

ℒ_X(d(x^i)) = d(ℒ_X(x^i)) = d(α^i).

# Theorem 16.4.8 (Local Coordinate Expression for Lie Derivative). Let M be a smooth m-manifold, let X be a vector field in 𝔛(M), and let 𝒜 be a tensor field in T^r_s(M). In local coordinates, let

X = ∑_{i=1}^m α^i ∂/∂x^i,

and let 𝒜^{i_1…i_r}_{j_1…j_s} be the components of 𝒜. Then the components of ℒ_X(𝒜) are

ℒ_X(𝒜)^{i_1…i_r}_{j_1…j_s} = ∑_{k=1}^m α^k ∂𝒜^{i_1…i_r}_{j_1…j_s}/∂x^k − ∑_{l=1}^r (∑_{k=1}^m 𝒜^{i_1…i_{l-1} k i_{l+1}…i_r}_{j_1…j_s} ∂α^{i_l}/∂x^k) + ∑_{l=1}^s (∑_{k=1}^m 𝒜^{i_1…i_r}_{j_1…j_{l-1} k j_{l+1}…j_s} ∂α^k/∂x^{j_l}).

Proof. By Theorem 16.4.4,

ℒ_X(𝒜)^{i_1…i_r}_{j_1…j_s} = ℒ_X(𝒜)(d(x^{i_1}),…,d(x^{i_r}), ∂/∂x^{j_1},…,∂/∂x^{j_s}) = X(𝒜(d(x^{i_1}),…,d(x^{i_r}), ∂/∂x^{j_1},…,∂/∂x^{j_s})) − ∑_{l=1}^r 𝒜(d(x^{i_1}),…,ℒ_X(d(x^{i_l})),…,d(x^{i_r}), ∂/∂x^{j_1},…,∂/∂x^{j_s}) − ∑_{l=1}^s 𝒜(d(x^{i_1}),…,d(x^{i_r}), ∂/∂x^{j_1},…,[X,∂/∂x^{j_l}],…,∂/∂x^{j_s}). (16.4.2)

We seek alternative expressions for the terms in the last three lines of (16.4.2). For the first term,

X(𝒜(d(x^{i_1}),…,d(x^{i_r}), ∂/∂x^{j_1},…,∂/∂x^{j_s})) = X(𝒜^{i_1…i_r}_{j_1…j_s}) = (∑_{k=1}^m α^k ∂/∂x^k)(𝒜^{i_1…i_r}_{j_1…j_s}) = ∑_{k=1}^m α^k ∂𝒜^{i_1…i_r}_{j_1…j_s}/∂x^k. (16.4.3)