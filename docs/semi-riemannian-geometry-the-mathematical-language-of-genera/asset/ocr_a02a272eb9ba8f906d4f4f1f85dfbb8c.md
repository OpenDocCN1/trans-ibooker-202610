Proof. We use the algorithm illustrated in Example B.2.1 to compute sgn(σ) based on the number of transpositions required to permute (m − k + 1, …, m; 1, …, m − k) into (1, …, k; k + 1, …, m). It takes m − k transpositions to move m to the position after m − k; that is, permute (m − k + 1, …, m; 1, …, m − k) into (m − k + 1, …, m − 1; 1, …, m − k, m). It then takes m − k transpositions to move m − 1 to the position after m − k; that is, permute (m − k + 1, …, m − 1; 1, …, m − k, m) into (m − k + 1, …, m − 2; 1, …, m − k, m − 1, m). This process continues for a total of k steps. Thus, k(m − k) transpositions give the desired permutation.

Recall from Appendix A the multi-index I = (i₁, …, iₖ) in ℐₖ,ₘ and its complement Iᶜ = (iₖ₊₁, …, iₘ) in ℐₘ₋ₖ,ₘ. Let

(I, Iᶜ) = (i₁, …, iₖ, iₖ₊₁, …, iₘ)    and    (Iᶜ, I) = (iₖ₊₁, …, iₘ, i₁, …, iₖ).

Corresponding to (I, Iᶜ) and (Iᶜ, I), we define permutations in Sₘ by

σ₍I,Iᶜ₎ = ( 1   …   k   k+1   …   m )
            ( i₁  …  iₖ  iₖ₊₁  …  iₘ )

and

σ₍Iᶜ,I₎ = ( 1   …   m−k   m−k+1   …   m )
            ( iₖ₊₁ …  iₘ     i₁      …   iₖ ).

# Theorem B.2.5. With the above setup,

sgn(σ₍I,Iᶜ₎) sgn(σ₍Iᶜ,I₎) = (−1)ᵏ⁽ᵐ⁻ᵏ⁾.

Proof. We have

(σ₍Iᶜ,I₎)⁻¹ = ( i₁   …   iₖ   iₖ₊₁   …   iₘ )
                  ( m−k+1 …  m    1     …   m−k ),

hence

(σ₍Iᶜ,I₎)⁻¹ σ₍I,Iᶜ₎ = ( 1   …   k   k+1   …   m )
                        ( m−k+1 …  m    1     …   m−k ).

The result now follows from Theorem B.2.2 and Theorem B.2.4.

# Example B.2.6. Let m = 6 and k = 3, and let I = (2, 4, 6). Then Iᶜ = (1, 3, 5), and

(I, Iᶜ) = (2, 4, 6, 1, 3, 5)    and    (Iᶜ, I) = (1, 3, 5, 2, 4, 6),

hence

σ₍I,Iᶜ₎ = ( 1 2 3 4 5 6 )
          ( 2 4 6 1 3 5 )    and    σ₍Iᶜ,I₎ = ( 1 2 3 4 5 6 )
                                                   ( 1 3 5 2 4 6 ).

In cycle notation,

σ₍I,Iᶜ₎ = (1 2 4)(3 6 5) = (1 2)(2 4)(3 6)(6 5)