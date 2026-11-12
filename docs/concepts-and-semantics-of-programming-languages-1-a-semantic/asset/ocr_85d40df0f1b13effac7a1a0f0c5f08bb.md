then, by induction hypothesis, there exists an inference tree for $\langle \text{Mem}_2, c_4 \rangle \rightarrow_{\text{Env}} \text{Mem}'$:

```
\[ \begin{array}{c} \vee_2 \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \vee_3 \\ (C_j) \frac{\langle \text{Mem}_2, c \rangle \rightarrow_{\text{Env}} \text{Mem}_3}{\langle \text{Mem}_2, c_4 \rangle \rightarrow_{\text{Env}} \text{Mem}'} \quad (C_k) \frac{\langle \text{Mem}_3, c'_1 \rangle \rightarrow_{\text{Env}} \text{Mem}'}{} \\ \hline \langle \text{Mem}_2, c_4 \rangle \rightarrow_{\text{Env}} \text{Mem}' \end{array} \]
```

and we can construct the tree $\forall_4$:

```
\[ \begin{array}{c} \llbracket \text{not} \ e \rrbracket_{\text{Env}}^{\text{Mem}_1} = (\text{true}, \text{Mem}_2) \\ \quad \quad (C_j) \frac{\langle \text{Mem}_2, c \rangle \rightarrow_{\text{Env}} \text{Mem}_3}{} \vee_2 \quad \quad (C_k) \frac{\langle \text{Mem}_3, c'_1 \rangle \rightarrow_{\text{Env}} \text{Mem}'}{} \vee_3 \\ \hline (C_6) \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \langle \text{Mem}_1, c'_1 \rangle \rightarrow_{\text{Env}} \text{Mem}' \end{array} \]
```

meaning that we can conclude by constructing the tree:

```
\[ \begin{array}{c} \vee_1 \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \vee_4 \\ (C_i) \frac{\langle \text{Mem}, c \rangle \rightarrow_{\text{Env}} \text{Mem}_1}{} \quad (C_6) \frac{\langle \text{Mem}_1, c'_1 \rangle \rightarrow_{\text{Env}} \text{Mem}'}{} \\ \hline (C_3) \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \langle \text{Mem}, c_4 \rangle \rightarrow_{\text{Env}} \text{Mem}' \end{array} \]
```

For (P2), if $\langle \text{Mem}, c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}'$, then two cases are possible:

Case 1: If the tree for $\langle \text{Mem}, c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}'$ has been obtained using rule $C_4$:

```
\[ \begin{array}{c} \vdots \\ \llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{true}, \text{Mem}_1) \quad (C_i) \frac{\langle \text{Mem}_1, \text{repeat } c \text{ until not } e \rangle \rightarrow_{\text{Env}} \text{Mem}'}{} \\ \hline (C_4) \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \langle \text{Mem}, c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}' \end{array} \]
```

then, according to (P3), there exists a tree for $\langle \text{Mem}_1, c; \text{ while not not } e \text{ do } c \rangle \rightarrow_{\text{Env}} \text{Mem}'$, from which a tree for $\langle \text{Mem}_1, c; \text{ while } e \text{ do } c \rangle \rightarrow_{\text{Env}} \text{Mem}'$ can be obtained:

```
\[ \begin{array}{c} \vee_1 \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \vee_2 \\ (C_i) \frac{\langle \text{Mem}_1, c \rangle \rightarrow_{\text{Env}} \text{Mem}_2}{} \quad (C_j) \frac{\langle \text{Mem}_2, \text{while } e \text{ do } c \rangle \rightarrow_{\text{Env}} \text{Mem}'}{} \\ \hline (C_3) \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \langle \text{Mem}_1, c; \text{ while } e \text{ do } c \rangle \rightarrow_{\text{Env}} \text{Mem}' \end{array} \]
```

We conclude by constructing the tree:

```
\[ \begin{array}{c} \llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{true}, \text{Mem}_1) \quad \vee_1 \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \vee_2 \\ (C_i) \frac{\langle \text{Mem}_1, c \rangle \rightarrow_{\text{Env}} \text{Mem}_2}{} \quad (C_j) \frac{\langle \text{Mem}_2, c_1 \rangle \rightarrow_{\text{Env}} \text{Mem}'}{} \\ \hline (C_6) \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \langle \text{Mem}, c_1 \rangle \rightarrow_{\text{Env}} \text{Mem}' \end{array} \]
```