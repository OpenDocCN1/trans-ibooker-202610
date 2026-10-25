**Proof.** Let \(\dim(V)=m\), and let \(\mathcal E\) be an orthonormal basis for \(V\). By Theorem 4.6.12, the matrix \([A]_{\mathcal E}\) is in \(O(m)\), so \(([A]_{\mathcal E})^{\mathsf T}[A]_{\mathcal E}=I_m\). Then

\[
\left([\operatorname{id}_V-A]_{\mathcal E}\right)^{\mathsf T}
=\left(I_m-[A]_{\mathcal E}\right)^{\mathsf T}
=\left(-[A]_{\mathcal E}\right)^{\mathsf T}\left(I_m-[A]_{\mathcal E}\right)
=\left(-[A]_{\mathcal E}\right)\left([\operatorname{id}_V-A]_{\mathcal E}\right),
\]

hence

\[
\det(\operatorname{id}_V-A)=(-1)^m\det(A)\det(\operatorname{id}_V-A)=-\det(\operatorname{id}_V-A),
\]

so \(\det(\operatorname{id}_V-A)=0\). By Theorem 4.7.3, \(\kappa=1\) is an eigenvalue of \(A\).

According to Theorem 4.6.12, a linear isometry on an inner product space “preserves norms”, and as we will see in Section 9.3, this means that it also “preserves distance”. Such a linear isometry is said to produce a “rigid motion” about the origin. Theorem 4.7.10 has an interpretation that fits nicely with these observations. Suppose 1 is an eigenvalue of \(A\), with corresponding eigenvector \(v\). Then \(A(cv)=cv\) for all real numbers \(c\), so that \(A\) fixes the subspace \(\mathbb Rv\). Thus, the action of \(A\) is to rigidly rotate \(V\) about the origin, with \(\mathbb Rv\) as the “axis of rotation”.

# 4.8  Lorentz Vector Spaces

Lorentz vector spaces play a central role in the special theory of relativity and, by extension, in general relativity. The geometry of Lorentz vector spaces is considerably more complicated than that of inner product spaces. As we will see, when translated into physical terms, the implications can be surprising to our Euclidean sensibilities.

Let \((V,g)\) be a nonzero scalar product space. We say that \(g\) is a **Lorentz scalar product**, and that the pair \((V,g)\) is a **Lorentz vector space**, if \(\operatorname{ind}(g)=1\). Based on our convention on signs, the signature of \(g\) is therefore \((+1,\ldots,+1,-1)\). Recall from Section 3.1 that a vector \(v\) in \(V\) is:

+   - **spacelike** if \(v=0\) or \(\langle v,v\rangle>0\).
+   - **timelike** if \(\langle v,v\rangle<0\).
+   - **lightlike** if \(v\ne0\) and \(\langle v,v\rangle=0\).

Thus, by definition, \(e_1,\ldots,e_{m-1}\) are spacelike and \(e_m\) is timelike. The convention on signs adopted here is not the only one appearing in the literature. For instance, some authors take the signature to be \((-1,+1,\ldots,+1)\). The choice of one approach over another can lead to differences in the number and location of negative signs in expressions, but does not affect substance.

Let \(U\) be a nonzero subspace of \(V\). If \(g\) is nondegenerate on \(U\), then according to Theorem 4.2.6,

\[
\operatorname{ind}(g|_U)+\operatorname{ind}(g|_{U^\perp})=1.
\]