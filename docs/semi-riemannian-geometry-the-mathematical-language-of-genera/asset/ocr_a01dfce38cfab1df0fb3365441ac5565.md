Proof. For open balls, this is true by definition. Let \overline{B}_r(x) be a closed ball in X, and let y be a point in X\setminus\overline{B}_r(x), so that d(x,y) > r. Let s = d(x,y) − r, and let z be a point in B_s(y), which means d(y,z) < s. See Figure 9.2.1. From [D2] and [D3],

```
r + s = d(x,y) ≤ d(x,z) + d(z,y) < d(x,z) + s,
```

hence d(x,z) > r, so z is in X\setminus\overline{B}_r(x). Thus, B_s(y) ⊆ X\setminus\overline{B}_r(x). It follows that X\setminus\overline{B}_r(x) is the union of a collection of basis elements of B_d. By Theorem 9.1.3 and Theorem 9.2.2, X\setminus\overline{B}_r(x) is open with respect to the metric topology induced by d, hence \overline{B}_r(x) = X\setminus[X\setminus\overline{B}_r(x)] is closed with respect to that topology.

![](bbox=[0.18, 0.48, 0.82, 0.78])

# Figure 9.2.1. Diagram for Theorem 9.2.3

The next result expresses continuity in metric spaces in terms familiar from the differential calculus of one real variable.

# Theorem 9.2.4 (ε-δ Criterion). Let (X,d) and (Y,e) be metric spaces, each with its induced metric topology, let F : X ⟶ Y be a map, and let x be a point in X. Then F is continuous at x if and only if for every real number ε > 0, there is a real number δ > 0 (possibly depending on ε) such that:

```
d(x,y) < δ    ⇒    e(F(x),F(y)) < ε,
```

or equivalently,

```
y is in B_δ(x)    ⇒    F(y) is in B_ε(F(x)),
```

or equivalently,

```
B_δ(x) ⊆ F^{-1}(B_ε(F(x))).
```

Proof. The equivalence of the three conditions is clear. Let us work with the last one.