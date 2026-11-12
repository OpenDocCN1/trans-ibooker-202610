## 结论

本书两卷的目的是介绍编程语言的主要顺序特性的概念、语义和工具。在第一卷中，在简要介绍硬件和编码之后，我们考察了函数式和命令式特性。这些概念采用统一方法进行介绍，为两种范式定义并比较了几种不同的执行语义。我们还研究了类型语义及其应用、程序一致性验证、多态性和类型推断，这些对于减轻开发者工作量非常重要。相应的第 3、4 和 5 章初读时可能显得困难。可以先浏览，然后在书中内容被引用和使用时再重新研读。此外，语义概念的 OCaml 和 Python 编码旨在通过阅读和执行所提供的代码来便于掌握。

我们还详细介绍了数据类型以及自动或显式内存管理，示例大多用 C 和 C++ 编写。我们强调了硬件组件的某些特性，这些特性影响这些功能的语义和程序执行。最后一章专门介绍了异常、其语义以及在 OCaml、Java、C++ 和 Python 语言中的处理。

第二卷基于本卷奠定的基础。从分析开发者的某些需求入手，我们将逐步开发一个兼顾模块化和面向对象特性的原创语义模型。该模型旨在对这些构造的展示提供统一的观点。然而，出于合理的原因，它不具备当前卷中语义模型的严谨性。Ada 和 OCaml 的模块以及 C 中使用的头文件将作为该模型的第一个示例进行研究。随后将进一步发展该模型，以介绍面向对象语言的一般机制，并作为研究 Java、C++、OCaml 和 Python 语言中的类和包的基础。





### A.1. 语言语义简介

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

### A.2. 函数式特性的语义

### 练习 3.1 的解答

```
[[fun x \mapsto let y = 3 in x + y]]_{Env} = \langle x, \text{let } y = 3 \text{ in } x + y, Env \rangle \\
[[let y = 3 in fun x \mapsto x + y]]_{Env} = \langle x, x + y, (y, 3) \oplus Env \rangle
```





# 练习 3.2 解答

- 1) 我们有 [[let f = fun y -> e in (f x)]]Env = [[(f x)]]Env1 = [[e]]Env2 且 [[(f x)]]Env = [[e]]Env3，其中：

```
Env1 = (f, <y, e, Env>) ⊕ Env
Env2 = (y, [x]Env1) ⊕ Env = (y, 4) ⊕ Env
Env3 = (y, [x]Env) ⊕ (x, 3) ⊕ Env0 = (y, 4) ⊕ (x, 3) ⊕ Env0
```

我们有 Env2(y) = Env3(y) = 4 且 Env2(x) = 4 ≠ Env3(x) = 3。为了使 [[e]]Env2 = [[e]]Env3，表达式 e 不得包含任何自由出现的 x（由于 f 不是递归函数，表达式 e 不会包含任何自由出现的 f）。

- 2) 当 e = y + x 时，我们有：

```
[[let f = fun x -> x + (f x) in (f x)]]Env
= [[(f x)]]Env1 + [[(f x)]]Env2 = [[x + (f x)]]Env2 + [[(f x)]]Env2
= [[x]]Env2 + [[y + x]]Env3 = [[x]]Env2 + [[y]]Env3 + [[x]]Env3 = 4 + (4 + 3) = 11
```

其中：

```
Env1 = (f, <x, x + (f x), Env>) ⊕ Env
Env2 = (x, [x]Env1) ⊕ Env = (x, 4) ⊕ Env
Env3 = (y, [x]Env2) ⊕ (x, 3) ⊕ Env0 = (y, 4) ⊕ (x, 3) ⊕ Env0
```

# 练习 3.3 解答

- 1) 我们注意：

```
let x = 3 in (let f = e1 in (let g = e2 in (let h = e3 in e4)))
```

在环境 Env0 中对 e 的求值树：

(F15) Env1 |- e1 ~~ Ff       (F13)     1   2
                              -------------------------------------
                              (f, Ff) + Env1 |- e2' ~~ v
                                          Env2
(F1) Env0 |- 3 ~~ 3   (F13) -------------------------------------
                                          (x, 3) + Env0 |- e1' ~~ v
                                                  Env1
(F13) ------------------------------------------------
                                      Env0 |- e ~~ v

其中 $F_f = \langle x, \text{let } y = x \text{ in } y, \text{Env}_1 \rangle$ 且 $\nabla_1$ 和 $\nabla_2$ 是求值树：

\nabla_1 :
   (F11) Env2 |- x ~~ 3     (F15) (y, 3) (+) Env2 + fun x -> y ~~ Fg
   ---------------------------------------------------
   (F13) ----------------------- Env2 |- e2 ~~ Fg

\nabla_2 :
      (F15) Env3 |- e3 ~~ Fh
   -----------------------------------
   (h, Fh) (+) Env3 |- e4 ~~ v
   -----------------------------------
                   Env4
   (F13) ----------------------- (g, Fg) (+) Env2 |- e3' ~~ v
   -------------------------------------------
                   Env3

其中 $F_g = \langle x, y, (y, 3) (+) \text{Env}_2 \rangle$ 且 $F_h = \langle k, \text{fun } x \mapsto (k x), \text{Env}_3 \rangle$。

## 2) 在环境 $\text{Env}_4$ 中对 $e_4 = ((h f) 2)$ 的求值树：

      V   V   V           V   V   V
   1   2   3             4   5   6
(F17) Env4 |- (h f) ~~ F   (F1) Env4 |- 2 ~~ 2   (F17) (x, 2) (+) Env5 |- (k x) ~~ 2
   ---------------------------------------------------
                   Env6
(F17) -------------------------------------------------
                   Env4 |- e4 ~~ 2

其中：
\nabla_1 : (F11) \text{Env}_4 |- h ~~ Fh         \nabla_2 : (F11) \text{Env}_4 |- f ~~ Ff

\nabla_3 : (F15) \underline{(k, Ff) (+) \text{Env}_3 |- \text{fun } x \mapsto (k x) ~~ F}
             \text{Env}_5

与 $F = \langle x, (k x), \text{Env}_5 \rangle$ 及：
\nabla_4 : (F11) \text{Env}_6 |- k ~~ Ff         \nabla_5 : (F11) \text{Env}_6 |- x ~~ 2

\nabla_6 : (F13) ...
   (F11) \text{Env}_7 |- x ~~ 2   (F11) (y, 2) (+) \text{Env}_7 |- y ~~ 2
   ---------------------------------------------------
   (x, 2) (+) \text{Env}_1 |- \text{let } y = x \text{ in } y ~~ 2
             \text{Env}_7

## 3) 在环境 $\text{Env}_4$ 中对 $e_4 = ((h g) 2)$ 的求值树：

      V   V   V           V   V   V
   1   2   3             4   5   6
(F17) Env4 |- (h g) ~~ F   (F1) Env4 |- 2 ~~ 2   (F17) (x, 2) (+) Env5 |- (k x) ~~ 3
   ---------------------------------------------------
                   Env6
(F17) -------------------------------------------------
                   Env4 |- e4 ~~ 3

其中：

\(\nabla_1 : (F_{11})_{\text{Env}_4 \vdash h \rightsquigarrow F_h} \qquad \nabla_2 : (F_{11})_{\text{Env}_4 \vdash g \rightsquigarrow F_g}\)

\(\nabla_3 : (F_{15})_{\frac{(k, F_g) \oplus \text{Env}_3 \vdash \text{fun } x \mapsto (k \, x) \rightsquigarrow F}{\text{Env}_5}}\)

与 \(F = \langle x, (k \, x), \text{Env}_5 \rangle\) 及：

\(\nabla_4 : (F_{11})_{\text{Env}_6 \vdash k \rightsquigarrow F_g} \qquad \nabla_5 : (F_{11})_{\text{Env}_6 \vdash x \rightsquigarrow 2}\)

\(\nabla_6 : (F_{11})_{\frac{(x, 2) \oplus (y, 3) \oplus \text{Env}_2 \vdash y \rightsquigarrow 3}{\text{Env}_7}}\)

# 练习 3.4 解答

令 \(e'_1\) 为子表达式 let \(f = e_1\) in \(e\)。

- 1) 在环境 \(\text{Env}_0\) 中对 \(e_0\) 的求值树：

\(\begin{array}{c} \nabla \\ \text{( )} \\ \underline{(f, [e_1, \text{Env}_1]) \oplus \text{Env}_1 \vdash e \rightsquigarrow v} \\ \text{Env}_2 \\ (F'_{13}) \\ \underline{(x, [2, \text{Env}_0]) \oplus \text{Env}_0 \vdash e'_1 \rightsquigaro v} \\ \text{Env}_1 \\ (F'_{13}) \\ \underline{\text{Env}_0 \vdash e_0 \rightsquigaro v} \end{array}\)

- 2) 在环境 \(\text{Env}_2\) 中对 \(e\) 的求值树：

\(\begin{array}{c} \text{(F}_1 \text{)} \underline{\text{Env}_2 \vdash 3 \rightsquigaro 3} \qquad \text{(F}_1 \text{)} \underline{\text{Env}_0 \vdash 2 \rightsquigaro 2} \\ \text{(F}'_{11} \text{)} \underline{\text{Env}_3 \vdash x \rightsquigaro 3} \qquad \text{(F}'_{11} \text{)} \underline{\text{Env}_4 \vdash x \rightsquigaro 2} \\ \text{(F}'_{14} \text{)} \underline{\text{Env}_1 \vdash e_1 \rightsquigaro F_f} \qquad \text{(F}_4 \text{)} \underline{\text{Env}_4 \vdash y \rightsquigaro 3} \\ \text{(F}'_{11} \text{)} \underline{\text{Env}_3 \vdash f \rightsquigaro F_f} \\ \qquad \qquad \qquad \qquad \underline{\text{(y, [x, Env}_3 \text{])} \oplus \text{Env}_1 \vdash y + x \rightsquigaro 5} \\ \qquad \qquad \qquad \qquad \text{Env}_4 \\ \text{(F}'_{16} \text{)} \\ \qquad \qquad \qquad \qquad \underline{\text{(x, [3, Env}_2 \text{])} \oplus \text{Env}_2 \vdash (f \, x) \rightsquigaro 5} \\ \qquad \qquad \qquad \qquad \text{Env}_3 \\ \text{(F}'_{13} \text{)} \\ \qquad \qquad \qquad \qquad \underline{\text{Env}_2 \vdash e \rightsquigaro 5} \end{array}\)

与 \(F_f = \langle y, y + x, \text{Env}_1 \rangle\)。

- 3) 令 \( e' \) 为表达式 fun \( y \mapsto y + x \)。在环境 \( \text{Env}_2 \) 中对 \( e \) 的求值树：

\( (F_1) \overline{\text{Env}_2 \vdash 3 \leadsto 3} \)
\( (F'_{11}) \overline{\text{Env}_1 \vdash x \leadsto 3} \quad (F_1) \overline{\text{Env}_2 \vdash 3 \leadsto 3} \)
\( (F'_{11}) \overline{\text{Env}_1 \vdash x \leadsto 3} \quad (F'_{11}) \overline{\text{Env}_4 \vdash x \leadsto 3} \)
\( (F'_{14}) \overline{\text{Env}_3 \vdash e' \leadsto \langle y, y+x, \text{Env}_3 \rangle} \quad (F_4) \overline{[y, [x, \text{Env}_3]] \oplus \text{Env}_3 \vdash y+x \leadsto 6} \)
... 
\( (F'_{16}) \overline{\dots \vdash 6} \)
\( (F'_{13}) \overline{\dots \vdash 6} \)

- 4) 我们注意：

rec g z = fun k -> if z = 0 then (k x) else ((g (z - 1)) k)

带有标注 \( e_5, e_6, e_4, e_3 \)。

在环境 \( \text{Env}_2 \) 中对 \( e \) 的求值树：

涉及 \( (F'_{16}) \) 和 \( (F'_{13}) \) 的树。
结果：\( \text{Env}_2 \vdash e \leadsto 4 \)。

树 \( \nabla_1 \) 的构造：

涉及 \( (F'_{15}), (F'_{11}), (F'_{14}), (F'_{16}) \) 的树。
结果：\( \text{Env}_3 \vdash (g(x-1)) \leadsto F_k \)。

其中：

\( F_g = \langle z, e_3, (g, [e_2, \text{Env}_2]) \oplus \text{Env}_2 \rangle \)（下划线 \( \text{Env}_g \））

以及：

\( F_k = \langle k, e_4, \text{Env}_4 \rangle \)

## 树 $\nabla_2$ 的构造：

$(F_1) \overline{\text{Env}_0 \vdash 2 \rightsquigaro 2}$
$(F'_{11}) \overline{\text{Env}_3 \vdash x \rightsquigaro 2} (F_1) \overline{\text{Env}_3 \vdash 1 \rightsquigaro 1}$
$(F_4) \overline{\text{Env}_3 \vdash x - 1 \rightsquigaro 1}$
$(F'_{11}) \dots (F_1) \overline{\text{Env}_5 \vdash 0 \rightsquigaro 0}$
$(F_4) \dots \overline{\text{Env}_5 \vdash z = 0 \rightsquigaro false} \nabla_3$
$(F_8) \overline{(k, [f, \text{Env}_3]) \oplus \text{Env}_4 \vdash e_4 \rightsquigaro 4} \text{Env}_5$

## 树 $\nabla_3$ 的构造：

$(F'_{15}) \overline{\text{Env}_2 \vdash e_2 \rightsquigaro F_g}$
$(F'_{11}) \overline{\text{Env}_5 \vdash g \rightsquigaro F_g} (F'_{14}) \overline{(z, [z - 1, \text{Env}_5]) \oplus \text{Env}_g \vdash e_3 \rightsquigaro F'_k}$
$(F'_{16}) \dots \nabla_4$
$(F'_{16}) \dots$
$\text{Env}_5 \vdash e_6 \rightsquigaro 4$
与 $F'_k = \langle k, e_4, \text{Env}_6 \rangle$。

## 树 $\nabla_4$ 的构造：

$(F'_{14}) \overline{\text{Env}_1 \vdash e_1 \rightsquigaro \langle y, y + x, \text{Env}_1 \rangle} F_p$
$(F'_{11}) \overline{\text{Env}_3 \vdash f \rightsquigaro F_p}$
$(F'_{11}) \overline{\text{Env}_5 \vdash k \rightsquigaro F_p}$
$(F'_{11}) \overline{\text{Env}_7 \vdash k \rightsquigaro F_p} \nabla_6$
$(F'_{16}) \overline{\text{Env}_7 \vdash (k x) \rightsquigaro 4} \nabla_5$
$(F_7) \dots$
$(k, [k, \text{Env}_5]) \oplus \text{Env}_6 \vdash e_4 \rightsquigaro 4 \text{Env}_7$

## 树 $\nabla_5$ 的构造：

$(F_1) \overline{\text{Env}_0 \vdash 2 \rightsquigaro 2}$
$(F'_{11}) \overline{\text{Env}_3 \vdash x \rightsquigaro 2} (F_1) \overline{\text{Env}_3 \vdash 1 \rightsquigaro 1}$
$(F_4) \overline{\text{Env}_3 \vdash x - 1 \rightsquigaro 1}$
$(F'_{11}) \dots (F_1) \overline{\text{Env}_5 \vdash 1 \rightsquigaro 1}$
$(F_4) \dots \overline{\text{Env}_5 \vdash z - 1 \rightsquigaro 0}$
$(F'_{11}) \dots \overline{\text{Env}_7 \vdash z \rightsquigaro 0} (F_1) \overline{\text{Env}_7 \vdash 0 \rightsquigaro 0}$
$(F_4) \dots \overline{\text{Env}_7 \vdash z = 0 \rightsquigaro true}\)





## 树 $\nabla_6$ 的构造：

```
$(F_4)$
   $(F'_{11})$   $(F_1)$
   -------   -------
   \text{Env}_7 \ldots   \text{Env}_0 \ldots
   $(F_{11})$   $(F'_{11})$
   -------   -------
   \text{Env}_8 \ldots   \text{Env}_8 \ldots
   ---------------------------
   $(y, [x, \text{Env}_7]) \oplus \text{Env}_1 \vdash y + x \rightsquigarrow 4$
   ---------------------------
            \text{Env}_8
```

## 练习 3.5 的解答

我们对 $a$ 进行归纳（只证明单向蕴含，因为关系 $=_X$ 显然是对称的）。

- 若 $a = k \in \mathbb{Z}$，则 $\mathcal{F}(a) = \emptyset$，求 $\text{Env}_1 \vdash k \rightsquigarrow n$ 的求值树必然是使用规则 $F_1$（其中 $k = n$）得到的，并且，使用同一规则，我们显然可以构造出 $\text{Env}_2 \vdash k \rightsquigarrow n$ 的求值树。

- 若 $a = x \in \mathbf{X}$，因此 $\mathcal{F}(a) = \{x\}$，求 $\text{Env}_1 \vdash x \rightsquigarrow n$ 的求值树必然是使用规则 $F_{11}$（其中 $\text{Env}_1(x) = n$）得到的。由于根据假设 $\text{Env}_1 =_{\mathcal{F}(a)} \text{Env}_2$，我们得到 $\text{Env}_2(x) = n$，并且，应用规则 $F_{11}$，我们得到 $\text{Env}_2 \vdash x \rightsquigarrow n$ 的求值树。

- 若 $a = a_1 \text{ op } a_2$（其中 op $\in \{+, -, *\}$），求 $\text{Env}_1 \vdash a_1 \text{ op } a_2 \rightsquigarrow n$ 的求值树必然是使用规则 $F_4$ 得到的：

```
$(F_i)$   $(F_j)$
::   ::
$\text{Env}_1 \vdash a_1 \rightsquigarrow n_1$  $\text{Env}_1 \vdash a_2 \rightsquigarrow n_2$
---------------------------------
$(F_4)$ $\text{Env}_1 \vdash a_1 \text{ op } a_2 \rightsquigarrow n$
```

其中 $n = n_1 \text{ op}_{\mathbf{V}} n_2$。由于 $\mathcal{F}(a_1) \subseteq \mathcal{F}(a)$ 且 $\mathcal{F}(a_2) \subseteq \mathcal{F}(a)$（由此得到 $\text{Env}_1 =_{\mathcal{F}(a_1)} \text{Env}_2$ 和 $\text{Env}_1 =_{\mathcal{F}(a_2)} \text{Env}_2$），根据归纳假设，存在 $\text{Env}_2 \vdash a_1 \rightsquigarrow n_1$ 和 $\text{Env}_2 \vdash a_2 \rightsquigarrow n_2$ 的求值树。从这两棵树出发，我们可以应用规则 $F_4$ 构造出 $\text{Env}_2 \vdash a_1 \text{ op } a_2 \rightsquigarrow n$ 的求值树。

- 若 $a = (\text{let } x = a_1 \text{ in } a_2)$，求 $\text{Env}_1 \vdash \text{let } x = a_1 \text{ in } a_2 \rightsquigarrow n$ 的求值树必然是使用规则 $F_{13}$ 得到的：

```
$(F_i)$   $(F_j)$
::   ::
$\text{Env}_1 \vdash a_1 \rightsquigarrow n_1$  $(x, n_1) \oplus \text{Env}_1 \vdash a_2 \rightsquigarrow n_2$
-----------------------------------------------
$(F_{13})$ $\text{Env}_1 \vdash \text{let } x = a_1 \text{ in } a_2 \rightsquigarrow n_2$
```

由于根据定义，$\mathcal{F}(a) = \mathcal{F}(a_1) \cup (\mathcal{F}(a_2) \setminus \{x\})$，我们有 $\mathcal{F}(a_1) \subseteq \mathcal{F}(a)$，因此 $\text{Env}_1 =_{\mathcal{F}(a_1)} \text{Env}_2$，这使得我们可以通过对 $a_1$ 的归纳假设得到 $\text{Env}_2 \vdash a_1 \rightsquigarrow n_1$ 的求值树。此外，由于 $\text{Env}_1 =_{\mathcal{F}(a)} \text{Env}_2$，我们得到 $(x, n_1) \oplus \text{Env}_1 =_{\{x\} \cup \mathcal{F}(a)} (x, n_1) \oplus \text{Env}_2$，并且由于 $\mathcal{F}(a_2) \subseteq \mathcal{F}(a) \cup \{x\}$，我们有 $(x, n_1) \oplus \text{Env}_1 =_{\mathcal{F}(a_2)} (x, n_1) \oplus \text{Env}_2$，因此可以对 $a_2$ 应用归纳假设得到 $(x, n_1) \oplus \text{Env}_2 \vdash a_2 \rightsquigarrow n_2$ 的求值树。通过应用规则 $F_{13}$ 到以这种方式构造的两棵求值树，我们得到 $\text{Env}_2 \vdash \text{let } x = a_1 \text{ in } a_2 \rightsquigarrow n_2$ 的求值树，从而结束证明。

## 练习 3.6 的解答

我们对 $e$ 进行归纳。

- 若 $e = k \in \mathbb{Z}$，则 $[[k[x \leftarrow e']]]_{\text{Env}} = [[k]]_{\text{Env}} = k = [[k]]_{(x, [[e']]_{\text{Env}}) \oplus \text{Env}}$

- 若 $e = z \in X$，若 $z = x$，则得到：

```
[[x[x \leftarrow e']]]_{\text{Env}} = [[e']]_{\text{Env}} = [[x]]_{(x, [[e']]_{\text{Env}}) \oplus \text{Env}}
```

否则，若 $z \neq x$，则得到

```
[[z[x \leftarrow e']]]_{\text{Env}} = [[z]]_{\text{Env}} = \text{Env}(z) = [[z]]_{(x, [[e']]_{\text{Env}}) \oplus \text{Env}}
```

- 若 $e = e_1 \text{ op } e_2$，则：

```
[[(e_1 \text{ op } e_2)[x \leftarrow e']]]_{\text{Env}}
= [[e_1[x \leftarrow e'] \text{ op } e_2[x \leftarrow e']]]_{\text{Env}}          (substitution)
= [[e_1[x \leftarrow e']]]_{\text{Env}} \operatorname{op}_{\mathbf{V}} [[e_2[x \leftarrow e']]]_{\text{Env}}   (evaluation function)
= [[e_1]]_{(x, [[e']]_{\text{Env}}) \oplus \text{Env}} \operatorname{op}_{\mathbf{V}} [[e_2]]_{(x, [[e']]_{\text{Env}}) \oplus \text{Env}} (induction hypothesis)
= [[e_1 \text{ op } e_2]]_{(x, [[e']]_{\text{Env}}) \oplus \text{Env}}   (evaluation function)
```

## 练习 3.7 的解答

- 1) 转换序列如下：

```
⟨(z + (x - y)) * (x + 7), Env⟩ ↔ ⟨(2 + (x - y)) * (x + 7), Env⟩
↔ ⟨(2 + (1 - y)) * (x + 7), Env⟩ → ⟨(2 + (1 - 3)) * (x + 7), Env⟩
↔ ⟨(2 + -2) * (x + 7), Env⟩ → ⟨0 * (x + 7), Env⟩ ↔ ⟨0 * (1 + 7), Env⟩
↔ ⟨0 * 8, Env⟩ → ⟨0, Env⟩
```

- 2) a) 我们考虑在 $\mathbb{N} \times \mathbb{N}$ 上定义的字典序 $\sqsubseteq$，该顺序由 $\mathbb{N}$ 上的顺序关系 $\leq$ 定义：

```
(n_1, n_2) \sqsubseteq (n'_1, n'_2) \text{ if } (n_1 < n'_1) \text{ or } (n_1 = n'_1 \text{ and } n_2 \leq n'_2)
```

- b) 设 $P(\langle e, \text{Env} \rangle \mapsto \langle e', \text{Env} \rangle)$ 为属性：

$(\mathcal{N}_V(e'), \mathcal{N}_O(e')) \prec (\mathcal{N}_V(e), \mathcal{N}_O(e))$

并通过归纳，证明在推理系统的方框 3.5 中考虑每条规则时，该属性都成立。

- 规则 $AS_1$：由于 $\text{Env}(x) \in \mathbb{Z}$，我们有 $\mathcal{N}_V(x) = 1$ 且 $\mathcal{N}_V(\text{Env}(x)) = 0$，这结束了我们的证明。

- 规则 $AS_2$：一方面，$\mathcal{N}_V(n_1 + n_2) = \mathcal{N}_V(n) = 0$（因为 $n, n_1, n_2 \in \mathbb{Z}$），另一方面，$\mathcal{N}_O(n) = \mathcal{N}_O(n_1 + n_2) - 1$，这结束了我们的证明。

- 规则 $AS_3$ 和 $AS_4$：与规则 $AS_2$ 使用的推理相似。

- 规则 $AS_5$：根据归纳假设，我们有：

$(\mathcal{N}_V(e'_1), \mathcal{N}_O(e'_1)) \prec (\mathcal{N}_V(e_1), \mathcal{N}_O(e_1))$

此外，若 $\mathcal{N}_V(e'_1) < \mathcal{N}_V(e_1)$，则得到：

$\mathcal{N}_V(e'_1 + e_2) = \mathcal{N}_V(e'_1) + \mathcal{N}_V(e_2) < \mathcal{N}_V(e_1) + \mathcal{N}_V(e_2) = \mathcal{N}_V(e_1 + e_2)$

这结束了我们的证明，或者 $\mathcal{N}_V(e'_1) = \mathcal{N}_V(e_1)$ 且 $\mathcal{N}_O(e'_1) < \mathcal{N}_O(e_1)$，则得到：

$\mathcal{N}_V(e'_1 + e_2) = \mathcal{N}_V(e'_1) + \mathcal{N}_V(e_2) = \mathcal{N}_V(e_1) + \mathcal{N}_V(e_2) = \mathcal{N}_V(e_1 + e_2)$ 且
$\mathcal{N}_O(e'_1 + e_2) = \mathcal{N}_O(e'_1) + \mathcal{N}_O(e_2) + 1 < \mathcal{N}_O(e_1) + \mathcal{N}_O(e_2) + 1 = \mathcal{N}_O(e_1 + e_2)$

这也使我们得出结论。

- 规则 $AS_7$ 和 $AS_9$：与规则 $AS_5$ 使用的推理相似。

- 规则 $AS_6$：根据归纳假设，我们有：

$(\mathcal{N}_V(e'_2), \mathcal{N}_O(e'_2)) \prec (\mathcal{N}_V(e_2), \mathcal{N}_O(e_2))$

此外，若 $\mathcal{N}_V(e'_2) < \mathcal{N}_V(e_2)$，则得到：

$\mathcal{N}_V(n_1 + e'_2) = \mathcal{N}_V(e'_2) < \mathcal{N}_V(e_2) = \mathcal{N}_V(n_1 + e_2)$

这结束了我们的证明，或者 $\mathcal{N}_V(e'_2) = \mathcal{N}_V(e_2)$ 且 $\mathcal{N}_O(e'_2) < \mathcal{N}_O(e_2)$，则得到：

$\mathcal{N}_V(n_1 + e'_2) = \mathcal{N}_V(e'_2) = \mathcal{N}_V(e_2) = \mathcal{N}_V(n_1 + e_2)$
$\mathcal{N}_O(n_1 + e'_2) = \mathcal{N}_O(e'_2) + 1 < \mathcal{N}_O(e_2) + 1 = \mathcal{N}_O(n_1 + e_2)$

这也使我们得出结论。

- 规则 $AS_8$ 和 $AS_{10}$：与规则 $AS_6$ 使用的推理相似。

- c) 对于每一步 $\langle e_i, \text{Env} \rangle \leftrightarrow \langle e_{i+1}, \text{Env} \rangle$，我们有：

$(\mathcal{N}_V(e_{i+1}), \mathcal{N}_O(e_{i+1})) \prec (\mathcal{N}_V(e_i), \mathcal{N}_O(e_i))$

由于 $\preceq$ 是 $\mathbb{N} \times \mathbb{N}$ 上的良基序关系，不存在严格递减的无穷序列：

$(n_0, n'_0) \succ (n_1, n'_1) \succ (n_2, n'_2) \succ \cdots$

因此我们可以得出结论，任何转换序列都是有限长度的。

- d) 如果添加规则 $AS_{11}$，该属性不再成立，因为：

$(\mathcal{N}_V(e_1 + e_2), \mathcal{N}_O(e_1 + e_2)) = (\mathcal{N}_V(e_2 + e_1), \mathcal{N}_O(e_2 + e_1))$

例如，这将给出以下无限转换序列：

$\langle e_1 + e_2, \text{Env} \rangle \leftrightarrow \langle e_2 + e_1, \text{Env} \rangle \leftrightarrow \langle e_1 + e_2, \text{Env} \rangle \leftrightarrow \langle e_2 + e_1, \text{Env} \rangle \leftrightarrow \cdots$

- 3) 我们通过对 $\text{Env} \vdash e \rightsquigarrow n$ 的求值树进行归纳，证明存在转换序列 $\langle e, \text{Env} \rangle \stackrel{*}{\leftrightarrow} n$。

- 若 $\text{Env} \vdash e \rightsquigarrow n$ 的求值树是使用规则 $F_1$ 得到的，则 $e = n \in \mathbb{Z}$，并且显然有 $\langle n, \text{Env} \rangle \stackrel{*}{\leftrightarrow} n$。

- 若 $\text{Env} \vdash e \rightsquigarrow n$ 的求值树是使用规则 $F_{11}$ 得到的，则 $e = x \in \mathbf{X}$ 且 $n = \text{Env}(x) \in \mathbb{Z}$。可以使用规则 $AS_1$ 构造 $\langle x, \text{Env} \rangle \leftrightarrow \langle \text{Env}(x), \text{Env} \rangle$ 的树，并且我们可以建立 $\langle x, \text{Env} \rangle \stackrel{*}{\leftrightarrow} \text{Env}(x)$。

- 若 $\text{Env} \vdash e \rightsquigarrow n$ 的求值树是使用规则 $F_4$ 和二元运算符 $+$ 得到的：

```
  \vdots (F_i)   \text{Env} \vdash e_1 \rightsquigarrow n_1       \vdots (F_j)   \text{Env} \vdash e_2 \rightsquigarrow n_2
--------------------------------------------------------------------------------------------------
                     (F4) \text{Env} \vdash e_1 + e_2 \rightsquigarrow n_1 + n_2
```

那么，根据归纳假设，我们有：

$\langle e_1, \text{Env} \rangle \stackrel{*}{\leftrightarrow} n_1 \text{ 且 } \langle e_2, \text{Env} \rangle \stackrel{*}{\leftrightarrow} n_2$

并且存在两个转换序列：

$\langle e_1, \text{Env} \rangle = \langle e_1^0, \text{Env} \rangle \leftrightarrow \langle e_1^1, \text{Env} \rangle \leftrightarrow \langle e_1^2, \text{Env} \rangle \leftrightarrow \cdots \leftrightarrow \langle e_1^{k_1}, \text{Env} \rangle = \langle n_1, \text{Env} \rangle$ [A.1]





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

### A.3. 命令式特性的语义

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





we can obtain the tree:

```
(C_i) \xrightarrow{1} (C_j) \xrightarrow{2} (C_k)
(C_3) \xrightarrow{} (C_3) \xrightarrow{} (C_3)
<Mem, (c_1 ; c_2) ; c_3> \rightarrow>_{Env} Mem_3
```

反之亦然。

- 2) 令 \( c_1 \) 为指令 while \( e \) do \( c \)，\( c_2 \) 为指令 if \( e \) then \( (c ; c_1) \) else skip。令 Mem\(_1\) 和 Mem\(_2\) 为两个内存状态。我们需要证明：

<Mem\(_1\), \( c_1 \)> \rightarrow>_{Env} Mem\(_2\) \leftrightarrow <Mem\(_1\), \( c_2 \)> \rightarrow>_{Env} Mem\(_2\)

我们需要从 <Mem\(_1\), \( c_1 \)> \rightarrow>_{Env} Mem\(_2\) 的推导树构造出 <Mem\(_1\), \( c_2 \)> \rightarrow>_{Env} Mem\(_2\) 的推导树，反之亦然。

(\(\Rightarrow\)) 若 <Mem\(_1\), \( c_1 \)> \rightarrow>_{Env} Mem\(_2\)，则存在两种可能的情况。

### 情况 1：最后应用的规则是 \( C_6 \)：

[[e]]_{Env}^{Mem_1} = (true, Mem_1)   (C_j) ... (C_k)
(C_6)
<Mem\(_1\), \( c_1 \)> \rightarrow>_{Env} Mem\(_2\)   [A.5]

并且，可以构造如下推导树：

[[e]]_{Env}^{Mem_1} = (true, Mem_1)   (C_j) ... (C_k)
(C_4)
<Mem\(_1\), \( c_2 \)> \rightarrow>_{Env} Mem\(_2\)   [A.6]

### 情况 2：最后应用的规则是 \( C_7 \)：

(C_7) [[e]]_{Env}^{Mem_1} = (false, Mem\(_2\))
<Mem\(_1\), \( c_1 \)> \rightarrow>_{Env} Mem\(_2\)   [A.7]

并且，可以构造如下推导树：

```
⟦e⟧_Env^Mem1 = (false, Mem2)   (C2)   ⟨Mem2, skip⟩ →Env Mem2  
                                    ⟨Mem1, c2⟩ →Env Mem2       [A.8]  
(C5) ───────────────────────────────────────────────────
```

(⇐) 若 ⟨Mem1, c2⟩ →Env Mem2，则存在两种可能的情况。要么最后应用的规则是 C4，可由树 [A.6] 构造出树 [A.5]；要么最后应用的规则是 C5，可由树 [A.8] 构造出树 [A.7]。两种情况均结束证明。

## 习题 4.2 的解答

在整个练习中，我们假设对于任意表达式 e，⟦e⟧_Env^Mem1 = (true, Mem2)（分别，⟦e⟧_Env^Mem1 = (false, Mem2)) 当且仅当 ⟦not e⟧_Env^Mem1 = (false, Mem2)（分别，⟦not e⟧_Env^Mem1 = (true, Mem2))。

- 1) repeat until 循环的语义可定义如下：

```
⟨Mem1, c⟩ →Env Mem2   ⟦e⟧_Env^Mem2 = (true, Mem3)  
(C8) ────────────────────────────────────────────  
        ⟨Mem1, repeat c until e⟩ →Env Mem3
```

```
⟨Mem1, c⟩ →Env Mem2   ⟦e⟧_Env^Mem2 = (false, Mem3)   ⟨Mem3, repeat c until e⟩ →Env Mem4  
(C9) ────────────────────────────────────────────────────────────────────────  
                  ⟨Mem1, repeat c until e⟩ →Env Mem4
```

- 2) 程序 c 的执行树：

```
repeat (z := z * ↓x; x := ↓x - 1) until ↓x = 0  
       c1      c2      e
```

```
∇1 ⟦e⟧_Env^Mem2 = (false, Mem2)   (C8)   ...  
(C9) ───────────────────────────────────────────  
      ⟨Mem, repeat c1 ; c2 until e⟩ →Env Mem3
```

其中 ∇1 是如下树：

```
Env(z) = rz    Env(x) = rx  
⟦z * ↓x⟧_Env^Mem = (6, Mem)   ⟦↓x - 1⟧_Env^Mem1 = (1, Mem1)  
(C1) ⟨Mem, c1⟩ →Env Mem [rz := 6]   (C1) ⟨Mem1, c2⟩ →Env Mem1 [rx := 1]  
(C3) ─────────────────────────────────────────────────────  
           ⟨Mem, c1 ; c2⟩ →Env Mem2
```

而 ∇2 是如下树：

```
\((C_1) \frac{\text{Env}(z) = r_z \llbracket \downarrow z * \downarrow x \rrbracket^{Mem_2}_{Env} = (6, Mem_2)}{\langle Mem_2, c_1 \rangle \to_{Env} Mem_2} \quad (C_1) \frac{\text{Env}(x) = r_x \llbracket \downarrow x - 1 \rrbracket^{Mem_2}_{Env} = (0, Mem_2)}{\langle Mem_2, c_2 \rangle \to_{Env} Mem_2 [r_x := 0]}\)
\((C_3) \frac{}{\langle Mem_2, c_1 ; c_2 \rangle \to_{Env} Mem_3}\)
```

- 3) 令 \(c_1\) 为指令 repeat \(c\) until \(e\)，\(c_2\) 为指令 \(c\) ; if \(e\) then skip else \(c_1\)。\(\langle Mem, c_1 \rangle \to_{Env} Mem'\) 的推导树可由 \(\langle Mem, c_2 \rangle \to_{Env} Mem'\) 的推导树构造而来，反之亦然。只需考虑可用于构造这些树的可能规则，并考察 \(e\) 的两个可能取值。在两种情况下，我们只需按如下方式“组合”推导树。

### 情况 1：当 \(\llbracket e \rrbracket^{Mem_1}_{Env} = (true, Mem')\) 时，从以下树开始：

```
\((C_i) \frac{\nabla}{\langle Mem, c \rangle \to_{Env} Mem_1} \quad (C_8) \frac{\llbracket e \rrbracket^{Mem_1}_{Env} = (true, Mem')}{\langle Mem, c_1 \rangle \to_{Env} Mem'}\)
```

可得到如下树：

```
\((C_i) \frac{\nabla}{\langle Mem, c \rangle \to_{Env} Mem_1} \quad \frac{\llbracket e \rrbracket^{Mem_1}_{Env} = (true, Mem')}{(C_4)} \quad (C_2) \frac{\langle Mem', \text{skip} \rangle \to_{Env} Mem'}{\langle Mem_1, \text{if e then skip else } c_1 \rangle \to_{Env} Mem'}\)
\((C_3) \frac{}{\langle Mem, c_2 \rangle \to_{Env} Mem'}\)
```

反之亦然。

### 情况 2：当 \(\llbracket e \rrbracket^{Mem_1}_{Env} = (false, Mem_2)\) 时，从以下树开始：

```
\((C_i) \frac{\nabla 1}{\langle Mem, c \rangle \to_{Env} Mem_1} = \llbracket e \rrbracket^{Mem_1}_{Env} = (false, Mem_2) \quad (C_j) \frac{\nabla 2}{\langle Mem_2, c_1 \rangle \to_{Env} Mem'}\)
\((C_9) \frac{}{\langle Mem, c_1 \rangle \to_{Env} Mem'}\)
```

可得到如下树：

```
\((C_i) \frac{\nabla 1}{\langle Mem, c \rangle \to_{Env} Mem_1} \quad \frac{\llbracket e \rrbracket^{Mem_1}_{Env} = (false, Mem_2)}{(C_5)} \quad (C_j) \frac{\nabla 2}{\langle Mem_2, c_1 \rangle \to_{Env} Mem'}\)
\((C_3) \frac{\langle Mem_1, \text{if e then skip else } c_1 \rangle \to_{Env} Mem'}{\langle Mem, c_2 \rangle \to_{Env} Mem'}\)
```

反之亦然。

- 4) 令 \( c'_1 \) 为指令 while not \( e \) do \( c \)，\( c'_2 \) 为指令 repeat \( c \) until not \( e \)（参见第 4.8.3 节）。

对于 (P1)，让我们通过对 \(\langle \text{Mem}_H, c_H \rangle \to_{\text{Env}} \text{Mem}_H\) 进行结构归纳（使用 Box 4.4 给出的归纳方案）来证明，若：

```
\langle \text{Mem}_H, c_H \rangle \to_{\text{Env}} \text{Mem}_H = \langle \text{Mem}, c_1 \rangle \to_{\text{Env}} \text{Mem}'
```

则 \(\langle \text{Mem}, c_2 \rangle \to_{\text{Env}} \text{Mem}'\)。若 \(\langle \text{Mem}_H, c_H \rangle \to_{\text{Env}} \text{Mem}_H\) 是利用规则 \( C_1 \) 至 \( C_5 \) 中的某一条，或 \( C_8 \) 或 \( C_9 \) 得到的，则上述等式不成立，从而结束证明。因此，我们只需考虑规则 \( C_6 \) 和 \( C_7 \)。

### 情况 1：若 \(\langle \text{Mem}, c_1 \rangle \to_{\text{Env}} \text{Mem}'\) 是利用规则 \( C_6 \) 得到的：

```
          [[e]]^{\text{Mem}}_{\text{Env}} = (true, \text{Mem}_1) \quad (C_i) \quad \frac{1}{\langle \text{Mem}_1, c \rangle \to_{\text{Env}} \text{Mem}_2} \quad \dots \quad \frac{}{\langle \text{Mem}_2, c_1 \rangle \to_{\text{Env}} \text{Mem}'} \quad (C_j)
----------------------------------------------------------------------------------------------------------------
                                             \langle \text{Mem}, c_1 \rangle \to_{\text{Env}} \text{Mem}' \quad (C_6)
```

那么，根据归纳假设，存在 \(\langle \text{Mem}_2, c_2 \rangle \to_{\text{Env}} \text{Mem}'\) 的推导树。存在两种子情况。

### 情况 1.a：若该树是利用规则 \( C_4 \) 得到的：

```
          [[e]]^{\text{Mem}_2}_{\text{Env}} = (true, \text{Mem}_3) \quad (C_k) \quad \frac{2}{\langle \text{Mem}_3, c'_2 \rangle \to_{\text{Env}} \text{Mem}'}
-----------------------------------------------------------------------------------------------
                                      \langle \text{Mem}_2, c_2 \rangle \to_{\text{Env}} \text{Mem}' \quad (C_4)
```

那么我们构造树 \(\nabla_3\)：

```
          \frac{1}{\langle \text{Mem}_1, c \rangle \to_{\text{Env}} \text{Mem}_2} \quad [[\text{not } e]]^{\text{Mem}_2}_{\text{Env}} = (false, \text{Mem}_3) \quad \frac{2}{\langle \text{Mem}_3, c'_2 \rangle \to_{\text{Env}} \text{Mem}'}
-------------------------------------------------------------------------------------------------------------------
                                    \langle \text{Mem}_1, c'_2 \rangle \to_{\text{Env}} \text{Mem}' \quad (C_9) \quad (C_i) \quad (C_k)
```

我们得出结论，因为可以构造如下树：

```
          [[e]]^{\text{Mem}}_{\text{Env}} = (true, \text{Mem}_1) \quad (C_9) \quad \frac{3}{\langle \text{Mem}_1, c'_2 \rangle \to_{\text{Env}} \text{Mem}'}
-----------------------------------------------------------------------------------
                                  \langle \text{Mem}, c_2 \rangle \to_{\text{Env}} \text{Mem}' \quad (C_4)
```

### 情况 1.b：若该树是利用规则 \( C_5 \) 得到的：

```
          [[e]]^{\text{Mem}_2}_{\text{Env}} = (false, \text{Mem}') \quad (C_2) \quad \langle \text{Mem}', \text{skip} \rangle \to_{\text{Env}} \text{Mem}'
-----------------------------------------------------------------------------------
                                    \langle \text{Mem}_2, c_2 \rangle \to_{\text{Env}} \text{Mem}' \quad (C_5)
```





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




## 练习 4.3 解答

为便于校对，我们引入如下记法：

```
(a := 0 ; b := x) ; while ↓b ≥ y do b := ↓b − y ; a := ↓a + 1
c1     c2         ew         c3         c4
     cw
          cd
```

- 1) 在以下内容中，我们假设 Env(a) 和 Env(b) 是 R 中的元素，分别记为 ra 和 rb。令 vx 和 vy 为 Env(x) 和 Env(y) 的值。按定义，与此程序各组件相关的函数如下：

```
[[c1]]Env : Mem ↦ Mem[ra := 0]
[[c2]]Env : Mem ↦ Mem[rb := vx]
[[c1; c2]]Env : Mem ↦ Mem[ra := 0][rb := vx]
[[c3]]Env : Mem ↦ Mem[rb := Mem(rb) − vy]
[[c4]]Env : Mem ↦ Mem[ra := Mem(ra) + 1]
[[c3; c4]]Env : Mem ↦ Mem[rb := Mem(rb) − vy][ra := Mem(ra) + 1]
[[cw]]Env = lfp(F) where F : f ↦ cond([[ew]]Env, f ◦ [[c3; c4]]Env, id)
F(f) : Mem ↦ { f([[c3; c4]]Env(Mem)) if Mem(rb) ≥ vy
       Mem       if Mem(rb) < vy
```

函数 [[c3; c4]]Env 描述了执行一次循环体后获得的状态。当循环体执行两次时，函数 [[c3; c4]]2Env = [[c3; c4]]Env ◦ [[c3; c4]]Env 定义如下：

```
Mem ↦ ( Mem [rb := Mem(rb) − vy][ra := Mem(ra) + 1]
       [rb := Mem(rb) − vy − vy][ra := Mem(ra) + 1 + 1] )
   = Mem[rb := Mem(rb) − 2vy][ra := Mem(ra) + 2]
```

更一般地，当循环体执行 n 次时，我们有（关于 n 的简单归纳）：

```
[[c3; c4]]nEnv : Mem ↦ Mem[rb := Mem(rb) − nvy][ra := Mem(ra) + n]
```

使用定理 4.2，让我们从 F 0(⊥) = ⊥ : Mem ↦ 未定义 计算 lfp(F) = ⊔({Fn(⊥) | n ≥ 0})。按定义，我们有：

```
F 1(⊥) = F(⊥) : Mem ↦ { undef if Mem(rb) ≥ vy
                    Mem if Mem(rb) < vy
```

同样：

```
F 2(⊥) = F(F 1(⊥)) : Mem ↦ { F 1(⊥)([[c3; c4]]Env(Mem)) if Mem(rb) ≥ vy
                      Mem       if Mem(rb) < vy
```

并且由于 \([[c_3; c_4]]_{\text{Env}}(\text{Mem})(r_b) = \text{Mem}(r_b) - v_y\)，我们得到：

\(F^2(\bot) : \text{Mem} \mapsto \left\{ \begin{array}{ l l } \text{未定义} & \text{如果 } \text{Mem}(r_b) \ge 2v_y \text{ 且 } \text{Mem}(r_b) \ge v_y \\ [[c_3; c_4]]_{\text{Env}}(\text{Mem}) & \text{如果 } \text{Mem}(r_b) < 2v_y \text{ 且 } \text{Mem}(r_b) \ge v_y \\ \text{Mem} & \text{如果 } \text{Mem}(r_b) < v_y \end{array} \right.\)

再进行一次迭代，我们得到：

\(F^3(\bot) = F(F^2(\bot)) : \text{Mem} \mapsto \left\{ \begin{array}{ l l } F^2(\bot)([[c_3; c_4]]_{\text{Env}}(\text{Mem})) & \text{如果 } \text{Mem}(r_b) \ge v_y \\ \text{Mem} & \text{如果 } \text{Mem}(r_b) < v_y \end{array} \right.\)

因此：

\(F^3(\bot) : \text{Mem} \mapsto \left\{ \begin{array}{ l l } \text{未定义} & \text{如果 } \text{Mem}(r_b) \ge 3v_y \text{ 且 } \text{Mem}(r_b) \ge 2v_y \text{ 且 } \text{Mem}(r_b) \ge v_y \\ [[c_3; c_4]]_{\text{Env}}^2(\text{Mem}) & \text{如果 } \text{Mem}(r_b) < 3v_y \text{ 且 } \text{Mem}(r_b) \ge 2v_y \text{ 且 } \text{Mem}(r_b) \ge v_y \\ [[c_3; c_4]]_{\text{Env}}(\text{Mem}) & \text{如果 } \text{Mem}(r_b) < 2v_y \text{ 且 } \text{Mem}(r_b) \ge v_y \\ \text{Mem} & \text{如果 } \text{Mem}(r_b) < v_y \end{array} \right.\)

通过对 \(n\) 的简单归纳，我们最终得到：

\(F^n(\bot) : \text{Mem} \mapsto \left\{ \begin{array}{ l l } \text{未定义} & \text{如果 } \text{Mem}(r_b) \ge nv_y \text{ 且 } \dots \text{ 且 } \text{Mem}(r_b) \ge (n-1)v_y \\ [[c_3; c_4]]_{\text{Env}}^{n-1}(\text{Mem}) & \text{如果 } \dots \text{ 且 } \text{Mem}(r_b) < nv_y \text{ 且 } \text{Mem}(r_b) \ge (n-1)v_y \\ \dots & \\ [[c_3; c_4]]_{\text{Env}}^1(\text{Mem}) & \text{如果 } \text{Mem}(r_b) < 2v_y \text{ 且 } \text{Mem}(r_b) \ge v_y \\ \text{Mem} & \text{如果 } \text{Mem}(r_b) < v_y \end{array} \right.\)

这可以写成更简单的形式：

\(F^n(\bot) : \text{Mem} \mapsto \left\{ \begin{array}{ l l } \text{未定义} & \text{如果 } \text{Mem}(r_b) \ge nv_y \\ [[c_3; c_4]]_{\text{Env}}^{p-1}(\text{Mem}) & \text{如果 } (p-1)v_y \le \text{Mem}(r_b) < pv_y \\ \text{Mem} & \text{如果 } \text{Mem}(r_b) < v_y \end{array} \right.\)

因此函数 \([[c_w]]_{\text{Env}} = \text{lfp}(F) = \sqcup_{n \ge 0}\{F^n(\bot)\}\) 定义如下：

\([[c_w]]_{\text{Env}} : \text{Mem} \mapsto \left\{ \begin{array}{ l l } [[c_3; c_4]]_{\text{Env}}^{p-1}(\text{Mem}) & \text{如果 } (p-1)v_y \le \text{Mem}(r_b) < pv_y \\ \text{Mem} & \text{如果 } \text{Mem}(r_b) < v_y \end{array} \right.\)

程序 \(c_d\) 的指称语义因此定义为：

\([[c_d]]_{\text{Env}} : \text{Mem} \mapsto [[c_w]]_{\text{Env}}(\text{Mem}[r_a := 0][r_b := v_x])\)

我们最终得到：

\([[c_d]]_{\text{Env}} : \text{Mem} \mapsto \left\{ \begin{array}{ l l } \text{Mem}[r_b := v_x - (p-1)v_y][r_a := p-1] & \text{如果 } (p-1)v_y \le v_x < pv_y \\ \text{Mem}[r_a := 0][r_b := v_x] & \text{如果 } v_x < v_y \end{array} \right.\)

当 \(v_x < v_y\) 时，\(v_x\) 除以 \(v_y\) 的商为：

\([[c_d]]_{Env}(Mem)(a) = 0,\)

而该除法的余数为

\([[c_d]]_{Env}(Mem)(b) = v_x.\)

当 \(v_x \ge v_y\) 时，我们有：

\(v_x = (v_y \times [[c_d]]_{Env}(Mem)(r_a)) + [[c_d]]_{Env}(Mem)(r_b)\)

且 \([[c_d]]_{Env}(Mem)(r_b) < v_y\)，这再次完成了证明。

- 2) 让我们证明以下 Hoare 三元组的全部正确性：

\(\{ \underline{x \ge 0 \text{ 且 } y > 0} \}_{P_1} c_d \{ \underline{x = (\downarrow a \times y) + \downarrow b \text{ 且 } \downarrow b < y} \}_{P_2}\)

```
\(\nabla_2\)
\((W_T)\)
\(\downarrow a\)
\(\{x = (\downarrow a \times y) + \downarrow b\}\)
\(\{P_1\} c_w \{ \text{and } \downarrow b \ge 0 \text{ and } y > 0 \text{ and not } \downarrow b \ge y \}\)
\((Cons_G) \downarrow 1\)
\(\{P_1\} c_1 ; c_2 (P_1) (Cons_D)\)
---------------------------------------------------
\((Seq)\)
\(\{P_1\} c_d {P_2}\)
```

规则 (ConsD) 的应用基于以下蕴含：

\((x = (\downarrow a \times y) + \downarrow b \text{ 且 } \downarrow b \ge 0 \text{ 且 } y > 0 \text{ 且不 } \downarrow b \ge y) \Rightarrow P_2\)

此处使用的循环不变式 \(P_I\) 为：

\(x = (\downarrow a \times y) + \downarrow b \text{ 且 } \downarrow b \ge 0 \text{ 且 } y > 0\)

而变体为 \(V = \downarrow b\)。由于 \(P_I \Rightarrow \downarrow b \ge 0\)，可以应用规则 \((W_T)\)。推理树 \(\nabla_2\) 的构造如下：

```
(Aff)
\(\{P'_I \text{ and } \downarrow b - y < n\} \downarrow c_3 \{P'_I \text{ and } \downarrow b < n\}\)
(Cons_G)
\(\{P_I \text{ and } \downarrow b \ge y \text{ and } \downarrow b = n\} \downarrow c_3 \{P'_I \text{ and } \downarrow b < n\}\)
(Aff)
\(\{P'_I \text{ and } \downarrow b < n\} \downarrow c_4 \{P_I \text{ and } \downarrow b < n\}\)
(Seq)
\(\{P_I \text{ and } \downarrow b \ge y \text{ and } \downarrow b = n\} c_3 ; c_4 \{P_I \text{ and } \downarrow b < n\}\)
```

其中 \(P'_I\) 是将 \(\downarrow a\) 替换为 \(\downarrow a + 1\) 后的断言 \(P_I\)：

\(x = ((\downarrow a + 1) \times y) + \downarrow b \text{ 且 } \downarrow b \ge 0 \text{ 且 } y > 0\)

而 \(P''_I\) 是将 \(\downarrow b\) 替换为 \(\downarrow b - y\) 后的断言 \(P'_I\)：

\(x = ((\downarrow a + 1) \times y) + \downarrow b - y \text{ 且 } \downarrow b - y \ge 0 \text{ 且 } y > 0\)

规则 (Cons$_G$) 的应用基于以下蕴含：

\((P_I \text{ 且 } \downarrow b \ge y \text{ 且 } \downarrow b = n) \Rightarrow (P''_I \text{ 且 } \downarrow b - y < n)\)

这很容易建立。最后，树 \(\nabla_1\) 的构造基于蕴含 \(P_1 \Rightarrow (P_1 \text{ 且 } x = 0 \times y + x)\)，并按如下方式得到：

```
(Aff) \(\{P_1 \text{ and } x = 0 \times y + x\} c_2 \{P''_I\}\)          (Aff) \(\{P''_I\} c_2 \{P_I\}\)
(Seq) \(\{P_1 \text{ and } x = 0 \times y + x\} c_1 ; c_2 \{P_I\}\)
```

其中 \(P''_I\) 是将 \(\downarrow b\) 替换为 \(x\) 后的断言 \(P_I\)：

\(x = (\downarrow a \times y) + x \text{ 且 } x \ge 0 \text{ 且 } y > 0\)

## 符号列表

- F, 120
- Maj, 122
- Min, 122
- [A -> B], 121
- Mem[r := v], 19
- Env |- e ~~ v, 55, 56, 62
- -->Env, 89, 109, 115
- <Mem, c> -->Env Mem', 89, 109, 115
- ⊥, 122
- [[e]]EnvMem, 24, 81, 131
- codom(theta), 149
- cond(fb, f1, f2), 120
- Env |- d : Env', 169
- [[c]]Env, 119
- dom(Env), 16
- dom(Mem), 19
- dom(theta), 149
- Env, 16, 42
- ==, 69, 96
- ==_F, 71
- Err, 22
- ErrE, 42, 81
- ErrT, 42
- ErrU, 42, 81
- [[e]]EnvU, 42
- Env |- e : tau, 169
- [[_]]_, 81
- forall alpha_1 ... alpha_n. tau, 145
- F(e), 40
- F(sigma), 147
- <--*Env, 94
- Gen(tau, Env), 147
- undef, 119
- Env |- e :> (tau, theta), 154
- Env |- c : WT, 169
- Inst(sigma), 159
- Lang_1, 29
- Lang_3, 79, 109, 115, 118, 168
- <=, 210
- lfp(f), 122
- -->Def_3, 113
- -->Def_1, 26
- -->Lang_1, 29
- -->Def_3, 86
- Mem, 18
- Mgu(Q), 156
- +, 16
- ↓e, 78
- ↑e, 78
- preceq, 173
- A_S, 65
- K, 35, 77
- Def_1, 26
- Def_3, 77, 112
- E, 16, 42
- I_E, 43, 81
- Exp_1, 21
- Exp_2, 35




- Expr₃, 77
- IF, 43
- M, 18
- R, 18
- V, 16, 22, 43, 81
- ∀, 22, 42, 43, 81
- X, 16, 35
- X^{est}_{Env}, 20

- X^{ref}_{Env}, 20
- ⟨c₁, Mem₁⟩ ↪_{Env} ⟨c₂, Mem₂⟩, 93
- ⊥, 122
- ⊑, 121
- {P₁}c{P₂}, 129
- id, 163
- Env ⊢ e : τ, 151

## 程序索引

- 'a const1 (类型), 23
- 'a def1 (类型), 27
- 'a def4 (类型), 79, 113
- 'a exp1 (类型), 22
- 'a exp2 (类型), 37
- 'a exp4, 79
- 'a option (类型), 17
- 'a pgn4 (类型), 80, 110, 115
- 'a val_err2 (类型), 44
- 'a valeur2 (类型), 44
- 'a valeurs1 (类型), 23
- 'a valeurs4 (类型), 82, 113
- And2, 37
- And4, 78
- App2, 38
- B_Op2, 38
- B_Op4, 79
- Bang1, 22
- CBool2, 37
- CInt1, 23
- CInt2, 37
- CRef1, 23
- Clos2, 43
- ClosRec2, 44
- Cste1, 22
- Cste2, 37
- Cste4, 79
- Cycle, 157

- Deref4, 79
- Div2, 37
- Div4, 78
- E_Exec2, 43
- E_Typ2, 43
- E_Undef2, 43
- Eq2, 37
- Eq4, 78
- Err2, 44
- Erreur1, 23
- Eval_Error (异常), 83
- Exec_Error (异常), 91
- Fern4, 113, 114
- Fun2, 37
- IAffect4, 80
- IBloc4, 110
- ICall4, 115
- ICond4, 79
- ISeq4, 80
- ISkip4, 80
- IWhile4, 80
- If2, 38
- Incompatible_tys, 157
- Leq2, 37
- Leq4, 78
- Let_def1, 27
- Let_def4, 79
- Let_proc4, 112
- Letin2, 37
- bin_op4 (类型), 78
- Mgu, 156
- compose, 162
- Minus2, 37
- const2 (类型), 37
- Minus4, 78
- const4 (类型), 78
- Mult2, 37
- dom_env, 18
- Mult4, 78
- e25, 52
- Not2, 37
- empty, 149
- Not4, 78
- erreur2 (类型), 44
- Not_found, 159
- eval2, 50
- Or2, 37
- eval_exp1, 25
- Or4, 78
- eval_exp4, 85
- Plus1, 22
- ex_e0, 29
- Plus2, 37
- ex_e1, 30
- Plus4, 78
- ex_exp1, 22, 38
- Rec2, 38
- ex_la0, 30
- Ref4, 79
- ex_ld0, 29
- Ref_Var1, 27
- ex_ld4_1, 87
- T_bool, 144
- ex_m0, 29
- T_fun, 144
- ex_m1, 30
- T_int, 144
- ex_pgm4_1, 80
- T_var, 144
- ex_pgm4_2, 112
- U_Op2, 37
- exec_pgm4, 91, 110, 117
- U_Op4, 79
- find_id_ty (类型), 145
- Unbound_var (类型), 145
- find_vars_in_env, 148
- VBool2, 43
- find_vars_in_type, 148
- VCste1, 23
- free_var2, 41
- VCste4, 82
- generalize, 148
- VInt2, 43
- list_assoc, 159
- Val2, 44
- list_mem_assoc, 162
- Vari1, 22
- new_ty_var, 144
- Var2, 37
- occur_check, 157
- Var4, 79
- refer (类型), 27
- Var_def1, 27
- scheme_t (类型), 145
- a_fact, 38
- singleton, 149
- add_mem, 83
- specialize, 159
- ajout_liaison_env, 18
- subst_env, 161
- apply2_binary_op, 45
- subst_t (类型), 149
- apply2_unary_op, 45
- trans_def1_exec, 28
- apply4_binary_op, 83
- trans_def1, 28
- apply4_unary_op, 83
- trans_def4_exec, 86
- apply_one_transf, 150
- trans_def4, 86, 114
- apply, 150
- trans_lang1_exec, 30
- bin_op2 (类型), 37
- trans_lang1, 30
- trivial_scheme, 146
- ty_env_t (类型), 145
- type_expr, 165
- type_t (类型), 144
- unary_op2 (类型), 37
- unary_op4 (类型), 78
- unify, 157
- union_singleton, 18
- valeur_ref, 19
- var_name_t (类型), 144
- write_mem, 20

## 参考文献

- [ACC 92] ACCART HARDIN T., VIGUIÉ DONZEAU-GOUGE V., *编程概念与工具*, InterEditions, 巴黎, 1992.
- [ARM 10] ARM, Cortex-M3器件通用用户指南：修订版 A, 可在以下网址查阅：https://developer.arm.com/documentation/dui0552/a/the-cortex-m3-processor/programmers-model/core-registers, 2010.
- [DAM 82] DAMAS L., MILNER R., “函数程序的主类型方案”, *第9届ACM编程语言原理研讨会论文集 (POPL)*, 第 207–212 页, ACM Press, 1982.
- [DOL 93] DOLIGEZ D., LEROY X., “一种用于多线程 ML 实现的并发分代垃圾收集器”, *第20届ACM编程语言原理研讨会论文集 (POPL)*, 第 113–123 页, ACM Press, 1993.
- [DOL 94] DOLIGEZ D., GONTHIER G., “面向多处理器系统的可移植、非侵入式垃圾回收”, *第21届ACM编程语言原理研讨会论文集 (POPL)*, 第 70–83 页, ACM Press, 1994.
- [DOW 09] DOWEK G., *编程语言原理*, Springer-Verlag, 伦敦, 2009.
- [DOW 11] DOWEK G., LÉVY J.-J., *编程语言理论导论*, Springer-Verlag, 伦敦, 2011.
- [DUB 18] DUBOIS C., GRINCHTEIN O., PEARSON J. _et al._, “使用交互式定理证明器探索带消息延迟的电信协议属性”, 在 JOHNSEN E., SCHAEFER I. (编), *第16届软件工程与形式方法国际会议论文集*, 6 月 27–29 日, 法国图卢兹, 2018.
- [FRI 01] FRIEDMAN D.P., WAND M., HAYNES C.T., *编程语言精要*, MIT Press, 剑桥, 2001.
- [HOA 83] HOARE C.A.R., “计算机编程的公理基础”, *ACM 通讯*, 第 26 卷, 第 1 期, 第 53–56 页, 1983.
- [JAU 16] JAUME M., 离散数学要素 – 课程、习题解答、Python 和 OCaml 实现, Ellipses, 巴黎, 2016.
- [KLE 52] KLEENE S., 元数学导论, North-Holland Publishing Company, 阿姆斯特丹, 1952.
- [LER 91] LEROY X., WEIS P., “多态类型推断与赋值”, 第18届ACM编程语言原理研讨会论文集 (POPL), 第 291–302 页, ACM Press, 1991.
- [MIP 13] MIPS Technologies, MIPS® 程序员架构 第二卷A: MIPS32® 指令集, MIPS Tech, 加利福尼亚, 2013.
- [NIE 07] NIELSON H.R., NIELSON F., 带应用的语义学：一本入门读物, Springer-Verlag, 伦敦, 2007.
- [PES 99] PESSAUX F., LEROY X., “基于类型的未捕获异常分析”, 第26届ACM编程语言原理研讨会论文集 (POPL), 第 276–290 页, ACM Press, 1999.
- [PIE 02] PIERCE B.C., 类型与编程语言, MIT Press, 剑桥, 2002.
- [PLO 04] PLOTKIN G.D., “运行时语义的结构化方法”, 逻辑与代数编程杂志, 第 60–61 卷, 第 17–139 页, 2004.
- [STO 77] STOY J.E., 指示语义学：斯科特-斯特雷奇编程语言理论方法, MIT Press, 剑桥, 1977.
- [TUR 95] TURING A., GIRARD J.-Y., 图灵机, Le Seuil, 巴黎, 1995.
- [WIN 93] WINSKEL G., 编程语言形式语义学：导论, MIT Press, 剑桥, 1993.

## A
- 代数
  - 类型, 143, 168, 194, 248
- ALUs, 4
- 分析
  - 词法, 10
  - 语义, 11
  - 语法, 11
- 数组, 191, 225
- 赋值, 29, 89

## B, C
- 绑定, 16
  - 词法, 57
  - 静态, 57
- 块, 109
- 字节码, 14
- 调用
  - 按名, 61, 62
  - 按需, 61, 62
  - 按值, 56, 62
- 按引用传递, 118, 226, 233
- 按值传递, 115, 226

- ccpo, 122
- 链, 122
- 字符, 186
  - 字符串, 194
- 闭包, 43, 46, 60, 113
- 替换的陪域, 149
- 编译器, 12
  - 连续函数, 124
- 反变, 174
- 正确性
  - 部分, 130
  - 全部, 133
- 协变, 174
- CPU, 3

## D, E
- 确定性, 65, 100
- 域
  - 内存的域, 18, 19
  - 替换的域, 149
  - 环境的域, 16, 17
- 字节序, 225
- 环境
  - 等价, 71
  - 求值, 16
  - 执行, 16
  - 类型化, 145
- 相等
  - 函数之间, 220
  - 函数的, 138
  - 物理, 217
  - 结构, 218
- 错误
  - 执行, 42, 81, 139
  - 初始化, 19, 42, 81
  - 类型化, 42
- 表达式的求值, 24, 42, 81
- 异常
  - 处理程序, 246
- 表达式
  - 闭合, 40
  - 等价, 69
  - 多态, 143

## F
- 定点, 122
- 浮点, 187
- 冻结变量/表达式, 61
- 函数
  - 闭合, 40
  - 组成, 119
  - 逻辑, 2
  - 单调, 123
  - 偏函数, 119
  - 严格, 124

## G
- GC, 239
  - 保守, 242
  - 复制, 240

- 分代, 241
- 标记-清除, 240
- 引用计数, 239
- 一般性, 210
- 泛化, 147
- 广义条件, 205
- 守卫, 213

## H, I
- Hoare 三元组, 129
- 标识符, 15
  - 定义, 26
- 输入输出模式, 228
- 归纳
  - 在推导树上, 135
  - 在求值树上, 65
  - 结构, 39, 65
  - 良基, 98
- 推理, 154, 159
  - 类型 (算法), 163
- 输入/输出, 7
- 实例, 143, 151, 159
- 整数
  - 转换 (C), 185
  - 溢出, 182
  - 有符号, 182
  - 大小, 181
  - 补数, 181
- 解释器, 12
- 中断, 8

## L
- 格, 122
- 最小上界, 122
- 链接, 13
- 循环
  - 不变式, 132
  - 变式, 133, 134
- 下界, 122




### M
- 匹配/with, 208
- 内存, 2, 18
  - 分配, 78, 234
  - 动态, 235
  - 静态, 234
- EPROM, 3
- 释放, 237
- 泄漏, 238
- RAM, 2, 3
- ROM, 3
- 最小值, 122

### N, O
- 名称绑定
  - 掩蔽, 16
- 目标代码, 12
- 顺序
  - 部分, 122
    - 在部分函数上, 121
  - 良基, 75

### P
- 参数
  - 实参, 36, 46, 57
  - 形参, 36, 46, 57
- 模式, 208
  - 析取, 214
  - 卫语句, 213
  - 匹配, 208
- 指针, 223
- 多态, 143, 145, 171
- 后条件, 129
- 前条件, 129
- 预序类型, 173
- 过程, 112
- 程序
  - 等价, 96

### R
- 记录, 196, 214
- 引用, 18, 229
  - 可访问, 20
  - 计数, 239
- 寄存器, 3
- 规则
  - 求值, 55, 56, 62
  - 推理, 54
  - 子类型, 174
  - 类型化, 139, 151, 169

### S
- 方案
  - 字段, 145
  - 平凡, 146
  - 类型, 143, 145
- 作用域
  - 动态, 46, 53, 113
  - 词法, 46, 53, 113
  - 静态, 46, 53, 113
- 语义
  - 公理, 129
  - 指称, 118
  - 动态, 42
  - 操作
    - 大步, 60, 89
    - 小步, 60, 74, 93
- 栈, 6
- 状态, 20
- 字符串, 194
- 结构, 196
- 替换, 131, 149, 210
  - 应用, 160
  - 复合, 161
- 幂等, 149
- 子类型, 173
- switch/case, 205
- 语法
  - 抽象, 21, 35
  - 具体, 21
- 项, 209
- 终止, 59, 63, 75, 98, 134
- 标记, 11
- 树
  - 抽象语法, 11, 36, 80
  - 求值, 65
  - 推理, 65
  - 类型化, 153
- 类型
  - 缩写, 195
  - 兼容, 151
  - 枚举, 200
  - 主, 154
  - 记录, 196
  - 和, 202
- 类型化
  - 完备性, 168
  - 正确性, 168
  - 动态, 139
  - 名义, 195
  - 多态, 143, 171
  - 安全, 167
  - 静态, 140
  - 强, 139
  - 结构, 195
  - 弱, 139

### U
- 统一, 155
  - 算法, 156
  - 统一子, 155
    - 主, 156
- 并, 202
- 上界, 122

### V
- 变量
  - 绑定, 39, 146
  - 虚, 39
  - 自由, 40, 147
  - 新鲜, 143
  - 一般化, 143, 145
  - 局部, 35
  - 可变, 15, 20, 87
  - 类型, 141
- 虚拟机
  - 解释器, 12–14

## 2021
DELHAYE Jean-Loic
*计算世界内部：技术、用途、挑战*

DUVAUT Patrick, DALLOZ Xavier, MENGA David, KOEHL François, CHRIQUI Vidal, BRILL Joerg
*增强自我之物联网，I.AM：为新的可持续未来赋能创新*

## 2020
DARCHE Philippe
*微处理器 1：引论 – 计算与存储功能 – 计算模型与计算机体系结构*
*微处理器 2：核心概念 – 数字系统中的通信*
*微处理器 3：核心概念 – 硬件方面*
*微处理器 4：核心概念 – 软件方面*
*微处理器 5：开发、调试与测试的软硬件方面 – 微型计算机*

LAFFLY Dominique
TORUS 1 – 面向服务的开放资源：环境数据的云计算
TORUS 2 – 面向服务的开放资源：环境数据的云计算
TORUS 3 – 面向服务的开放资源：环境数据的云计算

LAURENT Anne, LAURENT Dominique, MADERA Cédrine
数据湖
（数据库和大数据集 – 第 2 卷）

OULHADJ Hamouche, DAACHI Boubaker, MENASRI Riad
机器人元启发式
（优化启发式集 – 第 2 卷）

SADIQUI Ali
计算机网络安全

VENTRE Daniel
人工智能、网络安全与网络防御

## 2019
BESBES Walid, DHOUIB Diala, WASSAN Niaz, MARREKCHI Emma
解决运输问题：迈向绿色物流

CLERC Maurice
迭代优化器：难度度量与基准

GHALALA Riadh
SQL Server 2014/2016 中的分析 SQL

TOUNSI Wiem
网络警惕与数字信任：云计算和物联网时代的网络安全

## 2018
ANDRO Mathieu
数字图书馆与众包
（数字工具与用途集 – 第 5 卷）

ARNALDI Bruno, GUITTON Pascal, MOREAU Guillaume
虚拟现实与增强现实：神话与现实

BERTHIER Thierry, TEBOUL Bruno
从数字痕迹到算法投影

CARDON Alain
超越人工智能：从人类意识到人工意识

HOMAYOUNI S. Mahdi, FONTES Dalila B.M.M.
海上作业元启发式
（优化启发式集 – 第 1 卷）

JEANSOULIN Robert
JavaScript 与开放数据

PIVERT Olivier
NoSQL 数据模型：趋势与挑战
（数据库和大数据集 – 第 1 卷）

SEDKAOUI Soraya
数据分析与大数据

SALEH Imad, AMMI Mehdi, SZONIECKY Samuel
物联网的挑战：技术、用途、伦理
（数字工具与用途集 – 第 7 卷）

SZONIECKY Samuel
生态系统知识：信息与通信的建模与分析方法
（数字工具与用途集 – 第 6 卷）

## 2017
BENMAMMAR Badr
Java 中的并发、实时与分布式编程

HÉLIODORE Frédéric, NAKIB Amir, ISMAIL Boussaad, OUCHRAA Salma, SCHMITT Laurent
智能电网元启发式
（元启发式集 – 第 10 卷）

MA Haiping, SIMON Dan
基于生物地理学优化的进化计算
（元启发式集 – 第 8 卷）

PÉTROWSKI Alain, BEN-HAMIDA Sana
进化算法
（元启发式集 – 第 9 卷）

PAI G A Vijayalakshmi
投资组合优化元启发式
（元启发式集 – 第 11 卷）

## 2016
BLUM Christian, FESTA Paola
生物信息学中字符串问题的元启发式
（元启发式集 – 第 6 卷）

DEROUSSI Laurent
物流元启发式
（元启发式集 – 第 4 卷）

DHAENENS Clarisse and JOURDAN Laetitia
大数据元启发式
（元启发式集 – 第 5 卷）

LABADIE Nacima, PRINS Christian, PRODHON Caroline
车辆路径问题元启发式
（元启发式集 – 第 3 卷）

LEROY Laure
立体视觉中的视觉疲劳缓解

LUTTON Evelyne, PERROT Nathalie, TONDA Albert
食品科学与技术的进化算法
（元启发式集 – 第 7 卷）

MAGOULÈS Frédéric, ZHAO Hai-Xiang
建筑能耗分析中的数据挖掘与机器学习

RIGO Michel
高级图论与组合数学

## 2015
BARBIER Franck, RECOUSSINE Jean-Luc
COBOL 软件现代化：从原则到使用 BLU AGE® 方法的实现

CHEN Ken
基于仿真与分析及其在计算机网络中应用的性能评估

CLERC Maurice
优化中的引导随机性
（元启发式集 – 第 1 卷）

DURAND Nicolas, GIANAZZA David, GOTTELAND Jean-Baptiste, ALLIOT Jean-Marc
航空交通管理元启发式
（元启发式集 – 第 2 卷）

MAGOULÈS Frédéric, ROUX François-Xavier, HOUZEAUX Guillaume
并行科学计算

MUNEESAWANG Paisarn, YAMMEN Suchart
硬盘驱动器行业的目视检测技术

## 2014
BOULANGER Jean-Louis
应用于工业复杂系统的形式方法
BOULANGER Jean-Louis
应用于复杂系统的形式方法：B 方法的实现

GARDI Frédéric, BENOIST Thierry, DARLAY Julien, ESTELLON Bertrand, MEGEL Romain
基于局部搜索的数理规划求解器

KRICHEN Saoussen, CHAOUACHI Jouhaina
与图相关的优化与决策支持系统

LARRIEU Nicolas, VARET Antoine
航空电子系统软件快速原型：面向复杂系统认证的模型导向方法

OUSSALAH Mourad Chabane
软件体系结构 1
软件体系结构 2

PASCHOS Vangelis Th
组合优化 – 三卷本系列，第 2 版
组合优化概念 – 第 1 卷，第 2 版
问题与新方法 – 第 2 卷，第 2 版
组合优化应用 – 第 3 卷，第 2 版

QUESNEL Flavien
大规模虚拟化基础设施调度：迈向协作管理

RIGO Michel
形式语言、自动机与计数系统 1：词组合数学导论
形式语言、自动机与计数系统 2：可识别性与可判定性的应用




- SAINT-DIZIER Patrick  
  音乐修辞学：基础与注解方案

- TOUATI Sid, DE DINECHIN Benoit  
  高级后端优化

## 2013

- ANDRÉ Etienne, SOULAT Romain  
  逆方法：实时嵌入式系统的参数化验证

- BOULANGER Jean-Louis  
  基于软件设备的安全管理

- DELAHAYE Daniel, PUECHMOREL Stéphane  
  航空交通建模与优化

- FRANCOPOULO Gil  
  LMF — 词汇标记框架

- GHÉDIRA Khaled  
  约束满足问题

- ROCHANGE Christine, UHRIG Sascha, SAINRAT Pascal  
  可预测时间架构

- WAHBI Mohamed  
  分布式约束满足问题的算法与排序启发式

- ZELM Martin et al.  
  企业互操作性

## 2012

- ARBOLEDA Hugo, ROYER Jean-Claude  
  模型驱动与软件产品线工程

- BLANCHET Gérard, DUPOUY Bertrand  
  计算机体系结构

- BOULANGER Jean-Louis  
  *形式化方法在工业中的应用：形式化验证*

- BOULANGER Jean-Louis  
  *形式化方法：从模型到代码的工业应用*

- CALVARY Gaëlle, DELOT Thierry, SÈDES Florence, TIGLI Jean-Yves  
  *计算机科学与环境智能*

- MAHOUT Vincent  
  *汇编语言程序设计：ARM Cortex-M3 2.0：组织、创新与领域*

- MARLET Renaud  
  *程序特化*

- SOTO Maria, SEVAUX Marc, ROSSI André, LAURENT Johann  
  *嵌入式系统中的内存分配问题：优化方法*

## 2011

- BICHOT Charles-Edmond, SIARRY Patrick  
  *图分割*

- BOULANGER Jean-Louis  
  *软件静态分析：抽象解释*

- CAFERRA Ricardo  
  *面向计算机科学与人工智能的逻辑*

- HOMES Bernard  
  *软件测试基础*

- KORDON Fabrice, HADDAD Serge, PAUTET Laurent, PETRUCCI Laure  
  *分布式系统：设计与算法*

- KORDON Fabrice, HADDAD Serge, PAUTET Laurent, PETRUCCI Laure  
  *分布式系统中的模型与分析*

- LORCA Xavier  
  *基于树的图分割约束*

- TRUCHET Charlotte, ASSAYAG Gerard  
  音乐中的约束编程

- VICAT-BLANC PRIMET Pascale et al.  
  计算网络：从集群到云计算

## 2010

- AUDIBERT Pierre  
  面向信息学与计算机科学的数学

- BABAU Jean-Philippe et al.  
  面向分布式实时嵌入式系统的模型驱动工程

- BOULANGER Jean-Louis  
  计算机体系结构安全

- MONMARCHE Nicolas et al.  
  人工蚁

- PANETTO Hervé, BOUDJLIDA Nacer  
  企业软件与应用互操作性 2010

- SIGAUD Olivier et al.  
  人工智能中的马尔可夫决策过程

- SOLNON Christine  
  蚁群优化与约束编程

- AUBRUN Christophe, SIMON Daniel, SONG Ye-Qiong et al.  
  面向可信网络化控制系统的协同设计方法

- LECOUTRE Christophe  
  约束网络 / 面向技术与算法的简约目标

## 2009

- FOURNIER Jean-Claude  
  图论与应用

- GUEDON Jeanpierre  
  莫杰特变换 / 理论与应用

- JARD Claude, ROUX Olivier  
  通信嵌入式系统 / 软件与设计

## 2008

- BANAÔTRE Michel, MARRÓN Pedro José, OLLERO Hannibal, WOLITZ Adam  
  协作嵌入式系统与无线传感器网络

- MERZ Stephan, NAVET Nicolas  
  实时系统建模与验证

- PASCHOS Vangelis Th  
  组合优化与理论计算机科学：接口与前景

- WALDNER Jean-Baptiste  
  纳米计算机与群体智能

## 2007

- BENHAMOU Frédéric, JUSSIEN Narendra, O’SULLIVAN Barry  
  约束编程趋势

- JUSSIEN Narendra  
  数独 A 到 Z

## 2006

- BABAU Jean-Philippe et al.  
  从 MDD 概念到实验与示例 – DRES 2006

- HABRIAS Henri, FRAPPIER Marc  
  软件规格说明方法

- MURAT Cecile, PASCHOS Vangelis Th  
  图上的概率组合优化

- PANETTO Hervé, BOUDJLIDA Nacer  
  企业软件与应用互操作性 2006 / IFAC-IFIP I-ESA ’2006

## 2005

- GÉRARD Sébastien et al.  
  面向分布式实时嵌入式系统的模型驱动工程

- PANETTO Hervé  
  企业软件与应用互操作性 2005

## WILEY 最终用户许可协议

访问 www.wiley.com/go/eula 以获取 Wiley 的电子书 EULA。

