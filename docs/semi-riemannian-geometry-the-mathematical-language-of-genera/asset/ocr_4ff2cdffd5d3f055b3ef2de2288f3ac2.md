**Proof.** (a): By Theorem 18.9.3,

\[
\sigma_s(r)=\sigma(r,s)=\exp_p(r\psi(s))=\rho_{p,\psi(s)}(r)
\]

for all \(r\) in \([0,1]\).

(b): This follows from part (a).

(c), (d): We define a smooth map

\[
\widetilde{\sigma}(r,s):[0,1]\times(-\varepsilon,\varepsilon)\longrightarrow T_p(M)
\]

by

\[
\widetilde{\sigma}(r,s)=r\psi(s),
\]

so that \(\sigma=\exp_p\circ\widetilde{\sigma}\). Then

\[
\frac{\partial\widetilde{\sigma}}{\partial r}(r,s)=\psi(s)
\qquad\text{and}\qquad
\frac{\partial\widetilde{\sigma}}{\partial s}(r,s)=r\frac{d\psi}{ds}(s).
\]

hence

\[
\frac{\partial\sigma}{\partial r}(r,s)
=\frac{\partial(\exp_p\circ\widetilde{\sigma})}{\partial r}(r,s)
=d_{\sigma(r,s)}(\exp_p)\left(\frac{\partial\widetilde{\sigma}}{\partial r}(r,s)\right)
=d_{r\psi(s)}(\exp_p)(\psi(s)).
\]

Similarly,

\[
\frac{\partial\sigma}{\partial s}(r,s)
=\frac{\partial(\exp_p\circ\widetilde{\sigma})}{\partial s}(r,s)
=d_{\sigma(r,s)}(\exp_p)\left(\frac{\partial\widetilde{\sigma}}{\partial s}(r,s)\right)
=d_{r\psi(s)}(\exp_p)\left(r\frac{d\psi}{ds}(s)\right)
=r\,d_{r\psi(s)}(\exp_p)\left(\frac{d\psi}{ds}(s)\right).
\]

# **Theorem 18.11.5.**

*Let \((M,\nabla)\) be a smooth manifold with a connection, let \(p\) be a point in \(M\), and let \(v,w\) be vectors in \(T_p(M)\). Let \(\rho_{p,v}(r)=\exp_p(rv):[0,1]\to M\) be the radial geodesic with starting point \(p\) and initial velocity \(v\). Let \(\sigma(r,s):[0,1]\times(-\varepsilon,\varepsilon)\to M\) be the geodesic variation of \(\rho_{p,v}(r)\) defined by*

\[
\sigma(r,s)=\exp_p(r(v+sw)),
\]

*where it is assumed that \(r(v+sw)\) is in \(E_p\) for all \((r,s)\in[0,1]\times(-\varepsilon,\varepsilon)\). Then:*

(a) *The variation field \(J\) of \(\sigma\) is given by*

\[
J(r)=r\,d_{rv}(\exp_p)(w)
\]

*for all \(r\) in \([0,1]\).*