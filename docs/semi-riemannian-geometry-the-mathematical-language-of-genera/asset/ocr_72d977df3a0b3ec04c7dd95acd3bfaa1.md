# 4.3  Adjoints

*Proof.* We have

```
$$\langle v,w\rangle=\left\langle\sum_i a^i e_i,\sum_j b^j e_j\right\rangle=\sum_{ij}a^i b^j\langle e_i,e_j\rangle=\sum_i\varepsilon_i a^i b^i.$$
```

# 4.3  Adjoints

We previously defined what it means for a matrix to be symmetric. In this section, we introduce a type of linear map that under certain circumstances has a related symmetry property (see Theorem 4.6.10).

Let $(V,\mathfrak g)$ and $(W,\mathfrak h)$ be scalar product spaces, and let $A:V\to W$ be a linear map. We define a map

```
$$A^\dagger:W\longrightarrow V,$$
```

called the **adjoint of $A$**, by

```
$$A^\dagger=S\circ A^*\circ\mathcal F,$$  (4.3.1)
```

where $S$ and $\mathcal F$ are the sharp map and flat map corresponding to $\mathfrak g$ and $\mathfrak h$, respectively, and $A^*$ is the pullback by $A$. Let us denote

```
$$(A^\dagger)^\dagger\quad\text{by}\quad A^{\dagger\dagger}.$$
```

**Theorem 4.3.1.** *With the above setup:*  
(a) $A^\dagger$ is the unique linear map such that

```
$$\langle A(v),w\rangle=\langle v,A^\dagger(w)\rangle$$

for all vectors $v$ in $V$ and $w$ in $W$.
```

(b) *If $V=W$, then* $\det(A^\dagger)=\det(A)$.

*Proof.* (a): We have

```
$$A^\dagger=S\circ A^*\circ\mathcal F$$
$$\Longleftrightarrow\quad A^*\circ\mathcal F=F\circ A^\dagger$$
$$\Longleftrightarrow\quad (A^*\circ\mathcal F)(w)=(F\circ A^\dagger)(w)\ \text{for all }w\text{ in }W$$
$$\Longleftrightarrow\quad A^*(\mathcal F(w))(v)=F(A^\dagger(w))(v)\ \text{for all }v\text{ in }V\text{ and }w\text{ in }W$$
$$\Longleftrightarrow\quad \mathcal F(w)(A(v))=F(A^\dagger(w))(v)\ \text{for all }v\text{ in }V\text{ and }w\text{ in }W$$
$$\Longleftrightarrow\quad \langle w,A(v)\rangle=\langle A^\dagger(w),v\rangle\ \text{for all }v\text{ in }V\text{ and }w\text{ in }W$$
$$\Longleftrightarrow\quad \langle A(v),w\rangle=\langle v,A^\dagger(w)\rangle\ \text{for all }v\text{ in }V\text{ and }w\text{ in }W.$$
```

This shows that $A^\dagger$ has the desired property, and at the same time that it is uniquely determined by this property.  
(b): With $\mathcal F=F$, we have $S=F^{-1}$, so the result follows from Theorem 2.4.5 and (4.3.1).