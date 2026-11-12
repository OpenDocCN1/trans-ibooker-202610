If $\langle \text{Mem}_1, c \rangle \mapsto_{\text{Env}} \text{Mem}_2$ has been obtained using rule $C_1$:

\((C_1) \frac{\text{Env}(x) = r \in \mathbf{R} \quad \llbracket e \rrbracket^{\text{Mem}_1}_{\text{Env}} = (v, \text{Mem}) \in \mathbf{V} \times \mathbf{M}}{\langle \text{Mem}_1, x := e \rangle \mapsto_{\text{Env}} \text{Mem}[r := v]}\)

then rule $S_1$ gives us $\langle x := e, \text{Mem}_1 \rangle \mapsto_{\text{Env}} \langle \text{skip}, \text{Mem}[r := v] \rangle$ and since $\langle \text{skip}, \text{Mem}[r := v] \rangle$ is a terminal configuration, we get:

\(\langle x := e, \text{Mem}_1 \rangle \mapsto^*_{\text{Env}} \text{Mem}[r := v]\)

If $\langle \text{Mem}_1, c \rangle \mapsto_{\text{Env}} \text{Mem}_2$ has been obtained using rule $C_2$, then $c$ is the instruction skip and $\text{Mem}_1 = \text{Mem}_2$, and since $\langle \text{skip}, \text{Mem}_2 \rangle$ is a terminal configuration, by considering a sequence of length zero we get:

\(\langle \text{skip}, \text{Mem}_1 \rangle \mapsto^*_{\text{Env}} \text{Mem}_2\)

If $\langle \text{Mem}_1, c \rangle \mapsto_{\text{Env}} \text{Mem}_2$ has been obtained using rule $C_3$:

\((C_3) \frac{\begin{array}{c} (C_i) \vdots \\ \langle \text{Mem}_1, c_1 \rangle \mapsto_{\text{Env}} \text{Mem} \end{array} \quad \begin{array}{c} (C_j) \vdots \\ \langle \text{Mem}, c_2 \rangle \mapsto_{\text{Env}} \text{Mem}_2 \end{array}}{\langle \text{Mem}_1, c_1 ; c_2 \rangle \mapsto_{\text{Env}} \text{Mem}_2}\)

by induction hypothesis, we have $\langle c_1, \text{Mem}_1 \rangle \mapsto^*_{\text{Env}} \text{Mem}$ and $\langle c_2, \text{Mem} \rangle \mapsto^*_{\text{Env}} \text{Mem}_2$. Hence, we have two transition sequences:

\(\langle c_1, \text{Mem}_1 \rangle \mapsto_{\text{Env}} \cdots \mapsto_{\text{Env}} \langle \text{skip}, \text{Mem} \rangle\) [4.4]

\(\langle c_2, \text{Mem} \rangle \mapsto_{\text{Env}} \cdots \mapsto_{\text{Env}} \langle \text{skip}, \text{Mem}_2 \rangle\) [4.5]

Applying rule $S_2$ to each of the transitions in sequence [4.4], we can build the following sequence:

\(\langle c_1 ; c_2, \text{Mem}_1 \rangle \mapsto_{\text{Env}} \cdots \mapsto_{\text{Env}} \langle \text{skip} ; c_2, \text{Mem} \rangle\) [4.6]

and since rule $S_3$ allows us to construct the transition:

\(\langle \text{skip} ; c_2, \text{Mem} \rangle \mapsto_{\text{Env}} \langle c_2, \text{Mem} \rangle\) [4.7]

by concatenating [4.6], [4.7] and [4.5], we get the following sequence:

\(\langle c_1 ; c_2, \text{Mem}_1 \rangle \mapsto_{\text{Env}} \cdots \mapsto_{\text{Env}} \langle \text{skip}, \text{Mem}_2 \rangle\)

and thus $\langle c_1 ; c_2, \text{Mem}_1 \rangle \mapsto^*_{\text{Env}} \text{Mem}_2$.

- If $\langle \text{Mem}_1, c \rangle \mapsto_{\text{Env}} \text{Mem}_2$ has been obtained using rule $C_4$:

\((C_4) \frac{\llbracket e \rrbracket^{\text{Mem}_1}_{\text{Env}} = (\text{true}, \text{Mem}) \quad (C_i) \vdots \\ \langle \text{Mem}, c_1 \rangle \mapsto_{\text{Env}} \text{Mem}_2}{\langle \text{Mem}_1, \text{if } e \text{ then } c_1 \text{ else } c_2 \rangle \mapsto_{\text{Env}} \text{Mem}_2}\)