# 18.2 Christoffel Symbols

**Proof.** We have

$$\nabla_{\partial/\partial \tilde{x}^p}\left(\frac{\partial}{\partial \tilde{x}^q}\right)=\sum_l\tilde{\Gamma}^{l}_{pq}\frac{\partial}{\partial \tilde{x}^l}\tag{18.2.1}$$

$$=\sum_l\tilde{\Gamma}^{l}_{pq}\left(\sum_k\frac{\partial x^k}{\partial \tilde{x}^l}\frac{\partial}{\partial x^k}\right)\tag{Th 15.1.8}$$

$$=\sum_k\left(\sum_l\tilde{\Gamma}^{l}_{pq}\frac{\partial x^k}{\partial \tilde{x}^l}\right)\frac{\partial}{\partial x^k}.\tag{18.2.5}$$

On the other hand,

$$\nabla_{\partial/\partial \tilde{x}^p}\left(\frac{\partial}{\partial \tilde{x}^q}\right)=\nabla\!\sum_{ij}\left(\frac{\partial x^j}{\partial \tilde{x}^p}\right)\left(\frac{\partial}{\partial x^i}\right)\left(\sum_j\frac{\partial x^j}{\partial \tilde{x}^q}\frac{\partial}{\partial x^j}\right)$$

$$=\sum_{ij}\frac{\partial x^i}{\partial \tilde{x}^p}\nabla_{\partial/\partial x^i}\left(\frac{\partial x^j}{\partial \tilde{x}^q}\frac{\partial}{\partial x^j}\right)$$

$$=\sum_{ij}\frac{\partial x^i}{\partial \tilde{x}^p}\left[\nabla_{\partial/\partial x^i}\left(\frac{\partial x^j}{\partial \tilde{x}^q}\right)\frac{\partial}{\partial x^j}+\frac{\partial x^j}{\partial \tilde{x}^q}\nabla_{\partial/\partial x^i}\left(\frac{\partial}{\partial x^j}\right)\right]$$

$$=\sum_{ij}\frac{\partial x^i}{\partial \tilde{x}^p}\left[\nabla_{\partial/\partial x^i}\left(\frac{\partial x^j}{\partial \tilde{x}^q}\right)\frac{\partial}{\partial x^j}+\frac{\partial x^j}{\partial \tilde{x}^q}\sum_k\Gamma^k_{ij}\frac{\partial}{\partial x^k}\right].\tag{18.2.6}$$

where the first equality follows from Theorem 15.1.8, and the last equality from (18.2.1). We also have

$$\nabla_{\partial/\partial x^i}\left(\frac{\partial x^j}{\partial \tilde{x}^q}\right)=\sum_k\left(\frac{\partial \tilde{x}^k}{\partial x^i}\right)\left(\frac{\partial}{\partial \tilde{x}^k}\right)\left(\frac{\partial x^j}{\partial \tilde{x}^q}\right)$$

$$=\sum_k\frac{\partial \tilde{x}^k}{\partial x^i}\nabla_{\partial/\partial \tilde{x}^k}\left(\frac{\partial x^j}{\partial \tilde{x}^q}\right)\tag{18.2.7}$$

$$=\sum_k\frac{\partial \tilde{x}^k}{\partial x^i}\frac{\partial^2x^j}{\partial \tilde{x}^k\partial \tilde{x}^q},$$

where the first equality follows from Theorem 15.1.8. Combining (18.2.6) and (18.2.7) yields

$$\nabla_{\partial/\partial \tilde{x}^p}\left(\frac{\partial}{\partial \tilde{x}^q}\right)$$

$$=\sum_{ij}\frac{\partial x^i}{\partial \tilde{x}^p}\left[\left(\sum_k\frac{\partial \tilde{x}^k}{\partial x^i}\frac{\partial^2x^j}{\partial \tilde{x}^k\partial \tilde{x}^q}\right)\frac{\partial}{\partial x^j}+\frac{\partial x^j}{\partial \tilde{x}^q}\sum_k\Gamma^k_{ij}\frac{\partial}{\partial x^k}\right]\tag{18.2.8}$$

$$=\sum_{ijk}\frac{\partial x^i}{\partial \tilde{x}^p}\frac{\partial x^j}{\partial \tilde{x}^q}\Gamma^k_{ij}\frac{\partial}{\partial x^k}+\sum_{ijk}\frac{\partial x^i}{\partial \tilde{x}^p}\frac{\partial \tilde{x}^k}{\partial x^i}\frac{\partial^2x^j}{\partial \tilde{x}^k\partial \tilde{x}^q}\frac{\partial}{\partial x^j}.$$