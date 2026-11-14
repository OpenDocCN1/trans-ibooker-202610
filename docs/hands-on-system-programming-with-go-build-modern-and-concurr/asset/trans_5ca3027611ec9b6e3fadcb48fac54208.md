
然后，我们深入标准库包以查看 context 已在何处被使用。这包括请求的 HTTP 功能，其中可将其用于值传递、取消和超时，以及服务器关闭操作。我们还了解了 TCP 包如何允许我们通过实际示例以类似方式使用它，并列出了 database 包中允许我们使用 context 取消操作的函数。

在使用 context 构建我们自己的功能之前，我们探讨了一些应避免的用法，从使用错误类型的键到使用 context 传递本应位于函数或方法签名中的值。接着，我们利用前三章学到的并发知识，创建了一个搜索文件和内容的函数。

下一章将通过展示最常见的 Go 并发模式及其用法，结束本书的并发部分。这将使我们能够将迄今为止所学的所有并发知识组合成一些非常常见且有效的配置。

## 问题

1. Go 中什么是 context？
2. 取消、截止时间和超时有什么区别？
3. 使用 context 传递值的最佳实践是什么？
4. 哪些标准库包已经使用了 context？

# 实现并发模式

本章将介绍并发模式以及如何使用它们构建健壮的系统应用。我们已经了解了并发涉及的所有工具（goroutine 和 channel、`sync` 和 `atomic` 以及 context），现在我们将探讨一些将它们组合成模式的常见方式，以便在程序中使用。

本章将涵盖以下主题：

* 从生成器开始
* 使用管道进行排序
* 多路复用与解复用
* 其他模式
* 资源泄漏

## 技术要求

本章要求安装 Go 并设置好你喜欢的编辑器。更多信息请参阅 [第 3 章](#602a92d5-25f7-46b8-83d4-10c6af1c6750.xhtml) *Go 概览*。

## 从生成器开始

生成器是一种函数，每次调用时返回序列的下一个值。使用生成器最大的优势是序列新值的惰性创建。在 Go 中，这可以用接口或 channel 来表示。生成器配合 channel 使用的优势之一是它们在单独的 goroutine 中并发生成值，使主 goroutine 能够对这些值执行其他类型的操作。

它可以用一个非常简单的接口来抽象：

```go
type Generator interface {
    Next() interface{}
}

type GenInt64 interface {
    Next() int64
}
```

接口的返回类型将取决于用例，在我们的例子中是 `int64`。其基本实现可以是一个简单的计数器：

```go
type genInt64 int64

func (g *genInt64) Next() int64 {
    *g++
    return int64(*g)
}
```

此实现不是线程安全的，因此如果我们尝试配合 goroutine 使用它，可能会在过程中丢失一些元素：

```go
func main() {
    var g genInt64
    for i := 0; i < 1000; i++ {
        go func(i int) {
            fmt.Println(i, g.Next())
        }(i)
    }
    time.Sleep(time.Second)
}
```

使生成器并发安全的一个简单方法是在整数上执行原子操作。

这将使并发生成器变得线程安全，且只需极少的代码更改：

```go
type genInt64 int64

func (g *genInt64) Next() int64 {
    return atomic.AddInt64((*int64)(g), 1)
}
```

这将避免应用中的竞态条件。不过，还有另一种可能的实现，但这需要使用 channel。思路是在 goroutine 中生成值，然后通过共享 channel 传递给 `Next` 方法，如下例所示：

```go
type genInt64 struct {
    ch chan int64
}

func (g genInt64) Next() int64 {
    return <-g.ch
}

func NewGenInt64() genInt64 {
    g := genInt64{ch: make(chan int64)}
    go func() {
        for i := int64(0); ; i++ {
            g.ch <- i
        }
    }()
    return g
}
```

循环将永远进行下去，当生成器用户停止调用 `Next` 方法请求新值时，发送操作将阻塞。

之所以这样构建代码，是因为我们试图实现开头定义的接口。我们也可以直接返回一个 channel 并用于接收：

```go
func GenInt64() <-chan int64 {
    ch := make(chan int64)
    go func() {
        for i := int64(0); ; i++ {
            ch <- i
        }
    }()
    return ch
}
```

直接使用 channel 的主要优势是可以将其包含在 `select` 语句中，以便在不同的 channel 操作之间进行选择。以下展示了在两个不同生成器之间的 `select`：

```go
func main() {
    ch1, ch2 := GenInt64(), GenInt64()
    for i := 0; i < 20; i++ {
        select {
        case v := <-ch1:
            fmt.Println("ch 1", v)
        case v := <-ch2:
            fmt.Println("ch 2", v)
        }
    }
}
```

### 避免泄漏

允许循环结束以避免 goroutine 和资源泄漏是个好主意。其中一些问题如下：

* 当 goroutine 挂起而不返回时，内存空间仍被占用，增加应用的内存占用。只有当 goroutine 返回或 panic 时，goroutine 及其在栈上定义的变量才会被 GC 回收。
* 如果文件保持打开状态，可能会阻止其他进程对其执行操作。如果打开的文件数量达到操作系统施加的限制，进程将无法打开其他文件（或接受网络连接）。

解决此问题的一个简单方法是始终使用 `context.Context`，以便为 goroutine 提供一个明确的退出点：

```go
func NewGenInt64(ctx context.Context) genInt64 {
    g := genInt64{ch: make(chan int64)}
    go func() {
        for i := int64(0); ; i++ {
            select {
            case g.ch <- i:
                // 什么都不做
            case <-ctx.Done():
                close(g.ch)
                return
            }
        }
    }()
    return g
}
```

这可用于生成值直到不再需要它们，并在不需要新值时取消 context。相同的模式也可应用于返回 channel 的版本。例如，我们可以直接使用 `cancel` 函数或在 context 上设置超时：

```go
func main() {
    ctx, cancel := context.WithTimeout(context.Background(), time.Millisecond*100)
    defer cancel()
    g := NewGenInt64(ctx)
    for i := range g.ch {
        go func(i int64) {
            fmt.Println(i, g.Next())
        }(i)
    }
    time.Sleep(time.Second)
}
```

生成器将生成数字，直到提供的 context 过期。此时，生成器将关闭 channel。

## 使用管道进行排序

管道是一种构建应用流程的方式，通过将主执行流拆分为多个阶段来实现，这些阶段可以使用特定的通信手段相互交谈。这可以是以下任一种：

* 外部的，如网络连接或文件
* 应用内部的，如 Go 的 channel

第一个阶段通常称为生产者，而最后一个阶段通常称为消费者。

Go 提供的并发工具集允许我们高效地使用多个 CPU，并通过阻塞输入或输出操作来优化其使用。Channel 特别适合作为内部管道通信的完美工具。它们可以用接收入站 channel 并返回出站 channel 的函数来表示。基本结构看起来像这样：
