# DEFINITION 4.1.- Equivalent programs: two programs \(c_1\) and \(c_2\) are equivalent, noted \(c_1 \equiv c_2\), if and only if:

\(\forall Env \in \mathbf{E} \forall Mem, Mem' \in \mathbf{M} \langle Mem, c_1 \rangle \Rightarrow_{Env} Mem' \Leftrightarrow \langle Mem, c_2 \rangle \Rightarrow_{Env} Mem'\)

# EXAMPLE 4.5.- Consider the following two programs:

\(c_1 : (if \ e \ then \ c \ else \ c') ; c'' \quad c_2 : if \ e \ then \ c; c'' \ else \ c' ; c''\)

To prove that \(c_1 \equiv c_2\), we build an inference tree for \(\langle Mem, c_1 \rangle \Rightarrow_{Env} Mem'\) from an inference tree for \(\langle Mem, c_2 \rangle \Rightarrow_{Env} Mem'\) and vice versa. To this end, we examine all the possible rules for constructing such a tree, distinguishing between two possible cases for the evaluation of \(e\). We must then simply “combine” the inference trees. If \(e\) is evaluated as \(true\), then, from the tree

![](bbox=[0.112, 0.362, 0.885, 0.459])

we can obtain the tree

![](bbox=[0.112, 0.503, 0.885, 0.599])

and vice versa. A similar reasoning applies when \(e\) is evaluated as \(false\).

# EXAMPLE 4.6.- Consider the following two programs:

\(c_1 : c ; (if \ e \ then \ c' else \ c'') \quad c_2 : if \ e \ then \ c; c' else \ c; c''\)

To show that \(c_1 \not\equiv c_2\), it is enough to build a counterexample where \(c\), \(c'\) and \(c''\) are instructions \(x := 0\), \(x := \downarrow x + 1\) and \(x := \downarrow x + 5\), respectively, and where \(e\) is the expression \(1 \le \downarrow x\). Starting from a state \((Env, Mem)\) such that \(Env(x) = r_x \in \mathbf{R}\) and \(Mem(r_x) = 2\), we obtain:

\(\langle Mem, c ; (if \ e \ then \ c' else \ c'') \rangle \Rightarrow_{Env} Mem_1 \text{ with } Mem_1(x) = 5\)

\(\langle Mem, if \ e \ then \ c; c' else \ c; c'' \rangle \Rightarrow_{Env} Mem_2 \text{ with } Mem_2(x) = 1\)