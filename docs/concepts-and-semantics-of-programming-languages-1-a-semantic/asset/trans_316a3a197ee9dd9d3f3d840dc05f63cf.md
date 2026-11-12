

## A.1. 语言语义简介

### 练习 2.1 的解答

- 1) \(\mathbf{X}^{\text{ref}}_{\text{Env}_0} = \{y, z\}\) 且 \(\mathbf{X}^{\text{cst}}_{\text{Env}_0} = \{x\}\)。

- 2) 定义序列 \(\mathbf{d} = [\text{var } z = 2;\text{let } x = 8;\text{var } y = 5;\text{let } x = 3;]\) 的求值过程导致状态 \(\eta_{\text{at}0}\)，如下所示：

```
([], []) \(\xrightarrow{\text{var } z = 2;\rightarrow_{\text{Def}1}} [((z, r_z)], [(r_z, 2))] \xrightarrow{\text{let } x = 8;\rightarrow_{\text{Def}1}} [((x, 8), (z, r_z)], [(r_z, 2))] \xrightarrow{\text{var } y = 5;\rightarrow_{\text{Def}1}} [((y, r_y), (x, 8), (z, r_z)], [(r_y, 5), (r_z, 2))] \xrightarrow{\text{let } x = 3;\rightarrow_{\text{Def}}} \eta_{\text{at}0}\)
```

存在多个可导致状态 \(\eta_{\text{at}0}\) 的定义序列。例如，在序列 d 中，定义 let \(x = 8\) 可以替换为 let \(x = !z + 6\)，因为在状态 \([((z, r_z)], [(r_z, 2))]\) 中，表达式 \(!z + 6\) 的求值结果为 8。

- 3) 让我们如下展开该表达式：

```
\([[(!y + !z) + x]^{\text{Memo}}_{\text{Env}_0}] \text{=} [[!y + !z]^{\text{Memo}}_{\text{Env}_0}] + [[x]^{\text{Memo}}_{\text{Env}_0}] \text{=} [[!y]^{\text{Memo}}_{\text{Env}_0}] + [[!z]^{\text{Memo}}_{\text{Env}_0}] + \text{Env}_0(x) \text{=} (\text{Mem}_0(\text{Env}_0(y)) + \text{Mem}_0(\text{Env}_0(z))) + 3 \text{=} (\text{Mem}_0(r_y) + \text{Mem}_0(r_z)) + 3 \text{=} (5 + 2) + 3 \text{=} 10\)
```

- 4) 在状态 \(\eta_{\text{at}0}\) 中，表达式 \(!x\) 的求值会触发错误，因为 \(x\) 在 \(\text{Env}_0\) 中不是可变变量；表达式 \(y + x\) 的求值会触发错误，因为 \(y\) 被求值为引用，而加法不能应用于引用；标识符 \(w\) 的求值会触发错误，因为它在 \(\text{Env}_0\) 中未绑定。

- 5) 通过应用题目给出的规则，我们得到以下状态和环境：

etat1 = (((x, 3), (y, ry), (x, 8), (z, rz)), [(rz, 2), (ry, 3)])
etat2 = (((y, 3), (x, 3), (y, ry), (x, 8)), (z, rz)), [(rz, 2), (ry, 3)])
X^{ref}_{Env_1} = {y, z}   X^{cst}_{Env_1} = {x}   X^{ref}_{Env_2} = {z}   X^{cst}_{Env_2} = {x, y}

- 6) 应用题目中的规则，我们得到以下结果：

etat3 = (((x, ry), (x, 3), (y, ry), (x, 8), (z, rz)), [(rz, 2), (ry, 5)])
etat4 = (((x, ry), (x, 3), (y, ry), (x, 8), (z, rz)), [(rz, 2), (ry, 4)])
[[!y]]^{Memo_{Env_0}}_5 \neq 4 = [[!y]]^{Mem_4_{Env_4}}_4

### 练习 2.2 的解答

- 1) 取决于表达式 e1 和 e2 的求值顺序以及这些求值所在的状态，表达式 e1 + e2 有几种可能的求值选项：

- [[k]]^{Mem}_{Env} = (k, (Env, Mem))（其中 k ∈ Z）；
- [[x]]^{Mem}_{Env} = (Env(x), (Env, Mem)) 当 x ∈ X 且 x ∈ dom(Env) 时；
- [[x]]^{Mem}_{Env} = (Err, (Env, Mem)) 当 x ∈ X 且 x ∉ dom(Env) 时；
- [[e_1 + e_2]]^{Mem}_{Env} = (v_1 + v_2, (Env_2, Mem_2)) 当 [[e_1]]^{Mem}_{Env} = (v_1, (Env_1, Mem_1)) 且 [[e_2]]^{Mem_1}_{Env_1} = (v_2, (Env_2, Mem_2)) 且 v_1, v_2 ∈ Z 时；
- [[e_1 + e_2]]^{Mem}_{Env} = (Err, (Env_2, Mem_2)) 当 [[e_1]]^{Mem}_{Env} = (v_1, (Env_1, Mem_1)) 且 [[e_2]]^{Mem_1}_{Env_1} = (v_2, (Env_2, Mem_2)) 且 v_1 ∈ Z 且 v_2 ∉ Z 时；
- [[e_1 + e_2]]^{Mem}_{Env} = (Err, (Env_1, Mem_1)) 当 [[e_1]]^{Mem}_{Env} = (v_1, (Env_1, Mem_1)) 且 v_1 ∉ Z 时；
- [[!x]]^{Mem}_{Env} = (Mem(Env(x)), (Env, Mem)) 当 x ∈ X^{ref}_{Env} 时；
- [[!x]]^{Mem}_{Env} = (Err, (Env, Mem)) 当 x ∉ X^{ref}_{Env} 时；
- [[x++]]^{Mem}_{Env} = (Mem(Env(x)), (Env, Mem[Env(x) := Mem(Env(x)) + 1])) 当 Env(x) ∈ R 且 Mem(Env(x)) ∈ Z 时；
- [[x++]]^{Mem}_{Env} = (Err, (Env, Mem)) 当 Env(x) ∉ R 或 Mem(Env(x)) ∉ Z 时；
- [[++x]]^{Mem}_{Env} = (Mem(Env(x)) + 1, (Env, Mem[Env(x) := Mem(Env(x)) + 1])) 当 Env(x) ∈ R 且 Mem(Env(x)) ∈ Z 时；
- [[++x]]^{Mem}_{Env} = (Err, (Env, Mem)) 当 Env(x) ∉ R 或 Mem(Env(x)) ∉ Z 时。

- 2) 我们有 [[!x]]^{Mem}_{Env} = (6, (Env, Mem))，因此 [[x++]]^{Mem}_{Env} = (6, (Env, [(r_x, 7)])) 进而 [[x + (x++)]]^{Mem}_{Env} = (12, (Env, [(r_x, 7)]))。此外，由于 [[!x]]^{Mem}_{Env}[(r_x, 7)] = (7, (Env, [(r_x, 7)]))，则 [[(x++) + !x]]^{Mem}_{Env} = (13, (Env, [(r_x, 7)]))，故 [[x + (x++)]]^{Mem}_{Env} \neq [[(x++) + !x]]^{Mem}_{Env}。

- 3) 如果 x 不是可变变量，或 x 在内存中存储的值不是整数，则：

[[++x]]^{Mem}_{Env} = [[(x++) + 1]]^{Mem}_{Env} = (Err, (Env, Mem))

否则，根据定义，我们有：

[[++x]]^{Mem}_{Env} = (Mem(Env(x)) + 1, (Env, \underline{Mem[Env(x) := Mem(Env(x)) + 1]})_{Mem_1})

同样，根据定义，[[x++]]^{Mem}_{Env} = (Mem(Env(x)), (Env, Mem_1))，且因为 [[1]]^{Mem_1}_{Env} = (1, (Env, Mem_1))，我们得到：

[[(x++) + 1]]^{Mem}_{Env} = (Mem(Env(x)) + 1, (Env, Mem_1))

因此 [[++x]]^{Mem}_{Env} = [[(x++) + 1]]^{Mem}_{Env}。

- 4) 关系的扩展如下：

(\text{Env}, \text{Mem}) \xrightarrow{x:=e}_{\mathbf{Lang}_1} (\text{Env}', \text{Mem}'[\text{Env}(x) := v])

当：

[[e]]^{\text{Mem}}_{\text{Env}} = (v, (\text{Env}', \text{Mem}')), v \neq \text{Err} 且 x \in \mathbf{X}^{\text{ref}}_{\text{Env}}

(\text{Env}, \text{Mem}) \xrightarrow{x:+:=e}_{\mathbf{Lang}_1} (\text{Env}', \text{Mem}'[\text{Env}(x) := v + \text{Mem}(\text{Env}(x))])

当：

[[e]]^{\text{Mem}}_{\text{Env}} = (v, (\text{Env}', \text{Mem}')), v \in \mathbb{Z}, \text{Mem}(\text{Env}(x)) \in \mathbb{Z} 且 x \in \mathbf{X}^{\text{ref}}_{\text{Env}}

- 5) 根据定义

[[++x]]^{\text{Mem}_0}_{\text{Env}_0} = \begin{pmatrix} \text{Mem}_0(\text{Env}_0(x)) + 1, \\ (\text{Env}_0, \underline{\text{Mem}_0[\text{Env}_0(x) := \text{Mem}_0(\text{Env}_0(x)) + 1]}) \\ = (3, \text{Env}_0, \underline{[(r_x, 3)]})_{\text{Mem}_0} \end{pmatrix}

因此：

```
(Env_0, Mem_0) \xrightarrow{x:=++x}_{\text{Lang}_1} (Env_0, Mem'_0[Env_0(x) := 3 + Mem_0(Env_0(x))])
```

我们因此得到 etat_1 = [((x, r_x)], Mem'_0[r_x := 3+2]) = [((x, r_x)], [(r_x, 5)]]。

- 6) 令 (Env_0, Mem_0) 为任意状态。如果 x ∉ \mathbf{X}^{ref}_{Env_0}，或 Mem_0 中 x 对应的值不是整数，则这三个赋值都会导致相同的状态 (Env_0, Mem_0)。否则，根据定义：

```
[[x++]]^{Mem_0}_{Env_0} = \left( \begin{array}{c} Mem_0(Env_0(x)), \\ (Env_0, Mem_0[Env_0(x) := Mem_0(Env_0(x)) + 1]) \end{array} \right) \text{Mem}_1 \\
[[++x]]^{Mem_0}_{Env_0} = \left( \begin{array}{c} Mem_0(Env_0(x)) + 1, \\ (Env_0, Mem_0[Env_0(x) := Mem_0(Env_0(x)) + 1]) \end{array} \right) \text{Mem}_2 \\
[[1]]^{Mem_0}_{Env_0} = (1, (Env_0, Mem_0))
```

因此：

```
(Env_0, Mem_0) \xrightarrow{x:=x++}_{\text{Lang}_1} (Env_0, Mem_1[Env_0(x) := Mem_0(Env_0(x))]) \\
\phantom{(Env_0, Mem_0)} \xrightarrow{x:=x++}_{\text{Lang}_1} \underline{= Mem_0} \\
(Env_0, Mem_0) \xrightarrow{x:=++x}_{\text{Lang}_1} (Env_0, Mem_2[Env_0(x) := Mem_0(Env_0(x)) + 1]) \\
\phantom{(Env_0, Mem_0)} \xrightarrow{x:=++x}_{\text{Lang}_1} \underline{= Mem_2 = Mem_0[Env_0(x) := 1 + Mem_0(Env_0(x))]} \\
(Env_0, Mem_0) \xrightarrow{x:=+1}_{\text{Lang}_1} (Env_0, Mem_0[Env_0(x) := 1 + Mem_0(Env_0(x))]) \\
\phantom{(Env_0, Mem_0)} \xrightarrow{x:=+1}_{\text{Lang}_1} \underline{= Mem_2}
```

当从相同状态执行时，赋值 x := ++x 和 x := + 1 会导致相同的状态，但该状态与执行赋值 x := x++ 所到达的状态不同，因为后者以任何方式都不会修改其执行所在的状态。

## A.2. 函数式特性的语义

### 练习 3.1 的解答

```
[[fun x \mapsto let y = 3 in x + y]]_{Env} = \langle x, \text{let } y = 3 \text{ in } x + y, Env \rangle \\
[[let y = 3 in fun x \mapsto x + y]]_{Env} = \langle x, x + y, (y, 3) \oplus Env \rangle
```

