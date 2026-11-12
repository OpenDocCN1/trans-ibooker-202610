

函数 filter 用于检查模式 p 是否与值 v 匹配，其定义是通过递归比较 p 和 v 的结构。它使用空初始替换调用函数 filter'。作为 filter' 输入的替换累积了模式与值之间比较所创建的绑定。Box 6.1 展示了这两个函数的定义，仅提供名义情况。匹配过程在所有其他情况下隐式失败。

```
x 
\notin dom(phi)
filter'(phi, x, v) = phi \oplus (x, v)           phi(x) = v
filter'(phi, x, v) = phi

phi1 = filter'(phi, p1, v1)   ...   phi_n = filter'(phi_{n-1}, p_n, v_n)
filter'(phi, C(p1, ..., pn), C(v1, ..., vn)) = phi_n

filter(p, v) = filter'(empty, p, v)
```

### Box 6.1. 匹配函数

操作语义的规则显示在 Box 6.2 中，并使用 Box 6.1 的匹配函数。规则 E_match1 对应于第一个模式 p 匹配表达式 e 的值 v 的情况，返回替换 phi。值得注意的是，替换和环境的结构是相同的，即变量和值的关联列表。因此，我们可以用 filter 返回的替换 phi 扩展环境，以求值 e1，即与第一个模式关联的表达式。规则 E_match2 对应于第一个模式与 e 的值 v 不匹配的情况，表示无法找到替换 phi。计算的结果是求值与第二个模式关联的表达式 e2，它捕获任何值。

```
(E_match1)    Env |- e ~~ v   phi = filter(p, v)   phi \oplus Env |- e1 ~~ v1
                    Env |- match e with p -> e1 | _ -> e2 ~~ v1

(E_match2)    Env |- e ~~ v   not(phi = filter(p, v))   Env |- e2 ~~ v2
                    Env |- match e with p -> e1 | _ -> e2 ~~ v2
```

### Box 6.2. 模式匹配的操作语义

## 6.5.2.1.3. 类型规则

match/with 构造的类型推导与条件表达式存在相似之处，即所有分支必须返回相同类型的值。值构造子的类型是函数类型，其参数类型是其参数类型的乘积，结果类型是 t（即使用该构造子构建的值的类型）。我们假设存在一个函数 TypeCstr，它提供构造子的类型方案。当和类型被定义时，其值构造子会在类型环境中绑定到其类型方案。

规则 T_{pvar} 和 T_{pcons} 对模式进行类型检查，并返回由模式变量绑定到其各自类型所扩展的环境。规则 T_{match} 确保模式和被检查的值具有相同的类型。它在模式变量绑定所扩展的环境中对第一个分支的主体进行类型检查，并在初始环境中对第二个分支的主体进行类型检查，验证这些主体的类型是否相同。

```
\( (T_{pvar}) \text{Env} \vdash x : \tau \Rightarrow (x, \tau) \oplus \text{Env} \
(\tau_1 * ... * \tau_n) \rightarrow \tau \le \text{TypeCstr}(C) \
\\
(T_{pcons}) \frac{\text{Env} \vdash p_1 : \tau_1 \Rightarrow \text{Env}_1 \quad ... \quad \text{Env}_{n-1} \vdash p_n : \tau_n \Rightarrow \text{Env}_n}{\text{Env} \vdash C(p_1, ..., p_n) : \tau \Rightarrow \text{Env}_n} \
\\
(T_{match}) \frac{\text{Env} \vdash e : \tau \quad \text{Env} \vdash p : \tau \Rightarrow \text{Env}' \quad \text{Env}' \vdash e_1 : \tau' \quad \text{Env} \vdash e_2 : \tau'}{\text{Env} \vdash \text{match } e \text{ with } p \rightarrow e_1 | \_ \rightarrow e_2 : \tau'}
```

### Box 6.3. 模式匹配的类型规则

## 6.5.2.2. 扩展和其他形式的匹配

### 6.5.2.2.1. 带守卫的模式

许多使用模式匹配的语言将一个称为守卫（guard）的条件与模式关联（OCaml、C# 和 F# 中的 `when` 构造，或 Scala 中的 `if`）。只有在模式匹配被检查的值且条件满足时，相关分支才会在运行时被采用，如示例 6.46 所示。

### 示例 6.46.

```ocaml
card type = Jack | Queen | King | Number of int

let card_name c =
  match c with
  | Jack -> "jack"
  | Queen -> "queen"
  | King -> "king"
  | Number 1 -> "ace"
  | Number n when n > 1 && n < 11 -> string_of_int n
  | Number _ -> "illegal card!"
```

card_name 函数单独处理 Number 1 的情况，使其能够返回更常见的字符串，即 "ace" 而不是 "1"。其他合法的数值，从 2 到 10 含两端，也单独处理。这是通过使用匹配分支 Number n 完成的，该分支仅限于该区间内的数值。为该模式添加守卫意味着它无法匹配 card 类型的所有剩余值，即使它们在结构上兼容。因此，还需要一个额外的分支来处理由守卫拒绝的值。

## 6.5.2.2.2. 模式的析取

有些情况需要对多个分支应用相同的处理。在这种情况下，与其逐个枚举模式并每次重复相同的主体，不如使用由（子）模式析取定义的单个模式，然后可以应用单个处理，如示例 6.47 所示。

### 示例 6.47.

```ocaml
let crowned_head c = match c with Queen -> true | King -> true | _ -> false
let crowned_head2 c = match c with Queen | King -> true | _ -> false
```

crowned_head 函数（用于指定纸牌上是否显示皇冠）生成了两次相同的代码：一次用于 Queen，一次用于 King。在第二个版本中，前两个模式被替换为一个同时接受 Queen 和 King 的单一模式。该模式可读作“匹配 Queen 或 King”。在每种情况都应用相同的（且冗长）处理代码的情况下，使用“或”模式特别有用。在这些情况下共享代码可减少源代码和可执行文件的大小。

鉴于析取模式对每个子模式使用相同的处理，如果其中一个子模式包含变量，则该变量也必须出现在其他子模式中，并且类型相同。此条件是必要的，以确保该模式变量能够被正确绑定，无论使用哪个特定子模式来捕获被检查的值，并且它只有一个类型。这一点在示例 6.48 中得到说明。

### 示例 6.48.

```ocaml
type expirith =
  | Plus of (expirith * expirith) | Minus of (expirith * expirith)
  | Mult of (expirith * expirith) | Div of (expirith * expirith) | Num of int

let rec count_operators e =
  match e with
  | Plus (e1, e2) | Minus (e1, e2) | Mult (e1, e2) | Div (e1, e2) ->
      1 + (count_operators e1) + (count_operators e2)
  | Num _ -> 0
```

count_operators 函数的目的是统计算术表达式中存在的运算符数量。关于 +、-、*、/ 的四种情况都需要两次递归调用，一次用于每个子表达式。相关构造子的处理可以被提取，因为它们允许在每种情况下将相同的变量 e1 和 e2 绑定到相同的类型。

## 6.5.2.2.3. 匹配记录

模式语法还包括带有命名字段的记录，如第 6.4.2 节所述。在模式中，只需命名与处理相关的字段。这避免了需要用 `_` 作为子模式包含所有字段，如示例 6.49 所示。

### 示例 6.49.

```ocaml
type transport = Bus | Metro | Tram
type line = { number : int ; kind : transport ; company : string }

let rec filter_metro l =
  match l with
  | [] -> []
  | li :: rest -> (
    match li with
    | { number = n ; kind = Metro } -> n :: (filter_metro rest)
    | _ -> filter_metro rest
    )
```

filter_metro 函数以公共交通线路列表作为输入，并返回仅包含地铁线路编号的列表。由于 company 字段与此函数无关，因此它未包含在模式中。在实践中，最终模式 `_` 具有记录类型 line，隐含地表示“该类型所有字段的任何值”。

## 6.5.2.2.4. 基于类型的匹配

有些语言能够测试执行过程中遇到的值的类型。这种能力通常与对象机制相关，我们将在第二卷第 4 章中更详细地介绍。

例如，C# 或 Scala 的类可以用于定义结构化类型的层次结构，并逐步丰富，同时包含用于测试其处理的对象类型并相应调整其处理的函数。

### 示例 6.50.

```scala
import scala.math.sqrt

abstract class Shape

case class Point (x : Double, y : Double) extends Shape
case class Line (x1 : Double, y1 : Double, x2 : Double, y2 : Double)
  extends Shape
case class Circle (x : Double, y : Double, radius : Double) extends Shape

def perimeter (shape : Shape) : Double = {
  shape match {
    case Point (_, _) => 0
    case Line (x1, y1, x2, y2) =>
      sqrt ((x2 - x1) * (x2 - x1) + (y2 - y1) * (y2 - y1))
    case Circle (_, _, r) => 2.0 * 3.14159 * r
  }
}
```

