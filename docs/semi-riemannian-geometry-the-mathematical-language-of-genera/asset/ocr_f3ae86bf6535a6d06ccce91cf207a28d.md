![](bbox=[0.11, 0.09, 0.88, 0.62])

# Figure 11.2.3. Diagram for Theorem 11.2.8

[C1'] φ|U' : U' → ℝ³ is an immersion.

[C2'] φ|U'(U') is an open set in M.

[C3'] φ|U' : U' → φ|U'(U') is a homeomorphism.

The proofs are as follows:

[C1']: This follows from [C1].

[C2']: Since U' is open in U, it follows from [C3] and Theorem 9.1.14 that φ(U') is open in φ(U). According to [C2], φ(U) is open in M. By Theorem 9.1.5, φ|U'(U') = φ(U') is open in M.

[C3']: Let U'' be a subset of U'. By assumption, U' is open in U, and according to [C3], φ : U → φ(U) is a homeomorphism. We therefore have from Theorem 9.1.14 that U'' is open in U' if and only if φ|U'(U'') = φ(U'') is open in φ|U'(U') = φ(U'). The result now follows from Theorem 9.1.14.

By definition, a regular surface is a patchwork of images of parametrized surfaces. The next result shows that instead of images of parametrized surfaces, we can use graphs of smooth functions.

# Theorem 11.2.11. If M is a regular surface and p is a point in M, then there is a chart (V, ψ) at p and a function f in C^∞(V) such that ψ(V) = graph(f),