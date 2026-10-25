
## 19.7 Isometries

where it is assumed that $r\psi(s)$ is in $\mathcal{E}_p$ for all $(r,s)$ in $[0,1]\times(-\varepsilon,\varepsilon)$. Then, for all $(r,s)$ in $[0,1]\times(-\varepsilon,\varepsilon)$:

-   (a)

$$\left\langle \frac{\partial\sigma}{\partial r}(r,s),\frac{\partial\sigma}{\partial r}(r,s)\right\rangle=\langle\psi(s),\psi(s)\rangle.$$

-   (b)

$$\left\langle\frac{\partial\sigma}{\partial r}(r,s),\frac{\partial\sigma}{\partial s}(r,s)\right\rangle=r\left\langle\frac{d\psi}{ds}(s),\psi(s)\right\rangle.$$

**Proof.** (a): Recall from Section 14.9 that for a given point $s$ in $(-\varepsilon,\varepsilon)$, the longitude curve $\sigma_s(r):[0,1]\longrightarrow M$ is defined by $\sigma_s(r)=\sigma(r,s)$. By Theorem 18.11.4(a), $\sigma_s$ is a geodesic with initial velocity $\psi(s)$; that is, $(d\sigma_s/dr)(0)=\psi(s)$. It follows from Theorem 19.9.1(a) that the function $\langle d\sigma_s/dr,d\sigma_s/dr\rangle:[0,1]\longrightarrow M$ is constant, hence

$$\left\langle\frac{\partial\sigma}{\partial r}(r,s),\frac{\partial\sigma}{\partial r}(r,s)\right\rangle=\left\langle\frac{d\sigma_s}{dr}(r),\frac{d\sigma_s}{dr}(r)\right\rangle=\left\langle\frac{d\sigma_s}{dr}(0),\frac{d\sigma_s}{dr}(0)\right\rangle=\langle\psi(s),\psi(s)\rangle.$$

(b): By Theorem 19.3.1,

$$\frac{\partial}{\partial r}\left(\left\langle\frac{\partial\sigma}{\partial r},\frac{\partial\sigma}{\partial s}\right\rangle\right)=\left\langle\nabla_{\frac{\partial}{\partial r}}\left(\frac{\partial\sigma}{\partial r}\right),\frac{\partial\sigma}{\partial s}\right\rangle+\left\langle\frac{\partial\sigma}{\partial r},\nabla_{\frac{\partial}{\partial r}}\left(\frac{\partial\sigma}{\partial s}\right)\right\rangle.$$

Since $\sigma_s$ is a geodesic,

$$\nabla_{\frac{\partial}{\partial r}}\left(\frac{\partial\sigma}{\partial r}\right)=0,$$

and according to Theorem 18.6.4,

$$\left\langle\frac{\partial\sigma}{\partial r},\nabla_{\frac{\partial}{\partial r}}\left(\frac{\partial\sigma}{\partial s}\right)\right\rangle=\left\langle\frac{\partial\sigma}{\partial r},\nabla_{\frac{\partial}{\partial s}}\left(\frac{\partial\sigma}{\partial r}\right)\right\rangle.$$

Combining the preceding three identities gives

$$\frac{\partial}{\partial r}\left(\left\langle\frac{\partial\sigma}{\partial r},\frac{\partial\sigma}{\partial s}\right\rangle\right)=\left\langle\frac{\partial\sigma}{\partial r},\nabla_{\frac{\partial}{\partial s}}\left(\frac{\partial\sigma}{\partial r}\right)\right\rangle.$$

We have

$$2\left\langle\frac{\partial\sigma}{\partial r},\nabla_{\frac{\partial}{\partial s}}\left(\frac{\partial\sigma}{\partial r}\right)\right\rangle=\frac{\partial}{\partial s}\left(\left\langle\frac{\partial\sigma}{\partial r},\frac{\partial\sigma}{\partial r}\right\rangle\right)\qquad[\text{Th 19.3.1}]$$

$$=\frac{d}{ds}(\langle\psi,\psi\rangle)\qquad[\text{part (a)}]$$

$$=2\left\langle\frac{d\psi}{ds},\psi\right\rangle.\qquad[\text{Th 19.3.1}]$$

Then the preceding two identities give

$$\frac{\partial}{\partial r}\left\langle\frac{\partial\sigma}{\partial r}(r,s),\frac{\partial\sigma}{\partial s}(r,s)\right\rangle=\left\langle\frac{d\psi}{ds}(s),\psi(s)\right\rangle,$$

where we observe that the right-hand side is independent of $r$. From the differential calculus of one real variable,

$$\left\langle\frac{\partial\sigma}{\partial r}(r,s),\frac{\partial\sigma}{\partial s}(r,s)\right\rangle=c+r\left\langle\frac{d\psi}{ds}(s),\psi(s)\right\rangle \tag{19.7.3}$$

for some real number $c$. We have from Theorem 18.9.4 and Theorem 18.11.4(c) that

$$\frac{\partial\sigma}{\partial r}(0,s)=d_0(\exp_p)(\psi(s))=\psi(s),$$

and from Theorem 18.11.4(d) that

$$\frac{\partial\sigma}{\partial s}(0,s)=0.$$

Setting $r=0$ in (19.7.3) and using the preceding two identities yields $c=0$.

Let $(M,\mathfrak g)$ be a semi-Riemannian manifold, and let $p$ be a point in $M$. Let $v$ and $w$ be vectors in $\mathcal E_p$, and recall from (18.9.4) the differential map

$$d_v(\exp_p):T_v(T_p(M))\longrightarrow T_{\exp_p(v)}(M).$$

The next result shows that $d_v(\exp_p)$ can be viewed as a “linear isometry along radial geodesics”.

### **Theorem 19.7.5 (Gauss’s Lemma).** *With the above setup,*

$$\langle v,w\rangle=\left\langle d_v(\exp_p)(v),d_v(\exp_p)(w)\right\rangle.$$

*Proof.* Let $\psi(s):(-\varepsilon,\varepsilon)\longrightarrow T_p(M)$ be a smooth curve such that $\psi(0)=v$ and $(d\psi/ds)(0)=w$, as for example, $\psi(s)=v+sw$. Consider the parametrized surface $\sigma(r,s):[0,1]\times(-\varepsilon,\varepsilon)\longrightarrow M$ defined by $\sigma(r,s)=\exp_p(r\psi(s))$, where we assume that $r\psi(s)$ is in $\mathcal E_p$ for all $(r,s)$ in $[0,1]\times(-\varepsilon,\varepsilon)$. It follows from parts (c) and (d) of Theorem 18.11.4 that

$$\frac{\partial\sigma}{\partial r}(1,0)=d_v(\exp_p)(v)\quad\text{and}\quad\frac{\partial\sigma}{\partial s}(1,0)=d_v(\exp_p)(w),$$

and then from Theorem 19.7.4(b) that

$$\left\langle d_v(\exp_p)(v),d_v(\exp_p)(w)\right\rangle=\left\langle\frac{d\psi}{ds}(0),\psi(0)\right\rangle=\langle w,v\rangle.$$

## 19.8 Riemann Curvature Tensor

Let $(M,\mathfrak{g})$ be a semi-Riemannian manifold. In this setting, the curvature tensor (field) $R$, originally introduced in the context of smooth manifolds with a connection, is called the **Riemann curvature tensor (field)**. We define a map

```
\mathcal{R}: \mathfrak{X}(M)^4 \longrightarrow C^{\infty}(M),
```

also called the Riemann curvature tensor (field) by

```
\mathcal{R}(X, Y, Z, W) = \langle R(X, Y) Z, W \rangle
```

for all vector fields $X, Y, Z, W$ in $\mathfrak{X}(M)$.

### **Theorem 19.8.1 (Representation of $\mathcal{R}$).** If $(M,\mathfrak{g})$ is a semi-Riemannian manifold, then $\mathcal{R}$ is a map in $\mathrm{Mult}_{C^{\infty}(M)}(\mathfrak{X}(M)^4, C^{\infty}(M))$ and can be identified with a tensor field in $\mathcal{T}_4^0(M)$.

*Proof.* It follows from Theorem 18.7.1(b) that $\mathcal{R}$ is $C^{\infty}(M)$-linear in its first three arguments, and clearly it is $C^{\infty}(M)$-linear in its fourth. This proves the first assertion. The second assertion follows from Theorem 15.7.2.

Working through the details of Figure 19.5.1, Theorem 19.5.5(b), and Theorem 19.8.1, we find that $\mathcal{R}$ can be identified with $R$. It is usual to think of $\mathcal{R}$ as being obtained from $R$ by “lowering an index”. This explains the practice of referring to both $\mathcal{R}$ and $R$ as the Riemann curvature tensor.

### **Theorem 19.8.2.** If $(M,\mathfrak{g})$ is a semi-Riemannian $m$-manifold with a connection, then, in local coordinates, for $i,j,k,l = 1,\dots,m$:
(a) The components of $\mathcal{R}$ with respect to the coordinate frame are

```
\mathcal{R}_{ijkl} = \sum_{n} \mathfrak{g}_{ln} R^{n}_{ijk}.
```

(b)

```
R^{l}_{ijk} = \sum_{n} \mathfrak{g}^{ln} \mathcal{R}_{ijkn}.
```

*Proof.* (a): It follows from (18.7.2) that

```
\mathcal{R}_{ijkl} = \mathcal{R}\left(\frac{\partial}{\partial x^{i}}, \frac{\partial}{\partial x^{j}}, \frac{\partial}{\partial x^{k}}, \frac{\partial}{\partial x^{l}}\right) = \left\langle R\left(\frac{\partial}{\partial x^{i}}, \frac{\partial}{\partial x^{j}}\right)\frac{\partial}{\partial x^{k}}, \frac{\partial}{\partial x^{l}}\right\rangle
```

```
= \left\langle \sum_{n} R^{n}_{ijk} \frac{\partial}{\partial x^{n}}, \frac{\partial}{\partial x^{l}}\right\rangle = \sum \mathfrak{g}_{ln} R^{n}_{ijk}.
```

(b): By part (a),

```
\sum_{n} \mathfrak{g}^{ln} \mathcal{R}_{ijkn} = \sum_{n} \mathfrak{g}^{ln} \left(\sum_{p} \mathfrak{g}_{np} R^{p}_{ijk}\right) = \sum_{p} \left(\sum_{n} \mathfrak{g}^{ln} \mathfrak{g}_{np}\right) R^{p}_{ijk}
```

```
= \sum_{p} \delta^{l}_{p} R^{p}_{ijk} = R^{l}_{ijk}.
```

### Theorem 19.8.3 (First Bianchi Identity).

Let $(M,g,\nabla)$ be a semi-Riemannian manifold with a connection, and let $X,Y,Z,W$ be vector fields in $\mathfrak{X}(M)$. Then:

-   (a)

$$\sum_{(X,Y,Z)} \mathcal{R}(X,Y,Z,W)=\sum_{(X,Y,Z)}\langle\nabla_X(T(Y,Z)),W\rangle+\sum_{(X,Y,Z)}\langle T(X,[Y,Z]),W\rangle,$$

where $T$ is the torsion tensor.

-   (b) If $\nabla$ is symmetric (in particular, if $\nabla$ is the Levi-Civita connection), then

$$\sum_{(X,Y,Z)}\mathcal{R}(X,Y,Z,W)=0.$$

*Proof.* This follows from Theorem 18.7.3.

### Theorem 19.8.4 (Second Bianchi Identity).

Let $(M,g,\nabla)$ be a semi-Riemannian manifold with a connection, and let $X,Y,Z,V,W$ be vector fields in $\mathfrak{X}(M)$. Then:

-   (a)

$$\sum_{(X,Y,Z)}\nabla_X(\mathcal{R})(Y,Z,V,W)=-\sum_{(X,Y,Z)}\mathcal{R}(T(X,Y),Z,V,W),\tag{19.8.1}$$

where $T$ is the torsion tensor.

-   (b) If $\nabla$ is symmetric (in particular, if $\nabla$ is the Levi-Civita connection), then

$$\sum_{(X,Y,Z)}\nabla_X(\mathcal{R})(Y,Z,V,W)=0.$$

*Proof.* (a): We make repeated use of (18.7.3). By Theorem 18.1.4,

$$\nabla_X(\mathcal{R})(Y,Z,V,W)=X(\mathcal{R}(Y,Z,V,W))-\mathcal{R}(\nabla_X(Y),Z,V,W)$$
$$\qquad-\mathcal{R}(Y,\nabla_X(Z),V,W)-\mathcal{R}(Y,Z,\nabla_X(V),W)$$
$$\qquad-\mathcal{R}(Y,Z,V,\nabla_X(W)).$$
