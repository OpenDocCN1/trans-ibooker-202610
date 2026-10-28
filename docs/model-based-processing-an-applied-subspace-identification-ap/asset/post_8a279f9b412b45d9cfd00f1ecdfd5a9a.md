
+   - Compute the LQ-decomposition of the augmented data matrix \(\mathcal{D}\) of Eq. (7.115).
+   - Solve for the unknown \(\mathcal{L}\)-matrices of Eqs. (7.116) and (7.117) to compute the projections.
+   - Perform the SVD of the projections: \(\mathcal{P}^{\parallel}_{\mathcal{Y}_f|\mathcal{D}_p}\mathcal{U}_f\) in Eq. (7.113) to extract \(\mathcal{O}_k\) and \(\mathcal{O}_{k-1}\).
+   - Compute \(\hat{x}_{k+1}\) and \(\hat{x}_{k+1}^{\wedge}\) of Eq. (7.120).
+   - Construct the “balanced” state-space system of Eq. (7.125).
+   - Extract \(A, B, C, D\) by solving the least-squares problem as in Eq. (7.126).
+   - Estimate \(R_{\omega\omega}^{e}, R_{\omega\omega}^{e}, R_{\nu\nu}^{e}\) from the residual (INV) sequence as in Eq. (7.128).
+   - Compute the residual (INV) covariance and Kalman gain from Eq. (7.129).

completing the oblique projection solution to the stochastic realization problem.

We summarize the N4SID algorithm in Table 7.7. Next we consider the relationship between these algorithms.

## 7.6.2 Relationship: Oblique (N4SID) and Orthogonal (MOESP) Algorithms

The relationship of the oblique-projection based N4SID method to that of the orthogonal projection-based PO-MOESP method is well established [36]. Essentially, both techniques use identical instrumental variables \(\mathcal{I}_p\) as shown in Eq. (7.77); therefore, there exists a well-defined relationship⁹ between them. This relationship can be seen by examining projections of each, that is, the relations between orthogonal and oblique projection operators.

The *oblique projection operator* \(\mathcal{P}_{\mathrm{oblique}}\) can be expressed in terms of the common instrumental variable \(\mathcal{I}_p\) of past inputs and outputs as [17, 37, 38]

\(\mathcal{P}_{\mathrm{oblique}} = \mathcal{P}_{\mathcal{U}_f}^{\perp}\mathcal{I}_p'(\mathcal{I}_p\mathcal{P}_{\mathcal{U}_f}^{\perp}\mathcal{I}_p')^{-1}\mathcal{I}_p\mathcal{P}_{\mathcal{U}_f}^{\perp}\)  (7.130)

which is applied directly to the future data \(\mathcal{Y}_f\), while the *orthogonal projection operator* (\(\mathcal{P}_{\mathrm{orthogonal}}\)) is given by

\(\mathcal{P}_{\mathrm{orthogonal}} = \mathcal{P}_{\mathcal{U}_f}^{\perp}\mathcal{I}_p'(\mathcal{I}_p\mathcal{P}_{\mathcal{U}_f}^{\perp}\mathcal{I}_p')^{-1}\mathcal{I}_p\mathcal{P}_{\mathcal{U}_f}^{\perp} \times \mathcal{P}_{\mathcal{U}_f}^{\perp}\)  (7.131)

Therefore, we see that

\(\mathcal{P}_{\mathrm{orthogonal}} = \mathcal{P}_{\mathrm{oblique}} \times \mathcal{P}_{\mathcal{U}_f}^{\perp}\)  (7.132)

as shown in [17, 36].

⁹ The *canonical variate analysis* (CVA) method is also included, but it is beyond the scope of our discussion [36]. Details are available [7, 25].

## Table 7.7 Numerical algorithm for (stochastic) subspace state-space system identification (N4SID).

### Oblique projection (LQ-decomposition)

D_{0|2k-1} = [U_{0|k-1}; U_{k|k}; U_{k+1|2k-1}; Y_{0|k-1}; Y_{k|k}; Y_{k+1|2k-1}] = [L_{11} 0 0 0 0 0; L_{21} L_{22} 0 0 0 0; L_{31} L_{32} L_{33} 0 0 0; L_{41} L_{42} L_{43} L_{44} 0 0; L_{51} L_{52} L_{53} L_{54} L_{55} 0; L_{61} L_{62} L_{63} L_{64} L_{65} L_{66}] × [Q'_1; Q'_2; Q'_3; Q'_4; Q'_5; Q'_6] (Data Matrix)

### L-Matrix solutions

{ L_{U0|k-1}, L_{U|k2k}, L_{Y0|k-1} }; { L_{U0|k}, L_{U|k+1|2k-1}, L_{Y0|k} } (L-Matrices)

### Singular-value decomposition

P^||_{Y_{k|2k-1}|D_{0|k-1}} o U_{k|2k-1} = [U_S | U_N] [Σ_S 0; - - ; 0 Σ_N] Q'_{1:4} × [V'_S; --; V'_N] ≈ U_S Σ_S V'_S (Projection Matrix)

### Observability/state extractions

P^||_{Y_{k|2k-1}|D_{0|k-1}} o U_{k|2k-1} = O_k X_k = [L_{U0|k-1} | 0 | L_{Y0|k-1}] L_{1:4,1:4} Q'_{1:4} (Oblique Projections)

P^||_{Y_{k+1|2k}|D_{0|k}} o U_{k+1|2k} = O_{k-1} X_{k+1} = [L_{U0|k} | 0 | L_{Y0|k}] L_{1:5,1:5} Q'_{1:5}

O_k = U_S Σ_S^{1/2}; O_{k-1} = U_S Σ_{S_k-1}^{1/2}; (Observability Matrices)

X̂_k = O_k^† × P^||_{Y_{k|2k-1}|D_{0|k-1}} o U_f = (Σ_{S_k}^{-1/2} U_S^†) × P^||_{Y_{k|2k-1}|D_{0|k-1}} o U_f (State Estimates)

X̂_{k+1} = O_{k-1}^† × P^||_{Y_{k+1|2k}|D_{0|k}} o U_f = (Σ_{S_k-1}^{-1/2} U_{S_k-1}^†) × P^||_{Y_{k+1|2k}|D_{0|k}} o U_f

### System matrices

[Â | B̂; Ĉ | D̂] = ([X̂_{k+1}; Y_{k|k}] [X̂_k; U_{k|k}])' ([X̂_k; U_{k|k}] [X̂_k; U_{k|k}])^{-1}

### Covariance equations

R̂_{ξξ} = E{ξ̂ ξ̂'} = [R_{ωω}^e | R_{ωv}^e; R_{ωv}^{e'} | R_{vv}^e] = [K R_{ee} K' | K R_{ee}^{1/2}; (K R_{ee}^{1/2})' | R_{ee}]

### Kalman filter equations

R̂_{ee} ≈ R_{vv}^e; K̂ ≈ R_{ωv}^e R̂_{ee}^{-1/2} (Innovations Covariance/ Kalman Gain)

In the literature, this relationship has also been developed in terms of the full-rank (left) weighting matrix W_L and (right) column matrix W_R such that the rank of the projection matrix P is preserved, that is, ρ(I_p × W_R) = ρ(I_p) = N_x

ρ(I_p × W_R) = ρ(I_p) = N_x

We start with the order estimation problem and then proceed to validate the performance of the identified models.

## 7.7.1 Order Estimation: Stochastic Realization Problem

Order estimation for MIMO systems has evolved specifically from the identification problem [40] where the first step is to estimate or guess the system order based on physical principles and then attempt to validate prediction error or INV through residual testing of the estimated model. Much effort has been devoted to this problem [40–43] with the most significant statistical results evolving from the information theoretical point of view [39, 44, 45]. We consider some equivalent order estimation methods based on the SVD method applied to data, covariance, or projection matrices. The methods can be statistical in nature following the AIC and MDL (BIC) approaches or deterministic based on the Hankel singular values or the best rank approximation [39, 44, 46] as before. These approaches are convenient for both classical and SID solutions to the stochastic realization problem.

We know from “theoretical” systems (realization) theory that the key element is determining the order or number of states of the underlying system under investigation. For the stochastic problem, order estimation is again based on the rank condition that evolves from the Hankel covariance data matrices (classical) or the projection matrices (subspace) with their observability/controllability-like factors. However, the root of achieving a robust and reliable estimate still lies in the SVD of the covariance data/projection matrices. Classically, we have

Λ_{K,K} = [ Λ₁ ⋯ Λ_K ; ⋮ ⋮ ⋮ ; Λ_K ⋯ Λ_{2K-1} ] = [U_S | U_N] {U} [ Σ_S | 0 ; - - - ; 0 | Σ_N ] {Σ} [ V_S' ; -- ; V_N' ] {V} = U_S Σ_S V_S' + U_N Σ_N V_N' Signal Noise (7.134)

where the covariance sequence is characterized by the Markov parameters Λ_ℓ = C A^{ℓ-1} (A Π C' + R_{wv}) with Π ≥ 0 of the unknown Gauss–Markov or INV models.

Ideally, Σ_N ≪ Σ_S and the number of nonzero singular values residing in Σ_S reveal the order or number of states of the underlying system; however, in contrast to the deterministic case, this is no longer true for noisy data. Although, in theory, it is a simple matter to perform the SVD of Λ_{K,K} and count the number of nonzero singular values to obtain N_x, since ρ(Λ_{K,K}) = ρ(Σ) = ρ(Σ_S) = N_x. Therefore, the key first step in SID lies in the extraction of the singular values either employing the classical Hankel covariance matrix of the SVD-based

stochastic realization algorithm of Section 7.4 or the SVD-based projection matrices of the subspace algorithms of Section 7.5 or 7.6.

Let us consider classical properties of the Hankel covariance matrix that enable a solution to the order estimation problem. This set of covariance matrix singular values allows us to estimate its rank and therefore the minimal number of states of the underlying system. However, when noise or computational error occurs, then it is necessary to apply judgment when evaluating these singular values. One approach is simply to search for a meaningful change in these values, referred to as a “gap” or “jump,” selecting the order corresponding to this abrupt change as illustrated in Figures 6.1 and 6.2 for a 4th-order or 6th-order system, respectively.

As before, another approach is to employ best rank approximation given by the ratio of the summation of singular values up to the selected order over all the values, that is, β(%) based on the ratio of singular values [47]:

β(Ňₓ) := ( Σₖ=1^Ňₓ σₖ / Σₖ=1^K σₖ ) × 100  (7.135)

The model order estimator (MOE) is given by [48]

Ňₓ = max { N : log σ̂_N > (log σ̂_max + log σ̂_min) }  (7.136)

and is specified as the largest integer corresponding to the singular value (σ_Nₓ) greater than the geometric mean of the largest and smallest singular values extracted. Here the idea is to formally search for the gap or jump in singular values.

Again with the order is selected, then the error in this choice can be specified by the Ňₓ+1 singular value σ_Nₓ+1 as a particular metric or the so-called truncation or SVD model error metric defined by the summation over all of the remaining singular values as [49, 50]

ℳₑ := 2 Σₖ=Ňₓ+1^K σₖ  (7.137)

Also recall that the Hankel norm is a measure of the energy stored in a system and is specified by the “largest” singular value of the Hankel matrix:

‖ ℋ_K,K ‖ = max{σ_k} = σ₁;  k = 1, …, K

while the ‖ ℋ_K,K ‖₂ norm is the RMS sum of the impulse responses or covariance sequences [50]. In any case, it is clear that the Hankel data/covariance or projection matrices are critical entities in identifying a system from measured data.

Most of the statistical validation techniques to follow are primarily founded on an “error-based” metric where the error was defined in the previous chapter
