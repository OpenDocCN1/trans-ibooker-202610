

$$ \langle e_2, \text{Env} \rangle = \langle e_2^0, \text{Env} \rangle \leftrightarrow \langle e_2^1, \text{Env} \rangle \leftrightarrow \langle e_2^2, \text{Env} \rangle \leftrightarrow \cdots \leftrightarrow \langle e_2^{k_2}, \text{Env} \rangle = \langle n_2, \text{Env} \rangle $$

规则 $AS_5$ 允许我们将序列 [A.1] 中的每个转移 $\langle e_1^i, \text{Env} \rangle \leftrightarrow \langle e_1^{i+1}, \text{Env} \rangle$ 转换为转移 $\langle e_1^i + e_2, \text{Env} \rangle \leftrightarrow \langle e_1^{i+1} + e_2, \text{Env} \rangle$。我们可以因此获得以下转移序列：

$$ \langle e_1 + e_2, \text{Env} \rangle = \langle e_1^0 + e_2, \text{Env} \rangle \leftrightarrow \langle e_1^1 + e_2, \text{Env} \rangle \leftrightarrow \langle e_1^2 + e_2, \text{Env} \rangle \leftrightarrow \cdots \leftrightarrow \langle e_1^{k_1} + e_2, \text{Env} \rangle = \langle n_1 + n_2, \text{Env} \rangle $$

规则 $AS_6$ 允许我们将序列 [A.2] 中的每个转移 $\langle e_2^i, \text{Env} \rangle \leftrightarrow \langle e_2^{i+1}, \text{Env} \rangle$ 转换为转移 $\langle n_1 + e_2^i, \text{Env} \rangle \leftrightarrow \langle n_1 + e_2^{i+1}, \text{Env} \rangle$。我们因此得到以下序列：

$$ \langle n_1 + e_2, \text{Env} \rangle = \langle n_1 + e_2^0, \text{Env} \rangle \leftrightarrow \langle n_1 + e_2^1, \text{Env} \rangle \leftrightarrow \langle n_1 + e_2^2, \text{Env} \rangle \leftrightarrow \cdots \leftrightarrow \langle n_1 + e_2^{k_2}, \text{Env} \rangle = \langle n_1 + n_2, \text{Env} \rangle $$

最后，规则 $AS_2$ 允许我们考虑转移 $\langle n_1 + n_2, \text{Env} \rangle \leftrightarrow \langle n, \text{Env} \rangle$，其中 $n$ 是 $n_1 + n_2$ 的值，并通过将此转移与序列 [A.3] 和 [A.4] 连接，我们最终得到 $\langle e_1 + e_2, \text{Env} \rangle \leftrightarrow^* n_1 + n_2$。

- 对于二元运算符 * 和 -，采用与上面所示类似的推理。

4) 我们对表达式 $e_1$ 进行归纳。

- 如果 $e_1 = n \in \mathbb{Z}$，则假设 $\langle e_1, \text{Env} \rangle \leftrightarrow \langle e_2, \text{Env} \rangle$ 会导致矛盾，因为 $\langle e_1, \text{Env} \rangle$ 是一个终端配置。

- 如果 $e_1 = x \in \mathbf{X}$，则必须有 $e_2 = \text{Env}(x) \in \mathbb{Z}$ 且 $v = \text{Env}(x)$。因此，通过构造树可以结束我们的证明：

$$ (F_{11}) \overline{\text{Env} \vdash x \leadsto v} $$

- 如果 $e_1 = a_1 \text{ op } a_2$（其中 $\text{op} \in \{+, *, -\}$），则有三种可能的情况。

情况 1：如果 $a_1, a_2 \in \mathbb{Z}$，则 $e_2 = v$，其中 $v = a_1 \text{ op}_{\mathbf{V}} a_2 \in \mathbb{Z}$，我们可以通过构造树来结束证明：

$$ (F_4) \frac{(F_1) \overline{\text{Env} \vdash a_1 \leadsto a_1} \quad (F_1) \overline{\text{Env} \vdash a_2 \leadsto a_2}}{\text{Env} \vdash a_1 \text{ op } a_2 \leadsto v} $$

情况 2：如果 $a_1 \in E_A \setminus \mathbb{Z}$，则 $e_2 = a_1' \text{ op } a_2$，其中 $\langle a_1, \text{Env} \rangle \leftrightarrow \langle a_1', \text{Env} \rangle$。$\text{Env} \vdash a_1' \text{ op } a_2 \rightsquigarrow v$ 的求值树必须包含以下子树：

$$ (F_j) \frac{\vdots}{\text{Env} \vdash a_1' \rightsquigarrow v_1'} \qquad (F_k) \frac{\vdots}{\text{Env} \vdash a_2 \rightsquigarrow v_2'} $$

因此，根据归纳假设，我们得到 $\text{Env} \vdash a_1 \rightsquigarrow v_1'$，并且，从 $\text{Env} \vdash a_2 \rightsquigarrow v_2'$ 的求值树中，规则 $F_4$ 允许我们得到 $\text{Env} \vdash a_1 \text{ op } a_2 \rightsquigarrow v$ 的求值树。

情况 3：如果 $a_1 \in \mathbb{Z}$ 且 $a_2 \in E_A \setminus \mathbb{Z}$，那么与前一种情况使用的类似推理将带我们得出结论。

5) 既然 $\langle e, \text{Env} \rangle \leftrightarrow^* v$，就存在一个转移序列：

$$ \langle e, \text{Env} \rangle = \langle e^0, \text{Env} \rangle \leftrightarrow \langle e^1, \text{Env} \rangle \leftrightarrow \cdots \leftrightarrow \langle e^k, \text{Env} \rangle $$

其中 $e^k = v \in \mathbb{Z}$。证明是通过对此序列长度 $k$ 进行归纳获得的。让我们为 $k=0$、$k=1$ 和 $k>1$ 证明此属性，我们假设它对于 $k-1$ 和 $k-2$ 成立。

- 如果 $k=0$，则 $e = v \in \mathbb{Z}$，规则 $F_1$ 结束我们的证明。否则，如果 $k=k_0+1$，则我们对序列中的第一个转移进行分情况讨论。

- 如果 $k=1$，则有两种可能的情况。

情况 I：如果该转移是使用规则 $AS_1$ 获得的：$\langle x, \text{Env} \rangle \leftrightarrow \langle \text{Env}(x), \text{Env} \rangle$，则 $e=x$ 且 $v=\text{Env}(x)$，规则 $F_{11}$ 结束我们的证明。

情况 2：如果该转移是使用规则 $AS_2$、$AS_3$ 或 $AS_4$ 获得的：

$$ \langle n_1 \text{ op } n_2, \text{Env} \rangle \leftrightarrow \langle n, \text{Env} \rangle $$

其中 $\text{op} \in \{+, -, *\}$，则 $n = n_1 \text{ op}_{\mathbb{V}} n_2$ 且规则 $F_4$ 结束我们的证明：

$$ (F_1) \frac{\text{Env} \vdash n_1 \rightsquigarrow n_1}{} \qquad (F_1) \frac{\text{Env} \vdash n_2 \rightsquigarrow n_2}{} \qquad (F_4) \frac{\text{Env} \vdash n_1 \text{ op } n_2 \rightsquigarrow n}{} $$

- 如果 $k \ge 2$，则有两种可能的情况。

情况 I：如果 $\langle e_1 \text{ op } e_2, \text{Env} \rangle \leftrightarrow \langle e'_1 \text{ op } e_2, \text{Env} \rangle \leftrightarrow \cdots \leftrightarrow \langle e^k, \text{Env} \rangle$ 是所考虑的序列，且 $\langle e_1, \text{Env} \rangle \leftrightarrow \langle e'_1, \text{Env} \rangle$（第一个转移是使用规则 $AS_5$、$AS_7$ 或 $AS_9$ 获得的），那么，根据对长度为 $k-1$ 的转移序列的归纳假设，我们有 $\text{Env} \vdash e'_1 \text{ op } e_2 \rightsquigarrow v$，并且，根据前一个问题，我们得到 $\text{Env} \vdash e_1 \text{ op } e_2 \rightsquigarrow v$。

情况 2：如果 $\langle e_1 \text{ op } e_2, \text{Env} \rangle \leftrightarrow \langle e_1 \text{ op } e'_2, \text{Env} \rangle \leftrightarrow \dots \leftrightarrow \langle e^k, \text{Env} \rangle$ 是所考虑的序列，且 $\langle e_2, \text{Env} \rangle \leftrightarrow \langle e'_2, \text{Env} \rangle$ 且 $e_1 \in \mathbb{Z}$（第一个转移是使用规则 $AS_6$、$AS_8$ 或 $AS_{10}$ 获得的），则此序列可分解如下：

$$ \langle e_1 \text{ op } e_2, \text{Env} \rangle \leftrightarrow \underbrace{\langle e_1 \text{ op } e'_2, \text{Env} \rangle \leftrightarrow \dots \leftrightarrow \langle e_1 \text{ op } v'_2, \text{Env} \rangle}_{\text{length } k-2} \leftrightarrow \langle v, \text{Env} \rangle $$

其中最后一个转移是使用规则 $AS_2$、$AS_3$ 或 $AS_4$ 获得的，因此 $v'_2 \in \mathbb{Z}$ 且 $v = e_1 \text{ op}_{\mathbf{V}} v'_2$。长度为 $k-2$ 的序列对应于表达式 $e_2$ 的简化序列，从中我们可以构造长度为 $k-2$ 的序列 $\langle e'_2, \text{Env} \rangle \leftrightarrow \dots \leftrightarrow \langle v'_2, \text{Env} \rangle$。因为 $v'_2 \in \mathbb{Z}$，根据归纳假设我们有 $\text{Env} \vdash e'_2 \rightsquigarrow v'_2$，并且因为 $e_1 \in \mathbb{Z}$，规则 $F_1$ 允许我们得到 $\text{Env} \vdash e_1 \rightsquigarrow e_1$。根据规则 $F_4$，我们就有 $\text{Env} \vdash e_1 \text{ op } e'_2 \rightsquigarrow v$。根据前一个问题，由于 $\langle e_1 \text{ op } e_2, \text{Env} \rangle \leftrightarrow \langle e_1 \text{ op } e'_2, \text{Env} \rangle$，我们得到 $\text{Env} \vdash e_1 \text{ op } e_2 \rightsquigarrow v$。

6) 规则 $AS_1$、$AS_2$、$AS_3$ 和 $AS_4$ 保持不变。其他规则被以下规则取代（$n_2 \in \mathbb{Z}$）：

| | |
|---|---|
| (AS5) $\frac{\langle e_2, \text{Env} \rangle \leftrightarrow \langle e'_2, \text{Env} \rangle}{\langle e_1 + e_2, \text{Env} \rangle \leftrightarrow \langle e_1 + e'_2, \text{Env} \rangle}$ | (AS6) $\frac{\langle e_1, \text{Env} \rangle \leftrightarrow \langle e'_1, \text{Env} \rangle}{\langle e_1 + n_2, \text{Env} \rangle \leftrightarrow \langle e'_1 + n_2, \text{Env} \rangle}$ |
| (AS7) $\frac{\langle e_2, \text{Env} \rangle \leftrightarrow \langle e'_2, \text{Env} \rangle}{\langle e_1 * e_2, \text{Env} \rangle \leftrightarrow \langle e_1 * e'_2, \text{Env} \rangle}$ | (AS8) $\frac{\langle e_1, \text{Env} \rangle \leftrightarrow \langle e'_1, \text{Env} \rangle}{\langle e_1 * n_2, \text{Env} \rangle \leftrightarrow \langle e'_1 * n_2, \text{Env} \rangle}$ |
| (AS9) $\frac{\langle e_2, \text{Env} \rangle \leftrightarrow \langle e'_2, \text{Env} \rangle}{\langle e_1 - e_2, \text{Env} \rangle \leftrightarrow \langle e_1 - e'_2, \text{Env} \rangle}$ | (AS10) $\frac{\langle e_1, \text{Env} \rangle \leftrightarrow \langle e'_1, \text{Env} \rangle}{\langle e_1 - n_2, \text{Env} \rangle \leftrightarrow \langle e'_1 - n_2, \text{Env} \rangle}$ |

## A.3. 命令式特性的语义

## 练习 4.1 的解答

1) 设 Mem 和 $\text{Mem}_3$ 为两个内存状态。我们需要证明：

$$ \langle \text{Mem}, c_1 ; (c_2 ; c_3) \rangle \Rightarrow_{\text{Env}} \text{Mem}_3 \Leftrightarrow \langle \text{Mem}, (c_1 ; c_2) ; c_3 \rangle \Rightarrow_{\text{Env}} \text{Mem}_3 $$

$\langle \text{Mem}, (c_1 ; c_2) ; c_3 \rangle \Rightarrow_{\text{Env}} \text{Mem}_3$ 的推导树可以从 $\langle \text{Mem}, c_1 ; (c_2 ; c_3) \rangle \Rightarrow_{\text{Env}} \text{Mem}_3$ 的推导树中得到，反之亦然。事实上，从以下树：

```
(Ci) 1       (Cj) 2              (Ck) 3
<mem, c1> -->env mem1   <mem1, c2> -->env mem2   <mem2, c3> -->env mem3
   \___________________________________________/
               <mem1, c2 ; c3> -->env mem3
   \___________________________________________________________/
                         <mem, c1 ; (c2 ; c3)> -->env mem3
```

