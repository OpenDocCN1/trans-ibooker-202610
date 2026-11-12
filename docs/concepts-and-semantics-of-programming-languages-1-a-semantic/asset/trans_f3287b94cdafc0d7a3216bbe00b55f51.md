

Mem' 的计算不一定终止。我们现在希望用一个真正的数学函数来定义这个语义：

[[ ]] : (E x Lang_3) -> (M -> M)

这是直译语义的目的。在这个框架内，关于程序及其执行的某些属性更容易证明，该框架在 [STO 77] 中有详细阐述。

当然，这个函数 [[ ]] 与操作语义必须在语义上等价。用正式术语说：

```
forall Env in E forall c in Lang_3 [[c]]_Env(Mem) = Mem' <-> <Mem, c> -->_Env Mem'
forall Mem, Mem' in M
[4.13]
```

因此，[[c]]_Env 是一个从 M 到 M（记作 M -> M）的偏函数，它给定一个内存状态 Mem，在有限时间内计算出在环境 Env 中执行程序 c 后得到的内存状态 Mem'。如果程序 c 在状态 (Env, Mem) 中的执行不终止，则函数 [[c]]_Env 对 Mem 未定义（我们用 [[c]]_Env(Mem) = undef 表示这种情况）。因此，直译语义定义了一个函数 [[ ]]_Env，为每个程序 c 提供一个描述执行 c 时发生的内存状态转换的函数。这个函数 [[ ]]_Env 是偏函数，而且也是组合式的：函数 [[c]]_Env 是通过组合与 c 的组件对应的函数得到的。exec_pgm 的情况并非如此，它通过在不同状态下递归使用相同指令的语义来定义某个状态下 while e do c 的语义，该状态是在循环体执行一次后得到的。

```
[[x := e]]_Env = Mem |-> { Mem[r := v] if Env(x) = r in R and v = [[e]]_Env^Mem in V
                             undef      otherwise

[[skip]]_Env = id = Mem |-> Mem

[[c1 ; c2]]_Env = [[c2]]_Env o [[c1]]_Env = Mem |-> [[c2]]_Env([[c1]]_Env(Mem))

[[if e then c1 else c2]]_Env = cond([[e]]_Env, [[c1]]_Env, [[c2]]_Env)
cond : (M -> IB) x (M -> M) x (M -> M) -> (M -> M)

cond(fb, f1, f2) = Mem |-> { f1(Mem) if fb(Mem) = 1
                             f2(Mem) if fb(Mem) = 0
                             undef    if fb(Mem) = undef

[[while e do c]]_Env = lfp(F) = Union { F^n(perp) | n >= 0 }
F : (M -> M) -> (M -> M)
F : f |-> cond([[e]]_Env, f o [[c]]_Env, id)
perp : Mem |-> undef
```

## 框 4.7. 直译语义

$[[c]]_{\text{Env}}$ 的定义在框 4.7 中给出。赋值指令 $x := e$ 与一个函数相关联，该函数通过将表达式 $e$ 的求值结果（如果不引发错误）存储在 $x$ 所表示的位置（如果 $x$ 表示一个引用）来转换内存状态。与指令 $\mathbf{skip}$ 相关联的函数是恒等函数。对于序列 $c_1 ; c_2$，我们组合与 $c_1$ 和 $c_2$ 相关联的两个函数；此外，如果其中一个函数应用于其未定义的状态，那么应用该组合的结果也是未定义的。与 if $e$ then $c_1$ else $c_2$ 相关联的函数使用函数 $\mathbf{cond}(f_b, f_1, f_2)$，该函数给定一个内存状态，根据该状态下布尔函数（$f_b$）的结果选择应用于该状态的函数（$f_1$ 或 $f_2$）。最后，考虑指令 $\mathbf{while}\ e\ \mathbf{do}\ c$。使用大步操作语义形式主义，可以很容易地证明（参见练习 4.1）：

\(\mathbf{while}\ e\ \mathbf{do}\ c \equiv \text{if } e \text{ then } (c; \mathbf{while}\ e\ \mathbf{do}\ c) \text{ else skip}\)

因此，使用上述定义，与循环 while e do c 相关联的函数可以是：

\([[\mathbf{while}\ e\ \mathbf{do}\ c]]_{\text{Env}} = \mathbf{cond}([[e]]_{\text{Env}}, [[\mathbf{while}\ e\ \mathbf{do}\ c]]_{\text{Env}} \circ [[c]]_{\text{Env}}, id)\)

但是，使用 $[[\mathbf{while}\ e\ \mathbf{do}\ c]]_{\text{Env}}$ 的这种定义不是组合式的。解决这个问题的一个方案是将函数 $[[\mathbf{while}\ e\ \mathbf{do}\ c]]_{\text{Env}}$ 抽象化以得到函数：

\(F : f \mapsto \mathbf{cond}([[e]]_{\text{Env}}, f \circ [[c]]_{\text{Env}}, id)\)

这个函数 $F$ 被称为泛函，因为它的参数本身也是一个函数。它的主体仅使用 $\mathbf{while}\ e\ \mathbf{do}\ c$ 的组件，因此 $F$ 是组合式的。此外，它验证：

\(F([[ \mathbf{while}\ e\ \mathbf{do}\ c ]]_{\text{Env}}) = [[ \mathbf{while}\ e\ \mathbf{do}\ c ]]_{\text{Env}}\)

换句话说，$[[\mathbf{while}\ e\ \mathbf{do}\ c]]_{\text{Env}}$ 是 $F$ 的一个不动点。要采用这种方法，我们必须建立 $F$ 至少存在一个不动点，并且当存在多个不动点时，确定使用哪一个。

例如，考虑程序 $\mathbf{while}\ \downarrow x = 0\ \mathbf{do}\ \mathbf{skip}$，它可以与泛函 $F : f \mapsto \mathbf{cond}((\downarrow x = 0)_{\text{Env}}, f, id)$ 相关联，因为 $[[\mathbf{skip}]]_{\text{Env}} = id$ 且 $f \circ id = f$。根据 $\mathbf{cond}$ 的定义，我们因此有：

\(F : f \mapsto \left( \text{Mem} \mapsto \begin{cases} f(\text{Mem}) & \text{if } \text{Mem}(\text{Env}(x)) = 0 \text{ and } \text{Env}(x) \in \mathbf{R} \\ \text{Mem} & \text{if } \text{Mem}(\text{Env}(x)) \neq 0 \text{ and } \text{Env}(x) \in \mathbf{R} \\ \text{undef} & \text{else} \end{cases} \right)\)

给定一个函数 $h : \mathbf{M} \rightarrow \mathbf{M}$，我们定义如下函数 $g_h : \mathbf{M} \rightarrow \mathbf{M}$：

\(g_h : \text{Mem} \mapsto \begin{cases} h(\text{Mem}) & \text{if } \text{Mem}(\text{Env}(x)) = 0 \text{ and } \text{Env}(x) \in \mathbf{R} \\ \text{Mem} & \text{if } \text{Mem}(\text{Env}(x)) \neq 0 \text{ and } \text{Env}(x) \in \mathbf{R} \\ \text{undef} & \text{else} \end{cases}\)

根据 $F$ 的定义，我们因此有：

$$
F(g_h) : \text{Mem} \mapsto \left\{\begin{array}{ll} g_h(\text{Mem}) & \text{if } \text{Mem}(\text{Env}(x)) = 0 \text{ and } \text{Env}(x) \in \mathbf{R} \\ \text{Mem} & \text{if } \text{Mem}(\text{Env}(x)) \neq 0 \text{ and } \text{Env}(x) \in \mathbf{R} \\ \text{undef} & \text{else} \end{array}\right.
$$

并且从 $g_h$ 的定义我们得到：

$$
F(g_h) : \text{Mem} \mapsto \left\{\begin{array}{ll} h(\text{Mem}) & \text{if } \text{Mem}(\text{Env}(x)) = 0 \text{ and } \text{Env}(x) \in \mathbf{R} \\ \text{Mem} & \text{if } \text{Mem}(\text{Env}(x)) \neq 0 \text{ and } \text{Env}(x) \in \mathbf{R} \\ \text{undef} & \text{else} \end{array}\right.
$$

因此 $F(g_h) = g_h$，所以对于每个函数 $h : \mathbf{M} \nrightarrow \mathbf{M}$，$g_h$ 都是 $F$ 的一个不动点。这个例子表明泛函 $F$ 存在多个不动点。

我们现在需要确定这些不动点中哪一个与操作语义刻画完全相同的状态（参见属性 [4.13]）。对于满足 $\text{Mem}(\text{Env}(x)) = 2$ 的内存状态 Mem，循环体不执行，内存也不修改。对于满足 $\text{Mem}(\text{Env}(x)) = 0$ 的内存状态 Mem，循环体执行无穷多次，程序不终止，因此我们希望 $[[\text{while } \downarrow x = 0 \text{ do skip}]]_{\text{Env}}(\text{Mem}) = \text{undef}$。从函数 $k : \text{Mem} \mapsto \text{undef}$，我们定义 $g_k$：

$$
g_k : \text{Mem} \mapsto \left\{\begin{array}{ll} \text{Mem} & \text{if } \text{Mem}(\text{Env}(x)) \neq 0 \text{ and } \text{Env}(x) \in \mathbf{R} \\ \text{undef} & \text{else} \end{array}\right.
$$

我们看到 $g_k$ 是 $F$ 的一个不动点，而且这是我们希望与 $[[\text{while } \downarrow x = 0 \text{ do skip}]]_{\text{Env}}$ 关联的不动点。

对于 $F$ 中的任何其他不动点 $g_h$，如果 $g_k(\text{Mem}) \neq \text{undef}$，那么 $g_k(\text{Mem}) = g_h(\text{Mem})$。如我们将看到的，通过定义从 $\mathbf{M}$ 到 $\mathbf{M}$ 的偏函数上的偏序关系，这个不动点对应于泛函 $F$ 的最小不动点，记为 $\text{lfp}(F)$。这个不动点的存在性和唯一性证明，以及用于计算它的程序，将在下面介绍。

## 4.7.1.2. 泛函最小不动点的存在性、唯一性与计算

### 4.7.1.2.1. 偏函数上的偏序关系

为了定义泛函最小不动点的概念，我们在从 $A$ 到 $B$ 的偏函数集合（记作 $[A \to B]$）上引入了序关系 $\sqsubseteq$：

$$
f_1 \sqsubseteq f_2 \Leftrightarrow (\forall x \in A \ f_1(x) \neq \text{undef} \Rightarrow f_1(x) = f_2(x))
$$

直观上，$f_1 \sqsubseteq f_2$ 当且仅当 $f_2$ 扩展了 $f_1$，即 $f_2$ 保留了 $f_1$ 的所有结果，并可能提供一些新结果。我们可以轻松证明该关系定义了

