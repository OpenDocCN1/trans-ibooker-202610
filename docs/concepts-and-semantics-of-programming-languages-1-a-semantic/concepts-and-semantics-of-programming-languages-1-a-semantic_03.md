## 2. 编程语言语义介绍

本章直观地介绍了名称、环境、内存等概念，并对这些概念给出了初步的形式化描述。它使读者能够熟悉我们与多位其他作者共享的编程语义方法 [ACC 92, DOW 09, DOW 11, FRI 01, WIN 93]。

任何高级编程语言都使用名称来表示程序处理的实体。这些名称通常称为标识符，强调了它们是按照所选语言的语法规则构建的这一事实。它们可用于表示特定于程序的值或在执行期间计算的值。它们也可以表示位置（即内存中的地址），此时它们称为可变变量。并且，标识符还可以表示运算符、函数、过程、模块、对象等，具体取决于语言中存在的语法结构。例如，π 通常用于表示 π 的近似值；+ 也是一个标识符，表示加法运算符，并且通常放在两个操作数之间，即中缀位置，如 2 + 3。表达式 2 * x + 1 使用标识符 x，要计算其值，我们需要知道 x 所表示的值。检索给定标识符关联的值是任何高级语言核心机制。语言的语义为这种机制提供了一个模型，以简化形式在第 2.1 节中介绍。

以下给出的所有语言、指令、算法等的形式定义，都在编程语言 OCaml 和 Python 中进行了编码，力求对这些定义进行意译，并在这两种语言中生成非常相似的代码版本，即使这些语言的开发者可能会发现这里使用的编程风格相当不同。对于不熟悉这些语言的读者，在代码展示中给出了一些非常简短的说明。但是，OCaml 和 Python 的几乎所有特性都将在第一卷或第二卷中予以考虑，第二卷中涉及面向对象编程。我们希望，正式概念的这两种编码可以帮助那些不太熟悉数学形式化的读者。

### 2.1. 环境、内存和状态

#### 2.1.1. 求值环境

令 **X** 为标识符集，**V** 为值集。将标识符 \(x \in \mathbf{X}\) 与值 \(v \in \mathbf{V}\) 关联称为 *绑定*（标识符到其值的绑定），而一组绑定 ENV 称为 *执行环境* 或 *求值环境*。ENV(\(x\)) 表示 ENV 中标识符 \(x\) 所关联的值。环境集记作 **E**。

在实践中，实际使用的标识符集 **X** 是有限的：通常，我们只考虑出现在程序中的那些标识符。因此，环境可以由一组绑定列表表示，也称为 ENV：

```
[(x_1, ENV(x_1)), (x_2, ENV(x_2)), ... , (x_n, ENV(x_n))]
```

其中 \(\{x_1, x_2, \dots, x_n\}\) 表示 **X** 的一个有限子集，称为环境的 *域*，记作 dom(ENV)。按惯例，ENV(\(x\)) 表示值 \(v\)，该值出现在从头部读取列表 ENV（即从左到右）时遇到的第一个 \((x, v)\) 绑定中。

在此模型中，可以使用运算符 \(\oplus\) 将绑定添加到环境中。按惯例，绑定被添加到表示环境的列表头部（左侧）：

```
\(\(x_{new}, v_{new}) \oplus [(x_1, v_1), (x_2, v_2), \dots , (x_n, v_n)]\)
\(= [(x_{new}, v_{new}), (x_1, v_1), (x_2, v_2), \dots , (x_n, v_n)]\)
```

假设某个操作引入了标识符的新绑定，而该标识符已存在于环境中，例如 \((x_2, v_{new})\)：

```
\(\(x_2, v_{new}) \oplus [(x_1, v_1), (x_2, v_2), \dots , (x_n, v_n)]\)
\(= [(x_2, v_{new}), (x_1, v_1), (x_2, v_2), \dots , (x_n, v_n)]\)
```

由此获得的环境 \((x_2, v_{new}) \oplus \text{ENV}\) 包含两个针对 \(x_2\) 的绑定。查找绑定从环境头部开始，并且按照我们的惯例，新绑定被添加到头部。因此，最近添加的 \((x_2, v_{new})\) 将首先被找到。绑定 \((x_2, v_2)\) 并未被删除，但被认为被新绑定 \((x_2, v_{new})\) *遮蔽*。因此，同一环境中可能存在针对单个标识符 \(x\) 的多个绑定，而为 \(x\) 添加的最后一个绑定将被用来确定环境中 \(x\) 的关联值。正式地，环境 \((x, v) \oplus \text{ENV}\) 验证以下属性：

\(\big((x, v) \oplus \text{ENV}\big)(x') = \begin{cases} v & \text{if } x = x' \\ \text{ENV}(x') & \text{if } x \neq x' \end{cases}\)

按惯例，记号 \((x_2, v_2) \oplus (x_1, v_1) \oplus \text{ENV}\) 用于表示环境 \((x_2, v_2) \oplus \big((x_1, v_1) \oplus \text{ENV}\big)\)。例如，\(\big((x, v_2) \oplus (x, v_1) \oplus \text{ENV}\big)(x) = v_2\)

当环境由绑定列表表示时，值 \(\text{ENV}(x)\) 的查找方式如下：

```
Python
def valeur_de(env,x):
   for (x1,v1) in env:
      if x==x1: return v1
   return None
```

```
OCaml
let rec valeur_de env x = match env with
 | [] -> None
 | (x1, v1) :: t -> if x = x1 then Some v1 else (valeur_de t x)
val valeur_de : ('a * 'b) list -> 'a -> 'b option
```

如果环境中找不到给定标识符的绑定，该函数将返回一个特殊值，以指示绑定的缺失。在 Python 中，使用常量 None 来表达这种值的缺失，而在 OCaml 中，使用预定义的和类型 'a option：

```
OCaml
type 'a option = Some of 'a | None
```

'a option 类型的值要么是 'a 类型的值，要么是常量 None。将类型 'a 的值 \(v\) 转换为类型 'a option 的值，是通过将构造器 Some 应用于 \(v\) 来完成的（参见第 5 章）。值 None 用于表示类型 'a 的值的缺失；更准确地说，None 是一个不是类型 'a 的值的常量。这种类型 'a option 将进一步用于表示某种无意义或缺失的值，但这些值对于完全完成某些定义是必需的。

环境的域可以通过遍历表示它的列表来简单地计算。这里定义的有限集是所有无重复元素的列表。

```
Python
def union_singleton(e,l):
    if e in l: return l
    else: return [e]+l

def dom_env(env):
    r=[]
    for (x,v) in env: r = union_singleton(x,r)
    return r
```

```
OCaml
let rec union_singleton e l = if (List.mem e l) then l else e::l
val union_singleton : 'a -> 'a list -> 'a list
let rec dom_env env = match env with
| [] -> [] | (x, v) :: t -> (union_singleton x (dom_env t))
val dom_env : ('a * 'b) list -> 'a list
```

由于函数 valeur_de 返回的值是通过从列表头部遍历获得的，因此将新绑定 \((x,v)\) 添加到环境中是在列表头部进行的，而 \(x\) 的先前绑定（如果有）会被遮蔽，但不会被删除。

```
Python
def ajout_liaison_env(env,x,v): return [(x,v)]+env
```

```
OCaml
let ajout_liaison_env x v = (x, v) :: env
val ajout_liaison_env : ('a * 'b) list -> 'a -> 'b -> ('a * 'b) list
```

#### 2.1.2. 内存

下面介绍的内存形式模型不对第 1 章中描述的各种物理内存做出区分。内存的状态通过将值与存储该值的单元的位置关联来描述。位置本身被视为值，称为引用。正如我们所见，高级语言允许我们用绑定到单元 c 的引用 r 的标识符 x 来命名包含值 v 的位置 c。

令 \(\mathbf{R}\) 为引用集，\(\mathbf{V}\) 为值集。将引用 \(r \in \mathbf{R}\) 与值 \(v \in \mathbf{V}\) 关联表示为一对 \((r,v)\)，而这样的一对集合 Mem 在这里称为内存。Mem(r) 表示在 Mem 中引用 r 处存储的值。令 M 为内存集。在实践中，实际使用的引用集是有限的：再次强调，通常只考虑程序使用的那些位置。这意味着内存可以由一个列表表示，也称为 Mem：

```
[(r_1, Mem(r_1)), (r_2, Mem(r_2)), \cdots , (r_n, Mem(r_n))]
```





内存中存在一对 $(r, v)$ 记录表明在这个位置已经执行过一次初始化或写入操作。每个被引用的内存单元都可以通过 *读取* 来查询，并可以通过 *写入* 赋予新值。在这种情况下，单元中先前保存的值被删除，它已经 "崩溃"。

在地址 $r$ 上写入值 $v$ 会将内存 $\text{Mem}$ 转换为记为 $\text{Mem}[r := v]$ 的内存；如果在位置 $r$ 上在 $\text{Mem}$ 中存储了一个值，那么这个 "旧" 值将被 $v$ 替换；否则，将一对新的添加到 $\text{Mem}$ 中以考虑写入操作。不存在屏蔽，这与环境的情况相反。在一个已经包含值的位置 $r$ 上写入新值 $v$ 会删除旧值 $\text{Mem}(r)$：

```
[(r_1, v_1), \cdots, (r_i, v_i), \cdots, (r_n, v_n)][r_i := v'_i] = [(r_1, v_1), \cdots, (r_i, v'_i), \cdots, (r_n, v_n)]
```

内存的域 $\text{dom}(\text{Mem})$ 取决于当前环境，并表示引用的空间，这些引用可以（直接或间接）从当前执行环境中绑定且未被屏蔽的标识符访问。将绑定 $(x, r)$ 添加到环境 $\text{Env}$ 具有双重效果，即创建 $(x, r) \oplus \text{Env}$ 并将 $\text{Mem}$ 扩展为 $\text{Mem}[r := v]$。

注。—— 取决于所涉及的语言或程序，值 $v$ 可能在引入 $x$ 时提供，或者稍后提供，或者从不提供。如果在首次使用之前未提供任何值，程序的结果是不可预测的，从而导致称为初始化错误的错误。确实，一个位置始终包含一个值，如果该值未由程序显式确定，则不需要适合当前计算。

请注意，在环境 $\text{Env}$ 中添加绑定 $(x, r)$（其域包含 $x$）可能会屏蔽 $\text{Env}$ 中 $x$ 的先前绑定，但如果 $r$ 已经存在于 $\text{Mem}$ 的域中，则不会将一对新的 $(r, v)$ 添加到 $\text{Mem}$。因此，表示内存的任何对列表都不能包含两个针对相同引用的不同对。内存 $\text{Mem}[r := v]$ 验证以下属性：

```
\text{Mem}[r := v](r') = \begin{cases} v & \text{if } r = r' \\ \text{Mem}(r') & \text{if } r \neq r' \end{cases}
```

内存 $(\text{Mem}[r_1 := v_1])[r_2 := v_2]$ 记为 $\text{Mem}[r_1 := v_1][r_2 := v_2]$。

例如，$(\text{Mem}[r := v_1][r := v_2])(r) = v_2$。

> 函数 *valeur_ref* 计算给定位置存储的值。如果此位置之前没有写入任何内容，该函数返回一个特殊值 (None)，表示没有已知值（即由初始化或计算产生的值）。

```python
def valeur_ref(mem,a):
    if len(mem) == 0: return None
    else:
        a1,v1 = mem[0]
        if a == a1: return v1
        else: return valeur_ref(mem[1:],a)
```

```ocaml
let rec valeur_ref mem a = match mem with
  | [] -> None
  | (a1, v1) :: t -> if a = a1 then Some v1 else (valeur_ref t a)
val valeur_ref : ('a * 'b) list -> 'a -> 'b option
```

以下函数将值写入内存：

```python
def write_mem(mem,a,v):
    if len(mem) == 0: return [(a,v)]
    else:
        a1,v1 = mem[0]
        if a == a1: return [(a1,v)] + mem[1:]
        else: return [(a1,v1)] + write_mem(mem[1:],a,v)
```

```ocaml
let rec write_mem mem a v = match mem with
  | [] -> [(a, v)]
  | (a1, v1) :: t ->
      if a = a1 then (a1, v) :: t else (a1, v1) :: (write_mem t a v)
val write_mem : ('a * 'b) list -> 'a -> 'b -> ('a * 'b) list
```

#### 2.1.3. 状态

状态定义为一对 $(\text{Env}, \text{Mem}) \in \mathbf{E} \times \mathbf{M}$，使得 Mem 域中的任何引用都可以从 Env 中的绑定访问。如果一个引用的值可以通过 Env 中包含的标识符，通过一系列读取、写入或引用操作来读取或写入，则该引用被认为是 *可访问* 的。

给定环境 Env，标识符集合 **X** 被划分为两个子集：$\mathbf{X}^{\text{ref}}_{\text{Env}}$，包含绑定到引用的标识符，以及 $\mathbf{X}^{\text{cst}}_{\text{Env}}$，包含其他标识符：

$\mathbf{X}^{\text{ref}}_{\text{Env}} = \{x \in \mathbf{X} \mid \text{Env}(x) \in \mathbf{R}\} \qquad \mathbf{X}^{\text{cst}}_{\text{Env}} = \{x \in \mathbf{X} \mid \text{Env}(x) \in \mathbf{V} \setminus \mathbf{R}\}$

与 $\mathbf{X}^{\text{ref}}_{\text{Env}}$ 中标识符 $x$ 关联的值是一个引用 $\text{Env}(x) = r$，其中存储了一个值 $\text{Mem}(r)$，该值可以通过写入来修改。$\mathbf{X}^{\text{ref}}_{\text{Env}}$ 的标识符通常称为 *可变变量*。

### 2.2. 表达式的求值

表达式的值是根据求值环境和内存计算的，即在给定状态下计算。此计算由表达式的求值语义定义。

#### 2.2.1. 语法

此处使用的表达式语言 Exp1 将在第 3 章和第 4 章中扩展。其语法在表 2.1 中定义。

| e ::= k | 整数常量 | (k ∈ Z) |
|---|---|---|
| x | 标识符 | (x ∈ X) |
| e1 + e2 | 加法 | (e1, e2 ∈ Exp1) |
| !x | 解引用 | (x ∈ X) |

表 2.1. 表达式语言 Exp1

因此，表达式 e ∈ Exp1 要么是整数常量 k ∈ Z，要么是标识符 x ∈ X，要么是对 Exp1 中两个表达式应用加法运算符得到的表达式，要么是形式为 !x 的表达式，表示绑定到可变变量 x 的内存位置中存储的值。因此，这是集合 Exp1 的归纳定义。请注意，Exp1 不包括赋值构造。这是一个刻意的选择。这一点将通过 Exp1 的扩展在第 2.3 节中更详细地讨论。

注。—— 在定义表达式语法时使用的符号 + 并不表示整数加法运算符。它可以被任何其他符号替换（例如 ⊠）。其含义将由求值语义分配。常量符号也是如此：例如，符号 4 可以被解释为自然整数、相对整数或字符。

示例 2.1.— !x + y 是 Exp1 中的表达式，如同 (x + 2) + 3 一样。此处使用括号来构建表达式，它们属于所谓的具体语法，并在 AST 中消失。

> 语言中格式良好的表达式集合 Exp1 通过归纳定义，并直接用递归求和类型表示。这类类型可以在 OCaml 中构建，但在 Python 中不能；在后一种情况下，可以通过为每个求和类型构造函数定义一个类来部分模拟。每个类必须包含一个方法，其参数与其所实现的求和类型构造函数的参数完全对应。这种类型在 Python 中的实现是 naive 的，用户必须确保这些类被正确使用。我们知道在 Python 中有编程动态类型验证机制的可能性，这些机制模拟强类型（类似于 OCaml 中使用的类型）并确保代码被正确使用；然而，这些技术超出了本书的范围。本书中显示的所有实现的目的仅仅是说明并直观地证明概念的正确处理。如我们已经做过的那样，我们选择这种方法来实现求和类型。

使用 Python，我们定义以下类来表示集合 Exp1 的构造函数：

```python
class Cste1:
    def __init__(self,cste):
        self.cste = cste

class Var1:
    def __init__(self,symb):
        self.symb = symb

class Plus1:
    def __init__(self,exp1,exp2):
        self.exp1 = exp1
        self.exp2 = exp2

class Bang1:
    def __init__(self,symb):
        self.symb = symb
```

例如，示例 2.1 中定义的表达式 e1 = !x + y 写作：

```python
ex_exp1 = Plus1(Bang1("x"),Var1("y"))
```

使用 OCaml，算术表达式的类型直接定义如下：

```ocaml
type 'a exp1 =
    Cste1 of int | Var1 of 'a | Plus1 of 'a exp1 * 'a exp1 | Bang1 of 'a
```

因此，此类型的值可以通过以下方式获得：将 Cste1 构造函数应用于整数值，在这种情况下它们对应于常量表达式；或者将 Var1 构造函数应用于类型 'a 的值，该值对应于用于表示标识符的类型（因此类型 'a exp1 是多态的，因为它取决于另一个类型）；或者将 Plus1 构造函数应用于两个类型 'a exp1 的值；或者将 Bang1 构造函数应用于类型 'a 的值。例如，表达式 e1 = !x + y 写作：

```ocaml
let ex_exp1 = Plus1 (Bang1 ("x"), Var1 ("y"))
val ex_exp1 : string exp1
```

#### 2.2.2. 值

给定状态 (Env,Mem)，我们通过计算 e 在该状态中的值来确定表达式 e ∈ Exp1 的求值语义，即通过在该状态中求值 e 来



此状态。值可以是相对整数或引用，故 V = Z ∪ R。向集合 V 添加一个特定的值 Err；该结果作为“无意义”表达式的值返回。因此，在 Exp₁ 中对表达式求值的结果将是属于集合 V = V ∪ {Err} 的值。

V 中的值要么是相对整数，要么是引用。通过定义求和类型，这两组值可以被归组为一个单一类型。

```python
class CInt1:
    def __init__(self,cst_int):
        self.cst_int = cst_int

class CRef1:
    def __init__(self,cst_adr):
        self.cst_adr = cst_adr
```

每个类都有一个与类同名的（对象）构造函数：从整数 n（或分别从引用 r）获得的常量 k 因此写作 CInt1(n)（分别为 CRef1(r)），并且该整数（分别为引用）可以通过从（对象）k 编写 k.cst_int（分别为 k.cst_adr）来访问。使用 OCaml 时，V 中元素的类型直接定义如下：

```ocaml
type 'a const1 = CInt1 of int | CRef1 of 'a
```

此类型的值可以通过将构造函数 CInt1 应用于整数值，或将构造函数 CRef1 应用于类型为 'a 的值（该值对应于用于表示引用的类型）来获得。

通过应用相同的方法，定义了一个对 V = V ∪ {Err} 元素进行分组的类型：

```python
class VCste1:
    def __init__(self,cste):
        self.cste = cste

class Erreur1:
    pass
```

V 中的元素 v 要么是从常量 k 获得的 V 中的值，写作 VCste1(k)，要么是类 Erreur1 中的对象（此处使用 pass 来表示（对象）构造函数没有参数）。使用 OCaml 时，V 中元素的类型直接定义如下：

```ocaml
type 'a valeurs1 = VCste1 of 'a const1 | Erreur1
```

#### 2.2.3. 求值语义

可用于描述表达式求值的形式主义有几种，这些将在后面介绍。让我们构造一个求值函数：

```
[[-]]_- : E × M × Exp_1 → V
```

表达式 e 在环境 Env 和内存状态 Mem 中的求值表示为 [[e]]^Mem_Env = v，其中 v ∈ V。表 2.2 包含函数 [[-]]_- 的递归定义：

| [[k]]^Mem_Env = k | (k ∈ Z) |
| --- | --- |
| [[x]]^Mem_Env = Env(x) | 如果 x ∈ X 且 x ∈ dom(Env) |
| [[x]]^Mem_Env = Err | 如果 x ∈ X 且 x ∉ dom(Env) |
| [[e1 + e2]]^Mem_Env = [[e1]]^Mem_Env + [[e2]]^Mem_Env | 如果 [[e1]]^Mem_Env ∈ Z 且 [[e2]]^Mem_Env ∈ Z |
| [[e1 + e2]]^Mem_Env = Err | 如果 [[e1]]^Mem_Env ∉ Z 或 [[e2]]^Mem_Env ∉ Z |
| [[!x]]^Mem_Env = Mem(Env(x)) | 如果 x ∈ X^ref_Env |
| [[!x]]^Mem_Env = Err | 如果 x ∉ X^ref_Env |

表 2.2. Exp_1 中表达式的求值

整数常量的值是其表示的整数。标识符的值是绑定到它在环境中的值，或为 Err。由加法符号和两个表达式 e1 及 e2 构造的表达式的值，通过将 e1 和 e2 的求值得到的相对整数相加来获得；如果 e1 或 e2 不是整数，则结果为 Err。当 x 是可变变量时，!x 的值是存储在引用 Env(x) 处的值，否则为 Err。

因此，如果 e 被求值为引用，那么 e 只能是一个标识符。此外，Exp1 中某些表达式在语法上是正确的，但毫无意义：例如，表达式 !x 在 x 不是可变变量时，即当 x 在环境中不绑定引用时，或者 x1 + x2 在 x1（或 x2）是可变变量时。另一方面，!x + y 是一个有意义的表达式，当 y 绑定一个整数且 x 绑定一个指向整数的引用时，它表示一个值。

示例 2.2.– 让我们求值表达式 !x + y

```
in the state Env = [(x, rx), (y, 2)] and Mem = [(rx, 3)]:
```

```
[[!x + y]]^Mem_Env = [[!x]]^Mem_Env + [[y]]^Mem_Env
= Mem(Env(x)) + Env(y) = Mem(rx) + 2 = 3 + 2 = 5
```

求值函数 $[[\_]]\_- : E \times M \times \mathbf{Exp}_1 \rightarrow \mathbb{V}$ 直接定义如下：

```python
def eval_expl(env,mem,e):
    if isinstance(e,Cste1): return VCste1(CInt1(e.cste))
    if isinstance(e,Var1):
        x = valeur_de(env,e.symb)
        if isinstance(x,CInt1) or isinstance(x,CRef1): return VCste1(x)
        return Erreur1()
    if isinstance(e,Plus1):
        ev1 = eval_expl(env,mem,e.exp1)
        if isinstance(ev1,Erreur1): return Erreur1()
        v1 = ev1.cste
        ev2 = eval_expl(env,mem,e.exp2)
        if isinstance(ev2,Erreur1): return Erreur1()
        v2 = ev2.cste
        if isinstance(v1,CInt1) and isinstance(v2,CInt1):
            return VCste1(CInt1(v1.cst_int + v2.cst_int))
        return Erreur1()
    if isinstance(e,Bang1):
        x = valeur_de(env,e.symb)
        if isinstance(x,CRef1):
            y = valeur_ref(mem,x.cst_adr)
            if y is None: return Erreur1()
            return VCste1(y)
        return Erreur1()
    raise ValueError
```

```ocaml
let rec eval_expl env mem e = match e with
| Cste1 n -> VCste1 (CInt1 n)
| Var1 x ->
(match valeur_de env x with Some v -> VCste1 v | _ -> Erreur1)
| Plus1 (e1, e2) -> (
match ((eval_expl env mem e1), (eval_expl env mem e2)) with
| (VCste1 (CInt1 n1), VCste1 (CInt1 n2)) -> VCste1 (CInt1 (n1 + n2))
| _ -> Erreur1)
| _ -> Erreur1)
| Bang1 x -> (match valeur_de env x with
| Some (CRef1 a) ->
(match valeur_ref mem a with Some v -> VCste1 v | _ -> Erreur1)
| _ -> Erreur1)
val eval_expl : ('a * 'b const1) list -> ('b * 'b const1) list -> 'a expl
-> 'b valeurs1
```

考虑示例 2.2，我们得到：

```python
ex_env1 = [("x",CRef1("rx")),("y",CInt1(2))]
ex_mem1 = [("rx",CInt1(3))]
>>> (eval_expl(ex_env1,ex_mem1,ex_exp1)).cste.cst_int
5
```

```ocaml
let ex_env1 = [ ("x", CRef1 ("rx")); ("y", CInt1 (2)) ]
val ex_env1 : (string * string const1) list
let ex_mem1 = [ ("rx", CInt1 (3)) ]
val ex_mem1 : (string * 'a const1) list
# (eval_expl ex_env1 ex_mem1 ex_exp1) ;;
- : string valeurs1 = VCste1 (CInt1 5)
```

### 2.3. 定义和赋值

#### 2.3.1. 定义标识符

语言 Def1 通过添加标识符定义来扩展 Exp1。有两种结构使得可以引入一个命名可变或非可变变量（如 2.1.3 节中所定义）的标识符。请注意，在两种情况下，都必须提供初始值。该值对应于一个常量或由表达式 \( e \in \mathbf{Exp}_1 \) 指定的计算结果。这些结构修改系统的当前状态；在计算 \( [[e]]^{\text{Mem}}_{\text{Env}} \) 之后，求值 \(\text{let } x = e;\) 的下一步是将绑定 \( (x, [[e]]^{\text{Mem}}_{\text{Env}}) \) 添加到环境中，而求值 \(\text{var } x = e;\) 则将绑定 \( (x, r_x) \) 添加到环境中，并将值 \( [[e]]^{\text{Mem}}_{\text{Env}} \) 写入引用 \( r_x \)。在这种情况下，我们假设引用 \( r_x \) 所表示的位置由负责内存分配的外部机制计算。

| d ::= | let x = e; | 非可变变量的定义 | (x ∈ X, e ∈ Exp_1) |
| --- | --- | --- | --- |
| | var x = e; | 可变变量的定义 | (x ∈ X, e ∈ Exp_1) |

表 2.3. 定义语言 Def1

定义的求值表达如下：

(\text{Env}, \text{Mem}) \xrightarrow{\text{let } x = e;}_{\text{Def}_1} ((x, [[e]]^{\text{Mem}}_{\text{Env}}) \oplus \text{Env}, \text{Mem}) \quad \text{[2.1]}
(\text{Env}, \text{Mem}) \xrightarrow{\text{var } x = e;}_{\text{Def}_1} ((x, r_x) \oplus \text{Env}, \text{Mem}[r_x := [[e]]^{\text{Mem}}_{\text{Env}}])

这种求值 \(\rightarrow_{\text{Def}_1}\) 定义了一个状态、一个定义和一个结果状态之间的关系，或者用正式术语来说：

\(\rightarrow_{\text{Def}_1} \subseteq (\mathbf{E} \times \mathbf{M}) \times \text{Def}_1 \times (\mathbf{E} \times \mathbf{M})\)

从有限定义序列 \(\mathbf{d} = [d_1; \dots ; d_n]\) 和初始状态 \((\text{Env}_0, \text{Mem}_0)\) 开始，该关系产生状态 \((\text{Env}_n, \text{Mem}_n)\)：

\((\text{Env}_0, \text{Mem}_0) \xrightarrow{d_1}_{\text{Def}_1} (\text{Env}_1, \text{Mem}_1) \xrightarrow{d_2}_{\text{Def}_1} \cdots \xrightarrow{d_n}_{\text{Def}_1} (\text{Env}_n, \text{Mem}_n)\)


这个转换序列可以更简单地表示为 $(Env_0, Mem_0) \xrightarrow{d}_{\text{Def}_1}^{*} (Env_n, Mem_n)$。

**示例 2.3.–** 从一个没有可访问引用的内存和一个“空”环境开始，序列 `[var y = 2; let x = !y + 3;]` 构建了以下状态：

([], []) \(\xrightarrow{var~y=2;} \text{Def}_1\) (((y, r_y)), [(r_y, 2)])

(((y, r_y)), [(r_y, 2)]) \(\xrightarrow{let~x=!y+3;} \text{Def}_1\) (((x, 5), (y, r_y)), [(r_y, 2)])

在环境 $ENV = [(x, 5), (y, r_y)]$ 中，我们得到 $X_{Env}^{ref} = \{y\}$ 和 $X_{Env}^{cst} = \{x\}$。

**注.–** 在 [2.1] 中定义的两个转换时，我们假设表达式 $e$ 的求值结果（记作 $[[e]]_{Env}^{Mem}$）不是错误结果。在出错的情况下，不会产生任何状态并且求值停止。

语言 **Def$_1$** 的抽象语法可以定义如下：

```python
class Let_def1:
    def __init__(self, var, exp):
        self.var = var
        self.exp = exp

class Var_def1:
    def __init__(self, var, exp):
        self.var = var
        self.exp = exp
```

```ocaml
type 'a def1 = Let_def1 of 'a * 'a exp1 | Var_def1 of 'a * 'a exp1
```

我们选择使用应用于标识符的构造函数来构造与引用对应的值。

```python
class Ref_Var1:
    def __init__(self, idvar):
        self.idvar = idvar
```

```ocaml
type 'a refer = Ref_Var1 of 'a
```

因此，$r_x$ 将由 Ref_Var1("x") 表示。由于关系 $\to_{Def_1}$ 定义了一个函数，它可以直接如下实现：

```python
def trans_def1(st, d):
    (env, mem) = st
    if isinstance(d, Let_def1):
        v = eval_exp1(env, mem, d.exp)
        if isinstance(v, VCste1):
            return (ajout_liason_env(env, d.var, v.cste), mem)
        raise ValueError
    if isinstance(d, Var_def1):
        v = eval_exp1(env, mem, d.exp)
        if isinstance(v, VCste1):
            r = Ref_Var1(d.var)
            return (ajout_liason_env(env, d.var, CRef1(r)),
                    write_mem(mem, r, v.cste))
        raise ValueError
    raise ValueError
```

```ocaml
let trans_def1 (env, mem) d = match d with
| Let_def1 (x, e) -> (match eval_exp1 env mem e with
    | VCste1 v -> (ajout_liason_env env x v), mem
    | Erreur1 -> failwith "Erreur")
| Var_def1 (x, e) -> (match eval_exp1 env mem e with
    | VCste1 v -> (ajout_liason_env env x (CRef1 (Ref_Var1 x))),
            (write_mem mem (Ref_Var1 x) v))
    | Erreur1 -> failwith "Erreur")
val trans_def1 :
('a * 'a refer const1) list * ('a refer * 'a refer const1) list
-> 'a def1
-> ('a * 'a refer const1) list * ('a refer * 'a refer const1) list
```

通过迭代此函数，我们得到了 →* Def₁ 的实现。

```python
def trans_def1_exec(st, ld):
    (env, mem) = st
    if len(ld) == 0: return (env, mem)
    else: return trans_def1_exec(trans_def1((env, mem), ld[0]), ld[1:])
```

```ocaml
let trans_def1_exec (env, mem) ld = (List.fold_left trans_def1 (env, mem) ld)
val trans_def1_exec :
('a * 'a refer const1) list * ('a refer * 'a refer const1) list
-> 'a def1 list
-> ('a * 'a refer const1) list * ('a refer * 'a refer const1) list
```

现在，考虑示例 2.3，我们得到：

```python
ex__ld0 = [Var_def1("y", Cste1(2)), Let_def1("x", Plus1(Bang1("y"), Cste1(3)))]

(ex_e0, ex_m0) = trans_def1_exec(([], []), ex__ld0)
>>> eval_exp1(ex_e0, ex_m0, Var1("x")).cste.cst_int
5
>>> eval_exp1(ex_e0, ex_m0, Bang1("y")).cste.cst_int
2
```

```ocaml
let ex__ld0 = [ Var_def1 ("y", Cste1 2);
              Let_def1 ("x", Plus1 (Bang1 "y", Cste1 3)) ]
val ex__ld0 : string def_list
# (trans_def1_exec ( [], []) ex__ld0 ) ;;
- : (string * string refer const1) list *
    (string refer * string refer const1) list
= ([("x", CInt1 5); ("y", CRef1 (Ref_Var1 "y"))], [(Ref_Var1 "y", CInt1 2)])
```

#### 2.3.2. 赋值

语言 **Lang**$_1$ 通过添加 *赋值* 扩展了 **Def**$_1$。赋值指令的语法是：

$x := e$

其中 \(x \in \mathbf{X}\) 且 \(e \in \mathbf{Exp}_1\)。当可变变量 \(x\) 已在当前环境中绑定时，此指令允许我们修改 !\(x\) 的值。形式上，指令 \(x := e\) 的执行修改当前状态的内存，并由以下转换描述：

\((\text{Env}, \text{Mem}) \xrightarrow{x:=e}_{\mathbf{Lang}_1} (\text{Env}, \text{Mem}[\text{Env}(x) := \llbracket e \rrbracket_{\text{Env}}^{\text{Mem}}])\)

**注.–** 再次说明，如果标识符 \(x\) 在环境中未绑定或表达式 \(e\) 的求值结果为错误，则不会生成任何状态并且求值停止。

**示例 2.4.–** 基于示例 2.3 中获得的状态，可以执行以下两个赋值：

([(x, 5), (y, ry)], [(ry, 2)])
\(\xrightarrow{y:=y+x}_{\mathbf{Lang}_1} ([(x, 5), (y, ry)], [(ry, 7)])\)
\(\xrightarrow{y:=8}_{\mathbf{Lang}_1} ([(x, 5), (y, ry)], [(ry, 8)])\)

将赋值 \(x := e\) 的抽象语法表示为对 \((x, e)\)，关系 \(\rightarrow_{\mathbf{Lang}_1}\) 及其从一系列赋值中迭代的实现如下：

```python
def trans_lang1(st, a):
    (env, mem) = st
    (x, e) = a
    v = valeur_de(env, x)
    if isinstance(v, CRef1):
        ve = eval_expi(env, mem, e)
        if isinstance(ve, VCste1):
            return (env, write_mem(mem, v.idvar, ve.cste))
    raise ValueError

def trans_lang1_exec(st, la):
    (env, mem) = st
    if len(la) == 0: return (env, mem)
    else: return trans_lang1_exec(trans_lang1((env, mem), la[0]), la[1:])
```

```ocaml
let trans_lang1 (env, mem) (x, e) = match valeur_de env x with
  | Some (CRef1 (Ref_Var1 y)) -> (match eval_expi env mem e with
    | VCste1 v -> (env, (write_mem mem (Ref_Var1 y) v))
    | Erreur1 -> failwith "Eval error")
  | _ -> failwith "Undefined var"
val trans_lang1 :
  ('a * 'b refer const1) list * ('b refer * 'b refer const1) list
  -> 'a * 'a expi
  -> ('a * 'b refer const1) list * ('b refer * 'b refer const1) list
let trans_lang1_exec (env, mem) la =
  (List.fold_left trans_lang1 (env, mem) la)
val trans_lang1_exec :
  ('a * 'b refer const1) list * ('b refer * 'b refer const1) list
  -> ('a * 'a expi) list
  -> ('a * 'b refer const1) list * ('b refer * 'b refer const1) list
```

考虑示例 2.4，我们得到：

```python
ex_la0 = [("y", Plus1(Bang1("y"), Var1("x"))), ("y", Cste1(8))]
(ex_e1, ex_m1) = trans_lang1_exec((ex_e0, ex_m0), ex_la0)
>>> eval_expi(ex_e1, ex_m1, Bang1("y")).cste.cst_int
8
```

```ocaml
let ex_la0 = [ ("y", Plus1 (Bang1 "y", Var1 "x")); ("y", Cste1 8) ]
val ex_la0 : (string * string expi) list
# (trans_lang1_exec (trans_def1_exec ([], []) ex__ld0) ex_la0) ;;
- : (string * string refer const1) list *
  (string refer * string refer const1) list
= ([("x", CInt1 5); ("y", CRef1 (Ref_Var1 "y"))], [(Ref_Var1 "y", CInt1 8)])
```

**注.–** 在上述介绍中，赋值仅涉及可变变量。同样，解引用运算符 ! 只能应用于引用以获取该位置存储的值。因此，从给定引用向可变变量赋值在这里需要使用专用的语法结构：x := ! x + 1。然而，在许多语言中，赋值不明确提及解引用运算符，变量在赋值符号两边的语法使用是相同的：x = x + 1。因此，标识符 x 表示两个不同的概念：赋值符号左边的值 Env(x)，以及赋值符号右边的值 Mem(Env(x))。

这些语言根据变量在赋值中的位置掩盖了变量的不同角色。当变量 x 位于赋值符号左侧时，它被称为左值（l-value），表示应存储要分配的值的位置。通过这种方式，它充当指针。赋值符号右侧的变量 x 称为右值（r-value），并隐式充当解引用指针：要获取的值位于变量所表示的位置。因此，在表达式 x = x + 1 中，即使 x 从语法角度以相同方式使用，右侧的实例隐式表示 !x。在某些语言中，变量声明隐式涉及创建引用，除非另有说明；变量名表示编译器将存储分配给它的值的位置。

### 2.4. 练习

### 练习 2.1

考虑由以下定义的状态 etat0：

Env₀ = [(x, 3), (y, r_y), (x, 8), (z, r_z)] 和 Mem₀ = [(r_z, 2), (r_y, 5)]

- 1) 计算 $X^{ref}_{\text{Env}_0}$ 和 $X^{cst}_{\text{Env}_0}$。
- 2) 给出一组定义以从空状态 ([], []) 获得状态 etat0。是否有其他序列可用于获得此状态？
- 3) 计算 $[ [ (!y + !z) + x ] ]_{\text{Env}_0}^{\text{Mem}_0}$。
- 4) 给出三个表达式示例，它们在状态 etat0 中求值时会产生三种不同的错误类型。




- 5) 考虑如下序列

etat_0 --(y:=x)-->_{Lang_1} etat_1 --(let y = x;)-->_{Def_1} etat_2 。

确定状态 etat_1 = (Env_1, Mem_1) 和 etat_2 = (Env_2, Mem_2) 以及集合 X^{ref}_{Env_1}、X^{cst}_{Env_1}、X^{ref}_{Env_2} 和 X^{cst}_{Env_2} 。

- 6) 考虑如下序列

etat_0 --(let x = y;)-->_{Def_1} etat_3 --(x:=4)-->_{Lang_1} etat_4 。

确定状态 etat_3 = (Env_3, Mem_3) 和 etat_4 = (Env_4, Mem_4)。是否有 [[!y]^{Mem_0}_{Env_0}] = [[!y]^{Mem_4}_{Env_4}]？

## 练习 2.2

两种结构 x++ 和 ++x：

e = k | x | e_1 + e_2 | !x | x++ | ++x

被添加到语言 Exp_1 中。在这两个新结构中，标识符 x 必须是可变变量。直观地，我们可以看出表达式 x++ 的求值产生 x 所表示位置中存储的值，并将内存中的值加 1。表达式 ++x 的求值方式不同：x 所表示位置中存储的值加 1，这个新值即为求值结果。

- 1) 定义求值函数：

[[ _ ]]_- : E x M x Exp_1 -> V x (E x M)

用于语言 Exp_1 中的表达式，扩展方式使得 [[e]^{Mem}_{Env} = (v, (Env', Mem')) 表示在状态 (Env, Mem) 中对表达式 e 求值会将该状态转换为状态 (Env', Mem') 并产生值 v。

- 2) 设 Env = [(x, r_x)] 和 Mem = [(r_x, 6)]。计算：

[[!x + (x + 1)]]^{Mem}_{Env} 和 [[(x + 1) + !x]^{Mem}_{Env}

是否有 [[x + (x + 1)]]^{Mem}_{Env} = [[(x++) + !x]^{Mem}_{Env}？

- 3) 证明，对于任意状态 (Env, Mem)：

[[++x]^{Mem}_{Env} = [[(x++) + 1]^{Mem}_{Env}

现在，我们通过考虑扩展 Def1 语言的表达式并添加结构 x +:= e 来扩展语言 Lang1。从非正式的角度，在状态 (Env, Mem) 中执行这条指令包括：首先在 Mem 中找到引用 v 处存储的值 vx，然后在此状态下对表达式 e 求值以获得其值 ve 和新状态 (Env', Mem')，最后，将 ve 和 vx 相加的结果赋给 v。如果 ve 和 vx 中至少有一个不是整数，则执行失败。

- 4) 为扩展的 Lang1 语言重新定义关系 →Lang1。

- 5) 设 ENV0 = [(x, rx)] 和 Mem0 = [(rx, 2)]。确定状态 etat1，使得 (ENV0, Mem0) --x +:= +x-->_{Lang1} etat1。

- 6) 赋值 x := x + +、x := + + x 和 x + := 1 在相同状态下执行时是否产生相同的状态？
