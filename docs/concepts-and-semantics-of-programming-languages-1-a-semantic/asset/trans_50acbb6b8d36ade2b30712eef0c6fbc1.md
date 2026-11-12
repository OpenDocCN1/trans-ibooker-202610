

## 3.1.3. 通过归纳对表达式进行推理

集合 Exp₂ 以归纳方式定义：每个表达式 \(e \in \text{Exp}_2\) 都是通过有限次应用 Exp₂ 的构造规则获得的。这意味着，可以通过应用 Box 3.1 中所示的结构归纳方案，对 Exp₂ 的元素证明属性 P：我们只需针对每个构造项证明属性 P，并假设 P 对该构造项的每个组件都成立。这种归纳推理在本书余下部分中将多次使用。

```
if ∀v ∈ K P(v)
and ∀x ∈ X P(x)
and ∀e₁ ∈ Exp₂ P(e₁) ⇒ P(op e₁)
and ∀e₁, e₂ ∈ Exp₂ (P(e₁) and P(e₂)) ⇒ P(e₁ op e₂)
and ∀e₁, e₂, e₃ ∈ Exp₂ (P(e₁) and P(e₂) and P(e₃)) ⇒ P(if e₁ then e₂ else e₃)
and ∀x ∈ X ∀e₁, e₂ ∈ Exp₂ (P(e₁) and P(e₂)) ⇒ P(let x = e₁ in e₂)
and ∀x ∈ X ∀e ∈ Exp₂ P(e) ⇒ P(fun x ↦ e)
and ∀x, f ∈ X ∀e ∈ Exp₂ P(e) ⇒ P(rec f x = e)
and ∀e₁, e₂ ∈ Exp₂ (P(e₁) and P(e₂)) ⇒ P((e₁ e₂))
then ∀e ∈ Exp₂ P(e)
```

### Box 3.1. 针对 e ∈ Exp₂ 的结构归纳方案

## 3.1.4. 变量声明、绑定变量和自由变量

同一个标识符可以在同一个表达式中使用多次，每次使用都称为标识符的一次出现。同一个标识符的不同出现并不都起相同的作用。在构造项 (6) let x = e₁ in e₂ 中，标识符 x 被声明，并在环境中绑定到 e₁ 的值，该值将用于求值 e₂。构造项 (7) 在表达式 e 中抽象了变量 x。该参数 x 在表达式 fun x ↦ e 中被认为是绑定（或抽象）的。它可以重命名而不修改函数的定义；因此，它有时被称为哑变量。例如，在 fun x ↦ x + y 中将 x 替换为 z 得到 fun z ↦ z + y，两者是等价的。显然，我们不能将 x 替换为 y，因为 y 已经出现在函数体 x + y 中。在构造项 (8) 中，标识符 f 被声明，变量 x 在表达式 e 中被绑定。例如，表达式是：

```
let x₁ = 25 in let f = fun x₂ ↦ x₃ + 3 in (f 7) + x₄ [3.3]
```

包含四个 $x$ 的出现，索引从 1 到 4。出现 2 在 $f$ 的函数体中抽象了出现 3。这些哑变量 $x$ 的出现可以重命名。出现 1 声明了标识符 $x$，用于为出现 4 赋值。

当表达式中变量的出现既未被声明也未被绑定时，可以说该出现在该表达式中是 *自由* 的。对于 $e \in \mathbf{Exp}_2$，变量集合 $\mathcal{F}(e)$（即在 $e$ 中至少有一次自由出现的变量）以归纳方式定义为：

```
\mathcal{F} : \mathbf{Exp}_2 \to \wp(\mathbf{X})
\mathcal{F}(v) = \emptyset \quad (v \in \mathbf{K})
\mathcal{F}(x) = \{x\} \quad (x \in \mathbf{X})
\mathcal{F}(\mathbf{op} \ e_1) = \mathcal{F}(e_1)
\mathcal{F}(e_1 \mathbf{op} \ e_2) = \mathcal{F}(e_1) \cup \mathcal{F}(e_2)
\mathcal{F}(\mathbf{if} \ e_1 \ \mathbf{then} \ e_2 \ \mathbf{else} \ e_3) = \mathcal{F}(e_1) \cup \mathcal{F}(e_2) \cup \mathcal{F}(e_3)
\mathcal{F}(\mathbf{let} \ x = e_1 \ \mathbf{in} \ e_2) = \mathcal{F}(e_1) \cup (\mathcal{F}(e_2) \setminus \{x\})
\mathcal{F}(\mathbf{fun} \ x \mapsto e) = \mathcal{F}(e) \setminus \{x\}
\mathcal{F}(\mathbf{rec} \ f \ x = e) = \mathcal{F}(e) \setminus \{x, f\}
\mathcal{F}((e_1 e_2)) = \mathcal{F}(e_1) \cup \mathcal{F}(e_2)
```

不包含自由变量的表达式称为 *闭合*。对于函数 $\mathbf{fun}\ x \mapsto e$，如果它是常量函数（$\mathcal{F}(e) = \emptyset$ 且 $x$ 不出现在 $e$ 中）或者 $\mathcal{F}(e) = \{x\}$（即函数体 $e$ 除了 $x$ 外没有自由变量），则称其为闭合。例如，函数 $\mathbf{fun}\ x \mapsto x + 1$ 是闭合的。$\mathbf{fun}\ x_1 \mapsto \mathbf{fun}\ x_2 \mapsto x_1 + x_2$ 也是闭合的，但 $\mathbf{fun}\ x_2 \mapsto x_1 + x_2$ 不是，因为在此例中 $x_1$ 是自由变量：在切换到子表达式时，这一概念并不稳定（表达式是闭合的并不必然意味着其子表达式也是闭合的）。

示例 3.1. 对于在 [3.2] 中定义的表达式 $e_{fact}$ 和在 [3.3] 中定义的表达式 $e_{25}$，有 $\mathcal{F}(e_{fact}) = \mathcal{F}(e_{25}) = \emptyset$。在表达式 $e_{25}$ 中，$x$ 的出现 3 在子表达式 $x + 3$ 中是自由的，但在表达式 $\mathbf{fun}\ x \mapsto x + 3$ 中变为绑定。以下两个示例不是闭合表达式：

```
\mathcal{F}(\mathbf{fun}\ x_1 \mapsto x_1 + x_2) = \{x_2\}
\mathcal{F}(\mathbf{let}\ x_1 = x_1 + x_2\ \mathbf{in}\ x_1 + x_3) = \{x_1, x_2, x_3\}
```

直观地说，求值一个表达式需要知道其变量的值。表达式中自由变量的值在求值环境中查找。例如，要对表达式 $x + 1$ 求值，需要知道 $x$ 的值。相比之下，表达式 $\mathbf{let}\ x = 3\ \mathbf{in}\ 2 * x$ 是闭合的，可以在不涉及其所含（绑定）变量“外部”信息的情况下求值。

**Exp₂** 中表达式的自由变量集合可以递归定义如下：

```python
def free_var2(e):
    if isinstance(e,Cste2): return emptyset
    if isinstance(e,Var2): return [e.symb]
    if isinstance(e,U_Op2): return free_var2(e.exp1)
    if isinstance(e,B_Op2): return union(free_var2(e.exp1),free_var2(e.exp2))
    if isinstance(e,IF2):
        return union(union(free_var2(e.exp1),free_var2(e.exp2)),
                     free_var2(e.exp3))
    if isinstance(e,Letin2):
        return union(free_var2(e.exp1),
                     diff_set(free_var2(e.exp2),[e.symb]))
    if isinstance(e,Fun2): return diff_set(free_var2(e.corps),[e.param])
    if isinstance(e,Rec2): return diff_set(free_var2(e.corps),[e.param,e.nom])
    if isinstance(e,App2): return union(free_var2(e.exp1),free_var2(e.exp2))
    raise ValueError
```

```ocaml
let rec free_var2 e = match e with
  | Cste2 -> emptyset
  | Var2 x -> [x]
  | U_Op2 (op, e1) -> free_var2 e1
  | B_Op2 (op, e1, e2) -> union (free_var2 e1) (free_var2 e2)
  | IF2 (e1, e2, e3) ->
      union (union (free_var2 e1) (free_var2 e2)) (free_var2 e3)
  | Letin2 (x, e1, e2) -> union (free_var2 e1) (diff_set (free_var2 e2) [x])
  | Fun2 (x, e1) -> diff_set (free_var2 e1) [x]
  | Rec2 (f, x, e1) -> diff_set (free_var2 e1) [x; f]
  | App2 (e1, e2) -> union (free_var2 e1) (free_var2 e2)
val free_var2 : 'a exp2 -> 'a list
```

考虑示例 3.1 中的表达式，得到：

```python
>>> free_var2(ex_exp1)
['x2']
>>> free_var2(ex_exp2)
['x2', 'x1', 'x3']
>>> free_var2(a_fact)
[]
```

```ocaml
# free_var2_ex_exp1 ;;
- : string list = ["x2"]
# free_var2 ex_exp2 ;;
- : string list = ["x1"; "x2"; "x3"]
# free_var2 a_fact ;;
- : string list = []
```

## 3.2. 执行语义：求值函数

编程语言的执行语义（或 *动态语义*）描述了该语言中所有语法正确程序的执行。在本节中，我们研究“纯”函数式语言 **Exp$_2$**。由于该语言不提供对内存的操作，只需要求值环境的概念。请注意，与内存（内容可修改）不同，执行环境（也称为求值环境）只能通过将标识符绑定到新值来扩展；该标识符的任何现有绑定都会被新绑定简单地遮蔽。

**Exp$_2$** 中的程序是一个表达式。其执行包括对表达式求值，即计算其值。为此，我们首先定义一个求值函数，如第 2 章所示：

```
$[[ \_ ]]_{\_} : \mathbf{E} \times \mathbf{Exp}_2 \rightarrow \mathbb{V}$
```

值 $[[e]]_{Env}$ 是表达式 $e$ 在环境 $Env$ 中求值的结果。在这里，环境 $Env \in \mathbf{E}$ 是绑定 $(x, v) \in \mathbf{X} \times \mathbf{V}$ 的列表。集合 $\mathbf{V}$（在第 3.2.2 节中定义）是与变量关联的值的集合，而 $\mathbb{V}$ 是 $\mathbf{V}$ 与一组用于指示求值失败的特殊值的并集。

### 3.2.1. 求值错误

我们选择区分在对表达式 $e \in \mathbf{Exp}_2$ 求值期间产生的三种不同类型的错误。

- 1) 表达式 $e$ 中自由变量出现的值必须在当前执行环境中找到。如果该变量不属于环境的域，则对 $e$ 的求值返回一个标记为 Err$_U$ 的错误（U 表示未绑定）。

- 2) 算术错误，例如溢出或数学上未定义的值，标记为 Err$_E$（E 表示异常，即处理器用于指示“异常”情况的机制，这种情况使得无法继续正常执行）。出于我们的目的，我们将考虑限制在除以零的情况。

- 3) **Exp$_2$** 中的表达式表示整数、布尔值或函数。因此，可能尝试将布尔值与整数相加（3 + true），或者将整数应用于表达式（8(x + 1)）。此类表达式在语法上是正确的，但不能表示 V 中的值。在我们的模型中，对此类表达式的求值会导致一种称为类型错误 Err$_T$ 的错误。类型错误在执行前的检测将在第 5 章中介绍。请注意，某些语言为我们认为在 **Exp$_2$** 中无效的表达式赋予语义。例如，在 Python（v3.6）中，表达式 3 + True 返回值 4。

