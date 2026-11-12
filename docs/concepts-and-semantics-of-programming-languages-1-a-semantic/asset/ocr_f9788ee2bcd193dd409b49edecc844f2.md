$(C_1) \frac{\text{Env}(x) = r \in \mathbf{R} \quad \llbracket e \rrbracket_{\text{Env}}^{\text{Mem}_1} = (v, \text{Mem}_2) \in \mathbf{V} \times \mathbf{M}}{\langle \text{Mem}_1, x := e \rangle \rightarrow_{\text{Env}} \text{Mem}_2[r := v]}$

$(C_2) \frac{}{\langle \text{Mem}, \text{\textbf{skip}} \rangle \rightarrow_{\text{Env}} \text{Mem}}$

$(C_3) \frac{\langle \text{Mem}, c_1 \rangle \rightarrow_{\text{Env}} \text{Mem}_1 \quad \langle \text{Mem}_1, c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}_2}{\langle \text{Mem}, c_1 ; c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}_2}$

$(C_4) \frac{\llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{\textit{true}}, \text{Mem}_1) \quad \langle \text{Mem}_1, c_1 \rangle \rightarrow_{\text{Env}} \text{Mem}_2}{\langle \text{Mem}, \text{\textbf{if}} e \text{\textbf{then}} c_1 \text{\textbf{else}} c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}_2}$

$(C_5) \frac{\llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{\textit{false}}, \text{Mem}_1) \quad \langle \text{Mem}_1, c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}_2}{\langle \text{Mem}, \text{\textbf{if}} e \text{\textbf{then}} c_1 \text{\textbf{else}} c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}_2}$

$(C_6) \frac{\llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{\textit{true}}, \text{Mem}_1) \quad \langle \text{Mem}_1, c \rangle \rightarrow_{\text{Env}} \text{Mem}_2 \quad \langle \text{Mem}_2, \text{\textbf{while}} e \text{\textbf{do}} c \rangle \rightarrow_{\text{Env}} \text{Mem}_3}{\langle \text{Mem}, \text{\textbf{while}} e \text{\textbf{do}} c \rangle \rightarrow_{\text{Env}} \text{Mem}_3}$

$(C_7) \frac{\llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{\textit{false}}, \text{Mem}_1)}{\langle \text{Mem}, \text{\textbf{while}} e \text{\textbf{do}} c \rangle \rightarrow_{\text{Env}} \text{Mem}_1}$

# Box 4.2. Big-step operational semantics of Lang3

Rule \(C_2\) simply indicates that executing the instruction skip does not affect the state. Rule \(C_3\) describes the evolution of the memory during the execution of a sequence of two instructions. Rules \(C_4\) and \(C_5\) specify the memory transformation associated with a conditional; they contain a condition to apply them which depends on the result of the evaluation of expression \(e\).

Finally, rules \(C_6\) and \(C_7\) relate to the **while** \(e\) **do** \(c\) loop, and specify how many times the execution of the body \(c\) of the loop is to be repeated, according to the value of the conditional expression \(e\). Unlike the other instructions, **while** \(e\) **do** \(c\) allows us to write programs where execution does not terminate, and which, consequently, cannot be described by an inference tree since such a tree is a finite tree. For such non-terminating programs \(c\), there is no memory state \(\text{Mem}'\) such that \(\langle \text{Mem}, c \rangle \rightarrow_{\text{Env}} \text{Mem}'\). For example, taking the program \(\text{\textit{foo}} = \text{\textbf{while}} \text{\textit{true}} \text{\textbf{do}} c\), there is no memory state \(\text{Mem}'\) such that \(\langle \text{Mem}, \text{\textit{foo}} \rangle \rightarrow_{\text{Env}} \text{Mem}'\). In fact, this program contains an infinite loop, hence it is impossible to build a finite execution tree for it:

$\frac{ \begin{array}{c} \llbracket \text{\textit{true}} \rrbracket_{\text{Env}}^{\text{Mem}} \\ (C_2) \frac{ \langle \text{Mem}, \text{\textbf{skip}} \rangle \rightarrow_{\text{Env}} \text{Mem} }{ } \\ (C_6) \frac{ \vdots }{ \langle \text{Mem}, \text{\textit{foo}} \rangle \rightarrow_{\text{Env}} ? } \end{array} }{ \langle \text{Mem}, \text{\textit{foo}} \rangle \rightarrow_{\text{Env}} ? }$ \\
\((C_6) \frac{ \dots }{ \langle \text{Mem}, \text{\textit{foo}} \rangle \rightarrow_{\text{Env}} ? }\)

since \(\langle \text{Mem}, \text{\textit{foo}} \rangle \rightarrow_{\text{Env}} ?\) occurs both in the root and in the premise. Big-step operational semantics uses inference trees – which are finite – and cannot, therefore, take account of the execution of non-terminating programs. A finer approach,