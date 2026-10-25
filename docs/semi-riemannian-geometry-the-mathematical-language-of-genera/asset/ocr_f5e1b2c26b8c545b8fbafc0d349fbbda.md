Since $v_1,\dots,v_{s+s'}$ were arbitrary, $\sigma(\eta\otimes\zeta)=\zeta\otimes\eta$. Then

Alt$(\zeta\otimes\eta)=\text{Alt}(\sigma(\eta\otimes\zeta))$ $=\text{sgn}(\sigma)\text{Alt}(\eta\otimes\zeta)\quad[\text{Th 7.1.3(a)}]$ $=(-1)^{ss'}\text{Alt}(\eta\otimes\zeta),\quad[\text{Th B.2.4}]$

hence $\eta\wedge\zeta=\frac{(s+s')!}{s!s'!}\text{Alt}(\eta\otimes\zeta)=(-1)^{ss'}\frac{(s'+s)!}{s'!s!}\text{Alt}(\zeta\otimes\eta)$ $=(-1)^{ss'}\zeta\wedge\eta.$

+   (b): This follows from part (a).
+   (c): This follows from part (b).

# Theorem 7.2.4. If $V$ is a vector space, and $\mathcal{A}$ and $\mathcal{B}$ are multicovectors in $\Lambda^s(V)$ and $\Lambda^{s'}(V)$, respectively, then

$\text{Alt}(\text{Alt}(\mathcal{A})\otimes\mathcal{B})=\text{Alt}(\mathcal{A}\otimes\mathcal{B})=\text{Alt}(\mathcal{A}\otimes\text{Alt}(\mathcal{B})).$

Proof. Let us define a map $\iota:\mathcal{S}_s\longrightarrow\mathcal{S}_{s+s'}$ as follows. For each permutation $\sigma$ in $\mathcal{S}_s$, let $\iota(\sigma)$ be the permutation in $\mathcal{S}_{s+s'}$ that restricts to $\sigma$ on $\{1,\dots,s\}$ and fixes $s+1,\dots,s+s'$; that is,

$\iota(\sigma)=\begin{pmatrix}1&\cdots&s&s+1&\cdots&s+s'\\\sigma(1)&\cdots&\sigma(s)&s+1&\cdots&s+s'\end{pmatrix}.$

Evidently, $\iota$ is a group isomorphism between $\mathcal{S}_s$ and the subgroup $\iota(\mathcal{S}_s)$ of $\mathcal{S}_{s+s'}$. It is easily shown that $\text{sgn}(\iota(\sigma))=\text{sgn}(\sigma)\quad\text{and}\quad\iota(\sigma)(\mathcal{A}\otimes\mathcal{B})=\sigma(\mathcal{A})\otimes\mathcal{B}.\quad(7.2.2)$

Then $\text{Alt}(\text{Alt}(\mathcal{A})\otimes\mathcal{B})=\text{Alt}\left(\left[\frac{1}{s!}\sum_{\sigma\in\mathcal{S}_s}\text{sgn}(\sigma)\sigma(\mathcal{A})\right]\otimes\mathcal{B}\right)\quad[(7.1.4)]$ $=\text{Alt}\left(\frac{1}{s!}\sum_{\sigma\in\mathcal{S}_s}\text{sgn}(\sigma)\left(\sigma(\mathcal{A})\otimes\mathcal{B}\right)\right)\quad[\text{Th 5.1.2(a)}]$ $=\frac{1}{s!}\sum_{\sigma\in\mathcal{S}_s}\text{sgn}(\sigma)\text{Alt}\left(\sigma(\mathcal{A})\otimes\mathcal{B}\right)$ $=\frac{1}{s!}\sum_{\sigma\in\mathcal{S}_s}\text{sgn}(\iota(\sigma))\text{Alt}\left(\iota(\sigma)(\mathcal{A}\otimes\mathcal{B})\right)\quad[(7.2.2)]$ $=\frac{1}{s!}\sum_{\sigma\in\mathcal{S}_s}\text{Alt}(\mathcal{A}\otimes\mathcal{B})\quad[\text{Th 7.1.3(a)}]$ $=\text{Alt}(\mathcal{A}\otimes\mathcal{B}),\quad[\text{card}(\mathcal{S}_s)=s!]$

which proves the first equality. The proof of the second equality is similar.