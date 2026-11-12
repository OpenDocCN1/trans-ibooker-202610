

那么我们得出结论，因为我们可以构造树：

\(\begin{array}{c} \unknown \\
\begin{array}{c} [[e]]_{\text{Env}}^{\text{Mem}} = (\text{true}, \text{Mem}_1) \quad (C_i) \frac{1}{\langle \text{Mem}_1, c \rangle \rightarrow_{\text{Env}} \text{Mem}_2 = (\text{true}, \text{Mem}')} \quad [[\text{not } e]]_{\text{Env}}^{\text{Mem}_2} \\ (C_4) \quad \underline{\langle \text{Mem}_1, c'_2 \rangle \rightarrow_{\text{Env}} \text{Mem}'} \quad (C_8) \\ \underline{\langle \text{Mem}, c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}'} \end{array} \end{array}\)

情况 1：如果 \(\langle \text{Mem}, c_1 \rangle \rightarrow_{\text{Env}} \text{Mem}'\) 已使用规则 \(C_7\) 得到：

\(\(C_7) \frac{[[e]]_{\text{Env}}^{\text{Mem}} = (\text{false}, \text{Mem}')}{\langle \text{Mem}, c_1 \rangle \rightarrow_{\text{Env}} \text{Mem}'}\)

我们直接得出结论，构造树：

\(\(C_5) \frac{[[e]]_{\text{Env}}^{\text{Mem}} = (\text{false}, \text{Mem}') \quad (C_2) \frac{}{\langle \text{Mem}', \text{skip} \rangle \rightarrow_{\text{Env}} \text{Mem}'}}{\langle \text{Mem}, c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}'}\)

对于 (P3)，如同对待 (P1) 一样，我们对 \(\langle \text{Mem}, c_3 \rangle \rightarrow_{\text{Env}} \text{Mem}'\) 进行结构归纳，以证明 \(\langle \text{Mem}, c_4 \rangle \rightarrow_{\text{Env}} \text{Mem}'\)。因此，只需考虑规则 \(C_8\) 和 \(C_9\)。

情况 1：如果 \(\langle \text{Mem}, c_3 \rangle \rightarrow_{\text{Env}} \text{Mem}'\) 已使用规则 \(C_8\) 得到：

\(\begin{array}{c} \unknown \\
\begin{array}{c} (C_i) \frac{1}{\langle \text{Mem}, c \rangle \rightarrow_{\text{Env}} \text{Mem}_1} \quad [[e]]_{\text{Env}}^{\text{Mem}_1} = (\text{true}, \text{Mem}') \\ \underline{\langle \text{Mem}, c_3 \rangle \rightarrow_{\text{Env}} \text{Mem}'} \end{array} \end{array}\)

我们可以得出结论，因为我们可以构造树：

\(\begin{array}{c} \unknown \\
\begin{array}{c} (C_i) \frac{1}{\langle \text{Mem}, c \rangle \rightarrow_{\text{Env}} \text{Mem}_1} \quad (C_7) \frac{[[\text{not } e]]_{\text{Env}}^{\text{Mem}_1} = (\text{false}, \text{Mem}')}{\langle \text{Mem}_1, c'_1 \rangle \rightarrow_{\text{Env}} \text{Mem}'} \\ \underline{\langle \text{Mem}, c_4 \rangle \rightarrow_{\text{Env}} \text{Mem}'} \end{array} \end{array}\)

情况 2：如果 \(\langle \text{Mem}, c_3 \rangle \rightarrow_{\text{Env}} \text{Mem}'\) 已使用规则 \(C_9\) 得到：

\(\begin{array}{c} \unknown \\
\begin{array}{c} (C_i) \frac{1}{\langle \text{Mem}, c \rangle \rightarrow_{\text{Env}} \text{Mem}_1} \quad [[e]]_{\text{Env}}^{\text{Mem}_1} = (\text{false}, \text{Mem}_2) \quad (C_k) \frac{\vdots}{\langle \text{Mem}_2, c_3 \rangle \rightarrow_{\text{Env}} \text{Mem}'} \\ \underline{\langle \text{Mem}, c_3 \rangle \rightarrow_{\text{Env}} \text{Mem}'} \end{array} \end{array}\)

那么，根据归纳假设，存在 $\langle \text{Mem}_2, c_4 \rangle \rightarrow_{\text{Env}} \text{Mem}'$ 的推导树：

\[ \begin{array}{c} \vee_2 \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \vee_3 \\ (C_j) \frac{\langle \text{Mem}_2, c \rangle \rightarrow_{\text{Env}} \text{Mem}_3}{\langle \text{Mem}_2, c_4 \rangle \rightarrow_{\text{Env}} \text{Mem}'} \quad (C_k) \frac{\langle \text{Mem}_3, c'_1 \rangle \rightarrow_{\text{Env}} \text{Mem}'}{} \\ \hline \langle \text{Mem}_2, c_4 \rangle \rightarrow_{\text{Env}} \text{Mem}' \end{array} \]

并且我们可以构造树 $\vee_4$：

\[ \begin{array}{c} \llbracket \text{not} \ e \rrbracket_{\text{Env}}^{\text{Mem}_1} = (\text{true}, \text{Mem}_2) \\ \quad \quad (C_j) \frac{\langle \text{Mem}_2, c \rangle \rightarrow_{\text{Env}} \text{Mem}_3}{} \vee_2 \quad \quad (C_k) \frac{\langle \text{Mem}_3, c'_1 \rangle \rightarrow_{\text{Env}} \text{Mem}'}{} \vee_3 \\ \hline (C_6) \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \langle \text{Mem}_1, c'_1 \rangle \rightarrow_{\text{Env}} \text{Mem}' \end{array} \]

这意味着我们可以通过构造树来得出结论：

\[ \begin{array}{c} \vee_1 \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \vee_4 \\ (C_i) \frac{\langle \text{Mem}, c \rangle \rightarrow_{\text{Env}} \text{Mem}_1}{} \quad (C_6) \frac{\langle \text{Mem}_1, c'_1 \rangle \rightarrow_{\text{Env}} \text{Mem}'}{} \\ \hline (C_3) \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \langle \text{Mem}, c_4 \rangle \rightarrow_{\text{Env}} \text{Mem}' \end{array} \]

对于 (P2)，如果 $\langle \text{Mem}, c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}'$，则存在两种可能的情况：

情况 1：如果 $\langle \text{Mem}, c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}'$ 的树已使用规则 $C_4$ 得到：

\[ \begin{array}{c} \vdots \\ \llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{true}, \text{Mem}_1) \quad (C_i) \frac{\langle \text{Mem}_1, \text{repeat } c \text{ until not } e \rangle \rightarrow_{\text{Env}} \text{Mem}'}{} \\ \hline (C_4) \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \langle \text{Mem}, c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}' \end{array} \]

那么，根据 (P3)，存在 $\langle \text{Mem}_1, c; \text{ repeat } c \text{ until not } e \rangle \rightarrow_{\text{Env}} \text{Mem}'$ 的树，从中可以获得 $\langle \text{Mem}_1, c; \text{ while not not } e \text{ do } c \rangle \rightarrow_{\text{Env}} \text{Mem}'$ 的树：

\[ \begin{array}{c} \vee_1 \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \vee_2 \\ (C_i) \frac{\langle \text{Mem}_1, c \rangle \rightarrow_{\text{Env}} \text{Mem}_2}{} \quad (C_j) \frac{\langle \text{Mem}_2, \text{while } e \text{ do } c \rangle \rightarrow_{\text{Env}} \text{Mem}'}{} \\ \hline (C_3) \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \langle \text{Mem}_1, c; \text{ while } e \text{ do } c \rangle \rightarrow_{\text{Env}} \text{Mem}' \end{array} \]

我们通过构造树来得出结论：

\[ \begin{array}{c} \llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{true}, \text{Mem}_1) \quad \vee_1 \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \vee_2 \\ (C_i) \frac{\langle \text{Mem}_1, c \rangle \rightarrow_{\text{Env}} \text{Mem}_2}{} \quad (C_j) \frac{\langle \text{Mem}_2, c_1 \rangle \rightarrow_{\text{Env}} \text{Mem}'}{} \\ \hline (C_6) \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \langle \text{Mem}, c_1 \rangle \rightarrow_{\text{Env}} \text{Mem}' \end{array} \]

情况 2：如果 $\langle \text{Mem}, c_2 \rangle \Rightarrow_{\text{Env}} \text{Mem}'$ 的树是使用规则 $C_5$ 得到的：

$(C_5) \frac{[[e]]_{\text{Env}}^{\text{Mem}} = (\text{false}, \text{Mem}') \quad \langle \text{Mem}', \text{skip} \rangle \Rightarrow_{\text{Env}} \text{Mem}'}{\langle \text{Mem}, c_2 \rangle \Rightarrow_{\text{Env}} \text{Mem}'}$

那么我们通过构造树来得出结论：

$(C_7) \frac{[[e]]_{\text{Env}}^{\text{Mem}} = (\text{false}, \text{Mem}')}{\langle \text{Mem}, c_1 \rangle \Rightarrow_{\text{Env}} \text{Mem}'}$

对于 (P4)，如果 $\langle \text{Mem}, c_4 \rangle \Rightarrow_{\text{Env}} \text{Mem}'$，我们有如下树：

$(C_i) \frac{\nabla 1}{\langle \text{Mem}, c \rangle \Rightarrow_{\text{Env}} \text{Mem}_1} \qquad (C_j) \frac{\vdots}{\langle \text{Mem}_1, \text{while not } e \text{ do } c \rangle \Rightarrow_{\text{Env}} \text{Mem}'} \quad (C_3) \overline{\langle \text{Mem}, c_4 \rangle \Rightarrow_{\text{Env}} \text{Mem}'}$

根据 (P4)，存在以下形式的树：

$\langle \text{Mem}_1, \text{if not } e \text{ then repeat } c \text{ until not not } e \text{ else skip} \rangle \Rightarrow_{\text{Env}} \text{Mem}'$

从中我们可以获得以下形式的树：

$\langle \text{Mem}_1, \text{if not } e \text{ then repeat } c \text{ until } e \text{ else skip} \rangle \Rightarrow_{\text{Env}} \text{Mem}'$

通过使用规则 $C_4$ 或规则 $C_5$。

情况 1：如果它是使用规则 $C_4$ 得到的：

$\frac{\nabla 2}{} \quad [[\text{not } e]]_{\text{Env}}^{\text{Mem}_1} = (\text{true}, \text{Mem}_2) \quad (C_k) \frac{\langle \text{Mem}_2, \text{repeat } c \text{ until } e \rangle \Rightarrow_{\text{Env}} \text{Mem}'}{} \quad (C_4) \overline{\langle \text{Mem}_1, \text{if not } e \text{ then repeat } c \text{ until } e \text{ else skip} \rangle \Rightarrow_{\text{Env}} \text{Mem}'}$

那么我们通过构造树来得出结论：

$(C_i) \frac{\nabla 1}{\langle \text{Mem}, c \rangle \Rightarrow_{\text{Env}} \text{Mem}_1} \quad [[e]]_{\text{Env}}^{\text{Mem}_1} = (\text{false}, \text{Mem}_2) \quad (C_k) \frac{\nabla 2}{\langle \text{Mem}_2, c_3 \rangle \Rightarrow_{\text{Env}} \text{Mem}'} \quad (C_9) \overline{\langle \text{Mem}, c_3 \rangle \Rightarrow_{\text{Env}} \text{Mem}'}$

情况 2：如果它是使用规则 $C_5$ 得到的：

$[[\text{not } e]]_{\text{Env}}^{\text{Mem}_1} = (\text{false}, \text{Mem}') \quad (C_2) \frac{\langle \text{Mem}', \text{skip} \rangle \Rightarrow_{\text{Env}} \text{Mem}'}{} \quad (C_5) \overline{\langle \text{Mem}_1, \text{if not } e \text{ then repeat } c \text{ until } e \text{ else skip} \rangle \Rightarrow_{\text{Env}} \text{Mem}'}$

那么我们通过构造树来得出结论：

$(C_i) \frac{\nabla 1}{\langle \text{Mem}, c \rangle \Rightarrow_{\text{Env}} \text{Mem}_1} \quad [[e]]_{\text{Env}}^{\text{Mem}_1} = (\text{true}, \text{Mem}') \quad (C_8) \overline{\langle \text{Mem}, c_3 \rangle \Rightarrow_{\text{Env}} \text{Mem}'}$

