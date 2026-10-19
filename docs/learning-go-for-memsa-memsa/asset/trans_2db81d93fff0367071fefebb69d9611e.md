

你应该很少在函数之外声明变量，这被称为 *包块*（见第 4 章）。包级变量的值会发生变化是不好的做法。当你在函数之外有一个变量时，很难追踪对其所做的更改，这使得很难理解数据如何在你的程序中流动。这可能导致难以察觉的错误。作为一般规则，你只应在包块中声明那些本质上不可变的变量。

### Tip

避免在函数之外声明变量，因为它们会使数据流分析变得复杂。

你可能会想知道：Go 是否提供了一种 *确保* 值不可变的方法？它确实提供了，但与你在其他编程语言中看到的方式有点不同。是时候学习关于 `const` 了。

# Using const

许多语言都有一种声明值不可变的方法。在 Go 中，这通过 `const` 关键字完成。乍一看，它似乎与在其他语言中的工作方式完全相同。尝试在 [The Go Playground](https://oreil.ly/FdG-W) 上运行 [示例 1-4](#ch01.html#EX2_7) 中的代码，或在 [第 2 章仓库](https://oreil.ly/QxTTT) 的 `sample_code/const_declaration` 目录中运行。

#### Example 1-4. `const` declarations

```
package main

import "fmt"

const x int64 = 10

const (
    idKey   = "id"
    nameKey = "name"
)

const z = 20 * 10

func main() {
    const y = "hello"

    fmt.Println(x)
    fmt.Println(y)

    x = x + 1 // this will not compile!
    y = "bye" // this will not compile!
    fmt.Println(x)
    fmt.Println(y)
}
```

运行此代码时，编译会失败并出现以下错误信息：

```
./prog.go:20:2: cannot assign to x (constant 10 of type int64)
./prog.go:21:2: cannot assign to y (untyped string constant "hello")
```

正如你所见，你可以在包级或函数内声明常量。就像 `var` 一样，你可以（并且应该）在括号内声明一组相关的常量。

请注意，Go 中的 `const` 非常有限。Go 中的常量是为字面量命名的一种方式。它们只能保存编译器能在编译时确定的值。这意味着它们可以被赋值为：

*   数字字面量
*   `true` 和 `false`
*   字符串
*   字符
*   内置函数 `complex`、`real`、`imag`、`len` 和 `cap` 返回的值
*   由运算符和前述值组成的表达式

### Note

我将在下一章介绍 `len` 和 `cap` 函数。另一个可以用在 `const` 中的值称为 `iota`。我将在第 7 章讨论创建自己的类型时谈论 `iota`。

Go 不提供指定运行时计算得到的值不可变的方法。例如，以下代码将无法编译，错误为 `x + y (value of type int) is not constant`：

```
x := 5
y := 10
const z = x + y // this won't compile!
```

正如你将在下一章看到的，没有不可变的数组、切片、映射或结构体，也没有办法声明结构体字段不可变。这听起来限制性没那么大。在函数内部，很清楚变量是否被修改，因此不可变性就不那么重要了。在第 5 章，你将看到 Go 如何防止对作为函数参数传递的变量进行修改。

### Tip

Go 中的常量是为字面量命名的一种方式。在 Go 中 *没有* 办法声明变量不可变。

# Typed and Untyped Constants

常量可以是有类型的或无类型的。无类型常量与字面量完全一样；它本身没有类型，但在无法推断出其他类型时，会使用默认类型。有类型常量只能直接赋给该类型的变量。

是否使常量有类型取决于声明常量的原因。如果你为一个数学常量命名，该常量可能用于多种数值类型，则保持常量无类型。一般来说，让常量保持无类型会给你更多灵活性。在某些情况下，你希望常量强制一个类型。当我在第 7 章介绍使用 `iota` 的枚举时，你会看到有类型常量的用途。

这是一个无类型常量声明的示例：

```
const x = 10
```

以下所有赋值都是合法的：

```
var y int = x
var z float64 = x
var d byte = x
```

这是一个有类型常量声明的示例：

```
const typedX int = 10
```

此常量只能直接赋给 `int`。将其赋给任何其他类型都会产生如下编译时错误：

```
cannot use typedX (type int) as type float64 in assignment
```

# Exercises

1. 编写一个程序，声明类型为 `int` 且值为 7 的变量 `x`，以及类型为 `float64` 且值为 3.5 的变量 `y`。通过转换为适当的类型计算 `x + y`，将结果存储为名为 `sum` 的 `float64`。打印 `sum`。然后计算 `x` 除以 3 的整数除法和 `x` 除以 3 的模。打印两个结果并验证它们满足关系 `(x/3)*3 + (x%3) == x`。

2. 编写一个程序，声明一个无类型常量 `maxSpeed`，值为 299_792_458。将其赋给类型为 `int`、`float64`、`int64` 和 `uint32` 的变量。打印全部四个。然后声明一个有类型常量 `typedSpeed int64 = 299_792_458` 并尝试将其赋给一个 `int` 变量。会发生什么，为什么？

3. 编写一个程序，演示 `var` 和 `:=` 在三种场景下的区别：（a）用特定值声明一个 `byte` 变量（说明为什么 `var x byte = 20` 比 `x := byte(20)` 更受青睐），（b）声明一个变量为其零值，以及（c）`:=` 在 `if` 块内意外遮蔽现有变量的情况。添加注释说明每种情况下哪种风格是惯用的。

# Unused Variables

Go 的目标之一是让大型团队更轻松地协作编写程序。为此，Go 有一些在编程语言中独有的规则。在第 1 章中，你看到 Go 程序需要以特定方式格式化，使用 `go fmt`，以便更轻松地编写代码操作工具并提供编码标准。另一个 Go 要求是 *每个声明的局部变量都必须被读取*。声明局部变量却不读取其值是 *编译时错误*。

编译器的未使用变量检查并不详尽。只要变量被读取一次，编译器就不会抱怨，即使有对变量的写入从未被读取。以下是一个有效的 Go 程序，你可以在 [The Go Playground](https://oreil.ly/8JLA6) 上运行，或在 [第 2 章仓库](https://oreil.ly/FqALl) 的 `sample_code/assignments_not_read` 目录中运行：

```
func main() {
    x := 10 // this assignment isn't read!
    x = 20
    fmt.Println(x)
    x = 30 // this assignment isn't read!
}
```

虽然编译器和 `go vet` 没有捕获对 `x` 的 10 和 30 的未使用赋值，但第三方工具可以检测到它们。我将在代码质量部分谈论这些工具。

### Note

Go 编译器不会阻止你创建未读取的包级变量。这又是一个你应该避免创建包级变量的原因。

# Naming Variables and Constants

Go 对命名变量的规则与 Go 开发者在命名变量和常量时遵循的模式之间存在区别。与大多数语言一样，Go 要求标识符名称以字母或下划线开头，名称可以包含数字、下划线和字母。Go 对“字母”和“数字”的定义比许多语言更宽泛。任何被视为字母或数字的 Unicode 字符都是允许的。这使得 [示例 1-5](#ch01.html#EX1) 中的所有变量定义都是完全有效的 Go。

#### Example 1-5. Variable names you should never use

```
_0 := 0_0
_𝟙 := 20
π := 3
ａ := "hello" // Unicode U+FF41
__ := "double underscore" // two underscores
fmt.Println(_0)
fmt.Println(_𝟙)
fmt.Println(π)
fmt.Println(ａ)
fmt.Println(__)
```

