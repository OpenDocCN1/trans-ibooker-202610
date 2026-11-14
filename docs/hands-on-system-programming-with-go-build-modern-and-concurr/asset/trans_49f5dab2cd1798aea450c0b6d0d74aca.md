

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

完整示例请访问 [https://play.golang.org/p/zJCL5VGmMsC](https://play.golang.org/p/zJCL5VGmMsC)。

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

完整示例请访问 [https://play.golang.org/p/uSfm5zWN8-x](https://play.golang.org/p/uSfm5zWN8-x)。

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

完整示例请访问 [https://play.golang.org/p/RKcojupCruB](https://play.golang.org/p/RKcojupCruB)。

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

完整示例请访问 [https://play.golang.org/p/GUzgG4kf5ta](https://play.golang.org/p/GUzgG4kf5ta)。

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

完整示例请访问 [https://play.golang.org/p/ZgEPZ99PLJv](https://play.golang.org/p/ZgEPZ99PLJv)。

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

完整示例请访问 [https://play.golang.org/p/pPuqpfnq8jJ](https://play.golang.org/p/pPuqpfnq8jJ)。

# 等待接收者

我们在上一节中看到的大多数示例都是在 goroutine 中完成发送操作，在 `main` goroutine 中完成接收操作。也可能所有操作都由 goroutine 处理，那么我们如何将 `main` goroutine 与其他 goroutine 同步呢？

一种典型的技术是使用另一个通道，其唯一目的是发出 goroutine 已完成工作的信号。接收 goroutine 通过通信通道的关闭知道没有更多消息可获取，并在完成操作后关闭另一个与 `main` goroutine 共享的通道。`main` 函数可以在退出前等待该通道的关闭。

用于此目的的典型通道不携带任何额外信息，只表示它是打开还是关闭，因此通常是 `chan struct{}` 通道。这是因为空数据结构在内存中没有大小。我们可以通过对前面的示例进行一些更改来看到此模式的实际应用，从接收者函数开始：

