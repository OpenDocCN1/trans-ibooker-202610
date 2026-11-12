

在 e 求值为 true 的情况下，我们必须证明两个内存状态 Mem1 和 Mem2 之间的相等性，这两个状态以如下方式获得（注意表达式的求值是确定性的）：

```
[e]EnvMem = (true, Mem')    :
(Ck) <Mem1, while e do c>Env Mem2
(C6) <Mem, while e do c>Env Mem2

[e]EnvMem = (true, Mem')    :
(Ck) <Mem1, while e do c>Env Mem2
(C6) <Mem, while e do c>Env Mem2
```

关于 c 的归纳假设确立了 Mem1 = Mem1 的相等性。要推导 Mem2 = Mem2，还需要关于 while e do c 指令的归纳假设，但无法获得它。

因此，通过程序结构进行归纳推理不足以证明程序执行的确定性。在这种情况下，我们不应使用由 Lang3 的归纳定义生成的归纳方案，而应使用由定义关系 Mem, c Env Mem 的推理系统生成的归纳方案。这导致对执行而非程序进行归纳推理。该归纳方案的正式定义如 Box 4.4 所示。

```
if Mem M Env(x) = r R and [e]EnvMem = (v, Mem') V M
Mem, x := e Env Mem'[r := v]
and Mem M P(< Mem, skip > Env Mem)
and c1, c2 Lang3 Mem, Mem1, Mem2 M
(P(< Mem, c1 > Env Mem1) and P(< Mem1, c2 > Env Mem2))
< Mem, c1 ; c2 > Env Mem2)
and c1, c2 Lang3 Mem, Mem1, Mem2 M e Exp3
[e]EnvMem = (true, Mem1) and P(< Mem1, c1 > Env Mem2))
P(< Mem, if e then c1 else c2 > Env Mem2)
and c1, c2 Lang3 Mem, Mem1, Mem2 M e Exp3
[e]EnvMem = (false, Mem1) and P(< Mem1, c2 > Env Mem2))
P(< Mem, if e then c1 else c2 > Env Mem2)
and c Lang3 Mem, Mem1, Mem2, Mem3 M e Exp3
[e]EnvMem = (true, Mem1) and P(< Mem1, c > Env Mem2)
and P(< Mem2, while e do c > Env Mem3))
P(< Mem, while e do c > Env Mem3)
and c Lang3 Mem, Mem1 M e Exp3
[e]EnvMem = (true, Mem1) P(< Mem, while e do c > Env Mem1)
then c Lang3 Mem, Mem' M P(< Mem, c > Env Mem')
```

### Box 4.4. 关于 Mem, c Env Mem 的结构归纳推理方案

证明.–（这是命题 4.1 的证明）

要证明的属性 \(P(\langle Mem, c \rangle \rightarrow_{Env} Mem_1)\) 定义为：

\(\forall Mem_2 \in \mathbf{M} \langle Mem, c \rangle \rightarrow_{Env} Mem_2 \Rightarrow Mem_1 = Mem_2\)

我们证明以下几点。

- \(\langle Mem, x := e \rangle \rightarrow_{Env} Mem_1\) 的树形结构只能为：

\((C_1) \frac{Env(x) = r \in \mathbf{R} \quad \llbracket e \rrbracket_{Env}^{Mem} = (v, Mem') \in \mathbf{V} \times \mathbf{M}}{\langle Mem, x := e \rangle \rightarrow_{Env} Mem'[r := v]}\)

故 \(Mem_1 = Mem'[r := v]\)。由于表达式求值是确定性的，这是 \(\langle Mem, x := e \rangle \rightarrow_{Env} Mem_2\) 唯一可能的树，因此 \(Mem_1 = Mem_2\)。

- \(\langle Mem, \mathbf{skip} \rangle \rightarrow_{Env} Mem_1\) 的树形结构只能为：

\((C_2) \frac{}{\langle Mem, \mathbf{skip} \rangle \rightarrow_{Env} Mem}\)

这也是 \(\langle Mem, \mathbf{skip} \rangle \rightarrow_{Env} Mem_2\) 唯一可能的树，因此 \(Mem_1 = Mem = Mem_2\)。

- \(\langle Mem, c_1 ; c_2 \rangle \rightarrow_{Env} Mem_1\) 的树必须采用如下形式：

\((C_i) \vdots \quad (C_j) \vdots\)
\((C_3) \frac{\langle Mem, c_1 \rangle \rightarrow_{Env} Mem' \quad \langle Mem', c_2 \rangle \rightarrow_{Env} Mem_1}{\langle Mem, c_1 ; c_2 \rangle \rightarrow_{Env} Mem_1}\)

同样，\(\langle Mem, c_1 ; c_2 \rangle \rightarrow_{Env} Mem_2\) 的树必须采用如下形式：

\((C_{i'}) \vdots \quad (C_{j'}) \vdots\)
\((C_3) \frac{\langle Mem, c_1 \rangle \rightarrow_{Env} Mem'' \quad \langle Mem'', c_2 \rangle \rightarrow_{Env} Mem_2}{\langle Mem, c_1 ; c_2 \rangle \rightarrow_{Env} Mem_2}\)

根据归纳假设，我们有 \(P(\langle Mem, c_1 \rangle \rightarrow_{Env} Mem')\)，因此 \(Mem' = Mem''\)；并且仍根据归纳假设，我们有 \(P(\langle Mem', c_2 \rangle \rightarrow_{Env} Mem_1)\)，最终得到 \(Mem_1 = Mem_2\)。

- 当 \(\llbracket e \rrbracket_{Env}^{Mem} = (true, Mem')\) 时，\(\langle Mem, \mathbf{if} e \mathbf{then} c_1 \mathbf{else} c_2 \rangle \rightarrow_{Env} Mem_1\) 的树必须采用如下形式：

\(\llbracket e \rrbracket_{Env}^{Mem} = (true, Mem') \quad (C_i) \vdots\)
\((C_4) \frac{}{\langle Mem, \mathbf{if} e \mathbf{then} c_1 \mathbf{else} c_2 \rangle \rightarrow_{Env} Mem_1}\)

同样，\(\langle \text{Mem}, \text{if } e \text{ then } c_1 \text{ else } c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}_2\) 的树必须采用如下形式:

```
\(\begin{array}{c}
\llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{true}, \text{Mem}') \qquad (C_j) \qquad \vdots \\
\hline \\
(C_4) \quad \langle \text{Mem}, \text{if } e \text{ then } c_1 \text{ else } c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}_2
\end{array}\)
```

根据归纳假设，我们有 \(P(\langle \text{Mem}', c_1 \rangle \rightarrow_{\text{Env}} \text{Mem}_1)\)，因此 \(\text{Mem}_1 = \text{Mem}_2\)。当 \(\llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{false}, \text{Mem}')\) 时的推理过程类似。

- 当 \(\llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{true}, \text{Mem}')\) 时，\(\langle \text{Mem}, \text{while } e \text{ do } c \rangle \rightarrow_{\text{Env}} \text{Mem}_1\) 的树必须采用如下形式：

```
\(\begin{array}{c}
\llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{true}, \text{Mem}') \qquad (C_i) \qquad \vdots \qquad (C_j) \qquad \vdots \\
\hline \\
(C_6) \quad \langle \text{Mem}, \text{while } e \text{ do } c \rangle \rightarrow_{\text{Env}} \text{Mem}_1
\end{array}\)
```

同样，\(\langle \text{Mem}, \text{while } e \text{ do } c \rangle \rightarrow_{\text{Env}} \text{Mem}_2\) 的树必须采用如下形式：

```
\(\begin{array}{c}
\llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{true}, \text{Mem}') \qquad (C_{i'}) \qquad \vdots \qquad (C_{j'}) \qquad \vdots \\
\hline \\
(C_6) \quad \langle \text{Mem}, \text{while } e \text{ do } c \rangle \rightarrow_{\text{Env}} \text{Mem}_2
\end{array}\)
```

根据归纳假设，我们有 \(P(\langle \text{Mem}', c \rangle \rightarrow_{\text{Env}} \text{Mem}'_1)\)，因此 \(\text{Mem}'_1 = \text{Mem}'_2\)；并且再次根据归纳假设，我们有 \(P(\langle \text{Mem}'_1, \text{while } e \text{ do } c \rangle \rightarrow_{\text{Env}} \text{Mem}_1)\)，最终得到 \(\text{Mem}_1 = \text{Mem}_2\)。

- 当 \(\llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{false}, \text{Mem}')\) 时，\(\langle \text{Mem}, \text{while } e \text{ do } c \rangle \rightarrow_{\text{Env}} \text{Mem}_1\) 的树必须采用如下形式：

```
\(\begin{array}{c}
(C_7) \quad \llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{false}, \text{Mem}') \\
\hline \\
\langle \text{Mem}, \text{while } e \text{ do } c \rangle \rightarrow_{\text{Env}} \text{Mem}'
\end{array}\)
```

这也是 \(\langle \text{Mem}, \text{while } e \text{ do } c \rangle \rightarrow_{\text{Env}} \text{Mem}_2\) 唯一可能的树，因此我们得到 \(\text{Mem}_1 = \text{Mem}' = \text{Mem}_2\)。

## 4.5.4. 大步与小步

大步和小步运行语义提供了描述 \(\text{Lang}_3\) 中程序执行的两种方式。这些描述基于不同的形式主义，但从语义角度来看是等价的。正如我们从示例 4.3 和 4.4 所见，同一程序的有限执行在大步语义中可由一棵树描述，在小步语义中可由一个转移序列描述；两种方法得到的结果是一致的。我们现在正式证明这种语义等价性。我们首先证明以下引理。

引理 4.1.- 如果 \(\langle c_1 ; c_2, \text{Mem} \rangle \leftrightarrow^k_{\text{Env}} \text{Mem}''\)，则存在 \(\text{Mem}' \in \mathbf{M}\) 和两个整数 \(k_1\) 和 \(k_2\)，使得 \(\langle c_1, \text{Mem} \rangle \leftrightarrow^{k_1}_{\text{Env}} \text{Mem}'\) 且 \(\langle c_2, \text{Mem}' \rangle \leftrightarrow^{k_2}_{\text{Env}} \text{Mem}''\)，其中 \(k = k_1 + k_2 + 1\)。

证明.- 我们对 \(k\) 进行归纳。如果 \(k=0$，则该属性平凡成立（因为在这种情况下，假设 \(\langle c_1 ; c_2, \text{Mem} \rangle \leftrightarrow^0_{\text{Env}} \text{Mem}\) 无法验证，因为 \(\langle c_1 ; c_2, \text{Mem} \rangle\) 不是终端配置）。如果 \(k = k_0 + 1\)，则有两种可能的情况：

- 1) 如果假设中的转移序列为：

\(\langle c_1 ; c_2, \text{Mem} \rangle \leftrightarrow_{\text{Env}} \langle c'_1 ; c_2, \text{Mem}_1 \rangle \leftrightarrow_{\text{Env}} \dots \leftrightarrow_{\text{Env}} \langle \text{skip}, \text{Mem}'' \rangle\)
长度为 \(k_0\) 的序列

其中 \(\langle c_1, \text{Mem} \rangle \leftrightarrow_{\text{Env}} \langle c'_1, \text{Mem}_1 \rangle\)，那么，根据归纳假设，存在两个整数 \(k'_1\) 和 \(k'_2\)，使得 \(\langle c'_1, \text{Mem}_1 \rangle \leftrightarrow^{k'_1}_{\text{Env}} \text{Mem}'_1\) 且 \(\langle c_2, \text{Mem}'_1 \rangle \leftrightarrow^{k'_2}_{\text{Env}} \text{Mem}''\)，其中 \(k_0 = k'_1 + k'_2 + 1\)。我们构造如下长度为 \(k'_1 + 1\) 的序列：

\(\langle c_1, \text{Mem} \rangle \leftrightarrow_{\text{Env}} \langle c'_1, \text{Mem}_1 \rangle \leftrightarrow^{k'_1}_{\text{Env}} \text{Mem}'_1\)

这使我们可以得出 \(k_1 = k'_1 + 1\) 和 \(k_2 = k'_2\)。

- 2) 如果假设中的转移序列为：

\(\langle \text{skip} ; c_2, \text{Mem} \rangle \leftrightarrow_{\text{Env}} \langle c_2, \text{Mem} \rangle \leftrightarrow_{\text{Env}} \dots \leftrightarrow_{\text{Env}} \langle \text{skip}, \text{Mem}'' \rangle\)
长度为 \(k_0\) 的序列

那么我们可以直接得出 \(k_1 = 0\) 和 \(k_2 = k_0\)。□

现在可以证明大步和小步运行语义之间的等价性了。

命题 4.2.- \(\forall c \in \mathbf{Lang}_3 \forall \text{Env} \in \mathbf{E} \forall \text{Mem}_1, \text{Mem}_2 \in \mathbf{M}\)：
\(\langle \text{Mem}_1, c \rangle \to^*_{\text{Env}} \text{Mem}_2 \Leftrightarrow \langle c, \text{Mem}_1 \rangle \leftrightarrow^*_{\text{Env}} \text{Mem}_2\)

证明.-

\((\rightarrow)\) 我们对 \(\langle \text{Mem}_1, c \rangle \to^*_{\text{Env}} \text{Mem}_2\) 进行归纳（应用 Box 4.4 中提出的归纳方案）。

