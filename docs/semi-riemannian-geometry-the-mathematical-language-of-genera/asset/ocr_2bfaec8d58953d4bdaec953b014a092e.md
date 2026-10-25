# Open set in a regular surface.

As we now show, an open set in a regular surface is itself a regular surface.

# Theorem 11.4.1 (Open Set). Let M be a regular surface, let V be an open set in M, and view V as a topological subspace of ℝ³. Then:

+   (a) V is a regular surface.
+   (b) For all points p in V, T_p(V) = T_p(M).

Proof. (a): Let p be a point in V, and let (U, φ) be a chart on M at p. We have properties [C1]–[C3] of Section 11.2 at our disposal. According to [C2], φ(U) is open in M, and therefore, so is φ(U) ∩ V. It follows from [C3] and Theorem 9.1.14 that U′ = φ⁻¹(V ∩ φ(U)) is open in U. By Theorem 11.2.10, (U′, φ|U′) is a chart at p such that φ|U′(U′) ⊆ V. Since p was arbitrary, the result follows.
(b): Let v be a vector in T_p(V), and let λ(t) : (a, b) ⟶ V be a smooth curve such that λ(t₀) = p and (dλ/dt)(t₀) = v for some t₀ in (a, b). Since V ⊆ M, the same smooth curve shows that v is also a vector in T_p(M). Thus, T_p(V) ⊆ T_p(M). By Theorem 11.3.1(a), both T_p(V) and T_p(M) are 2-dimensional, and then Theorem 1.1.7(b) gives T_p(V) = T_p(M).

# Throughout, any open set in a regular surface is viewed as a regular surface.

# Graph of a function.

According to Theorem 11.2.11, a regular surface is covered by graphs of functions. We now consider the graph of a function in isolation. Let U be an open set in ℝ², and let f be a function in C^∞(U). Recall from Theorem 11.2.11 that

graph(f) = { (x, y, f(x, y)) ∈ ℝ³ : (x, y) ∈ U },

where we identify ℝ² with the xy-plane in ℝ³. Defining a map φ : U ⟶ ℝ³ by

φ(x, y) = (x, y, f(x, y))

for all (x, y) in U, we see that graph(f) is the image of φ; that is,

graph(f) = φ(U).

# Theorem 11.4.2 (Graph of Function). With the above setup, graph(f), viewed as a topological subspace of ℝ³, is a regular surface, and (U, φ) is a covering chart on graph(f).

Proof. Clearly, φ : U ⟶ ℝ³ is a parametrized surface. We need to prove that:

+   [C1] φ : U ⟶ ℝ³ is an immersion.
+   [C2] φ(U) is an open set in graph(f).
+   [C3] φ : U ⟶ φ(U) is a homeomorphism.

The proofs are as follows: