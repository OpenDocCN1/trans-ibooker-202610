# Chapter 17

# Smooth Manifolds with Boundary

It is not unusual in practice to encounter what would otherwise be a smooth manifold except for the presence of some type of “boundary”. In this chapter, we introduce smooth manifolds with boundary and prove one of most important results in differential geometry—Stokes’s theorem.

# 17.1 Smooth Manifolds with Boundary

The closed upper half-space of ℝ^m, defined by

ℍ^m = {(x¹, …, x^m) ∈ ℝ^m : x^m ≥ 0}

is the model for what we later call a smooth m-manifold with boundary. It is easily shown that

int_ℝ^m(ℍ^m) = {(x¹, …, x^m) ∈ ℝ^m : x^m > 0}
ext_ℝ^m(ℍ^m) = {(x¹, …, x^m) ∈ ℝ^m : x^m < 0}
bd_ℝ^m(ℍ^m) = {(x¹, …, x^m) ∈ ℝ^m : x^m = 0}.

For example, ℍ³ = {(x, y, z) ∈ ℝ³ : z ≥ 0} is the upper half of ℝ³ including the xy-plane, int_ℝ³(ℍ³) is the upper half of ℝ³ excluding the xy-plane, ext_ℝ³(ℍ³) is the lower half of ℝ³ excluding the xy-plane, and bd_ℝ³(ℍ³) is the xy-plane.

Throughout, ℍ^m is assumed to have the subspace topology induced by ℝ^m.