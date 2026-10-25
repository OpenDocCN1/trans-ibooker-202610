(b) \(S(V)\) is a 1-dimensional subspace of \(T^0_4(V)\) that is spanned by \(\mathcal D\).

**Proof.** (a): That \(\mathcal D\) is a tensor in \(S(V)\) follows from the properties of determinants, or perhaps more easily from the observation that

\[\mathcal D(v_1,v_2,v_3,v_4)=\langle v_1,v_3\rangle\langle v_2,v_4\rangle-\langle v_2,v_3\rangle\langle v_1,v_4\rangle.\]

Let \((e_1,e_2)\) be an orthonormal basis for \(V\). Then

\[\mathcal D(e_1,e_2,e_2,e_1)=-\langle e_1,e_1\rangle\langle e_2,e_2\rangle\ne 0,\]

hence \(\mathcal D\) is nonzero.

(b): Let \(\mathcal A\) be a tensor in \(S(V)\). We have from (6.6.5) that

\[\mathcal A(v_1,v_2,v_3,v_4)=-(a^1b^2-a^2b^1)(c^1d^2-c^2d^1)\overline{\mathcal A}(h_1,h_2)\]

and

\[\mathcal D(v_1,v_2,v_3,v_4)=-(a^1b^2-a^2b^1)(c^1d^2-c^2d^1)\overline{\mathcal D}(h_1,h_2),\]

hence

\[\mathcal A(v_1,v_2,v_3,v_4)=\frac{\overline{\mathcal A}(h_1,h_2)}{\overline{\mathcal D}(h_1,h_2)}\mathcal D(v_1,v_2,v_3,v_4),\]

where Theorem 6.6.2 and part (a) ensure that the denominator is nonzero. Since \(v_1,v_2,v_3,v_4\) were arbitrary,

\[\mathcal A=\frac{\overline{\mathcal A}(h_1,h_2)}{\overline{\mathcal D}(h_1,h_2)}\mathcal D,\]

so \(\mathcal D\) spans \(S(V)\).

# **Theorem 6.6.4.** Let \((V,g)\) be a 2-dimensional scalar product space, let \(\mathcal A\) be a tensor in \(S(V)\), and let \((h_1,h_2)\) and \((f_1,f_2)\) be bases for \(V\). Then

\[\frac{\overline{\mathcal A}(h_1,h_2)}{\overline{\mathcal D}(h_1,h_2)}=\frac{\overline{\mathcal A}(f_1,f_2)}{\overline{\mathcal D}(f_1,f_2)}.\]

**Proof.** Arguing as in the proof of part (b) of Theorem 6.6.3, but with (6.6.6) in place of (6.6.5), gives the result.