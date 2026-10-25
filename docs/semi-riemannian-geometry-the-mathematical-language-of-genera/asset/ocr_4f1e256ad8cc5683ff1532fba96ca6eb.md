By Theorem 12.8.2(a) and part (b),

h₁₂ = ⟨∂H₁/∂y, N ∘ φ⟩ = ε_graph(f) 1 / √|1 − (∂f/∂x)² − (∂f/∂y)²| ⟨(0,0,∂²f/∂x∂y), −(∂f/∂x, ∂f/∂y,1)⟩ = ε_graph(f) ∂²f/∂x∂y / √|1 − (∂f/∂x)² − (∂f/∂y)²|.

(e): By part (c),

det(g_H) = 1 − (∂f/∂x)² − (∂f/∂y)²,

and by part (d),

det(h_H) = [∂²f/∂x² ∂²f/∂y² − (∂²f/∂x∂y)²] / |1 − (∂f/∂x)² − (∂f/∂y)²|.

The result now follows from Theorem 12.8.4 and part (f).

(f): We have

⟨H₁ × H₂, H₁ × H₂⟩ = (∂f/∂x)² + (∂f/∂y)² − 1.

Then Theorem 12.2.11(b) gives

ε_graph(f) = sgn((∂f/∂x)² + (∂f/∂y)² − 1) = −sgn(1 − (∂f/∂x)² − (∂f/∂y)²).

# Theorem 12.10.3 (Surface of Revolution in ℝ₀³). Let ν = 0 and continue with the setup of Theorem 11.4.3. Also, denote derivatives with respect to t by an overdot; let the subscripts 1 and 2 refer to t and φ, respectively; and for brevity, drop t and φ from the notation (except for φ in the case of the trigonometric functions). Then:

(a)

H₁ = (ρ̇ cos(φ), ρ̇ sin(φ), ḣ)    H₂ = (−ρ sin(φ), ρ cos(φ), 0).