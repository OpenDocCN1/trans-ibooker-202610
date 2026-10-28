closely along with the estimated channel outputs overlaid onto the raw impulse response data – again indicating an accurate realization due to the high SNR.

In Figure 6.5b, we compare the estimated average power spectra from the SVD realization of the model to the three-output spectra and show the location of the extracted modal frequencies. Note how all of the peaks align indicating a valid realization of the underlying mechanical system. The pole–zero representation along with the extracted modal frequencies are shown in Figure 6.5c. This completes the example.

Next we consider another approach of estimating realizations in unique structural descriptions that have evolved from algebraic group theory – canonical forms.

# 6.3.3 Canonical Realization

In this section, we consider solving the realization problem in canonical coordinates of the state-space. Here, specific “invariant” entities will be extracted directly from the Hankel matrix impulse response data enabling considerable insights into the inherent structure of the identified system. There are advantages in this approach, since the minimal number of parameters required are utilized to represent a complex system in this state-space form. Canonical realizations characterize a unique representation of an unknown system. However, the disadvantage is that the error in realizing the model in this form can represent an entirely different system than the true system due to its uniqueness.

# 6.3.3.1 Invariant System Descriptions

A unique parametric representation of a system Σ_ABCD is called a canonical form [24]. Formally, this characterization evolves from algebraic module theory through the action of a transformation group (similarity transformation) acting on an equivalence class (transfer function/Markov sequence) [2]. The result of this action is a complete set of independent “invariants” that uniquely characterize the system under investigation and form the essential entities that represent that system. These invariants are the essentially critical parameters of the structure of a system revealing its internal interconnections and couplings at the subsystem level. From these invariants, we can construct a “canonical form” [18, 25] that uniquely represents Σ_ABCD revealing its internal structure.

For an observable pair (A, C), define ν_j as the jth observability index. There exists exactly one set of ordered scalars β_jkℓ such that

c'_j A^{ν_j} = Σ_{k=1}^{j-1} Σ_{ℓ=0}^{min(ν_j, ν_k)-1} β_jkℓ c'_k A^{ℓ} + Σ_{k=j}^{N_y} Σ_{ℓ=0}^{min(ν_j, ν_k)-1} β_jkℓ c'_k A^{ℓ}  (6.37)

where the set [{ν_j}, {β_jkℓ}, ]; j, k ∈ N_y, ℓ = 0, …, ν_j − 1 are defined as the observability invariants for the observable pair (A, C).