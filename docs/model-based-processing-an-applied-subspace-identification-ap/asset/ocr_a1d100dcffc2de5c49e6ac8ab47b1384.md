where \(\hat{X}_{\mathrm{MV}} \in R^{N_xN_x1}\), \(R_{xy}\) and \(K_{\mathrm{MV}} \in R^{N_xN_xN_yN_y}\), \(R_{yy} \in R^{N_yN_yN_yN_y}\), and \(Y \in R^{N_yN_x1}\). Similarly, the linear estimator can be expressed in terms of the \(N\) data samples as

\[\hat{X}_{\mathrm{MV}}(N) \triangleq R_{xy}(N)R_{yy}^{-1}(N)=K_{\mathrm{MV}}(N)Y(N)\tag{4.4}\]

where \(\hat{X}_{\mathrm{MV}}(N)=[\hat{x}'(1)\cdots\hat{x}'(N)]'\), \(Y(N)=[y'(1)\cdots y'(N)]'\), \(\hat{x}\in R^{N_x\times1}\), and \(y\in R^{N_y\times1}\).

Here we are investigating a “batch” solution to the state estimation problem, since all the \(N_y\)-vector data \(\{y(1)\cdots y(N)\}\) are processed in one batch. However, we require a recursive solution (see Chapter 1) to this problem of the form

\[\hat{X}_{\mathrm{new}}=\hat{X}_{\mathrm{old}}+KE_{\mathrm{new}}\tag{4.5}\]

In order to achieve the recursive solution, it is necessary to transform the covariance matrix \(R_{yy}\) to be block diagonal. \(R_{yy}\) block diagonal implies that all the off-diagonal block matrices \(R_{yy}(t,j)=0\), for \(i\ne j\), which in turn implies that the \(\{y(t)\}\) must be uncorrelated or equivalently orthogonal. Therefore, we must construct a sequence of independent \(N_y\)-vectors, say \(\{e(t)\}\), such that

\[E\{e(t)e'(k)\}=0\quad\text{for }t\ne k\]

The sequence \(\{e(t)\}\) can be constructed using the orthogonality property of the minimum variance estimator such that

\[[y(t)-E\{y(t)\mid Y(t-1)\}]\perp Y(t-1)\]

We define the *innovations* or new information [4] as

\[e(t)\triangleq y(t)-\hat{y}(t\mid t-1)\tag{4.6}\]

with the orthogonality property that

\[\operatorname{cov}[y(T),e(t)]=0\quad\text{for }T\le t-1\tag{4.7}\]

Since \(\{e(t)\}\) is a time-uncorrelated \(N_y\)-vector sequence by construction, we have that the block diagonal innovations covariance matrix is \(R_{ee}(N)=\operatorname{diag}[R_{ee}(1)\cdots R_{ee}(N)]\) with each \(R_{ee}(i)\in R^{N_y\times N_y}\).

The correlated measurement vector can be transformed to an uncorrelated innovations vector through a linear transformation, say \(L\), given by

\[Y=Le\tag{4.8}\]

where \(L\in R^{N_yN_xN_yN_yN}\) is a nonsingular transformation matrix and \(e\triangleq[e'(1)\cdots e'(N)]'\). Multiplying Eq. (4.8) by its transpose and taking expected values, we obtain

\[R_{yy}(N)=LR_{ee}(N)L'\quad\text{and}\quad R_{yy}^{-1}(N)=(L')^{-1}R_{ee}^{-1}(N)L^{-1}\]