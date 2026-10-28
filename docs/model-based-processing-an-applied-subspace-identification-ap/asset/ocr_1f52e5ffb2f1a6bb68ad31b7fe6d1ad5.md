Applying the projection operator to the stochastic data equation of Eq. (7.69) nullifies the input term, that is,

𝒫_{𝒰_{k|2k-1}^⊥}^{⊥} 𝒴_{k|2k-1} 𝒰_{k|2k-1}^† := 𝒴_{k|2k-1} × 𝒫_{𝒰_{k|2k-1}^⊥} = 𝒪_k \hat{𝒳}_k 𝒫_{𝒰_{k|2k-1}^⊥} + \underbrace{Γ_k 𝒰_{k|2k-1} 𝒫_{𝒰_{k|2k-1}^⊥}}_{0} + Ξ_k ℰ_{k|2k-1} 𝒫_{𝒰_{k|2k-1}^⊥} (7.74)

The INV sequence is unaffected by the projection operator 𝒫_{𝒰_{k|2k-1}^⊥} because it is orthogonal to the inputs, past and future, therefore ℰ_{k|2k-1} 𝒫_{𝒰_{k|2k-1}^⊥} = ℰ_{k|2k-1} × (I - 𝒰_{k|2k-1} (𝒰_{k|2k-1} 𝒰_{k|2k-1}^†)^{-1} 𝒰_{k|2k-1}) = ℰ_{k|2k-1}

That is, from the well-known properties of the INV, we have that the future INV sequence is uncorrelated with the past inputs [32] ℰ_{k|2k-1} 𝒰_{0|k-1} = 0

Therefore, postmultiplying Eq. (7.74) by 𝒰_{0|k-1} and using this property, we can extract the observability matrix 𝒴_{k|2k-1} 𝒫_{𝒰_{k|2k-1}^⊥} 𝒰_{0|k-1} = 𝒪_k \hat{𝒳}_k 𝒫_{𝒰_{k|2k-1}^⊥} 𝒰_{0|k-1} + \underbrace{Ξ_k ℰ_{k|2k-1} 𝒰_{0|k-1}}_{0} = 𝒪_k \hat{𝒳}_k 𝒫_{𝒰_{k|2k-1}^⊥} 𝒰_{0|k-1} (7.75)

It has been shown that the corresponding rank condition for the stochastic realization problem is satisfied as (see [19] for details) ρ(𝒳_k 𝒫_{𝒰_{k|2k-1}^⊥} 𝒰_{k|2k-1}) = N_x (7.76)

If the underlying stochastic representation incorporates process noise such as the INV and Gauss–Markov models, then the SID technique must include both “past” inputs and outputs leading to either the PO-MOESP or the N4SID approach [17, 19, 27]. These methods provide robust solutions to the stochastic realization problem in this case.

The “past” input PI-MOESP method of the previous chapter has been developed from an instrumental variable (IV) perspective in which the IV is defined as ℐ_p := 𝒰_{0|k-1}; however, in contrast, both PO-MOESP and N4SID techniques are based on an augmented IV that incorporates the past outputs as well, that is, ℐ_{0|k-1} := [ 𝒰_{0|k-1} ; ; 𝒴_{0|k-1} ] ∈ ℛ^{k(N_u+N_y)×K} (7.77)

Recall that instrumental variables are selected to remove the effects of the noise term, leaving the critical information intact. IVs must satisfy the following conditions: