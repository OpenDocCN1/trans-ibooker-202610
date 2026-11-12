

构造器。在第 6.1 节中，我们考察了现代编程语言中使用的若干基本类型。除了这些添加到类型代数中的基本类型之外，所有语言都包含用于定义新类型的构造。类型定义回应了三个正交需求。首先，用户可能希望创建不同于基本类型（如 int 或 bool）的值。其次，有时需要组合多种类型，以创建更复杂的数据结构。最后，重命名现有类型可能会有所帮助，从而使代码更易于阅读和维护。在所有情况下，类型定义涉及将一个称为类型构造器的新名称添加到类型代数中。

类型定义的引入修改了类型之间现有的相等关系。类型代数项的相等性，即结构相等性，在第 5 章中被隐式使用；如果两个类型的项相同，则认为这两个类型相等。当我们引入新的类型名称——无论是为了重命名现有类型还是创建结构化类型——类型相等性有两种可能的含义。在名义类型化的情况下，我们认为当且仅当两个类型具有相同的名称时，它们才相等。否则，相等性通过验证绑定到名称的类型的结构是否相等来定义：这被称为结构类型化。选择这两种选项之一是语义决策，不取决于语言中可用的定义形式。

## 6.4.1. 类型缩写

类型缩写由为类型表达式命名组成。在 C 和 C++ 中，此命名操作使用 typedef 构造来执行。在 OCaml 中，通过类型定义引入新名称，该定义必须与简单的类型表达式相关（即仅包含类型构造器的表达式）。

暂且不论第 2 卷第 1 章中介绍的封装机制。创建缩写是缩短类型表达式的一种方法。考虑用 C 编写的示例 6.20。我们可能希望使用更简洁的别名，而不是 const unsigned long long int，引入构造器 cullint_t 并在声明中使用它。

### 示例 6.20.

```c
typedef const unsigned long long int cullint_t ;

cullint_t t[10] ;

cullint_t add (cullint_t x, cullint_t x) {
return (x + y) ;
}
```

使用类型缩写也使代码更易于阅读和维护。考虑一个函数，其输入为压力和速度，并为机器返回功率命令。该函数的类型是 `float → float → float`。使用此类型，无法区分代表这些不同值的 `float` 数字，并且函数的类型对其参数没有提供任何语义信息。引入三个类型缩写，如示例 6.21 所示，允许更具信息量的函数类型。

### 示例 6.21.

```ocaml
# type pressure_t = float ;;
type pressure_t = float
# type speed_t = float ;;
type speed_t = float
# type command_t = float ;;
type command_t = float
# let regulate (p : pressure_t) (s : speed_t) : command_t = ... ;;
val regulate : pressure_t -> speed_t -> command_t = <fun>
```

在许多语言中，例如 C、C++ 和 OCaml，类型缩写不意味着名义类型化。别名仍然等于其定义的主体。这使得可以定义类型为 pressure_t 的变量 p 和类型为 speed_t 的变量 s，并比较它们或使用类型为 float → float → float 的运算器 +.，如示例 6.22 所示。

### 示例 6.22.

```ocaml
# let p : pressure_t = 2.0 ;;
val p : pressure_t = 2.
# let s : speed_t = 25.0 ;;
val s : speed_t = 25.
# p +. 2.5 = s ;;
- : bool = false
```

## 6.4.2. 记录

个人可以由包括姓、名和出生日期在内的数据来描述。姓和名是字符串，而出生日期由三个整数组成。由于这些数据元素的类型不同，它们不能存储在数组中。记录（或结构体）的概念用于在单一类型中聚合不同类型的数据。

每个数据元素都有名称，并表示记录的一个字段。因此，每个字段都分配了它旨在包含的数据元素对应的类型。记录类型的定义由类型名称和字段名称的枚举（以及各自的类型）给出。

### 示例 6.23.

```ocaml
type person = {
  surname : string ; firstname : string ; month : int ; day : int ; year : int ;
}
```

### 示例 6.24.

```c
struct person {
  char *surname ; char *firstname ; int month ; int day ; int year ;
};
```

如我们在第 6.3 节中所见，字符串不是 C 中的基本类型，这解释了它们声明的特定方式。

如果 e 是包含字段 fld 的记录类型的表达式，则“点表示法” e.fld 表示 e 的值的字段 fld。

记录类型值通过指定每个字段关联的值来构造。在 OCaml 中，如示例 6.25 所示，记录值通过指定字段名称来定义，以启用类型综合。这种看似冗长的方法的一个优点是，字段值可以以任何顺序定义。即使没有类型推理机制，Ada 也是如此。

### 示例 6.25.

```ocaml
let p = { firstname = "John" ; month = 7 ; day = 5 ;
          year = 1900 ; surname = "Doe" }
```

在 C 中，不会显示字段名称，因此必须按照声明记录类型时使用的相同顺序指定字段值。

### 示例 6.26.

```c
struct person p = { "Doe", "John", 7, 5, 1900 } ;
```

在许多编程语言中，记录的字段不能彼此定义，也不能相对于前面的字段定义。示例 6.27 和 6.28 说明了这一点。

### 示例 6.27.

```ocaml
# type t = { a : int ; b : int } ; ;
type t = { a : int ; b : int ; }
# let v = { a = 5 ; b = v.a } ; ;
Error: Unbound value v
# let rec v = { a = 5 ; b = v.a } ; ;
Error: This kind of expression is not allowed as right-hand side of 'let rec'
```

### 示例 6.28.

```c
#include <stdio.h>
struct t { int a ; int b ; } ;
void f () {
  struct t v = { 15, v.a } ;
  printf ("%d\n", v.b) ;
}
int main () {
  f () ;
  return (0) ;
}
```

```shell
$ gcc foo.c
foo.c:7:22: warning: variable 'v' is uninitialized when used within its own initialization [-Wuninitialized]
  struct t v = { 15, v.a } ;
            ~
1 warning generated.
$ ./a.out
0
```

在 C 中，编译器不会拒绝此类定义，但会警告用户，在定义记录本身时使用的记录字段的值未初始化。以这种方式获得的程序的执行清楚地表明，字段 b 未正确初始化。

## 6.4.2.1. 记录类型化

记录类型定义将一个新名称（类型构造器）引入类型环境。因此，需要考虑如何确定记录值的类型，并进而考虑具有相同结构但不同名称的记录类型如何相互作用。记录类型化有两种主要方法。

第一种方法（允许类型推断）包括根据表达式中找到的字段名称来指导类型。因此，记录表达式的值必须包括字段名称，而不仅仅是它们的值。OCaml 就是这样。因此，类型定义 type t = { a : int ; b : float } 将两个新名称 a 和 b 引入环境中的记录字段名称命名空间，同时记录它们属于类型 t 这一事实。分析记录表达式时，其字段都必须属于同一类型，以指导其验证。因此，两个不同的记录类型不能共享相同的字段名称（除非程序员付出大量努力）。但是，如果在不同的编译单元或模块中进行类型定义，可以通过指定包含这些定义的编译单元来避免歧义。示例 6.29 说明了这一点。

### 示例 6.29.

```ocaml
# type foo = { a : int ; b : float } ; ;
type foo = { a : int ; b : float ; }
# type bar = { a : int ; b : float } ; ;
type bar = { a : int ; b : float ; }
# let v = { a = 42 ; b = 3.14 } ;;
val v : bar = {a = 42; b = 3.14}
```

绑定到值 v 的表达式被推断为 bar 类型，因为它负责将字段名称 a 与记录类型的最近绑定。由于相应的字段名称被屏蔽，因此无法再创建 foo 类型的值。但是请注意，仍然可以通过操作其字段来操作 foo 类型的值。使用一种能够覆盖推断的类型注解形式，可以编写函数 get_foo_a 以从 foo 类型的值中获取字段 a 的值。

### 示例 6.30.

```ocaml
# let get_foo_a (x : foo) = x.a ;;
val get_foo_a : foo -> int = <fun>
```

第二种方法（例如在 C 中使用）使用分配给接受记录类型值的变量的类型。在这种情况下，无需在记录值中指定字段名称。因此，值 { 42, 3.14 } 本身没有类型，除了它是一个双字段结构体，第一个字段是整数，第二个字段是浮点数。但是，在赋值或初始化点，会检查在定义接收变量时给定的类型是否与表达式的字段兼容。这意味着两个不同的记录类型可以共享相同的字段名称，如示例 6.31 所示。

