

## 表 5.1. Exp$_2$ 的类型代数

| $\tau := \alpha$ | 变量 |
| :--- | :--- |
| int | 整数 |
| bool | 布尔值 |
| $\tau_1 \to \tau_2$ | 函数 |

这种记法反映了这样一个事实：带有 \(n\) 个参数的函数 \(f : \alpha_1 \times \alpha_2 \times \cdots \times \alpha_n \rightarrow \alpha_{n+1}\) 总可以定义为单参数函数，该函数返回一个带有 \(n - 1\) 个参数的函数。

```ocaml
type var_name_t = string
type type_t = T_var of var_name_t | T_int | T_bool | T_fun of (type_t * type_t)

let new_ty_var =
  let cpt = ref 0 in
  function () -> cpt := !cpt + 1 ; T_var ("t" ^ (string_of_int !cpt))
val new_ty_var : unit -> type_t
```

类型代数在 OCaml 中使用和类型实现。变量通过名称（字符串）来区分。我们还定义了一个函数 new_ty_var，用于创建一个新的类型变量，其名称与所有已创建的变量不同。这种名称上的唯一性是通过一个局部于该函数的计数器实现的；该计数器在每次创建变量时递增，其值用作变量的名称。与之前的实现一样，Python 中使用类来对 OCaml 的和类型进行编码。

```python
class T_int:
    def __init__(self):
        pass

class T_bool:
    def __init__(self):
        pass

class T_fun:
    def __init__(self, t1, t2):
        self.t1 = t1
        self.t2 = t2

class T_var:
    def __init__(self, vname):
        self.vname = vname

var_cpt = 0


def new_ty_var():
    global var_cpt
    var_cpt = var_cpt + 1
    return T_var("t" + str(var_cpt))
```

每个类都定义为表示类型代数的一个构造子。虽然 OCaml 允许创建局部于函数的持久引用，但在 Python 中，用于为每个新变量分配唯一名称的计数器必须定义为全局变量。显然，只有函数 new_ty_var 会使用这个计数器。例如，第一次应用此函数将返回字符串 "t0"，第二次将返回 "t1"，以此类推。

与使用自然语言给出类型规则不同，我们引入了类型关系 Env ⊢ e : τ 的数学定义，它表示在类型环境 Env 中，表达式 e 具有类型 τ。对表达式进行类型化时，我们需要一个类型环境，该环境将每个标识符与一个类型方案关联，正如我们在 5.2.4 节中看到的，一个表达式可能从同一个类型方案中导出多种类型。在这种情况下，每种类型都源自一个“骨架”类型方案：这就是多态的定义。

### 定义 5.1. 类型方案：类型方案的语法由以下文法定义：σ := ∀α1 ... αn.τ，其中 τ 是方案的主体，α1, ..., αn 是泛化变量。

在方案中普遍量化变量 αi 出现的顺序并不重要。假设它们的名称都不同。例如，∀α1 α2. α1 → α2 和 ∀α2 α1. α1 → α2 表示相同的方案。此外，仅因量化变量名称不同而不同的两个类型方案被视为相同（这些方案也可以说是模 α-变换等价）。例如，∀α1 α2. α1 → α2 和 ∀α3 α4. α3 → α4 表示相同的方案。

类型环境由一组绑定列表表示：

Env = [(x1, σ1), · · · , (xn, σn)]

其中每个标识符 xi 都绑定到其类型方案 σi。

```ocaml
type scheme_t = (var_name_t list * type_t)
```

类型方案在 OCaml 中编码为一对，第一个组件是泛化变量的列表，第二个组件是表示方案主体的类型。Python 不允许类型定义，因此无法给出该语言的编码。然而，我们在 Python 和 OCaml 中使用相同的数据结构（即第一个组件是列表的一对）。类型环境的表示与执行环境中使用的表示类似。绑定到标识符的值简单地被类型方案替换。

```ocaml
type ty_env_t = (string * scheme_t) list
exception Unbound_var of string

let rec find_id_ty idname = function
  | [] -> raise (Unbound_var idname)
  | h :: q -> if fst h = idname then snd h else find_id_ty idname q
val find_id_ty : string -> (string * 'a) list -> 'a
```

```python
class Unbound_var(Exception):
    def __init__(self, id):
        self.id = id

def find_id_ty(idname, env):
    for (n, ty) in env:
        if n == idname: return ty
    raise Unbound_var(idname)
```

类型环境定义为标识符与其类型方案之间的关联列表。函数 `find_id_ty` 在其第二个参数（编码当前环境的列表）中搜索其第一个参数（标识符）的第一个出现位置。它从列表的开头开始，如果在到达列表末尾时未找到该参数，则引发特定错误。这种表示与执行环境中使用的表示类似；绑定到标识符的值简单地被类型方案替换。

## 5.3.2. 泛化、替换和实例化

要构建环境，标识符必须绑定到类型方案，因此类型必须转换为方案。此转换需要两个步骤。然而，在少数不应进行泛化的情况下，类型会转换为所谓的平凡类型方案。

### 定义 5.2. 平凡类型方案：平凡方案是形式为 ∀0.τ 的方案。

```ocaml
let trivial_scheme ty = (([]), ty) : scheme_t
val trivial_scheme : type_t -> scheme_t
```

如果没有在 `trivial_scheme` 上显式添加类型注解 `scheme_t`，OCaml 将会将“更一般”的类型 `’a -> (’b list * ’a)` 与 `trivial_scheme` 关联。该注解会导致更精确的类型，这一点有助于开发人员。

```python
def trivial_scheme(ty):
    return ([], ty)
```

相反，对类型表达式中出现的变量进行泛化需要一个类型泛化操作，该操作返回一个多态类型方案。然而，只有某些变量才应被泛化：

- 在环境中任何位置都未出现的变量；
- 在环境中存在的类型方案中被泛化的变量；这些变量可以重命名而不改变方案的指称，称为绑定变量。

换句话说，环境中存在但未被 ∀ 绑定的变量不会被泛化：这些变量称为所涉环境的自由变量。自由变量表示一种尚未知晓的类型，环境中该变量的所有自由出现都表示同一类型。

### 定义 5.3. 类型方案的自由变量：类型方案 σ 的自由变量集合 F(σ) 包含 σ 的类型变量，即那些未被泛化的变量。该集合定义为：

F(∀α1 ... αn . τ) = F(τ) \ {α1, ..., αn}

其中 F(τ) 定义为：

- F(int) = F(bool) = ∅
- F(α) = {α}
- F(τ1 → τ2) = F(τ1) ∪ F(τ2)

此定义可扩展到类型环境：

F(Env) = ⋃ (x,σ)∈Env F(σ)

### 定义 5.4. 泛化：环境 Env 中类型 τ 的泛化是定义的类型方案：

Gen(τ, Env) = ∀α1 ... αn. τ，其中 {α1 ... αn} = F(τ) \ F(Env)

### 示例 5.1. 给定环境：

Env = [(x, ∀α1. α1 → α1), (y, ∀α1. α1 → α2), (z, ∀∅. α3), (w, ∀∅. int)]

我们有 F(Env) = {α2, α3}，并且：

Gen((α1 → α2) → (α3 → α4), Env) = ∀α1 α4. (α1 → α2) → (α3 → α4)

```ocaml
let find_vars_in_type in_ty =
  let rec rec_find accu = function
    | T_int | T_bool -> accu
    | T_fun (t1, t2) ->
        let new_accu = rec_find accu t1 in rec_find new_accu t2
    | T_var v_name ->
        if not (List.mem v_name accu) then v_name :: accu else accu
  in
  rec_find [] in_ty
val find_vars_in_type : type_t -> var_name_t list
```

```python
def find_vars_in_type(in_ty):
    def rec_find(accu, ty):
        if isinstance(ty, T_int) or isinstance(ty, T_bool):
            return []
        elif isinstance(ty, T_fun):
            return find_vars_in_type(ty.t1) + find_vars_in_type(ty.t2)
        elif isinstance(ty, T_var):
            if ty.vname in accu: return accu
            else: return [ty.vname] + accu
        else: raise ValueError
    return rec_find([], in_ty)
```

用于在类型中查找自由变量的函数在类型结构上递归操作。在类型变量的情况下，我们使用标准库中的 List.mem 函数来验证元素是否属于列表；这防止将变量多次插入到累加器中。

```ocaml
let rec find_vars_in_env = function
  | [] -> []
  | (_, (gen_vars, body)) :: rem ->
      let vars_of_body = find_vars_in_type body in
      let sch_free_vars =
        List.filter (fun var -> not (List.mem var gen_vars)) vars_of_body in
      sch_free_vars @ (find_vars_in_env rem)
val find_vars_in_env :
  ('a * (var_name_t list * type_t)) list -> var_name_t list
```

```python
def find_vars_in_env(env):
    accu = []
    for (_, (gen_vars, body)) in env:
        vars_of_body = find_vars_in_type(body)
        sch_free_vars = \
            list(filter(lambda var: not (var in gen_vars), vars_of_body))
        accu = accu + sch_free_vars
    return accu
```

```ocaml
let generalize env ty =
    let vars_of_ty = find_vars_in_type ty in
    let vars_of_env = find_vars_in_env env in
    let gen_vars =
      List.filter (fun var -> not (List.mem var vars_of_env)) vars_of_ty in
    (gen_vars, ty)
val generalize :
  ('a * ('b * type_t)) list -> type_t -> (var_name_t list * type_t)
```

```python
def generalize(env, ty):
    vars_of_ty = find_vars_in_type(ty)
    vars_of_env = find_vars_in_env(env)
    gen_vars = list(filter(lambda var: not (var in vars_of_env), vars_of_ty))
    return (gen_vars, ty)
```

