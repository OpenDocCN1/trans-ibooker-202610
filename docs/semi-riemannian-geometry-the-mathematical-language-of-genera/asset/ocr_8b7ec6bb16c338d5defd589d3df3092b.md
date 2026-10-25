Let (\widetilde{U}, \widetilde{\varphi}) be another chart in \mathfrak{A} at p, and let \widetilde{\mathcal{H}} be the corresponding coordinate frame. Since \mathfrak{A} is consistent, (U, \varphi) and (\widetilde{U}, \widetilde{\varphi}) are consistent, hence [\mathcal{H}_{\varphi^{-1}(p)}] = [\widetilde{\mathcal{H}}_{\widetilde{\varphi}^{-1}(p)}]. This shows that the definition of \mathfrak{O}(p) is independent of the choice of representative chart at p. We call the set of equivalence classes

\mathfrak{O} = \{\mathfrak{O}(p) : p \in M\}

the orientation induced by \mathfrak{A} and say that M is oriented by \mathfrak{A}. The notation (M, \mathfrak{g}, \mathfrak{O}), and sometimes (M, \mathfrak{g}, \mathfrak{A}, \mathfrak{O}), is used as an alternative to (M, \mathfrak{g}, \mathfrak{A}).

Consider the map \iota : \mathbb{R}^2 \longrightarrow \mathbb{R}^2 given by \iota(r^1, r^2) = (-r^1, r^2). Since \iota is a diffeomorphism and \iota^{-1} = \iota, (\iota(U), \varphi \circ \iota) is a chart on M, where, for brevity, we denote \iota|_{\iota(U)} by \iota. Because

\varphi \circ \iota(r^1, r^2) = (\varphi^1(-r^1, r^2), \varphi^2(-r^1, r^2), \varphi^3(-r^1, r^2)),

the corresponding coordinate frame and coordinate unit normal vector field are

-\mathcal{H} = (-H_1, H_2)

and -G. It is easily shown using Theorem 11.3.3 that

-\mathfrak{A} = \{(\iota(U), \varphi \circ \iota) : (U, \varphi) \in \mathfrak{A}\}

is a consistent atlas for M. The orientation of M induced by -\mathfrak{A} is

-\mathfrak{O} = \{-\mathfrak{O}(p) : p \in M\},

where

-\mathfrak{O}(p) = [-\mathcal{H}_{\varphi^{-1}(p)}] = [(-H_1|_{\varphi^{-1}(p)}, H_2|_{\varphi^{-1}(p)})].

We say that the orientation -\mathfrak{O} is the opposite of \mathfrak{O}.

# Theorem 12.7.3. If (M, \mathfrak{g}, \mathfrak{A}) is an oriented regular surface in \mathbb{R}^3_v, then:

+   (a) There is a unit normal vector field \mathcal{N} in \mathfrak{X}_{\mathbb{R}^3_v}(M), called the Gauss map, such that for every chart (U, \varphi) in \mathfrak{A}, \mathcal{N} \circ \varphi is the corresponding coordinate unit normal vector field.
+   (b) \mathcal{N} satisfies properties [V1] and [V2] of Section 12.2.

Remark. Calling \mathcal{N} a "map" is reasonable because \mathfrak{X}_{\mathbb{R}^3_v}(M) is, by definition, a set of maps.

Proof. (a): Since \mathfrak{A} is a consistent atlas for M, it follows from Theorem 12.7.2 that whenever two charts in \mathfrak{A} overlap, the corresponding coordinate unit normal vector fields agree on the overlap. We can therefore assign to each point p in M a vector \mathcal{N}_p that is unit normal at p, and do so in such a way that \mathcal{N}_p is independent of the choice of chart at p selected from \mathfrak{A}. More specifically, given the chart (U, \varphi) in \mathfrak{A} and the corresponding coordinate unit normal vector field G, we define \mathcal{N}_p = G_{\varphi^{-1}(p)} for all p in \varphi(U). Since G is smooth on U, it follows