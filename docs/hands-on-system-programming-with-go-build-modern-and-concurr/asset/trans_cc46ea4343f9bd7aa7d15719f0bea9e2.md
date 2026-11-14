

# 并发模式实现

## 使用私有通道的简单实现

通过 `main` goroutine 和发送者之间的私有通道，可以获得该场景的一个非常简单的实现。我们可以定义一个非常简单的结构体，它同时携带消息和一个 `Wait` 通道。它将有两个方法——一个用于将事务标记为完成，另一个用于等待此类信号——底层使用通道实现。以下方法展示了这一点：

```go
type msg struct {
    value string
    done  chan struct{}
}

func (m *msg) Wait() {
    <-m.done
}

func (m *msg) Done() {
    m.done <- struct{}{}
}
```

## 生成器函数：`send`

我们可以使用生成器创建消息源。我们可以在 `send` 操作中使用随机延迟。每次发送后，我们通过调用 `Done` 方法等待信号。我们始终使用 context 以防止泄漏：

```go
func send(ctx context.Context, v string) <-chan msg {
    ch := make(chan msg)
    go func() {
        done := make(chan struct{})
        for i := 0; ; i++ {
            time.Sleep(time.Duration(float64(time.Second/2) * rand.Float64()))
            m := msg{fmt.Sprintf("%s msg-%d", v, i), done}
            select {
            case <-ctx.Done():
                close(ch)
                return
            case ch <- m:
                m.Wait()
            }
        }
    }()
    return ch
}
```

## 扇入：`merge` 函数

我们可以使用扇入将所有通道汇聚到一个单一的通道中：

```go
func merge(ctx context.Context, sources ...<-chan msg) <-chan msg {
    ch := make(chan msg)
    go func() {
        <-ctx.Done()
        close(ch)
    }()
    for i := range sources {
        go func(i int) {
            for {
                select {
                case v := <-sources[i]:
                    select {
                    case <-ctx.Done():
                        return
                    case ch <- v:
                    }
                }
            }
        }(i)
    }
    return ch
}
```

## 主应用程序

主应用程序将从合并后的通道接收消息，直到该通道关闭。当它从每个通道接收到一条消息时，通道将被阻塞，等待 `main` goroutine 调用 `Done` 方法发出信号。

这种特定的配置将允许 `main` goroutine 从每个通道仅接收一条消息。当消息计数达到 goroutine 的数量时，我们可以从 `main` goroutine 调用 `Done` 并重置列表，以便其他 goroutine 被解锁并能够再次发送消息：

```go
func main() {
    ctx, canc := context.WithTimeout(context.Background(), time.Second)
    defer canc()
    sources := make([]<-chan msg, 5)
    for i := range sources {
        sources[i] = send(ctx, fmt.Sprint("src-", i))
    }
    msgs := make([]msg, 0, len(sources))
    start := time.Now()
    for v := range merge(ctx, sources...) {
        msgs = append(msgs, v)
        log.Println(v.value, time.Since(start))
        if len(msgs) == len(sources) {
            log.Println("*** done ***")
            for _, m := range msgs {
                m.Done()
            }
            msgs = msgs[:0]
            start = time.Now()
        }
    }
}
```

## 运行应用程序

运行应用程序将导致所有 goroutine 各自向主 goroutine 发送一条消息。它们每个都将等待其他所有 goroutine 发送完消息。然后，它们将再次开始发送消息。这将导致消息按轮次发送，符合预期。

## 总结

在本章中，我们探讨了一些用于应用程序的特定并发模式。我们了解到生成器是返回通道的函数，并且用数据填充这些通道，在没有更多数据时关闭它们。我们还了解到可以使用 `context` 让生成器提前退出。

接下来，我们关注管道，它们是使用通道进行通信的执行阶段。它们可以是**源**（source），不需要任何输入；**目标**（destination），不返回通道；或**中间阶段**（intermediate），接收一个通道作为输入并返回一个作为输出。

另一种模式是多路复用和解复用，它包括将一个通道分发给不同的 goroutine 以及将多个通道合并为一个。它通常被称为*扇出-扇入*（*fan-out fan-in*），并允许我们在一组数据上并发执行不同的操作。

最后，我们学习了如何实现一个更好的限流器版本，称为**漏桶**（**leaky bucket**），它限制特定时间内的请求数量。我们还研究了排序模式，它使用私有通道向所有发送 goroutine 发出信号，告知它们何时被允许再次发送数据。

在下一章中，我们将介绍在*排序*部分展示的两个额外主题中的第一个。我们将演示如何使用反射构建适应任何用户提供类型的通用代码。

## 问题

1.  什么是生成器？它的职责是什么？
2.  如何描述管道？
3.  哪种阶段获取一个通道并返回一个通道？
4.  扇入和扇出有什么区别？

# 第 5 部分：反射与 CGO 使用指南

本部分重点关注两个极具争议的工具——反射，它允许创建通用代码但具有巨大的性能成本；以及 CGO，它允许在 Go 应用程序中使用 C 代码，但使调试和控制应用程序变得更加复杂。

本部分包含以下章节：

*   **第 15 章**，*使用反射*
*   **第 16 章**，*使用 CGO*

# 使用反射

本章讲述**反射**，这是一种允许应用程序检查自身代码的工具，克服了 Go 静态类型及其缺乏泛型所施加的一些限制。这非常有用，例如，用于生成能够处理接收到的任何类型输入的包。

本章将涵盖以下主题：

*   理解接口和类型断言
*   了解与基本类型的交互
*   在复杂类型上使用反射
*   评估反射的成本
*   学习反射使用的最佳实践

## 技术要求

本章要求已安装 Go 并设置好您喜欢的编辑器。更多信息请参阅**第 3 章**，*Go 概览*。

## 什么是反射？

反射是一项非常强大的功能，它允许**元编程**，即应用程序检查自身结构的能力。它在运行时分析应用程序中的类型非常有用，并被许多编码包（如 `JSON` 和 `XML`）所使用。

## 类型断言

我们在**第 3 章**，*Go 概览*中简要提到了类型断言的工作原理。类型断言是一种允许我们从接口转换到具体类型，反之亦然的操作。它采用以下形式：

```go
// 不安全的断言
v := SomeVar.(SomeType)

// 安全的断言
v, ok := SomeVar.(SomeType)
```

第一个版本是不安全的，它将值赋给单个变量。

将断言作为函数的参数使用也算作不安全。如果断言错误，此类操作会引发 panic：

```go
func main() {
    var a interface{} = "hello"
    fmt.Println(a.(string)) // 正常
    fmt.Println(a.(int))    // panic!
}
```

完整示例请见此处：<https://play.golang.org/p/hNN87SuprGR>。

第二个版本使用布尔值 `ok` 作为第二个返回值，它将显示操作是否成功。如果断言不可能，第一个值将始终是断言类型的零值。

