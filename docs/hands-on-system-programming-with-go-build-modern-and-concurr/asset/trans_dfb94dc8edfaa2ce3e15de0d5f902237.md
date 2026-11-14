

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

