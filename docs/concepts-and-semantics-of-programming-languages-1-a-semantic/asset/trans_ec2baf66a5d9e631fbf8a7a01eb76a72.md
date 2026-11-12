

### 示例 6.31

```c
struct foo { int a ; float b ; } ;
struct bar { int a ; float b ; } ;

void f ( ) {
    struct foo v1 = { 42, 3.14 } ;
    struct bar v2 = { 43, 3.15 } ;
}
```

请注意，在 C 中，记录表达式只能用于初始化变量。因此，形式为 `f ({ 42, 3.14 })` 的表达式是非法的。

在 C、Java 或 OCaml 中，如果两个记录类型结构相同但名称不同，则它们不兼容：记录的类型是名义类型，而不是结构类型。让我们在之前用 C 编写的程序中添加将 `v2` 赋值给 `v1` 的操作。在示例 6.32 中，尽管这两个变量均由相同形式的表达式初始化，但将 `v2` 赋值给 `v1` 的操作会被拒绝。

### 示例 6.32

```c
...
void f () {
    struct foo v1 = { 42, 3.14 } ;
    struct bar v2 = { 43, 3.15 } ;
    v1 = v2 ;
}
```

```
$ gcc -Wall foo.c
foo.c:14:6: error: assigning to 'struct foo' from incompatible type 'struct bar'
  v1 = v2 ;
  ~ ^ ~
1 error generated.
```

## 6.4.3. 枚举类型

交通灯的“值”为红色、黄色和绿色。我们希望编写一个函数 next，它接收交通灯的当前状态并返回下一个状态。

第一个解决方案是为交通灯的每个值选择一个基本类型（例如 int）的值：例如，取 red = 54，yellow = 12 和 green = 36。然而，没有任何东西能阻止将“交通灯值”与任何其他整数值组合。red/2 或 (next 23) 等表达式类型完全正确，尽管它们在语义上是不正确的。函数 next 的设计目的是仅针对三个特定值进行操作——那么如果它接收到一个意外的输入值，应该怎么做呢？

假设我们不小心同时定义了 red = 36 和 green = 36。尽管这两个本应互斥的交通灯值现在相等，但不会引发任何错误。用于测试交通灯值的表达式将不再区分红灯和绿灯。

我们明确选择了要分配给灯值的整数值。考虑到用于对颜色进行编码的值对作用于它们的算法没有任何影响，这种选择没有真正的理由。某些语言包含一种结构，允许用户执行枚举类型值的所有可能情况的测试并验证穷举性（没有遗漏任何值）。在光值由整数编码的情况下，我们最好需要编写一个永远不应该发生的一般情况，或者最坏的情况下需要编写所有其他永远不应该发生的可能情况。定义一个具有两两互斥值且仅与该类型的其他值兼容的新类型的能力消除了这些缺点。这种枚举类型定义枚举了赋予值的名称，称为 *值构造器*。

诸如 Java、C 和 C++ 等语言允许通过 enum 类型构造来创建这些定义。一旦引入了值构造器，它们就可以像其他任何值一样使用。示例 6.33 对此进行了说明。

### 示例 6.33

```c
enum signal_t { Green, Yellow, Red } ;

enum signal_t next (enum signal_t state) {
if (state == Green) return Yellow ;
if (state == Yellow) return Red ;
return Green ;
}
```

语言编译器负责在内部通过整数对这些值进行编码。值构造器的强类型程度可能因语言而异。在 C 中，它们本身就是整数：始终可以使用任何给定整数调用 next 函数。在 C++ 和 Java 中，构造器仅是声明它们的类型的值，且与整数不兼容。例如，在 C++ 中使用 0 作为参数调用 next 会导致类型错误：

```
foo.cpp:14:3: error: no matching function for call to 'next'
next (0) ;
^~~
bar.cpp:4:15: note: candidate function not viable: no known conversion from
'int' to 'enum signal_t' for 1st argument
enum signal_t next (enum signal_t state)
^
1 error generated.
```

## 6.4.4. 和类型

OCaml 使用的和类型比简单的构造器枚举更复杂。和类型用于将多种类型的值分组为单一类型。取两种类型 t1 和 t2。让我们定义一种类型 t，其中所有值要么是类型 t1 的值（由 C1 标记），要么是类型 t2 的值（由 C2 标记）。和类型的构造类似于集合上的不交并运算（因此得名和类型）。C1 定义了类型 t1 的值 v1 的注入，结果为类型 t 的值 C1(v1)；同样，类型 t2 的值 v2 被注入为 t 中的 C2(v2)。C1 和 C2 称为类型 t 的值构造器。

本书中展示的 OCaml 代码已经大量使用了和类型来表示语言的抽象语法和遇到的各种代数。在示例 6.34 中，扑克牌由一种和类型编码。

### 示例 6.34

```ocaml
type card_t = Jack | Queen | King | Number of int
```

一张牌可能是一张牌面卡，仅由其名称标识。或者，它可能是一张数字卡，必须知道数字才能将其与花色中的其他卡牌区分开来。值构造器 Number 应用于 int 类型的值。值构造器 Jack 定义了 card_t 类型的一个唯一值。

和类型可以是多态的。其中一些构造器可以应用于使用类型变量编写的类型，这些变量由和类型的定义引入。OCaml 标准库中的预定义列表由一种递归且多态的类型定义给出，类似于示例 6.35 所示的定义。

### 示例 6.35

```ocaml
type 'a list = Nil | Cons of ('a * 'a list)
```

在 C/C++ 中，没有用于值构造器的语法结构。程序员必须自行使用 union 和 struct 类型对构造器进行编码。联合类型本质上是其定义中提到的不同类型的不交并。联合的值在一个内存范围内表示，其大小为这些不同类型的值大小的最大值。因此，它与记录不同，记录中的数据是聚合的。联合的每个字段允许访问联合的数据，该数据将被解释为具有与字段相关联的类型。示例 6.36 说明了这一点。

### 示例 6.36

```c
#include <stdio.h>

union int_float_t { int as_int ; float as_float ; } ;

int main () {
  union int_float_t v ;
  v.as_float = 3.14 ;
  printf ("As the float %f as the integer %d\n", v.as_float, v.as_int) ;
  return (0) ;
}
```

```
$ ./a.out
As the float 3.140000 as the integer 1078523331
```

union 类型 int_float_t 的任何值都可以有两种解释。变量 v 通过将联合视为 float 来初始化。显示了该值的两种视图。第一个视图是作为 float，允许根据写入时所做的解释来解释内存中包含的数据。因此，我们找到值 3.14。第二个显示是通过将值解释为整数来执行的。由于整数和浮点数的编码根本不同，我们看到一个似乎无关的值。

C 中的联合类型不像 OCaml 中的和类型那样具有值构造器。因此，有必要添加一个标记，以便在运行时知道应使用联合的哪个字段以及如何解释联合的值。示例 6.36 表明，错误的解释会产生语义上不正确的结果。合适的标记可以是一个枚举类型，其值的数量至少与联合的字段数量相同。然后应通过记录将此标记和联合分组在一起。让我们通过示例 6.37 来看看这一点。

### 示例 6.37

```c
#include <stdio.h>

enum tag_t { Val_int, Val_float } ;
union int_float_t { int as_int ; float as_float ; } ;
struct sum_t { enum tag_t tag ; union int_float_t val ; } ;

struct sum_t read_int () {
  struct sum_t v ;
  scanf ("%d", &v.val.as_int) ;
  v.tag = Val_int ;
  return (v) ;
}

struct sum_t read_float () {
  struct sum_t v ;
```

