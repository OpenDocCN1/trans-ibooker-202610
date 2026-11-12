Case 2: If \( a_1 \in E_A \setminus \mathbb{Z} \), then \( e_2 = a_1' \text{ op } a_2 \) with \( \langle a_1, \text{Env} \rangle \leftrightarrow \langle a_1', \text{Env} \rangle \). The evaluation tree for \( \text{Env} \vdash a_1' \text{ op } a_2 \rightsquigarrow v \) must contain the subtrees:

\( (F_j) \frac{\vdots}{Env \vdash a_1' \rightsquigarrow v_1'} \qquad (F_k) \frac{\vdots}{Env \vdash a_2 \rightsquigarrow v_2'} \)

and thus, by induction hypothesis, we obtain \( \text{Env} \vdash a_1 \rightsquigarrow v_1' \) and, from the evaluation tree for \( \text{Env} \vdash a_2 \rightsquigarrow v_2' \), rule \( F_4 \) allows us to obtain the evaluation tree for \( \text{Env} \vdash a_1 \text{ op } a_2 \rightsquigarrow v \).

Case 3: If \( a_1 \in \mathbb{Z} \) and \( a_2 \in E_A \setminus \mathbb{Z} \), then a similar reasoning to that used in the previous case brings us to our conclusion.

+   5) Since \( \langle e, \text{Env} \rangle \stackrel{\star}{\rightarrowtail} v \), there exists a transition sequence:

\( \langle e, \text{Env} \rangle = \langle e^0, \text{Env} \rangle \leftrightarrow \langle e^1, \text{Env} \rangle \leftrightarrow \cdots \leftrightarrow \langle e^k, \text{Env} \rangle \)

with \( e^k = v \in \mathbb{Z} \). The proof is obtained by induction over the length \( k \) of this sequence. Let us prove this property for \( k=0 \), for \( k=1 \) and for \( k>1 \), which we assume to be true for \( k-1 \) and \( k-2 \).

+   – If \( k=0 \), then \( e = v \in \mathbb{Z} \) and rule \( F_1 \) concludes our proof. Else, if \( k=k_0+1 \), then we proceed by case over the first transition in the sequence.

+   – If \( k=1 \), then two cases are possible.

Case I: If the transition has been obtained using rule \( AS_1 \): \( \langle x, \text{Env} \rangle \leftrightarrow \langle \text{Env}(x), \text{Env} \rangle \), then \( e=x \) and \( v=\text{Env}(x) \) and rule \( F_{11} \) concludes our proof.

Case 2: If the transition has been obtained using rule \( AS_2 \), \( AS_3 \) or \( AS_4 \):

\( \langle n_1 \text{ op } n_2, \text{Env} \rangle \leftrightarrow \langle n, \text{Env} \rangle \)

with \( \text{op} \in \{+, -, *\} \), then \( n = n_1 \text{ op}_{\mathbb{V}} n_2 \) and rule \( F_4 \) concludes our proof:

\( (F_1) \frac{Env \vdash n_1 \rightsquigarrow n_1}{} \qquad (F_1) \frac{Env \vdash n_2 \rightsquigarrow n_2}{} \qquad (F_4) \frac{Env \vdash n_1 \text{ op } n_2 \rightsquigarrow n}{} \)

+   – If \( k \ge 2 \), then two cases are possible.

Case I: If \( \langle e_1 \text{ op } e_2, \text{Env} \rangle \leftrightarrow \langle e'_1 \text{ op } e_2, \text{Env} \rangle \leftrightarrow \cdots \leftrightarrow \langle e^k, \text{Env} \rangle \) is the considered sequence, with \( \langle e_1, \text{Env} \rangle \leftrightarrow \langle e'_1, \text{Env} \rangle \) (the first transition is obtained using rule \( AS_5 \), \( AS_7 \) or \( AS_9 \)), then, by induction hypothesis over a transition sequence of length \( k-1 \), we have \( \text{Env} \vdash e'_1 \text{ op } e_2 \rightsquigarrow v \) and, from the previous question, we obtain \( \text{Env} \vdash e_1 \text{ op } e_2 \rightsquigarrow v \).