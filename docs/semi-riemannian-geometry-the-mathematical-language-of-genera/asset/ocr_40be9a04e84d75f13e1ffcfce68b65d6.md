# 8.2 Orientation of Vector Spaces

**Throughout,** \(\mathbb{R}^m\) is assumed to have the standard orientation.

We now present an equivalent approach to orientation using multivectors. Let \(V\) be a vector space of dimension \(m\). In the present context, a nonzero multivector in \(\Lambda^m(V)\) is called an **orientation multivector** on \(V\). As an example, let \((h_1,\ldots,h_m)\) be a basis for \(V\), and let \((\theta^1,\ldots,\theta^m)\) be its dual basis. By Theorem 7.2.9, \(\theta^1\wedge\cdots\wedge\theta^m(h_1,\ldots,h_m)=1\), so \(\theta^1\wedge\cdots\wedge\theta^m\) is an orientation multivector on \(V\). More generally, we have the following result.

**Theorem 8.2.1.** Let \(V\) be a vector space of dimension \(m\), and let \(\varpi\) be a multivector in \(\Lambda^m(V)\). Then \(\varpi\) is an orientation multivector if and only if \(\varpi(h_1,\ldots,h_m)\neq 0\) for some (hence every) basis \((h_1,\ldots,h_m)\) for \(V\).

**Proof.** Let \(\mathcal{H}=(h_1,\ldots,h_m)\) be a basis for \(V\), and let \((\theta^1,\ldots,\theta^m)\) be its dual basis. We have from Theorem 7.2.13(b) that

\[\varpi=\varpi(h_1,\ldots,h_m)\,\theta^1\wedge\cdots\wedge\theta^m.\]

Thus, \(\varpi\) is nonzero if and only if \(\varpi(h_1,\ldots,h_m)\neq 0\). Let \(\mathcal{F}=(f_1,\ldots,f_m)\) be another basis for \(V\). By Theorem 7.3.3(b),

\[\varpi(h_1,\ldots,h_m)=\det\left([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}}\right)\varpi(f_1,\ldots,f_m),\]

and by Theorem 2.4.12, \(\det([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}})\neq 0\). Thus, \(\varpi(h_1,\ldots,h_m)\neq 0\) if and only if \(\varpi(f_1,\ldots,f_m)\neq 0\). The result follows.

**Theorem 8.2.2.** Let \(V\) be a vector space, let \(\mathcal{H}=(h_1,\ldots,h_m)\) and \(\mathcal{F}=(f_1,\ldots,f_m)\) be bases for \(V\), and let \((\theta^1,\ldots,\theta^m)\) be the dual basis corresponding to \(\mathcal{H}\). Then the following are equivalent:

+   (a) \(\mathcal{H}\) and \(\mathcal{F}\) are consistent.  
+   (b) \(\theta^1\wedge\cdots\wedge\theta^m(f_1,\ldots,f_m)>0\).  
+   (c) \(\varpi(h_1,\ldots,h_m)\) and \(\varpi(f_1,\ldots,f_m)\) have the same sign for some (hence every) orientation multivector \(\varpi\) on \(V\).

**Remark.** By Theorem 8.2.1, we have \(\varpi(h_1,\ldots,h_m),\varpi(f_1,\ldots,f_m)\neq 0\), so the assertion in part (c) makes sense.

**Proof.** (a) \(\Longleftrightarrow\) (b): By Theorem 7.2.9 and Theorem 7.3.3(b),

\[\det\left([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}}\right)\theta^1\wedge\cdots\wedge\theta^m(f_1,\ldots,f_m)=\theta^1\wedge\cdots\wedge\theta^m(h_1,\ldots,h_m)=1.\]

The result follows.  
(a) \(\Longleftrightarrow\) (c): By Theorem 7.3.3(b),

\[\varpi(h_1,\ldots,h_m)=\det\left([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}}\right)\varpi(f_1,\ldots,f_m),\]

and by Theorem 2.4.12, \(\det([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}})\neq 0\). The result follows.