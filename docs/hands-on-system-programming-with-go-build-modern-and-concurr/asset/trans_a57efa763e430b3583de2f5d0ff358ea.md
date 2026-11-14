
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

完整示例请访问 [https://play.golang.org/p/3gPGZkJtJYv](https://play.golang.org/p/3gPGZkJtJYv)。

如果我们运行上述示例的应用程序，会发现它没有产生任何输出。为什么？在 Go 中，当主 goroutine 结束时，应用程序就会终止，看起来就是这种情况。实际发生的是 `go` 语句创建了带有相应 `runtime.g` 的 goroutine，但这必须由 Go 调度器拾取，而由于程序在 goroutine 实例化后立即终止，所以这并没有发生。

使用 `time.Sleep()` 函数让主 goroutine 等待（即使只是一纳秒！）就足以让调度器拾取 goroutine 并执行其代码。如下代码所示：

```
func main() {
    go fmt.Println("Hello, playground")
    time.Sleep(time.Nanosecond)
}
```

完整示例请访问 [https://play.golang.org/p/2u125pTclv6](https://play.golang.org/p/2u125pTclv6)。

我们已经看到 Go 方法也算作函数，这就是为什么它们可以像普通函数一样通过 `go` 语句并发执行：

```
type a struct{}

func (a) Method() { fmt.Println("Hello, playground") }

func main() {
    go a{}.Method()
    time.Sleep(time.Nanosecond)
}
```

完整示例请访问 [https://play.golang.org/p/RUhgfRAPa2b](https://play.golang.org/p/RUhgfRAPa2b)。

闭包是匿名函数，因此也可以使用，这实际上是一种非常常见的做法：

```
func main() {
    go func() {
        fmt.Println("Hello, playground")
    }()
    time.Sleep(time.Nanosecond)
}
```

完整示例请访问 [https://play.golang.org/p/a-JvOVwAwUV](https://play.golang.org/p/a-JvOVwAwUV)。

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

完整示例请访问 [https://play.golang.org/p/Jaljd1padeX](https://play.golang.org/p/Jaljd1padeX)。

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

完整示例请访问 [https://play.golang.org/p/RV54AsYY-2y](https://play.golang.org/p/RV54AsYY-2y)。

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

完整示例请访问 [https://play.golang.org/p/r1dtBiTUMaw](https://play.golang.org/p/r1dtBiTUMaw)。

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

完整示例请访问 [https://play.golang.org/p/Yhz4bTxm5L8](https://play.golang.org/p/Yhz4bTxm5L8)。
