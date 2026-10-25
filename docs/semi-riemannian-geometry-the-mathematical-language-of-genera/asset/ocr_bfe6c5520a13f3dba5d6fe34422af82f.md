where the last equality is justified as above.

(b): For all transpositions τ in S_s, it follows from part (a) that τ(Alt(A)) = −Alt(A), so Alt(A) is alternating.

(c): We have

Alt(η) = 1/s! ∑_{σ∈S_s} sgn(σ) σ(η)   [(7.1.4)]
= 1/s! ∑_{σ∈S_s} η   [Th 7.1.2(b)]
= η.   [card(S_s) = s!]

(d): This follows from parts (b) and (c).

(e): We have from part (b) that Alt(T_s^0(V)) ⊆ Λ^s(V). Since Λ^s(V) ⊆ T_s^0(V), by part (c),

Λ^s(V) = Alt(Λ^s(V)) ⊆ Alt(T_s^0(V)).

In view of Theorem 7.1.3(b), we can replace the map in (7.1.4) with
Alt : T_s^0(V) ⟶ Λ^s(V).

# 7.2 Wedge Products

In Section 5.1, we introduced a type of multiplication of tensors called the tensor product. Our next task is to define a corresponding operation for multicovectors.

Let V be a vector space. Wedge product is the family of linear maps
∧ : Λ^s(V) × Λ^{s'}(V) ⟶ Λ^{s+s'}(V)

defined for s, s' ≥ 0 by

η ∧ ζ = (s + s')! / (s! s'!) Alt(η ⊗ ζ)   (7.2.1)

for all multicovectors η in Λ^s(V) and ζ in Λ^{s'}(V). That is,

(η ∧ ζ)(v₁, …, v_{s+s'}) = (s + s')! / (s! s'!) [ 1/(s + s')! ∑_{σ∈S_{s+s'}} sgn(σ) σ(η ⊗ ζ)(v_{σ(1)}, …, v_{σ(s+s')}) ] = 1/(s! s'!) ∑_{σ∈S_{s+s'}} sgn(σ) η(v_{σ(1)}, …, v_{σ(s)}) ζ(v_{σ(s+1)}, …, v_{σ(s+s')})

for all vectors v₁, …, v_{s+s'} in V, where the first equality follows from (7.2.1), and the second equality from (5.1.4) and (7.1.1).