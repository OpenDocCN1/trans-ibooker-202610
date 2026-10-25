Proof. (b) ⇒ (c): We have from Theorem 22.1.2(d) that

$$(a_m^1)^2 + \cdots + (a_m^{m-1})^2 = 0,$$

from which the result follows.

(c) ⇒ (b): By Theorem 22.1.2(d), $\gamma^2 = 1$, hence $\gamma = \pm 1$. Since $A$ is in $\mathrm{L}_m^\uparrow$, we have $\gamma = 1$.

(c) ⇔ (d): We have from Theorem 22.1.2(d) that

$$(a_m^1)^2 + \cdots + (a_m^{m-1})^2 = (a_1^m)^2 + \cdots + (a_{m-1}^m)^2,$$

from which the result follows.

(a) ⇒ (b): By definition.

(b) ⇒ (a): We have just shown that (b), (c), and (d) are equivalent, so $[A]$ is of the form

$$[A] = \begin{bmatrix} [A]_1^1 & O_{(m-1)\times 1} \\ O_{1\times (m-1)} & 1 \end{bmatrix}$$

for some matrix $[A]_1^1$ in $\mathrm{Mat}_{(m-1)\times (m-1)}$. By Theorem 22.1.2(a), $[A]_1^1 ([A]_1^1)^\mathrm{T} = I_{m-1}$, hence $[A]_1^1$ is in $\mathrm{O}(m-1)$. Since $A$ is in $\mathrm{L}_m^\uparrow$, we have $\det([A]_1^1) = \det([A]) = 1$, so $[A]_1^1$ is in $\mathrm{SO}(m-1)$. Thus, $A$ is in $\mathrm{R}_m$.

We say that a map $A$ in $\mathrm{Lin}(V,V)$ is a boost (with respect to $\mathcal{E}$ in the $(m-1)$st spacelike direction) if $[A]$ is of the form

$$[A] = \begin{bmatrix} I_{m-2} & O_{(m-2)\times 2} \\ O_{2\times (m-2)} & \begin{bmatrix} \gamma & -\beta\gamma \\ -\beta\gamma & \gamma \end{bmatrix} \end{bmatrix},$$

where $\gamma \ge 1$ and $\beta$ is computed using (22.1.2). For convenience of notation, we have defined boosts in the $(m-1)$st spacelike direction, but any other spacelike direction would do. For instance, a boost in the first spacelike direction takes the form

$$\begin{bmatrix} \gamma & O_{1\times (m-2)} & -\beta\gamma \\ O_{(m-2)\times 1} & I_{m-2} & O_{(m-2)\times 1} \\ -\beta\gamma & O_{1\times (m-2)} & \gamma \end{bmatrix}.$$

The set of boosts is

$$\mathrm{B}_m = \{A \in \mathrm{Lin}(V,V) : A \text{ a boost}\}.$$

It is clear that $\mathrm{B}_m$ contains the identity map. Let $A'$ be another boost, with

$$[A'] = \begin{bmatrix} I_{m-2} & O_{(m-2)\times 2} \\ O_{2\times (m-2)} & \begin{bmatrix} \gamma' & -\beta'\gamma' \\ -\beta'\gamma' & \gamma' \end{bmatrix} \end{bmatrix}.$$

Then

$$[A][A'] = \begin{bmatrix} I_{m-2} & O_{(m-2)\times 2} \\ O_{2\times (m-2)} & \begin{bmatrix} \gamma'' & -\beta''\gamma'' \\ -\beta''\gamma'' & \gamma'' \end{bmatrix} \end{bmatrix},$$