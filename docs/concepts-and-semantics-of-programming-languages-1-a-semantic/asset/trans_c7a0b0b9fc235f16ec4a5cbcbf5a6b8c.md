

泛型化函数分三个阶段进行。首先，获取待泛型化类型中存在的所有变量。然后，计算环境中的自由变量集合。最后一步计算类型中存在的变量列表，排除环境中找到的变量。在 OCaml 中，此过滤操作使用标准库中的 `List.filter` 函数执行，该函数仅保留列表中满足给定谓词的元素。在 Python 中，通过使用 `lambda` 构造定义此谓词来执行过滤。

相反，若要为标识符表达式进行类型推导，需要从环境中绑定到该标识符的类型方案生成一个类型。这由 `instantiation` 操作完成，该操作通过“特化”方案返回一个方案的 `instance`。一个实例的正式定义使用 `substitution` 的概念，如下所示。

**定义 5.5. 替换：** 替换是从类型变量集合到类型代数的偏函数，具有有限定义域。
\([\alpha_1 \leftarrow \tau_1; \dots; \alpha_n \leftarrow \tau_n]\) 表示替换 \(\theta\)。\(\theta\) 的定义域，记作 \(\text{dom}(\theta)\)，是集合 \(\{\alpha_1, \dots, \alpha_n\}\)。\(\theta\) 的值域，记作 \(\text{codom}(\theta)\)，是集合 \(\{\tau_1, \dots, \tau_n\}\)。\(\theta\) 将类型 \(\tau_i\) 关联到变量 \(\alpha_i\)。

_为简化起见，在以下内容中，我们假设_ \(\text{dom}(\theta)\) _中的变量互不相同，并且：_

\(\text{dom}(\theta) \cap \bigcup_{\tau \in \text{codom}(\theta)} \mathcal{F}(\tau) = \emptyset\)

替换自然扩展为从类型到类型的偏函数，仍具有有限定义域，方式如下：

\(\overline{\theta}(\text{int}) = \text{int} \quad \overline{\theta}(\text{bool}) = \text{bool}\)

\(\overline{\theta}(\alpha) = \begin{cases} \theta(\alpha) & \text{if } \alpha \in \text{dom}(\theta) \\ \alpha & \text{else} \end{cases}\)

\(\overline{\theta}(\tau_1 \to \tau_2) = \overline{\theta}(\tau_1) \to \overline{\theta}(\tau_2)\)

从今以后，我们将把替换 \(\theta\) 及其扩展 \(\overline{\theta}\) 视为同一对象，两者均记作 \(\theta\)。定义替换所用的假设保证，对于任意类型 \(\tau\)，\(\overline{\theta}(\overline{\theta}(\tau)) = \overline{\theta}(\tau)\)（在这种情况下，\(\theta\) 被称为 _幂等的_）。

```ocaml
type subst_t = (var_name_t * type_t) list
let empty_subst = ([]) : subst_t
val empty_subst : subst_t
let singleton var_name ty = ([(var_name, ty)] : subst_t)
val singleton : var_name_t -> type_t -> subst_t
```

```python
empty_subst = []
def singleton(var_name, ty):
    return [(var_name, ty)]
```

替换由关联列表表示，即一对对的列表，其中第一个组件表示类型变量的名称，第二个组件表示用于替换该变量的类型。如果关联列表只包含一个元素，则该替换称为初等替换。

```ocaml
let rec apply_one transf = function
| T_fun (t1, t2) -> T_fun (apply_one transf t1, apply_one transf t2)
| T_var var_name ->
  if (fst transf) = var_name then snd transf else T_var var_name
| any -> any
val apply_one : (var_name_t * type_t) -> type_t -> type_t
```

```python
def apply_one(transf, ty):
    if isinstance(ty, T_int) or isinstance(ty, T_bool):
        return ty
    elif isinstance(ty, T_fun):
        return T_fun(apply_one(transf, ty.t1), apply_one(transf, ty.t2))
    elif isinstance(ty, T_var):
        (fst_transf, snd_transf) = transf
        if fst_transf == ty.vname:
            return snd_transf
        else:
            return ty
    else:
        raise ValueError
```

函数 apply_one 的第一个参数是编码初等替换的列表，第二个参数是一个类型。该函数返回该类型的更新，其中初等替换涉及的变量被其关联的类型所替换。

```ocaml
let rec apply ty = function
| [] -> ty
| h :: q -> apply (apply_one h ty) q
val apply : type_t -> (var_name_t * type_t) list -> type_t
```

```python
def apply(ty, subst):
    ty_res = ty
    for transf in subst:
        ty_res = apply_one(transf, ty_res)
    return ty_res
```

apply 函数将替换应用于类型。换句话说，该类型的更新是通过迭代执行表示替换的列表中包含的初等替换来完成的。这种做法之所以正确，是基于定义 5.5 中确立的假设。

替换的概念用于定义类型方案的实例。

**定义 5.6.** 实例：如果存在替换 \(\theta\)，使得 \(\text{dom}(\theta) = \{\alpha_1, \dots, \alpha_k\}\) 且 \(\tau' = \theta(\tau)\)，则称 \(\tau'\) 是类型方案 \(\sigma = \forall \alpha_1 \dots \alpha_k . \tau\) 的一个实例，记作 \(\sigma \le \tau'\)。在这种情况下，类型 \(\tau'\) 与 \(\tau\) 兼容。

### 5.3.3. 类型规则与类型树

我们现在可以给出用于确保表达式具有良好类型的规则。这些规则在 Box 5.1 中给出，采用了 3.3 节中已用于描述 **Exp**<sub>2</sub> 的大步运行语义的符号。我们给出关系 \(\text{Env} \vdash e : \tau\) 的正式定义，这意味着在类型环境 Env 中，表达式 \(e\) 的类型为 \(\tau\)。

\((T_\text{true}) \frac{}{\text{Env} \vdash true : \text{bool}} \quad (T_\text{false}) \frac{}{\text{Env} \vdash false : \text{bool}} \quad (T_\text{int}) \frac{}{\text{Env} \vdash i : \text{int}}\)
\((T_\text{var}) \frac{\text{Env}(x) \le \tau}{\text{Env} \vdash x : \tau} \quad (T_\text{unop}) \frac{\text{Env} \vdash \text{op} : \tau_1 \to \tau_2 \quad \text{Env} \vdash e : \tau_1}{\text{Env} \vdash \text{op } e : \tau_2}\)
\((T_\text{binop}) \frac{\text{Env} \vdash \text{op} : \tau_1 \to \tau_2 \to \tau_3 \quad \text{Env} \vdash e_1 : \tau_1 \quad \text{Env} \vdash e_2 : \tau_2}{\text{Env} \vdash e_1 \text{ op } e_2 : \tau_3}\)
\((T_\text{ite}) \frac{\text{Env} \vdash e_1 : \text{bool} \quad \text{Env} \vdash e_2 : \tau \quad \text{Env} \vdash e_3 : \tau}{\text{Env} \vdash \text{if } e_1 \text{ then } e_2 \text{ else } e_3 : \tau}\)
\((T_\text{fun}) \frac{(x, \forall \emptyset . \tau_1) \oplus \text{Env} \vdash e : \tau_2}{\text{Env} \vdash \text{fun } x \mapsto e : \tau_1 \to \tau_2}\)
\((T_\text{let}) \frac{\text{Env} \vdash e_1 : \tau_1 \quad \sigma = \text{Gen}(\tau_1, \text{Env}) \quad (x, \sigma) \oplus \text{Env} \vdash e_2 : \tau_2}{\text{Env} \vdash \text{let } x = e_1 \text{ in } e_2 : \tau_2}\)
\((T_\text{rec}) \frac{(f, \forall \emptyset . \tau_1 \to \tau_2) \oplus (x, \forall \emptyset . \tau_1) \oplus \text{Env} \vdash e : \tau_2}{\text{Env} \vdash \text{rec } f \, x = e : \tau_1 \to \tau_2}\)
\((T_\text{app}) \frac{\text{Env} \vdash e_1 : \tau_2 \to \tau_1 \quad \text{Env} \vdash e_2 : \tau_2}{\text{Env} \vdash (e_1 \, e_2) : \tau_1}\)

**Box 5.1.** _**Exp**<sub>2</sub> 中的类型规则_

规则 \(T_\text{true}\)、\(T_\text{false}\) 和 \(T_\text{int}\) 表示布尔常量的类型为 \(\text{bool}\)，整数常量的类型为 \(\text{int}\)。根据规则 \(T_\text{var}\)，标识符的类型是其在类型环境中绑定的方案的一个实例，且 \(\text{Env}(x)\) 表示在 Env 中绑定到 \(x\) 的类型方案。规则 \(T_\text{unop}\) 指出，一元运算符作用于参数的表达式的类型是该运算符返回的类型，前提是参数的类型是该运算符要求的类型。类似地，规则 \(T_{binop}\) 指出，二元运算符作用于两个参数的表达式的类型是该运算符返回的类型，前提是两个参数的类型都是该运算符要求的类型。规则 \(T_{ite}\) 指出，条件表达式的类型是其 then 和 else 分支所构成的两个子表达式共享的类型，前提是条件子表达式的类型为 bool。

根据规则 \(T_{fun}\)，如果当变量 \(x\) 的类型被取为 \(\tau_1\) 时表达式 \(e\) 的类型为 \(\tau_2\)，那么一个主体为表达式 \(e\) 的变量 \(x\) 的函数的类型为 \(\tau_1 \to \tau_2\)。请注意，绑定到参数 \(x\) 的方案是平凡的：\(\tau_1\) 中可能出现的类型变量不会被泛型化。如果这些变量被泛型化，则可能对不正确的程序进行类型推导。例如，考虑表达式 fun \(x \mapsto\) if \(x\) then \(x + 1\) else \(x + 2\)。对该表达式进行类型推导必须失败，因为 \(x\) 既被用作布尔值（规则 \(T_{ite}\)），又被用作整数（规则 \(T_{binop}\)）。如果将 \(x\) 的类型泛型化为一个用于类型检查函数主体的方案，那么 \(x\) 的每个出现都将使用方案的一个新实例进行类型推导：第一个实例与布尔值兼容，而第二个实例与整数兼容。因此，类型 \(\tau_1\) 不会被泛型化，这意味着函数主体中 \(x\) 的所有出现都必须使用同一个实例。

规则 \(T_{let}\) 指出，表达式 let \(x = e_1\) in \(e_2\) 的类型是子表达式 \(e_2\) 的类型，假设 \(x\) 绑定到泛型化 \(e_1\) 类型的方案。根据规则 \(T_{rec}\)，一个主体为表达式 \(e\) 的变量 \(x\) 的递归函数 \(f\) 的类型为 \(\tau_1 \to \tau_2\)，其中 \(\tau_2\) 是 \(e\) 在以下假设下的类型：\(x\) 的类型为 \(\tau_1\)，以及 \(f\) 的类型方案与 \(\tau_1 \to \tau_2\) 兼容。最后，规则 \(T_{app}\) 指出，要将表达式 \(e_1\) 应用于参数 \(e_2\)，\(e_1\) 必须是函数类型 \(\tau_1 \to \tau_2\)，且 \(e_2\) 必须是类型 \(\tau_1\)。整体表达式的类型是函数结果的类型，即 \(\tau_2\)。

请注意，规则 \(T_{rec}\) 将递归定义的函数与一个平凡的类型方案之间的绑定引入环境。因此，表达式

```
rec f x = let y = (f 7) in (f true)
```

会被类型检查过程拒绝。确实，\(f\) 在该表达式中既被用作类型 int \(\to \alpha\)，也被用作类型 bool \(\to \alpha\)。

请注意，将 \(f\) 的类型泛型化，即给 \(f\) 赋予类型 \(\beta \to \alpha\)，在表达式求值所用的执行语义方面仍然是正确的。\(f\) 的方案必须限制为一个平凡方案，以保证类型推导的可判定性（即设计一种保证终止的类型计算算法的可能性）。

