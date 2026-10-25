Since [[v_j]_E  [v_2]_E ... [v_{m-1}]_E] has [v_j]_E in (at least) two positions, it follows from Theorem 2.4.2 that det([[v_j]_E [v_2]_E ... [v_{m-1}]_E]^{(i)^c}) = 0 for j = 2, ..., m - 1. Thus,

det([[v_1]_E [v_2]_E ... [v_{m-1}]_E]^{(i)^c}) = 0

for i = 1, ..., m. It follows from (8.4.1) that v_1 × ... × v_{m-1} = 0.

(⇐): Since the dimension of span({v_1, ..., v_{m-1}}) is at most m - 1, there is a vector v_0 in V \ span({v_1, ..., v_{m-1}}). Then

+   <v_0, v_1 × ... × v_{m-1}> = 0
+   ⇔ det([[v_0]_E [v_1]_E ... [v_{m-1}]_E]) = 0 [Th 8.4.5(a)]
+   ⇔ [v_0]_E, [v_1]_E, ..., [v_0]_E are linearly dependent [Th 2.4.9]
+   ⇔ v_0, v_1, ..., v_{m-1} are linearly dependent [Th 2.2.1]
+   ⇔ v_1, ..., v_{m-1} are linearly dependent. [Th 1.1.2]

Thus, if v_1 × ... × v_{m-1} = 0, then v_1, ..., v_{m-1} are linearly dependent.

Whether vectors in a vector space are linearly independent or linearly dependent is unrelated to the presence or absence of a scalar product. This means that in Theorem 8.4.8, linear independence can be checked using any convenient choice of scalar product and orthonormal basis.

# Theorem 8.4.9. Let (V, g) be a scalar product space of dimension m ≥ 2, let E be an orthonormal basis for V, let U be a subspace of V of dimension m - 1, and let (u_1, ..., u_{m-1}) be a basis for U. Then g is nondegenerate on U if and only if ||u_1 × ... × u_{m-1}|| ≠ 0.

Proof. The matrix of g|_U with respect to (u_1, ..., u_{m-1}) is [<u_i, u_j>], so

+   g is nondegenerate on U
+   ⇔ g|_U is nondegenerate on U [by definition]
+   ⇔ det([<u_i, u_j>]) ≠ 0 [Th 3.3.3]
+   ⇔ <u_1 × ... × u_{m-1}, u_1 × ... × u_{m-1}> ≠ 0 [Th 8.4.6(a)]
+   ⇔ ||u_1 × ... × u_{m-1}|| ≠ 0.

# Theorem 8.4.10. Let (V, g) be a scalar product space of dimension m ≥ 2, and let E be an orthonormal basis for V. Let U be a subspace of V of dimension m - 1 on which g is nondegenerate, let (u_1, ..., u_{m-1}) be a basis for U, and let ε be the sign of <u_1 × ... × u_{m-1}, u_1 × ... × u_{m-1}>, so that

ε = < u_1 × ... × u_{m-1} / ||u_1 × ... × u_{m-1}|| , u_1 × ... × u_{m-1} / ||u_1 × ... × u_{m-1}|| > . (8.4.5)

Then: