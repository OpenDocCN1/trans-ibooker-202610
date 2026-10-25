Looking again at Figure 4.8.2, we confirm using Theorem 4.8.3, Theorem 4.8.7, and Theorem 4.8.8, respectively, that \(\Pi_1\) is spacelike, \(\Pi_2\) is timelike, and \(\Pi_3\) is lightlike.

We conclude this section with yet another version of the Cauchy–Schwarz inequality. It is labeled “reversed” because of the direction of the inequality. This is a reminder of how differently behaved a Lorentz vector space is compared to an inner product space.

# **Theorem 4.8.10 (Reversed Cauchy–Schwarz Inequality).**

*If \((V,g)\) is a Lorentz vector space and \(v,w\) are timelike vectors, then*

\[\langle v,w\rangle^2 \ge \langle v,v\rangle\langle w,w\rangle,\]

with equality if and only if \(v,w\) are linearly dependent.

*Proof.* Since \(v\) is timelike, it follows from Theorem 4.8.2 that \(\mathbb{R}v\) is timelike, and then from Theorem 4.8.4(b) that \((\mathbb{R}v)^\perp\) is spacelike. Thus, \(g\) is an inner product on \((\mathbb{R}v)^\perp\). By Theorem 4.8.5, \(w=\overline{w}+cv\), where \(\overline{w}\) is a (spacelike) vector in \((\mathbb{R}v)^\perp\) and \(c\) is a real number. Then

\[\langle v,w\rangle=c\langle v,v\rangle \qquad\text{and}\qquad \langle w,w\rangle=\langle\overline{w},\overline{w}\rangle+c^2\langle v,v\rangle,\]

hence
\[
\langle v,w\rangle^2=c^2\langle v,v\rangle^2
=\langle v,v\rangle\bigl(\langle w,w\rangle-\langle\overline{w},\overline{w}\rangle\bigr)
=\langle v,v\rangle\langle w,w\rangle-\langle v,v\rangle\langle\overline{w},\overline{w}\rangle.
\]

Since \(v\) and \(w\) are timelike, \(\langle v,v\rangle\langle w,w\rangle>0\), and because \(v\) is timelike and \(\overline{w}\) is spacelike, \(\langle v,v\rangle\langle\overline{w},\overline{w}\rangle\le 0\). It follows that \(\langle v,w\rangle^2\ge\langle v,v\rangle\langle w,w\rangle\), with equality if and only if \(\langle\overline{w},\overline{w}\rangle=0\). Since \(g\) is an inner product on \((\mathbb{R}v)^\perp\), \(\langle\overline{w},\overline{w}\rangle=0\) if and only if \(\overline{w}=0\), which is equivalent to \(w=cv\), which in turn is equivalent to \(v\) and \(w\) being linearly dependent.

# ## 4.9  Time Cones

Let \((V,g)\) be a Lorentz vector space. The **time cone** of \(V\) is denoted by \(\mathcal{T}\) and defined to be the set of timelike vectors in \(V\):

\[\mathcal{T}=\{v\in V:\langle v,v\rangle<0\}.\]

In \(\mathbb{R}^m_1\), the time cone is denoted by \(\mathcal{T}_m\). For example,

\[\mathcal{T}_3=\{(x,y,z)\in\mathbb{R}^3_1:x^2+y^2-z^2<0\}.\]

Recall from Example 4.8.1 that the light cone of \(\mathbb{R}^3_1\) is

\[\Lambda_3=\{(x,y,z)\in\mathbb{R}^3_1:x^2+y^2-z^2=0\}\setminus\{(0,0,0)\}.\]

Thus, \(\mathcal{T}_3\) is “inside” \(\Lambda_3\).