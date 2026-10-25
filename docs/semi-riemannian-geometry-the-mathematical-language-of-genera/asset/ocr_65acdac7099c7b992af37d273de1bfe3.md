# Theorem 15.3.1. With the above setup, [X, Y] is a derivation on C^∞(M).

Proof. Since Y is in 𝔛(M) and f is in C^∞(M), by definition, Y(f) is in C^∞(M). Furthermore, since X is in 𝔛(M) and Y(f) is in C^∞(M), by definition, X(Y(f)) is in C^∞(M). Similarly, Y(X(f)) is in C^∞(M). It follows that [X, Y](f) is in C^∞(M). The remainder of the verification is straightforward.

Lie bracket is the map

[·,·] : 𝔛(M) × 𝔛(M) ⟶ 𝔛(M)

defined by the assignment

(X, Y) ⟼ [X, Y] = X ∘ Y − Y ∘ X

for all vector fields X, Y in 𝔛(M).

# Theorem 15.3.2. Let M be a smooth manifold, let X, Y, Z be vector fields in 𝔛(M), and let f, g be functions in C^∞(M). Then:

+   (a) [Y, X] = −[X, Y].
+   (b) [X + Y, Z] = [X, Z] + [Y, Z].
+   (c) [X, Y + Z] = [X, Y] + [X, Z].
+   (d) [fX, gY] = fg[X, Y] + fX(g)Y − gY(f)X.
+   (e) [X, [Y, Z]] + [Y, [Z, X]] + [Z, [X, Y]] = 0. (Jacobi's identity)

Proof. (a)–(d): Straightforward.

(e): We have

[X, [Y, Z]] = X ∘ [Y, Z] − [Y, Z] ∘ X = X ∘ (Y ∘ Z − Z ∘ Y) − (Y ∘ Z − Z ∘ Y) ∘ X = X ∘ Y ∘ Z − X ∘ Z ∘ Y − Y ∘ Z ∘ X + Z ∘ Y ∘ X.

Likewise,

[Y, [Z, X]] = Y ∘ Z ∘ X − Y ∘ X ∘ Z − Z ∘ X ∘ Y + X ∘ Z ∘ Y
[Z, [X, Y]] = Z ∘ X ∘ Y − Z ∘ Y ∘ X − X ∘ Y ∘ Z + Y ∘ X ∘ Z.

Summing the identities gives the result.

It was observed in Section 15.1 that 𝔛(M) is a module over C^∞(M). It follows from Theorem 15.3.2 that 𝔛(M) is also a Lie algebra over ℝ.

# Theorem 15.3.3. Let M be a smooth manifold, let X, Y be vector fields in 𝔛(M), and, in local coordinates, let

X = Σ_i α^i ∂/∂x^i    and    Y = Σ_j β^j ∂/∂x^j.

Then: