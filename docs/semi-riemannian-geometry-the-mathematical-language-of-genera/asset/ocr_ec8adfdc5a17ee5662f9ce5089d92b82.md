On the other hand, by Theorem 10.1.10,

dh/dt(t) = Σ_i r^i ∂g/∂r^i (tr^1, ..., tr^m),

so

∫_0^1 dh/dt(t) dt = Σ_i ( r^i ∫_0^1 ∂g/∂r^i (tr^1, ..., tr^m) ) dt
= Σ_i r^i g_i(r^1, ..., r^m). (14.3.6)

Combining (14.3.5) and (14.3.6) yields

g(r^1, ..., r^m) = g(0, ..., 0) + Σ_i r^i g_i(r^1, ..., r^m). (14.3.7)

By assumption,

(x^1(p), ..., x^m(p)) = φ(p) = (0, ..., 0). (14.3.8)

Let q = φ^{-1}(r^1, ..., r^m), so that

(x^1(q), ..., x^m(q)) = φ(q) = (r^1, ..., r^m). (14.3.9)

From (14.3.7)–(14.3.9), we obtain

g ∘ φ(q) = g ∘ φ(p) + Σ_i x^i(q) (g_i ∘ φ)(q). (14.3.10)

Let f be a function in C^∞(M). Setting g = f ∘ φ^{-1} and f_i = g_i ∘ φ in (14.3.10) gives

f(q) = f(p) + Σ_i x^i(q) f_i(q).

Since (r^1, ..., r^m), hence q, was arbitrary,

f = f(p) + Σ_i x^i f_i.

For a vector v in T_p(M), we have

v(f) = v( f(p) + Σ_i x^i f_i )
= Σ_i v(x^i f_i) [Th 14.3.2(b)]
= Σ_i x^i(p) v(f_i) + Σ_i f_i(p) v(x^i) [(14.3.1)]
= Σ_i f_i(p) v(x^i). [(14.3.8)] (14.3.11)