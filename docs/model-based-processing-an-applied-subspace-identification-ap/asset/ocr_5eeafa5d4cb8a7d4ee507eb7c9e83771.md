Note that applying the orthogonality properties of the SVD ($U' U = I$, $V' V = I$), we obtain the Gramians as

$$\Sigma_{AC}(K)=\mathcal{O}'_K\mathcal{O}_K=V_o(K)\Sigma_o^2(K)V'_o(K)$$

$$\Sigma_{AB}(K)=\mathcal{C}_K\mathcal{C}'_K=U_c(K)\Sigma_c^2(K)U'_c(K)$$  (6.16)

From the factorization of the Hankel matrix and its singular-value decomposition, we have

$$\mathcal{H}_{K,K}=\mathcal{U}_K\times\Sigma_K\times\mathcal{V}'_K$$
$$=(U_o(K)\Sigma_o(K)V'_o(K))\times(U_c(K)\Sigma_c(K)V'_c(K))$$  (6.17)

establishing the relationship between the singular values of the Hankel matrix and those of the so-called “second-order modes” of the observability/controllability product values (see [12] or [13] for details).

Formally, the Hankel singular values are taken as the square root of the observability and controllability Gramian product values, that is, the *Hankel singular values* are given by

$$\sigma_{ABC}=\sqrt{\Sigma_{AC}\times\Sigma_{AB}}$$

It is important to realize that while the system eigenvalues are invariant under a similarity transformation those of the Gramians are not, but the eigenvalues of the product of the Gramians *are* similarity invariant [14]. It is these values that are the invariants of the system and are used in model order reduction [15].

# 6.2.2  Balanced Realizations

With these system theoretic properties in mind, we can now formally express the concept of a “balanced realization” in terms of the observability, controllability, and the process of simultaneously diagonalizing these matrices. Therefore, we define a *balanced realization* for a stable system as one in which its Gramians are equal and diagonal, that is, [13]

$$\Sigma_{AB}(K)=\Sigma_{AC}(K)=\Sigma_K \qquad \text{(Balanced Realization)}$$

where $\Sigma_K$ is a diagonal matrix obtained from the SVD of the Hankel matrix of Eq. (6.17) such that

$$\Sigma_K=\operatorname{diag}[\sigma_1,\sigma_2,\ldots,\sigma_K]\quad\text{for}\quad\sigma_1\geq\sigma_2\geq\cdots\geq\sigma_K$$

and $\sigma_i$ are the *Hankel singular values* of the system based on the product of the Gramian matrices. These values are *invariant under* similarity transformation (see [12–15] for details).