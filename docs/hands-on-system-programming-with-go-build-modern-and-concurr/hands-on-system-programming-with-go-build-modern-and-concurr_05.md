# Go 概览

本章将提供 Go 语言及其基本功能的概览。我们将简要解释该语言及其特性，这些将在后续章节中详细阐述。这将帮助我们在使用所有特性和应用时更好地理解 Go。

本章将涵盖以下主题：

*   语言特性
*   包和导入
*   基本类型、接口和用户定义类型
*   变量和函数
*   流程控制
*   内置函数
*   并发模型
*   内存管理

## 技术要求

从本章开始，你需要在机器上安装 Go。请按以下步骤操作：

1.  从 [`golang.org/dl/`](https://golang.org/dl/) 下载最新版本的 Go。
2.  使用 `tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz` 解压。
3.  使用 `export PATH=$PATH:/usr/local/go/bin` 将其添加到 `PATH`。
4.  使用 `go version` 验证 Go 是否已安装。
5.  在你的 `.profile` 中添加 export 语句以自动添加。
6.  如果你想为代码使用不同的目录，也可以更改 `GOPATH` 变量（默认为 `~/go`）。



我还建议安装 Visual Studio Code ([`code.visualstudio.com/`](https://code.visualstudio.com/)) 及其 vscode-go ([`github.com/Microsoft/vscode-go`](https://github.com/Microsoft/vscode-go)) 扩展，其中包含一个助手，可安装改善 Go 开发体验所需的所有工具。

# 语言特性

Go 是一门现代服务端语言，拥有出色的并发原语和一个主要自动化的内存系统。有人认为它是 C 语言的继任者，并且在许多场景下确实做到了这一点，因为它性能优异、拥有庞大的标准库，且拥有活跃的社区，提供了许多覆盖、扩展和改进其功能的第三方库。

# Go 语言历史

Go 创建于 2007 年，旨在尝试解决 Google 的工程问题，于 2009 年公开发布，并于 2012 年达到 1.0 版本。主版本号保持不变（版本 1），而次版本号随功能增长（版本 1.1、1.2 等）。这是为了信守 Go 对所有主版本兼容性的承诺。两项新特性（泛型和错误处理）的草案于 2018 年发布，预计将包含在 2.0 版本中。

Go 语言的设计者如下：

* **Robert Griesemer**：Google 研究员，参与过许多项目，包括 V8 JavaScript 引擎的设计与实现，以及 Sawzall 的实现。
* **Rob Pike**：Unix 团队成员、Plan 9 和 Inferno 操作系统开发团队成员、Limbo 编程语言设计团队成员。
* **Ken Thompson**：计算机科学先驱、原始 Unix 设计者、B 语言发明者（C 语言的直接前身）。Ken 也是 Plan 9 操作系统的创建者和早期开发者之一。

# 优势与劣势

Go 是一门极具主见的语言；有人爱它，也有人恨它，主要归因于其某些设计选择。一些不太受欢迎的特性如下：

* 冗长的错误处理
* 缺乏泛型
* 缺失依赖和版本管理

前两点将在下一个主版本中解决，而后一点首先由社区解决（godep、glide 和 govendor），随后由 Google 通过 `dep` 解决依赖问题，并通过 `gopkg.in` ([`labix.org/gopkg.in`](http://labix.org/gopkg.in)) 解决版本管理问题。

该语言的优势数不胜数：

* 它是静态类型语言，拥有由此带来的所有优势，如静态类型检查。
* 它不需要**集成开发环境**（**IDE**），尽管它支持许多 IDE。
* 标准库令人印象深刻，可能是许多项目的唯一依赖。
* 它拥有并发原语（通道和 goroutine），隐藏了编写高效且安全的异步代码中最困难的部分。
* 它自带格式化工具 `gofmt`，统一了 Go 代码的格式，使他人的代码看起来非常熟悉。
* 它生成无依赖的二进制文件，使部署快速简便。
* 它极简且关键字很少，代码非常易读易懂。
* 它是鸭子类型，具有隐式接口定义（*如果它走起路来像鸭子、游起泳来像鸭子、叫起声来像鸭子，那么它很可能就是鸭子*）。这在测试系统的特定功能时非常方便，因为可以对其进行模拟。
* 它是跨平台的，意味着它能为不同于宿主机的架构和操作系统生成二进制文件。
* 拥有海量的第三方包，因此在功能方面几乎无所欠缺。托管在公共仓库中的每个包都会被索引并可搜索。

# 命名空间

现在，让我们看看 Go 代码是如何组织的。`GOPATH` 环境变量决定了代码所在的位置。其中包含三个子目录：

* `src` 包含所有源代码。
* `pkg` 包含编译后的包，按架构/操作系统划分。
* `bin` 包含编译后的二进制文件。

源码文件夹下的路径对应包名（`$GOPATH/src/my/package/name` 对应 `my/package/name`）。

`go get` 命令可用于获取并编译包。`go get` 调用 `http://package_name?go-get=1`，如果找到 `go-import` 元标签，则使用它来获取包。该标签应包含包名、所用的 VCS 和仓库 URL，均以空格分隔。让我们看一个例子：

```
<meta name="go-import" content="package-name vcs repository-url">
```

`go get` 下载包后，会尝试对其他无法解析的包递归执行相同操作，直到所有必要的源代码都可用。

每个文件以包定义开头，即 `package package_name`，同一目录下所有文件的包名必须相同。如果包生成二进制文件，则包名为 `main`。

# 导出与导入符号

包声明后跟随一系列 `import` 语句，指定所需的包。

导入未使用的包（除非被忽略）是编译错误，这就是 Go 格式化工具 `gofmt` 会移除未使用包的原因。有一些实验性或社区工具，如 `goimports` ([`godoc.org/golang.org/x/tools/cmd/goimports`](https://godoc.org/golang.org/x/tools/cmd/goimports)) 或 `goreturns` ([`github.com/sqs/goreturns`](https://github.com/sqs/goreturns))，它们也会向 Go 文件添加缺失的导入。重要的是要避免循环依赖，因为它们无法编译。

由于不允许循环依赖，包的设计需要与其他语言不同。为了打破循环依赖，最佳实践是从包中导出功能，或用接口替换依赖。

Go 将所有符号可见性简化为二元模型——导出和未导出——这与许多具有中间层级的语言不同。对于每个包，所有以大写字母开头的符号均被导出，其余仅在包内使用。导出的值也可被其他包使用，而未导出的值只能在包内部使用。

如果包路径元素之一为 `internal`（例如 `my/package/internal/pdf`），则作例外处理。这将其自身及其子包限制为仅可被相邻包导入（例如 `my/package`）。如果有大量未导出符号，并希望将其拆分为子包，同时防止其他包使用它们，从而基本实现私有子包，这非常有用。请看下面的内部包列表：

* `my/package/internal`
* `my/package/internal/numbers`
* `my/package/internal/strings`

这些只能被 `my/package` 使用，不能被任何其他包导入，包括 `my`。

导入可以有不同形式。标准导入形式是完整的包名：

```
import "math/rand"
...
rand.Intn
```

命名导入用自定义名称替换包名，引用包时必须使用该名称：

```
import r "math/rand"
...
r.Intn
```

点导入使符号无需命名空间即可使用：

```
import . "math/rand"
...
Intn
```

忽略导入用于导入包而无需在代码中引用它们。这使得在不引用包的情况下执行包的 `init` 函数成为可能：

```
import _ "math/rand"  
// 仍会执行 rand.init 函数
```

# 类型系统

Go 类型系统定义了一系列基本类型，包括字节、字符串和缓冲区，复合类型如切片或映射，以及应用程序定义的自定义类型。





# 基础类型

以下是 Go 语言的基础类型：

| 类别 | 类型 |
| --- | --- |
| 字符串 | `string` |
| 布尔值 | `bool` |
| 整型 | `int`, `int8`, `int16`, `int32`, 和 `int64` |
| 无符号整型 | `uint`, `uint8`, `uint16`, `uint32`, 和 `uint64` |
| 指针整型 | `uintptr` |
| 浮点型 | `float32` 和 `float64` |
| 复数 | `complex64` 和 `complex128` |

`int`、`uint` 和 `uintptr` 的位数取决于架构（例如，x86 为 32 位，x86_64 为 64 位）。

# 复合类型

除了基础类型，还有其他类型，称为复合类型。如下所示：

| 类型 | 描述 | 示例 |
| --- | --- | --- |
| 指针 | 变量在内存中的地址 | `*int` |
| 数组 | 具有固定长度的同类型元素容器 | `[2]int` |
| 切片 | 数组的连续片段 | `[]int` |
| 映射 | 字典或关联数组 | `map[int]int` |
| 结构体 | 可以拥有不同类型的字段集合 | `struct{ value int }` |
| 函数 | 具有相同参数和输出的一组函数 | `func(int, int) int` |
| 通道 | 用于同类型元素通信的类型管道 | `chan int` |
| 接口 | 特定的方法集合，带有支持它们的底层值 | `interface{}` |

空接口 `interface{}` 是一种泛型类型，可以包含任意值。由于该接口没有要求（方法），因此任何值都可以满足它。

接口、指针、切片、函数、通道和映射可以有空值，在 Go 中用 `nil` 表示：

*   指针不言自明；它们不指向任何变量地址。
*   接口的底层值可以为空。
*   其他指针类型，如切片或通道，也可以为空。

# 自定义类型

包可以使用 `type defined definition` 表达式定义自己的类型，其中 definition 是共享已定义内存表示的类型。自定义类型可以由基础类型定义：

```
type Message string    // 自定义字符串
type Counter int       // 自定义整数
type Number float32    // 自定义浮点数
type Success bool      // 自定义布尔值
```

它们也可以由复合类型定义，如切片、映射或指针：

```
type StringDuo [2]string            // 自定义数组
type News chan string               // 自定义通道
type Score map[string]int           // 自定义映射
type IntPtr *int                    // 自定义指针
type Transform func(string) string  // 自定义函数
type Result struct {                // 自定义结构体
    A, B int
}
```

它们也可以与其他自定义类型组合使用：

```
type Broadcast Message // 自定义 Message
type Timer Counter     // 自定义 Counter
type News chan Message // 自定义自定义类型 Message 的通道
```

自定义类型的主要用途是定义方法，并使类型针对特定作用域，例如定义一个名为 `Message` 的 `string` 类型。

接口定义的工作方式不同。它们可以通过指定一系列不同的方法来定义，例如：

```
type Reader interface {
    Read(p []byte) (n int, err error)
}
```

它们也可以是其他接口的组合：

```
type ReadCloser interface {
    Reader
    Closer
}
```

或者，它们可以是两种接口的组合：

```
type ReadCloser interface {
    Reader        // 组合
    Close() error // 方法
}
```

# 变量和函数

既然我们已经了解了类型，接下来将看看如何在语言中实例化不同的类型。我们将首先了解变量和常量的工作原理，然后讨论函数和方法。

## 处理变量

变量表示到一段连续内存内容的映射。它们有一个类型，定义了这块内存延伸多远，还有一个值，指定了内存中的内容。类型可以是基础、复合或自定义的，其值可以通过声明用其零值初始化，或通过赋值用另一个值初始化。

### 声明

变量的零值声明使用 `var` 关键字，并指定名称和类型来完成；例如 `var a int`。对于来自其他语言（如 Java）的人来说，这可能违反直觉，因为类型和名称的顺序是颠倒的，但它实际上更具可读性。

示例 `var a int` 描述了一个变量（`var`），名为 `a`，类型为整数（`int`）。这种表达式会使用所选类型的零值创建一个新变量：

| 类型 | 零值 |
| --- | --- |
| 数值类型（`int`、`uint` 和 `float` 类型） | `0` |
| 字符串（`string` 类型） | `""` |
| 布尔值 | `false` |
| 指针、接口、切片、映射和通道 | `nil` |

初始化变量的另一种方式是赋值，它可以具有推断类型或指定类型。推断类型通过以下方式实现：

*   变量名，后跟 `:=` 运算符和值（例如 `a := 1`），也称为**短声明**。
*   `var` 关键字，后跟名称、`=` 运算符和值（例如 `var a = 1`）。

请注意，这两种方法几乎等价，且有冗余，但 Go 团队为了 Go 1 的兼容性承诺决定保留两者。就短声明而言，主要区别在于类型无法指定，而是由值推断得出。

指定类型的赋值是通过声明、等号和值来完成的。如果声明的类型是接口，或者推断的类型不正确，这非常有用。以下是一些示例：

```
var a = 1             // 这将是一个 int
b := 1                // 这等价于上面
var c int64 = 1       // 这将是一个 int64

var d interface{} = 1 // 这被声明为 interface{}
```

某些类型需要使用内置函数才能正确初始化：

*   `new` 可以创建特定类型的指针，同时为底层变量分配空间。
*   `make` 初始化切片、映射和通道：
    *   切片需要一个额外参数指定大小，以及一个可选参数指定底层数组的容量。
    *   映射可以有一个参数指定其初始容量。
    *   通道也可以有一个参数指定其容量。它们与映射不同，映射不能更改（此处原文 "They are unlike maps, which cannot change" 可能指通道容量创建后不可变，或指 make 行为差异，译文保留原意）。

使用内置函数初始化类型如下：

```
a := new(int)                   // 新 int 变量的指针
sliceEmpty := make([]int, 0)    // 大小为 0，容量为 0 的 int 切片
sliceCap := make([]int, 0, 10)  // 大小为 0，容量为 10 的 int 切片
map1 = make(map[string]int)     // 具有默认容量的映射
map2 = make(map[string]int, 10) // 容量为 10 的映射
ch1 = make(chan int)            // 无容量的通道（无缓冲）
ch2 = make(chan int, 10)        // 容量为 10 的通道（有缓冲）
```

### 操作

我们已经看过了赋值操作，它使用 `=` 运算符给变量赋予新值。让我们看看更多运算符：

*   有比较运算符 `==` 和 `!=`，它们比较两个值并返回布尔值。





## 运算符

有一些数学运算可以在所有相同类型的数值变量上执行，即 `+`、`-`、`*` 和 `/`。求和运算也用于连接字符串。`++` 和 `--` 是将数字加一或减一的简写。`+=`、`-=`、`*=` 和 `/=` 在等号前的内容和等号后的内容之间执行等号前的操作，并将其赋值给左侧的变量。这四种运算产生的值类型与涉及的变量类型相同；还有一些专门用于数字的比较运算符：`<`、`<=`、`>` 和 `>=`。

一些运算专用于整数并产生其他整数：`%`、`&`、`|`、`^`、`&^`、`<<` 和 `>>`。

其他运算仅用于布尔值并产生另一个布尔值：`&&`、`||` 和 `!`。

有一个运算符专用于通道，即 `<-`，它用于从通道接收值或向通道发送值。

对于所有非指针变量，也可以使用 `&`（引用运算符）来获取变量地址，该地址可以赋值给指针变量。`*` 运算符使得在指针上执行解引用操作并获取其指向的变量的值成为可能：

| 运算符 | 名称 | 描述 | 示例 |
| --- | --- | --- | --- |
| `=` | 赋值 | 将值赋给变量 | `a = 10` |
| `:=` | 声明并赋值 | 声明一个变量并赋值 | `a := 0` |
| `==` | 等于 | 比较两个变量，若相同则返回布尔值 | `a == b` |
| `!=` | 不等于 | 比较两个变量，若不同则返回布尔值 | `a != b` |
| `+` | 加 | 相同数值类型间的求和 | `a + b` |
| `-` | 减 | 相同数值类型间的求差 | `a - b` |
| `*` | 乘 | 相同数值类型间的乘法 | `a * b` |
| `/` | 除 | 相同数值类型间的除法 | `a / b` |
| `%` | 取模 | 相同数值类型除法后的余数 | `a % b` |
| `&` | 与 | 按位与 | `a & b` |
| `&^` | 位清除 | 位清除 | `a &^ b` |
| `<<` | 左移 | 向左位移 | `a << b` |
| `>>` | 右移 | 向右位移 | `a >> b` |
| `&&` | 逻辑与 | 布尔逻辑与 | `a && b` |
| `||` | 逻辑或 | 布尔逻辑或 | `a || b` |
| `!` | 非 | 布尔逻辑非 | `!a` |
| `<-` | 接收 | 从通道接收 | `<-a` |
| `->` | 发送 | 向通道发送 | `a <- b` |
| `&` | 取地址 | 返回变量的指针 | `&a` |
| `*` | 解引用 | 返回指针指向的值 | `*a` |

## 类型转换

将一种类型转换为另一种类型的操作称为**类型转换**，其对于接口和具体类型的工作方式略有不同：

*   接口可以转换为实现它的具体类型。这种转换可以返回第二个值（一个布尔值），指示转换是否成功。如果省略布尔变量，转换失败时应用程序将发生 panic。
*   对于具体类型，转换可以发生在具有相同内存结构的类型之间，也可以发生在数值类型之间：

```
type N [2]int                // 用户定义类型
var n = N{1,2}
var m [2]int = [2]int(N)     // 因为 N 是 [2]int，所以这种转换是可能的

var a = 3.14                 // 这是一个 float64
var b int = int(a)           // 数值类型可以转换，这里 a 将被舍入为 3

var i interface{} = "hello"  // 一个包含字符串的新空接口
x, ok := i.(int)             // ok 将为 false
y := i.(int)                 // 这将导致 panic
z, ok := i.(string)          // ok 将为 true
```

有一种用于类型转换的特殊条件运算符，称为**类型开关**，它允许应用程序一次尝试多种转换。以下是使用 `interface{}` 检查底层值的示例：

```
func main() {
    var a interface{} = 10
    switch a.(type) {
    case int:
        fmt.Println("a is an int")
    case string:
        fmt.Println("a is a string")
    }
}
```

## 作用域

变量拥有作用域或可见性，这也与其生命周期相关。作用域可以是以下之一：

*   **包级**：变量在包内所有文件中可见；如果变量被导出，它也在其他包中可见。
*   **函数级**：变量在声明它的函数内部可见。
*   **控制块级**：变量在其定义所在的代码块内可见。

可见性由外向内，从包级到代码块级。由于代码块可以嵌套，外部代码块无法访问内部代码块的变量。

同一作用域中的两个变量不能同名，但内部作用域的变量可以复用标识符。当这种情况发生时，外部变量在内部作用域中不可见——这称为**变量遮蔽**，必须牢记这一点，以避免难以识别的问题，例如以下情况：

```
// 此变量存在于外部代码块
var err error
// 此变量仅存在于此代码块，遮蔽了外部的 err
if err := errors.New("Doh!"); err != nil {
    fmt.Println(err)           // 这不会改变外部的 err
}
fmt.Println(err)               // 外部的 err 未被改变
```

## 常量

Go 语言的变量没有不可变性，但定义了另一种不可变值类型，称为常量。常量由 `const` 关键字定义（而不是 `var`），它们是不能改变的值。这些值可以是基础类型和自定义类型，如下所示：

*   数值（整数、`float`）
*   复数
*   字符串
*   布尔值

赋值给变量时，指定的值没有类型。数值类型和基于字符串的类型都会自动转换，如下面的代码所示：

```
const PiApprox = 3.14

var PiInt int = PiApprox // 3，转换为整数
var Pi float64 = PiApprox // 是一个 float

type MyString string

const Greeting = "Hello!"

var s1 string = Greeting   // 是一个 string
var s2 MyString = Greeting // string 转换为 MyString
```

数值常量对于数学运算非常有用，因为它们只是普通数字，所以可以与任何数值变量类型一起使用。

## 函数和方法

Go 语言中的函数由 `func` 关键字标识，后跟标识符、可选参数和返回值。Go 语言中的函数可以一次返回多个值。参数和返回类型的组合称为**签名**，如下面的代码所示：

```
func simpleFunc()
func funcReturn() (a, b int)
func funcArgs(a, b int)
func funcArgsReturns(a, b int) error
```

大括号之间的部分是函数体，可以在其中使用 `return` 语句提前中断函数执行。如果函数有返回值，则 return 语句必须返回相同类型的值。

返回值可以在签名中命名；它们是零值变量，如果 `return` 语句未指定其他值，则返回这些命名变量的值：

```
func foo(a int) int {        // 返回类型无变量名
    if a > 100 {
        return 100
    }
    return a
}

func bar(a int) (b int) {    // 返回类型有变量名
    if a > 100 {
        b = 100
        return               // 等同于 return b
    }
    return a
}
```

函数在 Go 语言中是一等类型，也可以赋值给变量，每个签名代表一种不同的类型。它们也可以是匿名的；在这种情况下，它们被称为**闭包**。一旦变量用函数初始化，同一个变量就可以重新赋值为另一个具有相同签名的函数。以下是将闭包赋值给变量的示例：





```go
var a = func(item string) error { 
    if item != "elixir" {
        return errors.New("Gimme elixir!")
    }
    return nil 
}
```

接口声明的函数被称为方法，它们可以由自定义类型实现。方法实现看起来像函数，区别在于名称前面有一个实现类型的单个参数。这只是语法糖——方法定义在底层创建了一个函数，它接受一个额外参数，即实现该方法的类型。

这种语法使得为不同类型定义同名方法成为可能，每个类型都将充当函数声明的命名空间。这样，就可以用两种不同的方式调用方法，如下面的代码所示：

```go
type A int

func (a A) Foo() {}

func main() {
    A{}.Foo()  // 在类型实例上调用方法
    A.Foo(A{}) // 在类型上调用方法并将实例作为参数传递  
}
```

重要的是要注意，类型及其指针共享同一个命名空间，因此同一个方法只能为它们中的一个实现。不能同时为类型及其指针定义同一个方法，因为为类型和其指针分别声明方法会产生编译错误（方法重复声明）。方法不能为接口定义，只能为具体类型定义，但接口可以用在复合类型中，包括函数参数和返回值，如下面的例子所示：

```go
// use error interface with chan
type ErrChan chan error
// use error interface in a map
type Result map[string]error

type Printer interface{
    Print()
}
// use the interface as argument
func CallPrint(p Printer) {
    p.Print()
}
```

内置包已经定义了一个在标准库和所有在线可用包中广泛使用的接口——`error` 接口：

```go
type error interface {
    Error() string
}
```

这意味着任何拥有 `Error() string` 方法的类型都可以作为 error 使用，每个包可以根据需要定义自己的错误类型。这可以用来简洁地携带错误信息。在这个例子中，我们定义了 `ErrKey`，指明一个 `string` 类型的 key 未找到。除了 key 之外我们不需要其他东西来表示错误，如下面的代码所示：

```go
type ErrKey string

func (e ErrKey) Error() string {
    return fmt.Errorf("key %q not found", e)
}
```

## 值和指针

在 Go 中，一切都是按值传递的，所以当调用函数或方法时，变量的副本会在栈上创建。这意味着对值所做的更改不会反映到被调用函数的外部。即使是切片、映射和其他引用类型也是按值传递的，但由于它们的内部结构包含指针，它们表现得就像按引用传递一样。如果为类型定义了方法，则不能为其指针定义，反之亦然。下面的例子用于验证值仅在方法内部更新，且该更改不会反映到 `main` 函数中：

```go
package main

import (
    "fmt"
)

type A int

func (a A) Foo() {
    a++
    fmt.Println("foo", a)
}

func main() {
    var a A
    fmt.Println("before", a) // 0
    a.Foo() // 1
    fmt.Println("after", a) // 0
}
```

为了改变原始变量，参数必须是指向变量本身的指针——指针会被复制，但它会引用同一内存区域，从而可以改变其值。注意，赋值另一个指针值，而不是其内容，不会改变原始指针所指向的内容，因为它是一个副本。

如果我们为类型而不是其指针使用方法，我们将看不到更改传播到方法外部。

在下面的例子中，我们使用了值接收者。这使得 `Birthday` 方法中的 `User` 值成为 `main` 中 `User` 值的副本：

```go
type User struct {
    Name string
    Age int
}

func (u User) Birthday() {
    u.Age++
    fmt.Println(u.Name, "turns", u.Age)
}

func main() {
    u := User{Name: "Pietro", Age: 30}
    fmt.Println(u.Name, "is now", u.Age)
    u.Birthday()
    fmt.Println(u.Name, "is now", u.Age)
}
```

完整示例可在 <https://play.golang.org/p/hnUldHLkFJY> 查看。

由于更改应用于副本，原始值保持完好，正如我们从第二个打印语句看到的那样。如果我们想更改原始对象中的值，必须使用指针接收者，这样被复制的将是指针，更改将作用于底层值：

```go
func (u *User) Birthday() {
    u.Age++
    fmt.Println(u.Name, "turns", u.Age)
}
```

完整示例可在 <https://play.golang.org/p/JvnaQL9R7U5> 查看。

我们可以看到，使用指针接收者允许我们更改底层值，并且我们可以更改 `struct` 的一个字段或替换整个 `struct` 本身，如下面的代码所示：

```go
func (u *User) Birthday() {
    *u = User{Name: u.Name, Age: u.Age + 1}
    fmt.Println(u.Name, "turns", u.Age)
}
```

完整示例可在 <https://play.golang.org/p/3ugBEZqAood> 查看。

如果我们尝试更改指针的值而不是底层值，我们将编辑一个与在 `main` 中创建的对象无关的新对象，并且更改不会传播：

```go
func (u *User) Birthday() {
    u = &User{Name: u.Name, Age: u.Age + 1}
    fmt.Println(u.Name, "turns", u.Age)
}
```

完整示例可在 <https://play.golang.org/p/m8u2clKTqEU> 查看。

Go 中的某些类型会自动按引用传递。这是因为这些类型在内部被定义为包含指针的结构体。这创建了一个类型列表，连同它们的内部定义：

| **类型** | **内部定义** |
| --- | --- |
| `map` | ```go\nstruct {\n    m *internalHashtable\n}\n``` |
| `slice` | ```go\nstruct {\n    array *internalArray \n    len int\n    cap int\n}\n``` |
| `channel` | ```go\nstruct {\n    c *internalChannel\n}\n``` |

## 理解流程控制

为了控制应用程序的流程，Go 提供了不同的工具——一些语句如 `if`/`else`、`switch` 和 `for` 用于顺序场景，而其他如 `go` 和 `select` 用于并发场景。

## 条件

`if` 语句验证一个二元条件，并在条件为 `true` 时执行 `if` 块内的代码。当存在 `else` 块时，它在条件为 `false` 时执行。该语句还允许在条件前进行短声明，用 `;` 分隔。该条件可以与 `else if` 语句链式调用，如下面的代码所示：

```go
if r := a%10; r != 0 { // 带短声明的 if
    if r > 5 {         // 不带声明的 if 
        a -= r
    } else if r < 5 {  // else if 语句
        a += 10 - r 
    }
} else {               // else 语句
    a /= 10
}
```

另一个条件语句是 `switch`。它允许像 `if` 一样进行短声明，后面跟一个表达式。该表达式的值可以是任何类型（不仅仅是布尔值），并针对一系列 `case` 语句进行求值，每个 `case` 后面跟一个代码块。第一个与表达式匹配的语句（如果 `switch` 和 `case` 条件相等）将执行其代码块。

如果在代码块执行被中断的 `case` 中存在 `break` 语句，但存在 `fallthrough`，则会执行下一个 `case` 块中的代码。一个称为 `default` 的特殊 case 可用于在没有 case 满足时执行其代码，如下面的代码所示：





## Switch 语句

```go
switch tier {                        // switch 语句
case 1:                              // case 语句
    fmt.Println("T-shirt")
    if age < 18{
        break                        // 退出 switch 代码块
    }
    fallthrough                      // 执行下一个 case
case 2:
    fmt.Println("Mug")
    fallthrough                      // 执行下一个 case 
case 3:
    fmt.Println("Sticker pack")    
default:                             // 当没有 case 满足时执行
    fmt.Println("no reward")
}
```

# 循环

`for` 语句是 Go 中唯一的循环语句。这要求你指定三个表达式，用 `;` 分隔：

*   一个短声明或对现有变量的赋值
*   每次迭代前验证的条件
*   每次迭代结束时执行的操作

所有这些语句都是可选的，缺少条件意味着它始终为 `true`。`break` 语句中断循环的执行，而 `continue` 跳过当前迭代并继续下一次迭代：

```go
for {                    // 无限循环
    if condition {
        break            // 退出循环
    }
}

for i < 0 {              // 带条件的循环
    if condition {
        continue         // 跳过当前迭代并执行下一次    
    }
}

for i:=0; i < 10; i++ {  // 带声明、条件和操作的循环 
}
```

当 `switch` 和 `for` 的组合嵌套时，`continue` 和 `break` 语句指的是内层的流程控制语句。

可以使用 `name:` 表达式为外层循环或条件添加标签，其中 name 是其标识符，`break` 和 `continue` 都可以后跟该名称以指定干预位置，如下面的代码所示：

```go
label:
    for i := a; i<a+2; i++ {
        switch i%3 {
        case 0:
            fmt.Println("divisible by 3")
            break label                          // 这会中断外层的 for 循环
        default:
            fmt.Println("not divisible by 3")
        }
    }
```

# 探索内置函数

我们已经列出了一些用于初始化变量的内置函数，即 `make` 和 `new`。现在，让我们逐一了解每个函数的功能：

*   `func append(slice []Type, elems ...Type) []Type`：此函数将元素追加到切片的末尾。如果底层数组已满，它会在追加前将内容重新分配到一个更大的切片中。
*   `func cap(v Type) int`：返回数组的元素数量，如果参数是切片，则返回其底层数组的元素数量。
*   `func close(c chan<- Type)`：关闭一个通道。
*   `func complex(r, i FloatType) ComplexType`：给定两个浮点数，返回一个复数。
*   `func copy(dst, src []Type) int`：将元素从一个切片复制到另一个切片。
*   `func delete(m map[Type]Type1, key Type)`：从映射中删除一个条目。
*   `func imag(c ComplexType) FloatType`：返回复数的虚部。
*   `func len(v Type) int`：返回数组、切片、映射、字符串或通道的长度。
*   `func make(t Type, size ...IntegerType) Type`：创建一个新的切片、映射或通道。
*   `func new(Type) *Type`：返回一个指向指定类型变量的指针，该变量用零值初始化。
*   `func panic(v interface{})`：停止当前 `goroutine` 的执行，如果未被拦截，则停止整个程序。
*   `func print(args ...Type)`：将参数写入标准错误输出。
*   `func println(args ...Type)`：将参数写入标准错误输出，并在末尾添加一个换行符。
*   `func real(c ComplexType) FloatType`：返回复数的实部。
*   `func recover() interface{}`：停止 `panic` 序列并捕获 `panic` 值。

# Defer、Panic 和 Recover

一个非常重要的关键字隐藏了大量复杂性，但使得轻松执行许多操作成为可能，那就是 `defer`。它应用于函数、方法或闭包的执行，并使其前面的函数在函数返回前执行。一个常见且非常有用的用法是关闭资源。在成功打开资源后，一个延迟的 `close` 语句将确保无论从哪个退出点退出都会执行，如下面的代码所示：

```go
f, err := os.Open("config.txt")
if err != nil {
    return err
}
defer f.Close() // 无论如何都会被关闭

// 对 f 进行操作
```

在函数的生命周期内，所有延迟语句都被添加到一个列表中，在退出前，它们按逆序执行，从最后一个 `defer` 到第一个 `defer`。

即使发生 `panic`，这些语句也会执行，这就是为什么带有 `recover` 调用的延迟函数可用于拦截相应 `goroutine` 中的 `panic`，避免原本会终止应用程序的 `panic`。除了手动调用 `panic` 函数外，还有一组操作会导致 `panic`，包括以下内容：

*   访问负数或不存在的数组/切片索引（索引越界）
*   整数除以 `0`
*   向已关闭的通道发送数据
*   解引用 `nil` 指针
*   使用填满栈的递归函数调用（栈溢出）

`panic` 应用于不可恢复的错误，这也是为什么错误在 Go 中只是值的原因。恢复 `panic` 应该只是在退出应用程序前尝试对该错误做些处理。如果发生意外问题，是因为它没有被正确处理或缺少某些检查。这代表一个需要解决的严重问题，程序需要修改，这就是为什么它应该被拦截并忽略。

# 并发模型

并发是 Go 的核心，以至于它的两个基本工具只是关键字——`chan` 和 `go`。这是一种非常巧妙的方式，隐藏了一个设计良好且实现得当的并发模型的复杂性，使其易于使用和理解。

## 理解通道和 Goroutine

通道是为通信而设计的，这也是 Go 的座右铭所在：

> 不要通过共享内存来通信，而要通过通信来共享内存。

通道用于共享数据，它通常连接应用程序中的两个或多个执行线程，这使得在不担心数据安全的情况下发送和接收数据成为可能。Go 拥有由运行时而非操作系统管理的轻量级线程实现，让它们通信的最佳方式是使用通道。

创建一个新的 `goroutine` 非常简单——只需使用 `go` 操作符，后跟一个函数执行。这包括方法调用和闭包。如果函数有任何参数，它们将在例程启动前求值。一旦启动，如果不使用通道，外部作用域变量的更改将无法保证同步：

```go
a := myType{}
go doSomething(a)     // 函数调用
go func() {           // 闭包调用
    // ...
}()                   // 注意闭包被执行了
go a.someMethod()     // 方法调用
```

我们已经看过如何使用 `make` 函数创建新通道。如果通道是无缓冲的（`0` 容量），向通道发送数据是一个阻塞操作，将等待另一个 `goroutine` 从同一通道接收数据以解锁它。容量显示通道在发送下一个数据变为阻塞操作前能容纳多少条消息：

```go
unbuf := make(chan int)    // 无缓冲通道
buf := make(chan int, 3)   // 大小为 3 的通道
```

为了向通道发送数据，我们可以使用 `<-` 操作符。如果通道在操作符左侧，则为发送操作；如果在右侧，则为接收操作。从通道接收的值可以赋给一个变量，如下所示：




# 通道与并发

## 关闭通道

可以使用 `close()` 函数来关闭通道。此操作意味着不能再向通道发送值。这也是为什么这通常是发送者的责任。向已关闭的通道发送数据无论如何都会导致 `panic`，这就是为什么应该由接收者来完成。此外，当从通道接收时，可以在赋值中指定第二个布尔变量。如果通道仍然打开，该变量将为 true，以便接收者知道通道何时被关闭：

```go
var ch = make(chan int)
go func() {
    b, ok := <-ch        // channel open, ok is true
    b, ok = <-ch         // channel closed, ok is false
    b <- ch              // channel close, b will be a zero value
}()
ch <- 10                 // send to channel
close(ch)                // close the channel
```

## select 语句

有一个称为 `select` 的特殊控制语句，其工作方式完全类似于 `switch`，但仅用于通道操作：

```go
var ch1 = make(chan int)
var ch2 = make(chan int)
go func() { ch1 <- 10 }
go func() { <-ch2 }
switch {            // the first operation that completes is selected
case a := <-ch1:
    fmt.Println(a)
case ch2 <- 20:
    fmt.Println(b)    
}
```

# 理解内存管理

Go 具有垃圾回收机制；它以计算成本为代价管理自己的内存。编写高效的应用程序需要了解其内存模型和内部结构，以减少 `garbage collector`（垃圾回收器）的工作量并提高整体性能。

## 栈与堆

内存分为两个主要区域——`stack`（栈）和 `heap`（堆）。应用程序入口点函数（`main`）有一个栈，每个 goroutine 会创建额外的栈，这些栈存储在堆中。`stack`（栈）顾名思义，是一块随每次函数调用而增长、随函数返回而收缩的内存区域。`heap`（堆）由一系列动态分配的内存区域组成，它们的生命周期不像栈上的项目那样预先确定；堆空间可以随时分配和释放。

所有超出其定义所在函数生命周期的变量都存储在堆中，例如返回的指针。编译器使用一个称为 `escape analysis`（逃逸分析）的过程来检查哪些变量分配在堆上。可以使用 `go tool compile -m` 命令来验证这一点。

栈上的变量随函数的执行而创建和销毁。让我们通过一个实际例子来看看栈是如何工作的：

```go
func main() {
    var a, b = 0, 1
    f1(a,b)
    f2(a)
}

func f1(a, b int) {
    c := a + b
    f2(c)
}

func f2(c int) {
    print(c)
}
```

我们有 `main` 函数调用名为 `f1` 的函数，后者又调用另一个名为 `f2` 的函数。然后，同一个函数被 `main` 直接调用。

当 `main` 函数开始时，栈随正在使用的变量而增长。在内存中，这看起来类似于下表，其中每一列代表栈的伪状态，表示栈如何随时间变化，从左到右：

| 调用 `main` | 调用 `f1` | 调用 `f2` | `f2` 返回 | `f1` 返回 | 调用 `f2` | `f2` 返回 | `main` 返回 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `main()` | `main()` | `main()` | `main()` | `main()` | `main()` | `main()` | // 空 |
| `a = 0` | `a = 0` | `a = 0` | `a = 0` | `a = 0` | `a = 0` | `a = 0` |  |
| `b = 1` | `b = 1` | `b = 1` | `b = 1` | `b = 1` | `b = 1` | `b = 1` |  |
|  | `f1()` | `f1()` | `f1()` |  | `f2()` |  |  |
|  | `a = 0` | `a = 0` | `a = 0` |  | `c = 0` |  |  |
|  | `b = 1` | `b = 1` | `b = 1` |  |  |  |  |
|  | `c = 1` | `c = 1` | `c = 1` |  |  |  |  |
|  |  | `f2()` |  |  |  |  |  |
|  |  | `c = 1` |  |  |  |  |  |

当调用 `f1` 时，栈再次增长，通过在新部分复制 `a` 和 `b` 变量并添加新变量 `c`。`f2` 也是同样的情况。当 `f2` 返回时，栈通过移除函数及其变量而收缩，这也是 `f1` 结束时发生的情况。当直接调用 `f2` 时，它通过回收用于 `f1` 的相同内存部分而再次增长。

`garbage collector`（垃圾回收器）负责清理堆中未被引用的值，因此避免在其中存储数据是减少 `garbage collector`（`GC`）工作量的好方法，因为 GC 运行时会导致应用程序性能轻微下降。

## Go 中 GC 的历史

`GC` 负责释放堆中未被任何栈引用的区域。它最初是用 C 编写的，并具有 `stop the world`（停止世界）行为。程序会暂停一小段时间，释放内存，然后恢复执行。

Go 1.4 开始了将运行时（包括垃圾回收器）转换为 Go 语言的过程。将这些部分翻译为 Go 语言为更容易的优化奠定了基础，这种优化从 1.5 版本就已经开始，此时 `GC` 变得快得多，并能与其他 goroutine 并发运行。

从那时起，该过程经历了大量的优化和改进，成功将 `GC` 时间减少了几个数量级。

# 构建与编译程序

## 安装

在 Go 中，有不同的命令来构建包和应用程序。第一个是 `go install`，后跟路径或包名，它会在 `$GOPATH` 内的 `pkg` 目录中创建包的编译版本。

所有编译后的包按操作系统和架构组织，它们分别存储在 `$GOOS` 和 `$GOARCH` 环境变量中。这些设置可以通过 `go env` 命令查看，连同其他信息，如编译标志：

```bash
$ go env
GOARCH="amd64"
...
GOOS="linux"
GOPATH="/home/user/go"
...
GOROOT="/usr/lib/go-1.12"
...
```

对于当前的架构和操作系统，所有编译后的包将被放置在 `$GOOS_$GOARCH` 子目录中：

```bash
$ ls /home/user/go/pkg/
linux_amd64
```

如果包名为 `main` 且包含 `main` 函数，该命令将生成一个可执行二进制文件，存储在 `$GOPATH/bin` 中。如果包已经安装且源文件未更改，则不会再次编译，这将在首次编译后显著加快构建时间。

## 构建

也可以使用 `go build` 命令在特定位置构建二进制文件。可以使用 `-o` 标志定义特定的输出文件，否则它将在工作目录中构建，使用包名作为二进制文件名：

```bash
# building the current package in the working directory
$ go build . 

# building the current package in a specific location
$ go build . -o "/usr/bin/mybinary"
```

执行 `go build` 命令时，参数可以是以下之一：

*   相对路径的包（如当前包的 `go build .` 或 `go build ../name`）
*   绝对路径的包（`go build some/package`），将在 `$GOPATH` 中查找
*   特定的 Go 源文件（`go build main.go`）

后一种情况允许你构建位于 `$GOPATH` 之外的文件，并将忽略同一目录中的任何其他源文件。

## 运行

还有一个第三个命令，其工作方式类似于 build，但也会运行二进制文件。它使用 `build` 命令以临时目录作为输出来创建二进制文件，并即时执行该二进制文件：

```bash
$ go run main.go
main output line 1
main output line 2

$
```

当你对源代码进行更改时，可以使用 run 代替 build 或 install。如果代码相同，最好构建一次并执行多次。

# 总结



本章回顾了 Go 语言的历史及其当前的优缺点。在通过包系统和导入机制理解命名空间后，我们探讨了其类型系统，包括基本类型、复合类型和用户定义类型。

我们重点关注了变量，包括它们的声明和初始化、类型间允许的操作、如何将变量转换为其他类型，以及如何查看接口的底层类型。我们了解了作用域和遮蔽的工作原理，以及常量与变量的区别。随后我们深入研究了函数——作为一等类型，每个签名代表不同的类型。我们理解了方法本质上是伪装的函数，并附加到类型上，使自定义类型能够满足接口。

此外，我们学习了如何使用 `if`、`for` 和 `switch` 等语句控制应用流程。我们分析了各种控制语句与循环语句的区别，并查看了每个内置函数的功能。然后，我们了解了通道和 Goroutine 的基本并发工作原理。最后，我们初步了解了 Go 内部内存分配的工作原理、垃圾收集器的历史与性能，以及如何构建、安装和运行 Go 二进制文件。

在下一章中，我们将通过与文件系统交互，把这些知识付诸实践。

# 问题

1.  导出符号和非导出符号有什么区别？

2.  为什么自定义类型很重要？

3.  短声明的主要限制是什么？

4.  什么是作用域？它如何影响变量遮蔽？

5.  如何访问方法？

6.  解释一系列 `if`/`else` 与 `switch` 的区别。

7.  在典型用例中，通常由谁负责关闭通道？

8.  什么是逃逸分析？
