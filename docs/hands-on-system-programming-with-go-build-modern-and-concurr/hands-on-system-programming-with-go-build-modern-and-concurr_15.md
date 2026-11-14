## 处理通道和 Goroutine

本章将涵盖使用 Go 基本内置功能——通道和 Goroutine 进行并发编程。并发描述了在同一时间段内执行应用程序不同部分的能力。

使软件并发化可以成为构建系统应用程序的强大工具，因为某些操作可以在其他操作尚未结束时启动。

本章将涵盖以下主题：

*   理解 Goroutine
*   探索通道
*   优势用法

### 技术要求

本章要求安装 Go 并设置好你喜欢的编辑器。更多信息请参阅 第 3 章，《Go 概览》。

### 理解 Goroutine

Go 是一门以并发为中心的语言，以至于两大主要特性——通道和 Goroutine——都是内置包的一部分。我们现在将看到它们如何工作及其基本功能，从 Goroutine 开始，它使应用程序的部分能够并发执行。

#### 对比线程和 Goroutine

Goroutine 是用于并发的原语之一，但它们与线程有何不同？让我们在这里阅读关于它们各自的内容。

##### 线程

当前操作系统专为现代架构构建，这些架构拥有每 CPU 多核的处理器，或使用超线程等技术，允许单核支持多个线程。线程是可由操作系统调度器管理的进程部分，调度器可将它们分配给特定核心/CPU。像进程一样，线程携带关于应用程序执行的信息，但此类信息的大小小于进程。这包括程序中的当前指令、当前执行的栈以及所需的变量。

操作系统已负责进程间的上下文切换；它保存旧进程信息并加载新进程信息。这称为**进程上下文切换**，这是一项代价高昂的操作，甚至比进程执行更昂贵。

为了从一个线程跳转到另一个线程，可以在线程间执行相同操作。这称为**线程上下文切换**，这也是一项繁重的操作——尽管不如进程切换那么沉重——因为线程携带的信息比进程少。

##### Goroutine

线程在内存中有最小尺寸；通常为 MB 量级（Linux 为 2 MB）。最小尺寸对应用程序创建新线程设定了一些限制——如果每个线程至少几 MB，1000 个线程将占用至少几 GB 内存。Go 通过使用一种类似于线程但由语言运行时而非操作系统处理的构造来解决这些问题。Goroutine 在内存中的大小小三个数量级（每个 Goroutine 2 KB），意味着 1000 个 Goroutine 的最小内存用量与单个线程的内存用量相当。

这是通过定义 Goroutine 内部保留的数据来实现的，使用称为 `g` 的数据结构来描述 Goroutine 信息，如栈和状态。这是 `runtime` 包中未导出的数据类型，可在 Go 源代码中找到。Go 使用同一包中的另一个数据结构 `m` 来跟踪操作系统。为执行 Goroutine 而获取的逻辑处理器存储在 `p` 结构中。这可在 Go `runtime` 包文档中验证：

*   `type g`：[golang.org/pkg/runtime/?m=all#g](https://golang.org/pkg/runtime/?m=all#g)
*   `type m`：[golang.org/pkg/runtime/?m=all#m](https://golang.org/pkg/runtime/?m=all#m)
*   `type p`：[golang.org/pkg/runtime/?m=all#p](https://golang.org/pkg/runtime/?m=all#p)



这三个实体的交互方式如下——对于每个 goroutine，都会创建一个新的 `g`，`g` 被放入 `p` 的队列中，每个 `p` 尝试获取 `m` 来执行来自 `g` 的代码。有些操作会阻塞执行，例如：

*   内置同步（channel 和 `sync` 包）
*   阻塞的系统调用，例如文件操作
*   网络操作

当发生这类操作时，运行时会将 `p` 从 `m` 上分离，并使用（或创建，如果尚不存在）另一个专用的 `m` 来执行阻塞操作。线程在执行完此类操作后变为空闲状态。

# 新建 Goroutine

Goroutine 是 Go 如何在简单接口背后隐藏复杂性的最佳范例之一。编写应用程序以启动 goroutine 时，只需在函数前加上 `go` 关键字并执行即可：

```
func main() {
    go fmt.Println("Hello, playground")
}
```

完整示例请访问 [`play.golang.org/p/3gPGZkJtJYv`](https://play.golang.org/p/3gPGZkJtJYv)。

如果我们运行上述示例的应用程序，会发现它没有产生任何输出。为什么？在 Go 中，当主 goroutine 结束时，应用程序就会终止，看起来就是这种情况。实际发生的是 `go` 语句创建了带有相应 `runtime.g` 的 goroutine，但这必须由 Go 调度器拾取，而由于程序在 goroutine 实例化后立即终止，所以这并没有发生。

使用 `time.Sleep()` 函数让主 goroutine 等待（即使只是一纳秒！）就足以让调度器拾取 goroutine 并执行其代码。如下代码所示：

```
func main() {
    go fmt.Println("Hello, playground")
    time.Sleep(time.Nanosecond)
}
```

完整示例请访问 [`play.golang.org/p/2u125pTclv6`](https://play.golang.org/p/2u125pTclv6)。

我们已经看到 Go 方法也算作函数，这就是为什么它们可以像普通函数一样通过 `go` 语句并发执行：

```
type a struct{}

func (a) Method() { fmt.Println("Hello, playground") }

func main() {
    go a{}.Method()
    time.Sleep(time.Nanosecond)
}
```

完整示例请访问 [`play.golang.org/p/RUhgfRAPa2b`](https://play.golang.org/p/RUhgfRAPa2b)。

闭包是匿名函数，因此也可以使用，这实际上是一种非常常见的做法：

```
func main() {
    go func() {
        fmt.Println("Hello, playground")
    }()
    time.Sleep(time.Nanosecond)
}
```

完整示例请访问 [`play.golang.org/p/a-JvOVwAwUV`](https://play.golang.org/p/a-JvOVwAwUV)。

## 多个 Goroutine

将代码组织在多个 goroutine 中有助于在处理器之间分配工作，还有许多其他优势，我们将在后续章节中看到。由于它们非常轻量，我们可以非常容易地使用循环创建大量 goroutine：

```
func main() {
    for i := 0; i < 10; i++ {
        go fmt.Println(i)
    }
    time.Sleep(time.Nanosecond)
}
```

完整示例请访问 [`play.golang.org/p/Jaljd1padeX`](https://play.golang.org/p/Jaljd1padeX)。

此示例使用并发 goroutine 并行打印从 `0` 到 `9` 的数字列表，而不是在单个 goroutine 中按顺序执行相同操作。

## 参数求值

如果我们稍微修改此示例，使用不带参数的闭包，将会看到截然不同的结果：

```
func main() {
    for i := 0; i < 10; i++ {
         go func() { fmt.Println(i) }()
    }
    time.Sleep(time.Nanosecond)
}
```

完整示例请访问 [`play.golang.org/p/RV54AsYY-2y`](https://play.golang.org/p/RV54AsYY-2y)。

如果我们运行此程序，可以看到 Go 编译器在循环中发出警告：`loop variable i captured by func literal`（循环变量 i 被函数字面量捕获）。

循环中的变量在我们定义的函数中被引用——goroutine 的创建循环比 goroutine 的执行更快，结果是循环在单个 goroutine 启动之前就已结束，导致打印出最后一次迭代后循环变量的值。

为了避免捕获循环变量的错误，最好将同一变量作为参数传递给闭包。goroutine 函数的参数在创建时求值，这意味着对该变量的更改不会反映在 goroutine 内部，除非你传递的是指向值的引用，例如指针、map、slice、channel 或函数。我们可以通过运行以下示例来观察这种差异：

```
func main() {
    var a int
    // 传值
    go func(v int) { fmt.Println(v) }(a)
  
    // 传指针
    go func(v *int) { fmt.Println(*v) }(&a)
  
    a = 42
    time.Sleep(time.Nanosecond)
}
```

完整示例请访问 [`play.golang.org/p/r1dtBiTUMaw`](https://play.golang.org/p/r1dtBiTUMaw)。

传值不受程序最后赋值的影响，而传递指针类型意味着对指针内容所做的更改将被 goroutine 看到。

## 同步

Goroutine 允许代码并发执行，但值之间的同步并非开箱即用。我们可以通过以下示例查看尝试并发使用变量时会发生什么：

```
func main() {
    var i int
    go func(i *int) {
        for j := 0; j < 20; j++ {
            time.Sleep(time.Millisecond)
            fmt.Println(*i, j)
        }
    }(&i)
    for i = 0; i < 20; i++ {
        time.Sleep(time.Millisecond)
        fmt.Println(i)
    }
}
```

我们有一个在主例程中变化的整数变量——每次操作之间暂停一毫秒——更改后，打印该值。

在另一个 goroutine 中，有一个类似的循环（使用另一个变量）和另一个 `print` 语句来比较这两个值。考虑到暂停时间相同，我们期望看到相同的值，但事实并非如此。我们看到有时这两个 goroutine 是不同步的。

更改不会立即反映出来，因为内存不是即时同步的。我们将在下一章学习如何确保数据同步。

# 探索 Channel

Channel 是 Go 语言及其他少数编程语言特有的概念。Channel 是非常强大的工具，允许用一种简单的方法同步不同的 goroutine，这是解决上一个示例中提出的问题的方法之一。

## 属性和操作

Channel 是 Go 中的内置类型，像数组、slice 和 map 一样是有类型的。它以 `chan type` 的形式表示，并由 `make()` 函数初始化。

## 容量和大小

除了流经 channel 的类型之外，channel 还有另一个属性：其 `capacity`（容量）。这表示在发送新项目的尝试导致阻塞操作之前，channel 可以容纳的项目数量。channel 的容量在创建时决定，其默认值为 `0`：

```
// 隐式零容量的 channel
var a = make(chan int)

// 显式零容量的 channel
var a = make(chan int, 0)

// 显式指定容量的 channel
var a = make(chan int, 10)
```

channel 的容量创建后无法更改，可随时使用内置 `cap()` 函数读取：

```
func main() {
    var (
        a = make(chan int, 0)
        b = make(chan int, 5)
    )

    fmt.Println("a is", cap(a))
    fmt.Println("b is", cap(b))
}
```

完整示例请访问 [`play.golang.org/p/Yhz4bTxm5L8`](https://play.golang.org/p/Yhz4bTxm5L8)。




# `len` 函数用于通道

当 `len` 函数用于通道时，它会告诉我们通道中持有的元素数量：

```
func main() {
    var (
        a = make(chan int, 5)
    )
    for i := 0; i < 5; i++ {
        a <- i
        fmt.Println("a is", len(a), "/", cap(a))
    }
}
```

完整示例请访问 [`play.golang.org/p/zJCL5VGmMsC`](https://play.golang.org/p/zJCL5VGmMsC)。

从前面的示例中，我们可以看到通道容量保持为 `5`，而长度随每个元素的增加而增长。

# 阻塞操作

如果通道已满或其容量为 `0`，则操作将会阻塞。如果我们采用上一个示例，填满通道并尝试执行另一个发送操作，我们的应用程序就会卡住：

```
func main() {
    var (
        a = make(chan int, 5)
    )
    for i := 0; i < 5; i++ {
        a <- i
        fmt.Println("a is", len(a), "/", cap(a))
    }
    a <- 0 // 阻塞
}
```

完整示例请访问 [`play.golang.org/p/uSfm5zWN8-x`](https://play.golang.org/p/uSfm5zWN8-x)。

当所有 goroutine 都被锁定时（在该特定情况下，我们只有 `main` goroutine），Go 运行时会引发死锁——一个终止应用程序执行的致命错误：

```
fatal error: all goroutines are asleep - deadlock!
```

这可能发生在接收或发送操作上，它是应用程序设计错误的症状。让我们看看下面的例子：

```
func main() {
    var a = make(chan int)
    a <- 10
    fmt.Println(<-a)
}
```

在前面的示例中，既有 `a <- 10` 发送操作，也有匹配的 `<-a` 接收操作，但仍然会导致死锁。然而，我们创建的通道没有容量，所以第一个发送操作会阻塞。我们可以通过两种方式来干预：

* **增加容量**：这是一个相当简单的解决方案，涉及使用 `make(chan int, 1)` 初始化通道。仅当接收者的数量预先已知时，它效果最好；如果接收者数量高于容量，则问题会再次出现。

* **使操作并发**：这是一种更好的方法，因为它将通道用于其设计初衷——并发。

让我们尝试使用第二种方法使前面的示例正常工作：

```
func main() {
    var a = make(chan int)
    go func() {
        a <- 10
    }()
    fmt.Println(<-a)
}
```

现在，我们可以看到这里没有死锁，程序正确打印了值。使用容量方法也能使其工作，但这仅适用于我们发送单条消息的情况，而另一种方法将允许我们通过通道发送任意数量的消息，并从另一端相应地接收它们：

```
func main() {
    const max = 10
    var a = make(chan int)

    go func() {
        for i := 0; i < max; i++ {
            a <- i
        }
    }()
    for i := 0; i < max; i++ {
        fmt.Println(<-a)
    }
}
```

完整示例请访问 [`play.golang.org/p/RKcojupCruB`](https://play.golang.org/p/RKcojupCruB)。

我们现在有一个常量来存储执行的操作数，但有一种更好、更地道的方法来让接收者知道何时没有更多消息。我们将在下一章关于同步的内容中介绍这一点。

# 关闭通道

处理发送者和接收者之间同步结束的最佳方式是 `close` 操作。该函数通常由发送者执行，因为接收者可以在每次获取值时使用第二个变量来验证通道是否仍然打开：

```
value, ok := <-ch
```

第二个接收变量是一个布尔值，如果通道仍然打开则为 `true`，否则为 `false`。当在一个已 `close` 的通道上执行接收操作时，第二个接收变量将具有 `false` 值，第一个变量将具有通道类型的 `0` 值，例如：

* 数字为 `0`
* 布尔值为 `false`
* 字符串为 `""`
* 切片、映射或指针为 `nil`

发送多条消息的示例可以使用 `close` 函数重写，而无需预先知道将发送多少条消息：

```
func main() {
    const max = 10
    var a = make(chan int)

    go func() {
        for i := 0; i < max; i++ {
            a <- i
        }
        close(a)
    }()
    for {
        v, ok := <-a
        if !ok {
            break
        }
        fmt.Println(v)
    }
}
```

完整示例请访问 [`play.golang.org/p/GUzgG4kf5ta`](https://play.golang.org/p/GUzgG4kf5ta)。

还有一种更简洁优雅的方式来接收通道中的消息直到其关闭：使用我们用于迭代映射、数组和切片的同一个关键字。这是通过 `range` 完成的：

```
for v := range a {
    fmt.Println(v)
}
```

# 单向通道

处理通道变量的另一种可能性是指定它们仅用于发送或仅用于接收数据。这由 `<-` 箭头指示，如果仅用于接收，则箭头位于 `chan` 之前；如果仅用于发送，则位于 `chan` 之后：

```
func main() {
    var a = make(chan int)
    s, r := (chan<- int)(a), (<-chan int)(a)
    fmt.Printf("%T - %T", s, r)
}
```

完整示例请访问 [`play.golang.org/p/ZgEPZ99PLJv`](https://play.golang.org/p/ZgEPZ99PLJv)。

通道本身就是指针，因此将其中一个转换为其仅发送或仅接收版本将返回同一个通道，但会减少可以在其上执行的操作数量。通道的类型如下：

* 仅发送通道，`chan<-`，允许你发送项目、关闭通道，并阻止你接收数据（编译错误）。
* 仅接收通道，`<-chan`，允许你接收数据，任何发送或关闭操作都将是编译错误。

当函数参数是仅发送/仅接收通道时，转换是隐式的，这是一个很好的实践，因为它可以防止错误，例如从接收者关闭通道。我们可以采用另一个示例，并通过一些重构来利用单向通道。

我们还可以创建一个使用仅发送通道发送值的函数：

```
func send(ch chan<- int, max int) {
    for i := 0; i < max; i++ {
        ch <- i
    }
    close(ch)
}
```

对接收做同样的事情，使用仅接收通道：

```
func receive(ch <-chan int) {
    for v := range ch {
        fmt.Println(v)
    }
}
```

然后，使用它们，同一个通道将自动转换为单向版本：

```
func main() {
    var a = make(chan int)

    go send(a, 10)
  
    receive(a)
}
```

完整示例请访问 [`play.golang.org/p/pPuqpfnq8jJ`](https://play.golang.org/p/pPuqpfnq8jJ)。

# 等待接收者

我们在上一节中看到的大多数示例都是在 goroutine 中完成发送操作，在 `main` goroutine 中完成接收操作。也可能所有操作都由 goroutine 处理，那么我们如何将 `main` goroutine 与其他 goroutine 同步呢？

一种典型的技术是使用另一个通道，其唯一目的是发出 goroutine 已完成工作的信号。接收 goroutine 通过通信通道的关闭知道没有更多消息可获取，并在完成操作后关闭另一个与 `main` goroutine 共享的通道。`main` 函数可以在退出前等待该通道的关闭。

用于此目的的典型通道不携带任何额外信息，只表示它是打开还是关闭，因此通常是 `chan struct{}` 通道。这是因为空数据结构在内存中没有大小。我们可以通过对前面的示例进行一些更改来看到此模式的实际应用，从接收者函数开始：





# 特殊值

在某些情况下，通道的行为会有所不同。我们现在将了解当通道被设置为其零值——`nil`——或者当它已经关闭时会发生什么。

## nil 通道

我们之前讨论过通道在 Go 中属于指针类型，因此它们的默认值是 `nil`。但是当你向 `nil` 通道发送或从中接收时会发生什么？

如果我们创建一个非常简单的应用程序尝试向空通道发送数据，会发生死锁：

```go
func main() {
    var a chan int
    a <- 1
}
```

完整示例可在 [`play.golang.org/p/KHJ4rvxh7TM`](https://play.golang.org/p/KHJ4rvxh7TM) 查看。

如果我们对接收操作做同样的事情，会得到相同的死锁结果：

```go
func main() {
    var a chan int
    <-a
}
```

完整示例可在 [`play.golang.org/p/gIjhy7aMxiR`](https://play.golang.org/p/gIjhy7aMxiR) 查看。

最后要检查的是 `close` 函数对 `nil` 通道的行为。它会因 `close of nil channel` 这一明确值而 panic：

```go
func main() {
    var a chan int
    close(a)
}
```

完整示例可在 [`play.golang.org/p/5RjdcYUHLSL`](https://play.golang.org/p/5RjdcYUHLSL) 查看。

总结一下，我们已经看到 `nil` 通道的发送和接收是阻塞操作，而 `close` 会导致 panic。

## 关闭的通道

我们已经知道，从已关闭的通道接收会返回通道类型的零值，第二个布尔值为 `false`。但如果我们尝试在关闭通道后发送数据会发生什么？让我们通过以下代码来找出答案：

```go
func main() {
    a := make(chan int)
    close(a)
    a <- 1
}
```

完整示例可在 [`play.golang.org/p/_l_xZt1ZojT`](https://play.golang.org/p/_l_xZt1ZojT) 查看。

如果我们尝试在关闭后发送数据，它会返回一个非常具体的 panic：`send on closed channel`。当我们尝试关闭一个已经关闭的通道时，会发生类似的情况：

```go
func main() {
    a := make(chan int)
    close(a)
    close(a)
}
```

完整示例可在 [`play.golang.org/p/GHK7ERt1XQf`](https://play.golang.org/p/GHK7ERt1XQf) 查看。

此示例将导致一个带有特定值的 panic——`close of closed channel`。

# 管理多个操作

在许多情况下，不止一个 goroutine 正在执行其代码并通过通道进行通信。一个典型场景是等待其中一个通道的发送或接收操作被执行。

当你操作多个通道时，Go 允许使用一个特殊关键字，它执行类似于 `switch` 但针对通道操作的逻辑。这是通过 `select` 语句完成的，后跟一系列 `case` 语句和一个可选的 `default` 分支。

我们可以看一个快速示例，其中我们在一个 goroutine 中从通道接收值，在另一个 goroutine 中向不同的通道发送值。在主 goroutine 中，我们使用 `select` 语句与这两个通道交互，从第一个接收，向第二个发送：

```go
func main() {
    ch1, ch2 := make(chan int), make(chan int)
    a, b := 2, 10
    go func() { <-ch1 }()
    go func() { ch2 <- a }()
    select {
    case ch1 <- b:
        fmt.Println("ch1 got a", b)
    case v := <-ch2:
        fmt.Println("ch2 got a", v)
    }
}
```

完整示例可在 [`play.golang.org/p/_8P1Edxe3o4`](https://play.golang.org/p/_8P1Edxe3o4) 查看。

在 playground 中运行此程序时，我们可以看到从第二个通道的接收操作总是最先完成。如果我们交换 goroutine 的执行顺序，会得到相反的结果。最后执行的操作会被最先选中。之所以发生这种情况，是因为 playground 是一个 Web 服务，它在安全环境中运行和执行 Go 代码，并进行一些优化使此操作具有确定性。

## Default 分支

如果我们在前面的示例中添加一个 default 分支，应用程序执行的结果将截然不同，特别是如果我们更改 `select` 的顺序：

```go
select {
case v := <-ch2:
    fmt.Println("ch2 got a", v)
case ch1 <- b:
    fmt.Println("ch1 got a", b)
default:
    fmt.Println("too slow")
}
```

完整示例可在 [`play.golang.org/p/F1aE7ImBNFk`](https://play.golang.org/p/F1aE7ImBNFk) 查看。

`select` 语句将总是选择 `default` 语句。之所以发生这种情况，是因为当 `select` 语句执行时，goroutine 尚未被调度器选中。如果我们在 `select` 切换之前添加一个非常小的暂停（使用 `time.Sleep`），调度器将至少选中一个 goroutine，然后我们将执行两个操作中的一个：

```go
func main() {
    ch1, ch2 := make(chan int), make(chan int)
    a, b := 2, 10
    for i := 0; i < 10; i++ {
        go func() { <-ch1 }()
        go func() { ch2 <- a }()
        time.Sleep(time.Nanosecond)
        select {
        case ch1 <- b:
            fmt.Println("ch1 got a", b)
        case v := <-ch2:
            fmt.Println("ch2 got a", v)
        default:
            fmt.Println("too slow")
        }
    }
}
```

完整示例可在 [`play.golang.org/p/-aXc3FN6qDj`](https://play.golang.org/p/-aXc3FN6qDj) 查看。

在这种情况下，我们将得到一组混合的已执行操作，具体取决于哪一个被 Go 调度器选中。

# 定时器和计时器

`time` 包提供了一些工具，使编排 goroutine 和通道成为可能——定时器和计时器。

## 定时器

可以替代 `select` 语句中 `default` 分支的工具是 `time.Timer` 类型。它包含一个只接收通道，该通道将在其构造期间使用 `time.NewTimer` 指定的持续时间后返回一个 `time.Time` 值：

```go
func main() {
    ch1, ch2 := make(chan int), make(chan int)
    a, b := 2, 10
    go func() { <-ch1 }()
    go func() { ch2 <- a }()
    t := time.NewTimer(time.Nanosecond)
    select {
    case ch1 <- b:
        fmt.Println("ch1 got a", b)
    case v := <-ch2:
        fmt.Println("ch2 got a", v)
    case <-t.C:
        fmt.Println("too slow")
    }
}
```

完整示例可在 [`play.golang.org/p/vCAff1kI4yA`](https://play.golang.org/p/vCAff1kI4yA) 查看。

定时器暴露一个只读通道，因此无法关闭它。当使用 `time.NewTimer` 创建时，它会等待指定的持续时间，然后在通道中触发一个值。

`Timer.Stop` 方法将尝试避免通过通道发送数据，并返回是否成功。如果尝试停止定时器后返回 `false`，我们仍然需要从通道接收该值，然后才能再次使用该通道。

`Timer.Reset` 使用给定的持续时间重启定时器，并返回一个布尔值，这与 `Stop` 类似。此值为 `true` 或 `false`：

* `true` 表示定时器处于活动状态
* `false` 表示定时器已触发或已停止

我们将通过一个实际示例来测试这些功能：

```go
t := time.NewTimer(time.Millisecond)
time.Sleep(time.Millisecond / 2)
if !t.Stop() {
    panic("it should not fire")
}
select {
case <-t.C:
    panic("not fired")
default:
    fmt.Println("not fired")
}
```





# AfterFunc

一个非常有用的使用 `time.Timer` 的工具是 `time.AfterFunc` 函数，它返回一个定时器，当定时器触发时，会在其自己的 goroutine 中执行传入的函数：

```
func main() {
    time.AfterFunc(time.Millisecond, func() {
        fmt.Println("Hello 1!")
    })
    t := time.AfterFunc(time.Millisecond*5, func() {
        fmt.Println("Hello 2!")
    })
    if !t.Stop() {
        panic("should not fire")
    }
    time.Sleep(time.Millisecond * 10)
}
```

完整示例请访问 <https://play.golang.org/p/77HIIdlRlZ1>。

在前面的示例中，我们为两个不同的闭包定义了两个定时器，并停止了其中一个，让另一个触发。

# Tickers

`time.Ticker` 与 `time.Timer` 类似，但其通道会以等于持续时间的固定间隔传递更多元素。它们是在使用 `time.NewTicker` 创建时指定的。这使得可以使用 `Ticker.Stop` 方法停止定时器的触发：

```
func main() {
    tick := time.NewTicker(time.Millisecond)
    stop := time.NewTimer(time.Millisecond * 10)
    for {
        select {
        case a := <-tick.C:
            fmt.Println(a)
        case <-stop.C:
            tick.Stop()
        case <-time.After(time.Millisecond):
            return
        }
    }
}
```

完整示例请访问 <https://play.golang.org/p/8w8I7zIGe-_j>。

在本示例中，我们还使用了 `time.After`——一个返回匿名 `time.Timer` 通道的函数。当不需要停止定时器时可以使用它。还有一个函数 `time.Tick`，它返回匿名 `time.Ticker` 的通道。这两个函数都将返回一个应用程序无法控制的通道，该通道最终将被垃圾回收器回收。

这结束了通道的概述，从它们的属性和基本用法到一些更高级的并发示例。我们还检查了一些特殊情况以及如何同步多个通道。

# Combining channels and goroutines

现在我们已经了解了 Go 并发的基本工具和属性，可以利用它们为我们的应用程序构建更好的工具。我们将看到一些利用通道和 goroutine 解决实际问题的示例。

# Rate limiter

一个典型场景是拥有一个 Web API，它在一定时间内对调用次数有特定限制。如果超过此阈值，此类 API 会暂时禁止使用，使其在一段时间内不可用。在为该 API 创建客户端时，我们需要意识到这一点，并确保我们的应用程序不会过度使用它。

这是一个非常适合使用 `time.Ticker` 来定义调用间隔的场景。在本示例中，我们将为 Google Maps 地理编码服务创建一个客户端，该服务每 24 小时限制 100,000 次请求。首先让我们定义客户端：

```
type Client struct {
    client *http.Client
    tick   *time.Ticker
}
```

客户端由一个用于调用 maps 的 HTTP 客户端、一个有助于防止超过速率限制的定时器以及一个用于服务身份验证的 API 密钥组成。我们可以为我们的用例定义一个自定义的 `Transport` 结构体，它会将密钥注入请求，如下所示：

```
type apiTransport struct {
    http.RoundTripper
    key string
}

func (a apiTransport) RoundTrip(r *http.Request) (*http.Response, error) {
    q := r.URL.Query()
    q.Set("key", a.key)
    r.URL.RawQuery = q.Encode()
    return a.RoundTripper.RoundTrip(r)
}
```

这是 Go 接口如何允许扩展其自身行为的一个非常好的例子。我们定义了一个实现 `http.RoundTripper` 接口的类型，还有一个属性是同一接口的实例。该实现在执行底层传输之前将 API 密钥注入请求。此类型允许我们定义一个创建新客户端的辅助函数，其中我们将新定义的传输与默认传输一起使用：

```
func NewClient(tick time.Duration, key string) *Client {
    return &Client{
        client: &http.Client{
            Transport: apiTransport{http.DefaultTransport, key},
        },
        tick: time.NewTicker(tick),
    }
}
```

Maps 地理编码 API 返回一系列由各个部分组成的地址。详情请访问 <https://developers.google.com/maps/documentation/geocoding/intro#GeocodingResponses>。

结果以 JSON 编码，因此我们需要一个可以接收它的数据结构：

```
type Result struct {
    AddressComponents []struct {
        LongName  string   `json:"long_name"`
        ShortName string   `json:"short_name"`
        Types     []string `json:"types"`
    } `json:"address_components"`
    FormattedAddress string `json:"formatted_address"`
    Geometry struct {
        Location struct {
            Lat float64 `json:"lat"`
            Lng float64 `json:"lng"`
        } `json:"location"`
        // more fields
    } `json:"geometry"`
    PlaceID string `json:"place_id"`
    // more fields
}
```

我们可以使用该结构执行反向地理编码操作——通过使用相应的端点从坐标获取位置。我们在执行 HTTP 请求前等待定时器触发，并记得 `defer` 关闭响应体：

```
const url = "https://maps.googleapis.com/maps/api/geocode/json?latlng=%v,%v"
<-c.tick.C
resp, err := c.client.Get(fmt.Sprintf(url, lat, lng))
if err != nil {
    return nil, err
}
defer resp.Body.Close()
```

然后，我们可以将结果解码到一个使用我们已定义的 `Result` 类型的数据结构中，并检查 `status` 字符串：

```
var v struct {
    Results []Result `json:"results"`
    Status  string   `json:"status"`
}
// get the result
if err := json.NewDecoder(resp.Body).Decode(&v); err != nil {
    return nil, err
}
switch v.Status {
case "OK":
    return v.Results, nil
case "ZERO_RESULTS":
    return nil, nil
default:
    return nil, fmt.Errorf("status: %q", v.Status)
}
```

最后，我们可以使用客户端对一系列坐标进行地理编码，预期请求之间至少间隔 `860ms`：

```
c := NewClient(24*time.Hour/100000, os.Getenv("MAPS_APIKEY"))
start := time.Now()
for _, l := range [][2]float64{
    {40.4216448, -3.6904040},
    {40.4163111, -3.7047328},
    {40.4123388, -3.7096724},
    {40.4145150, -3.7064412},
} {
    locs, err := c.ReverseGeocode(l[0], l[1])
    e := time.Since(start)
    if err != nil {
        log.Println(e, l, err)
        continue
    }
    // just print the first location
    if len(locs) != 0 {
        locs = locs[:1]
    }
    log.Println(e, l, locs)
}
```

# Workers

前面的示例是一个使用 `time.Ticker` 通道来限制请求速率的 Google Maps 客户端。速率限制对于 API 密钥来说是合理的。让我们想象一下，我们有来自不同账户的更多 API 密钥，因此我们可以潜在地执行更多请求。




# Go 中的工作池和信号量

## 使用通道的工作池

一种非常典型的并发方法是工作池。在这里，你有一系列可以被选取来处理输入的客户端，应用程序的不同部分可以请求使用这些客户端，用完后将它们归还。

我们可以创建多个共享相同请求和响应通道的客户端，请求为坐标，结果为服务的响应。由于响应通道是唯一的，我们可以定义一个自定义类型来保存该通道所需的所有信息：

```
type result struct {
    Loc [2]float64
    Result []maps.Result
    Error error
}
```

下一步是创建通道——我们将从环境变量中读取一个逗号分隔的值列表。我们将创建一个用于请求的通道和一个用于响应的通道。在这种情况下，两个通道的容量都等于工作线程的数量，但即使通道是无缓冲的也照样工作。既然我们只使用通道，我们将需要另一个通道 `done`，它用于发信号告知工作线程是否已完成其最后一项工作：

```
keys := strings.Split(os.Getenv("MAPS_APIKEYS"), ",")
requests := make(chan [2]float64, len(keys))
results := make(chan result, len(keys))
done := make(chan struct{})
```

现在，我们将为每个 `keys` 创建一个 goroutine，在其中定义一个从 `requests` 通道获取数据、执行请求并将结果发送到专用通道的 `client`。当 `requests` 通道关闭时，goroutine 将退出 range 循环并向 `done` 通道发送一条消息，如下面的代码所示：

```
for i := range keys {
    go func(id int) {
        log.Printf("Starting worker %d with API key %q", id, keys[id])
        client := maps.NewClient(maps.DailyCap, keys[id])
        for j := range requests {
            var r = result{Loc: j}
            log.Printf("w[%d] working on %v", id, j)
            r.Result, r.Error = client.ReverseGeocode(j[0], j[1])
            results <- r
        }
        done <- struct{}{}
    }(i)
}
```

位置可以在另一个 goroutine 中按顺序发送到 `requests` 通道：

```
go func() {
    for _, l := range [][2]float64{
        {40.4216448, -3.6904040},
        {40.4163111, -3.7047328},
        {40.4123388, -3.7096724},
        {40.4145150, -3.7064412},
    } {
        requests <- l
    }
    close(requests)
}()
```

我们可以记录接收到的 `done` 信号数量，并在所有工作线程完成时关闭 `results` 通道：

```
go func() {
    count := 0
    for range done {
        if count++; count == len(keys) {
            break
        }
    }
    close(results)
}()
```

通道用于统计有多少工作线程已完成，一旦所有工作线程都完成，它将关闭 `results` 通道。这将允许我们直接遍历它以获取结果：

```
for r := range results {
    log.Printf("received %v", r)
}
```

使用通道只是等待所有 goroutine 完成的方法之一，我们将在下一章中结合 `sync` 包看到更多地道的做法。

## 工作池

通道可以用作资源池，允许我们按需请求资源。在下面的示例中，我们将创建一个小应用程序，使用来自 `github.com/tatsushid/go-fastping` 包的第三方客户端查找网络中哪些地址是有效的。

该池将有两个方法，一个用于获取新客户端，另一个用于将客户端返回池中。`Get` 方法将尝试从通道获取现有客户端，如果不可用则返回一个新的。`Put` 方法将尝试将客户端放回通道，否则丢弃它：

```
const wait = time.Millisecond * 250

type pingPool chan *fastping.Pinger

func (p pingPool) Get() *fastping.Pinger {
    select {
    case v := <-p:
        return v
    case <-time.After(wait):
        return fastping.NewPinger()
    }
}

func (p pingPool) Put(v *fastping.Pinger) {
    select {
    case p <- v:
    case <-time.After(wait):
    }
    return
}
```

客户端需要指定要扫描的网络，因此它需要一个可用网络列表，从 `net.Interfaces` 函数开始，遍历接口及其地址：

```
ifaces, err := net.Interfaces()
if err != nil {
    return nil, err
}
for _, iface := range ifaces {
    // ...
    addrs, err := iface.Addrs()
    // ...
    for _, addr := range addrs {
        var ip net.IP
        switch v := addr.(type) {
        case *net.IPNet:
            ip = v.IP
        case *net.IPAddr:
            ip = v.IP
        }
        // ...
        if ip = ip.To4(); ip != nil {
            result = append(result, ip)
        }
    }
}
```

我们可以接受一个命令行参数来选择接口，当参数不存在或错误时，我们可以向用户显示接口列表以供选择：

```
if len(os.Args) != 2 {
    help(ifaces)
}
i, err := strconv.Atoi(os.Args[1])
if err != nil {
    log.Fatalln(err)
}
if i < 0 || i > len(ifaces) {
    help(ifaces)
}
```

`help` 函数只是打印接口的 IP：

```
func help(ifaces []net.IP) {
    log.Println("please specify a valid network interface number")
    for i, f := range ifaces {
        mask, _ := f.DefaultMask().Size()
        fmt.Printf("%d - %s/%v\n", i, f, mask)
    }
    os.Exit(0)
}
```

下一步是获取需要检查的 IP 范围：

```
m := ifaces[i].DefaultMask()
ip := ifaces[i].Mask(m)
log.Printf("Lookup in %s", ip)
```

现在我们有了 IP，可以创建一个函数来获取同一网络中的其他 IP。Go 中的 IP 是字节切片，所以我们将替换最低有效位以获得最终地址。由于 IP 是切片，其值将被每次操作覆盖（切片是指针）。我们将更新原始 IP 的副本——因为切片指向同一个数组——以避免覆盖：

```
func makeIP(ip net.IP, i int) net.IP {
    addr := make(net.IP, len(ip))
    copy(addr, ip)
    b := new(big.Int)
    b.SetInt64(int64(i))
    v := b.Bytes()
    copy(addr[len(addr)-len(v):], v)
    return addr
}
```

然后，我们需要一个用于结果的通道和另一个用于跟踪 goroutine 的通道；对于每个 IP，我们需要检查是否可以为每个地址启动一个 goroutine。我们将使用一个包含 10 个客户端的池，在每个 goroutine 内部——我们将请求每个客户端，然后将它们归还给池。所有有效的 IP 将通过 `address` 通道发送：

```
done := make(chan struct{})
address := make(chan net.IP)
ones, bits := m.Size()
pool := make(pingPool, 10)
for i := 0; i < 1<<(uint(bits-ones)); i++ {
    go func(i int) {
        p := pool.Get()
        defer func() {
            pool.Put(p)
            done <- struct{}{}
        }()
        p.AddIPAddr(&net.IPAddr{IP: makeIP(ip, i)})
        p.OnRecv = func(a *net.IPAddr, _ time.Duration) { address <- a.IP }
        p.Run()
    }(i)
}
```

每当一个例程完成时，我们在 `done` 通道中发送一个值，以便我们在退出应用程序前统计接收到的 `done` 信号数量。这将是结果循环：

```
i = 0
for {
    select {
    case ip := <-address:
        log.Printf("Found %s", ip)
    case <-done:
        if i >= bits-ones {
            return
        }
        i++
    }
}
```

循环将持续直到通道中的计数达到 goroutine 的数量。这结束了通道和 goroutine 一起使用的更复杂的示例。

## 信号量




# 信号量

信号量是用于解决并发问题的工具。它们拥有一定数量的可用配额，用于限制对资源的访问；此外，各个线程可以从中请求一个或多个配额，并在完成后释放它们。如果可用配额的数量为一，则意味着该信号量一次仅支持一次访问，其行为类似于互斥锁。如果配额多于一，我们称之为最常见的类型——加权信号量。

在 Go 中，信号量可以使用容量等于配额的通道来实现，向通道发送消息以获取配额，从通道接收消息以释放：

```go
type sem chan struct{}

func (s sem) Acquire() {
    s <- struct{}{}
}

func (s sem) Release() {
    <-s
}
```

前面的代码展示了如何用几行代码使用通道实现信号量。以下是一个使用示例：

```go
func main() {
    s := make(sem, 5)
    for i := 0; i < 10; i++ {
        go func(i int) {
            s.Acquire()
            fmt.Println(i, "start")
            time.Sleep(time.Second)
            fmt.Println(i, "end")
            s.Release()
        }(i)
    }
    time.Sleep(time.Second * 3)
}
```

完整示例请访问 <https://play.golang.org/p/BR5GN2QopjQ>。

从前面的示例中我们可以看到，程序在第一轮获取时处理了一些请求，在第二轮处理其余请求，不允许同时有超过五个执行过程。

## 总结

在本章中，我们讨论了 Go 并发中的两个主要角色——`goroutine` 和 `channel`。我们首先解释了什么是 `thread`，`thread` 和 `goroutine` 的区别是什么，以及为什么它们如此方便。`Thread` 笨重且需要 `CPU 核心`，而 `goroutine` 轻量级且不绑定核心。我们看到了如何通过在函数前加上 `go` 关键字来轻松启动一个新的 `goroutine`，以及如何一次性启动一系列不同的 `goroutine`。我们了解到并发函数的参数是在创建 `goroutine` 时求值，而不是在它实际开始运行时。我们还看到，如果没有任何额外工具，要让不同的 `goroutine` 保持同步是非常困难的。

然后，我们介绍了用于在不同 `goroutine` 间共享信息并解决前述同步问题的 `channel`。我们了解到 `goroutine`（此处原文应为 channels，根据上下文修正）具有最大容量和当前大小——即它当前持有多少元素。大小不能超过容量，当向一个已满的 `channel` 发送额外元素时，该操作会阻塞，直到从 `channel` 中移除一个元素。从空 `channel` 接收也是一个阻塞操作。

我们看到了如何使用 `close` 函数关闭 `channel`，该操作应在发送数据的同一个 `goroutine` 中进行，以及在 `nil` 或已关闭的 `channel` 等特殊情况下操作的行为。我们介绍了 `select` 语句，用于在并发的 `channel` 操作中进行选择并控制应用流程。接着，我们介绍了 `time` 包中与并发相关的工具——`ticker` 和 `timer`。

最后，我们展示了一些实际案例，包括一个速率受限的 Google Maps 客户端和一个同时 `ping` 网络中所有地址的工具。

在下一章中，我们将探讨一些同步原语，它们将允许更好地处理 `goroutine` 和内存，使用更清晰简洁的代码。

## 问题

1.  什么是 `thread`，谁负责管理它？
2.  为什么 `goroutine` 与 `thread` 不同？
3.  启动 `goroutine` 时，参数何时求值？
4.  带缓冲和不带缓冲的 `channel` 有什么区别？
5.  为什么单向 `channel` 很有用？
6.  对 `nil` 或已关闭的 `channel` 进行操作会发生什么？
7.  `timer` 和 `ticker` 用于什么目的？
