
## 11.4 Types of Regular Surfaces in \(\mathbb{R}^3\)

For a given point \(t\) in \((a,b)\), we define a smooth curve

$$\varphi_t(\phi):(-\pi,\pi)\longrightarrow\mathbb{R},$$

called the **latitude curve** corresponding to \(t\), by

$$\varphi_t(\phi)=\varphi(t,\phi).$$

Similarly, for a given point \(\phi\) in \((-\pi,\pi)\), we define a smooth curve

$$\varphi_\phi(t):(a,b)\longrightarrow\mathbb{R},$$

called the **longitude curve** corresponding to \(\phi\), by

$$\varphi_\phi(t)=\varphi(t,\phi).$$

From

$$\varphi_t(\phi)=\rho(t)(\cos(\phi),\sin(\phi),0)+(0,0,h(t)),$$

we see that the image of \(\varphi_t\) is, except for a single missing point, a circle of radius \(\rho(t)\) centered on the \(z\)-axis and lying in the plane parallel to the \(xy\)-plane at a height \(h(t)\).

**Theorem 11.4.3 (Surface of Revolution).** *With the above setup, \(\operatorname{rev}(\sigma)\), viewed as a topological subspace of \(\mathbb{R}^3\), is a regular surface and \((U,\varphi)\) is a covering chart on \(\operatorname{rev}(\sigma)\).*

*Proof.* Since \(\rho\) and \(h\) are smooth, by definition, so is \(\varphi\). Thus, \(\varphi:U\longrightarrow\mathbb{R}^3\) is a parametrized surface. We need to prove that:

+   [C1] \(\varphi:U\longrightarrow\mathbb{R}^3\) is an immersion.
+   
+   [C2] \(\varphi(U)\) is an open set in \(\operatorname{rev}(\sigma)\).
+   
+   [C3] \(\varphi:U\longrightarrow\varphi(U)\) is a homeomorphism.

The proofs are as follows:

[C1]: The Jacobian matrix is

$$J_\varphi(t,\phi)=\begin{bmatrix}\dot{\rho}(t)\cos(\phi)&-\rho(t)\sin(\phi)\\\dot{\rho}(t)\sin(\phi)&\rho(t)\cos(\phi)\\\dot{h}(t)&0\end{bmatrix},$$

and the corresponding vector product is

$$(\dot{\rho}(t)\cos(\phi),\dot{\rho}(t)\sin(\phi),\dot{h}(t))\times(-\rho(t)\sin(\phi),\rho(t)\cos(\phi),0)$$
$$=\rho(t)(-\dot{h}(t)\cos(\phi),-\dot{h}(t)\sin(\phi),\dot{\rho}(t)),$$

which never equals \((0,0,0)\); for if it did, then taking the Euclidean inner product of the preceding vector with itself gives

$$0=\rho(t)^2\left([-\dot{h}(t)\cos(\phi)]^2+[-\dot{h}(t)\sin(\phi)]^2+\dot{\rho}(t)^2\right)=\rho(t)^2[\dot{h}(t)^2+\dot{\rho}(t)^2]$$

## 11.5  **Functions on Regular Surfaces in** \(\mathbb{R}^3\)

**Theorem 11.5.1 (Smoothness Criteria for Functions).** *If* \(M\) *is a regular surface and* \(f:M\to\mathbb{R}\) *is a function, then the following are equivalent:*

(a) \(f\) *is (extended) smooth.*

(b) *For every point* \(p\in M\), *there is a chart* \((U,\varphi)\) *on* \(M\) *at* \(p\) *such that the function* \(f\circ\varphi:U\to\mathbb{R}\) *is (Euclidean) smooth.*

(c) *For every chart* \((U,\varphi)\) *on* \(M\), *the function* \(f\circ\varphi:U\to\mathbb{R}\) *is (Euclidean) smooth.*

*Proof.* Setting \(n=1\) and \(F=f\) in Theorem 11.2.12 gives the result.

Let \(M\) be a regular surface. The set of smooth functions on \(M\) is denoted by \(C^\infty(M)\). We make \(C^\infty(M)\) into both a vector space over \(\mathbb{R}\) and a ring by defining operations as follows: for all functions \(f,g\) in \(C^\infty(M)\) and all real numbers \(c\), let

\[(f+g)(p)=f(p)+g(p),\]
\[(fg)(p)=f(p)g(p),\]
and
\[(cf)(p)=cf(p).\]

for all \(p\) in \(M\). The identity element of the ring is the constant function \(1_M\) that sends all points in \(M\) to the real number 1.

Let \(f\) be a function in \(C^\infty(M)\), and let \(p\) be a point in \(M\). The **differential of** \(f\) **at** \(p\) is the map

\[d_p(f):T_p(M)\longrightarrow\mathbb{R}\]

defined by

\[d_p(f)(v)=\frac{d(f\circ\lambda)}{dt}(t_0) \tag{11.5.1}\]

for all vectors \(v\) in \(T_p(M)\), where \(\lambda(t):(a,b)\to M\) is any smooth curve such that \(\lambda(t_0)=p\) and \((d\lambda/dt)(t_0)=v\) for some \(t_0\) in \((a,b)\).

**Theorem 11.5.2.** *Let* \(M\) *be a regular surface, let* \(f\) *be a function in* \(C^\infty(M)\), *let* \(p\) *be a point in* \(M\), *and let* \(v\) *be a vector in* \(T_p(M)\). *Then:*

(a) \(d_p(f)(v)\) *is independent of the choice of smooth curve used to express* \(v\).

(b) \(d_p(f)\) *is linear.*

*Proof.* Let \((U,\varphi)\) be a chart at \(p\), and let \(\lambda(t):(a,b)\to M\) be a smooth curve such that \(\lambda(t_0)=p\) and \((d\lambda/dt)(t_0)=v\) for some \(t_0\) in \((a,b)\). Suppose without loss of generality that \(\lambda((a,b))\subset\varphi(U)\), and let \(q=\varphi^{-1}(p)\). It follows from Theorem 10.1.17 and Theorem 11.2.8 that the map

\[\mu=(\mu^1,\mu^2)=\varphi^{-1}\circ\lambda:(a,b)\longrightarrow U\]

is (Euclidean) smooth, and from Theorem 10.1.9 that

\[v=\frac{d\lambda}{dt}(t_0)=\frac{d(\varphi\circ\mu)}{dt}(t_0)=d_q(\varphi)\left(\frac{d\mu}{dt}(t_0)\right).\]

By Theorem 11.3.1(d), $d_q(\varphi)$ is invertible, so

$$\frac{d\mu}{dt}(t_0)=d_q(\varphi)^{-1}(v).$$

Then

$$\begin{aligned}
d_p(f)(v)&=\frac{d(f\circ\lambda)}{dt}(t_0)\\
&=\frac{d(f\circ\varphi\circ\mu)}{dt}(t_0)\\
&=d_{\mu(t_0)}(f\circ\varphi)\left(\frac{d\mu}{dt}(t_0)\right)\\
&=d_q(f\circ\varphi)\circ d_q(\varphi)^{-1}(v).
\end{aligned}$$

The preceding identity makes sense because we have the maps $d_q(\varphi)^{-1}:T_p(M)\longrightarrow\mathbb{R}^2$ and $d_q(f\circ\varphi):\mathbb{R}^2\longrightarrow\mathbb{R}$, hence $d_p(f):T_p(M)\longrightarrow\mathbb{R}$.

(a): This follows from (11.5.3).

(b): Since $v$ was arbitrary, we have from (11.5.3) that

$$d_p(f)=d_q(f\circ\varphi)\circ d_q(\varphi)^{-1}.$$

Thus, $d_p(f)$ is the composition of linear maps, so it too is linear.

The next result is a counterpart of Theorem 10.1.3.

**Theorem 11.5.3.**

Let $M$ be a regular surface, let $f,g$ be functions in $C^\infty(M)$, let $p$ be a point in $M$, and let $c$ be a real number. Then:

(a) $d_p(cf+g)=c\,d_p(f)+d_p(g)$.

(b) $d_p(fg)=f(p)\,d_p(g)+g(p)\,d_p(f)$.

*Proof.* Let $v$ be a vector in $T_p(M)\), and let $\lambda(t):(a,b)\longrightarrow M$ be a smooth curve such that $\lambda(t_0)=p$ and $(d\lambda/dt)(t_0)=v$ for some $t_0$ in $(a,b)$. Then

$$\begin{aligned}
d_p(cf+g)(v)&=\frac{d((cf+g)\circ\lambda)}{dt}(t_0)\\
&=\frac{d((cf\circ\lambda)+(g\circ\lambda))}{dt}(t_0)\\
&=c\frac{d(f\circ\lambda)}{dt}(t_0)+\frac{d(g\circ\lambda)}{dt}(t_0)\\
&=c\,d_p(f)(v)+d_p(g)(v)\\
&=(c\,d_p(f)+d_p(g))(v).
\end{aligned}$$

Section 11.2, φ(U) is open in M, so there is an open set 𝒱 in ℝ³ such that φ(U) = M ∩ 𝒱. Then

M ∩ (𝒰 ∩ 𝒱) = (M ∩ 𝒱) ∩ 𝒰 = φ(U) ∩ 𝒰,

so F and F|φ(U) agree on M ∩ (𝒰 ∩ 𝒱), where we observe that 𝒰 ∩ 𝒱 is open in ℝ³. Since p was arbitrary, the result follows.

# Theorem 11.6.2. Let M, N, and P be regular surfaces, and let F : M ⟶ N and G : N ⟶ P be maps. If F and G are (extended) smooth, then so is G ∘ F.

Proof. Let p be a point in M. By Theorem 11.6.1, there is a chart (U, φ) on M at p and a chart (V₁, ψ₁) on N at F(p) such that ψ₁⁻¹ ∘ F ∘ φ is (Euclidean) smooth. For the same reason, there is a chart (V₂, ψ₂) on N at F(p) and a chart (W, μ) at G(F(p)) such that μ⁻¹ ∘ G ∘ ψ₂ is (Euclidean) smooth. By Theorem 11.2.9, ψ₂⁻¹ ∘ ψ₁ is (Euclidean) smooth. It follows from Theorem 10.1.12 that

μ⁻¹ ∘ (G ∘ F) ∘ φ = (μ⁻¹ ∘ G ∘ ψ₂) ∘ (ψ₂⁻¹ ∘ ψ₁) ∘ (ψ₁⁻¹ ∘ F ∘ φ)

is (Euclidean) smooth. Since p was arbitrary, the result follows from Theorem 11.6.1.

Let M and N be regular surfaces, let F : M ⟶ N be a smooth map, and let p be a point in M. The differential of F at p is the map

dₚ(F) : Tₚ(M) ⟶ T_F(p)(N)

defined by

dₚ(F)(v) = d(F ∘ λ)/dt (t₀)   (11.6.1)

for all vectors v in Tₚ(M), where λ(t) : (a, b) ⟶ M is any smooth curve such that λ(t₀) = p and (dλ/dt)(t₀) = v for some t₀ in (a, b). See Figure 11.6.1.

# Theorem 11.6.3. With the above setup:

+   (a) dₚ(F)(v) is independent of the choice of smooth curve used to express v.
+   (b) dₚ(F) is linear.

Proof. The proof is similar to that given for Theorem 11.5.2. Let (U, φ) be a chart on M at p, let (V, ψ) be a chart on N at F(p), and let λ(t) : (a, b) ⟶ M be a smooth curve such that λ(t₀) = p and (dλ/dt)(t₀) = v for some t₀ in (a, b). Suppose without loss of generality that λ((a, b)) ⊂ φ(U), and let q = φ(p). It follows from Theorem 10.1.17 and Theorem 11.2.8 that the map

μ = (μ¹, μ²) = φ⁻¹ ∘ λ : (a, b) ⟶ U

is (Euclidean) smooth, and from Theorem 10.1.9 that

v = dλ/dt (t₀) = d(φ ∘ μ)/dt (t₀) = d_q(φ) ( dμ/dt (t₀) ).
