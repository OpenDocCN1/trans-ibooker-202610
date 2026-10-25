point. We observe that a chart at an interior point is either an interior chart or a boundary chart, but a chart at a boundary point is necessarily a boundary chart. Since any point p in M is contained in at least one chart, it is either an interior point (with respect to that chart) or a boundary point (with respect to that chart). This raises the question of whether p can be an interior point with respect to one chart, and at the same time a boundary point with respect to another chart. It is a crucial fact that this cannot happen.

# Theorem 17.1.1 (Topological Invariance of Boundary). Each point in a topological manifold with boundary is either an interior point or a boundary point, but not both.

Let M be a topological manifold with boundary, and let p be a point in M. A consequence of Theorem 17.1.1 is that determining whether p is an interior point or a boundary point is independent of the choice of chart at p. The set of interior points of M is denoted by M° and called the (manifold) interior of M, and the set of boundary points of M is denoted by ∂M and called the (manifold) boundary of M. Thus, M is the disjoint union of M° and ∂M.

With the notion of chart in hand, the other core definitions presented in Section 14.1—transition map, tangent space, smooth atlas, and so on—transfer to the present setting virtually word for word. However, the boundary of ℍ^m must be taken into account. To that end, when smoothness is being considered, we adopt the “extended” definition of smoothness introduced in Section 10.1.

A topological manifold with boundary (M,𝔄) is said to be a smooth m-manifold with boundary if 𝔄 is a smooth atlas for M.

# Example 17.1.2 (ℍ^m). The obvious example of a smooth m-manifold with boundary is the topological space ℍ^m paired with the smooth atlas consisting of the single chart (ℍ^m, id_ℍ^m = (r¹,…,r^m)). The interior and boundary of ℍ^m are

(ℍ^m)° = int_ℝ^m(ℍ^m)      and      ∂ℍ^m = bd_ℝ^m(ℍ^m),

respectively. Mimicking the situation with ℝ^m, we refer to (r¹,…,r^m) as standard coordinates on ℍ^m, to (∂/∂r¹,…,∂/∂r^m) and (d(r¹),…,d(r^m)) as the corresponding standard coordinate frame and standard dual coordinate frame of ℍ^m, respectively, and to the orientation of ℍ^m induced by d(r¹)∧⋯∧d(r^m) as the standard orientation.

# Example 17.1.3 (Closed Interval). A closed interval [a,b] in ℝ is a smooth 1-manifold with boundary, where [a,b]° = (a,b) and ∂[a,b] = {a,b}. This example was anticipated in Section 16.8.

Let M be a smooth manifold with boundary, let (U,φ) be a boundary chart on M, and let p be a point in ∂M∩U. By definition, p is a boundary point, so we have from Theorem 17.1.1 that φ(p) is in ∂ℍ^m. Thus, a coordinate map sends boundary points to boundary points. More generally, we have the following result.