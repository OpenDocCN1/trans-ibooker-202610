# 实现并发模式

本章将介绍并发模式以及如何使用它们构建健壮的系统应用。我们已经了解了并发涉及的所有工具（goroutine 和 channel、`sync` 和 `atomic` 以及 context），现在我们将探讨一些将它们组合成模式的常见方式，以便在程序中使用。

本章将涵盖以下主题：

* 从生成器开始
* 使用管道进行排序
* 多路复用与解复用
* 其他模式
* 资源泄漏

## 技术要求

本章要求安装 Go 并设置好你喜欢的编辑器。更多信息请参阅 第 3 章 *Go 概览*。

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



```
func stage(in <-chan interface{}) <-chan interface{} {
    var out = make(chan interface{})
    go func() {
        for v := range in {
            v = v.(int)+1 // some operation
            out <- v
        }
        close(out)
    }()
    return out
}
```

我们创建一个与输入通道类型相同的通道并返回它。在一个单独的 goroutine 中，我们从输入通道接收数据，对数据执行操作，然后将其发送到输出通道。

通过使用 `context.Context`，该模式可以得到进一步改进，以便我们能更好地控制应用程序流程。代码大致如下：

```
func stage(ctx context.Context, in <-chan interface{}) <-chan interface{} {
    var out = make(chan interface{})
    go func() {
        defer close(out)
        for v := range in {
            v = v.(int)+1 // some operation
            select {
                case out <- v:
                case <-ctx.Done():
                    return
            }
        }
    }()
    return out
}
```

设计管道时，我们应遵循以下几条通用规则：

*   中间阶段将接收一个入站通道并返回另一个。
*   生产者不接收任何通道，但返回一个。
*   消费者接收一个通道但不返回任何通道。
*   每个阶段会在创建通道后，发送完消息时将其关闭。
*   每个阶段应持续从输入通道接收数据，直到该通道关闭。

让我们创建一个简单的管道，它使用特定字符串从读取器过滤行并打印过滤后的行，同时高亮显示搜索字符串。我们可以从第一阶段——源——开始，它的签名中不接收任何通道，而是使用读取器逐行扫描。我们传入 context 以响应提前退出请求（context 取消），并使用 `bufio` 扫描器逐行读取。以下代码展示了这一点：

```
func SourceLine(ctx context.Context, r io.ReadCloser) <-chan string {
    ch := make(chan string)
    go func() {
        defer func() { r.Close(); close(ch) }()
        s := bufio.NewScanner(r)
        for s.Scan() {
            select {
            case <-ctx.Done():
                return
            case ch <- s.Text():
            }
        }
    }()
    return ch
}
```

我们可以将剩余操作分为两个阶段：过滤阶段和写入阶段。过滤阶段只是将数据从源通道过滤到输出通道。我们仍然传递 context，以便在 context 已完成时避免发送额外数据。这是文本过滤器的实现：

```
func TextFilter(ctx context.Context, src <-chan string, filter string) <-chan string {
    ch := make(chan string)
    go func() {
        defer close(ch)
        for v := range src {
            if !strings.Contains(v, filter) {
                continue
            }
            select {
            case <-ctx.Done():
                return
            case ch <- v:
            }
        }
    }()
    return ch
}
```

最后，我们有最终阶段——消费者，它将输出打印到 writer 中，并同样使用 context 以实现提前退出：

```
func Printer(ctx context.Context, src <-chan string, color int, highlight string, w io.Writer) {
    const close = "\x1b[39m"
    open := fmt.Sprintf("\x1b[%dm", color)
    for {
        select {
        case <-ctx.Done():
            return
        case v, ok := <-src:
            if !ok {
                return
            }
            i := strings.Index(v, highlight)
            if i == -1 {
                panic(v)
            }
            fmt.Fprint(w, v[:i], open, highlight, close, v[i+len(highlight):], "\n")
        }
    }
}
```

该函数的用法如下：

```
func main() {
    var search string
    ...
    ctx := context.Background()
    src := SourceLine(ctx, ioutil.NopCloser(strings.NewReader(sometext)))
    filter := TextFilter(ctx, src, search)
    Printer(ctx, filter, 31, search, os.Stdout)
}
```

通过这种方法，我们学习了如何将复杂操作拆分为由各阶段执行并通过通道连接的简单任务。

# 多路复用与解复用

现在我们已经熟悉了管道和阶段，可以介绍两个新概念：

*   **多路复用或扇出**：从一个通道接收并发送到多个通道
*   **解复用或扇入**：从多个通道接收并通过一个通道发送

这种模式非常常见，允许我们以不同方式利用并发的力量。最明显的方式是将数据从一个比其后续步骤更快的通道分发出去，并创建多个此类步骤的实例来弥补速度差异。

## 扇出

多路复用的实现非常直接。同一个通道需要传递给不同的阶段，以便每个阶段都能从中读取数据。

每个 goroutine 在运行时调度中都在竞争资源，因此如果我们想占用更多资源，可以为管道的某个阶段或应用程序中的某个操作使用多个 goroutine。

我们可以创建一个小应用程序，使用这种方法统计文本中出现的单词次数。让我们创建一个初始生产者阶段，它从 writer 读取并返回该行的单词切片：

```
func SourceLineWords(ctx context.Context, r io.ReadCloser) <-chan []string {
    ch := make(chan []string)
    go func() {
        defer func() { r.Close(); close(ch) }()
        b := bytes.Buffer{}
        s := bufio.NewScanner(r)
        for s.Scan() {
            b.Reset()
            b.Write(s.Bytes())
            words := []string{}
            w := bufio.NewScanner(&b)
            w.Split(bufio.ScanWords)
            for w.Scan() {
                words = append(words, w.Text())
            }
            select {
            case <-ctx.Done():
                return
            case ch <- words:
            }
        }
    }()
    return ch
}
```

现在我们可以定义另一个阶段来统计这些单词的出现次数。我们将使用这个阶段进行扇出：

```
func WordOccurrence(ctx context.Context, src <-chan []string) <-chan map[string]int {
    ch := make(chan map[string]int)
    go func() {
        defer close(ch)
        for v := range src {
            count := make(map[string]int)
            for _, s := range v {
                count[s]++
            }
            select {
            case <-ctx.Done():
                return
            case ch <- count:
            }
        }
    }()
    return ch
}
```

为了将第一阶段作为多个第二阶段实例的源，我们只需使用相同的输入通道创建多个计数阶段：

```
ctx, canc := context.WithCancel(context.Background())
defer canc()
src := SourceLineWords(ctx,   
    ioutil.NopCloser(strings.NewReader(cantoUno)))
count1, count2 := WordOccurrence(ctx, src), WordOccurrence(ctx, src)
```

## 扇入

解复用稍微复杂一些，因为我们不需要在一个 goroutine 或另一个 goroutine 中盲目接收数据——我们实际上需要同步一系列通道。避免竞争条件的一个好方法是创建另一个通道，所有来自不同输入通道的数据都将在此接收。我们还需要确保一旦所有通道都完成，这个合并通道就会关闭。我们还必须记住，如果 context 被取消，通道也会关闭。我们在这里使用 `sync.WaitGroup` 来等待所有通道完成：

```
wg := sync.WaitGroup{}
merge := make(chan map[string]int)
wg.Add(len(src))
go func() {
    wg.Wait()
    close(merge)
}()
```

问题在于我们有两个可能的关闭通道触发器：常规传输结束和 context 取消。




我们必须确保如果上下文结束，则不会向出站通道发送消息。在这里，我们正在从输入通道收集值并将它们发送到 `merge` 通道，但仅当上下文尚未完成时才发送。我们这样做是为了避免向已关闭的通道发送操作，这会导致我们的应用程序 panic：

```
for _, ch := range src {
    go func(ch <-chan map[string]int) {
        defer wg.Done()
        for v := range ch {
            select {
            case <-ctx.Done():    
                return
            case merge <- v:
            }
        }
    }(ch)
}
```

最后，我们可以专注于最后一个操作，它使用 `merge` 通道来执行最终的单词计数：

```
count := make(map[string]int)
for {
    select {
    case <-ctx.Done():
        return count
    case c, ok := <-merge:
        if !ok {
            return count
        }
        for k, v := range c {
            count[k] += v
        }
    }
}
```

添加了扇入后，应用程序的 `main` 函数将如下所示：

```
func main() {
    ctx, canc := context.WithCancel(context.Background())
    defer canc()
    src := SourceLineWords(ctx, ioutil.NopCloser(strings.NewReader(cantoUno)))
    count1, count2 := WordOccurrence(ctx, src), WordOccurrence(ctx, src)
    final := MergeCounts(ctx, count1, count2)
    fmt.Println(final)
}
```

我们可以看到，扇入是应用程序中最复杂且最关键的部分。让我们回顾一下有助于构建一个无 panic 和无死锁的扇入函数的决策：

*   使用 `merge` 通道从各个输入收集值。
*   拥有一个计数器等于输入通道数量的 `sync.WaitGroup`。
*   在单独的 goroutine 中使用它并等待它关闭通道。
*   为每个输入通道创建一个将值传输到 `merge` 通道的 goroutine。
*   确保仅在上下文未完成时发送记录。
*   在退出此类 goroutine 之前使用等待组的 `Done` 函数。

遵循上述步骤将允许我们通过简单的 `range` 使用 `merge` 通道。在我们的示例中，我们还在从通道接收之前检查上下文是否完成，以允许从 goroutine 提前退出。

# 生产者和消费者

通道使我们能够轻松处理多个消费者从一个生产者接收数据，反之亦然的场景。

正如我们已经看到的，单个生产者和一个消费者的情况非常简单：

```
func main() {
    // one producer
    var ch = make(chan int)
    go func() {
        for i := 0; i < 100; i++ {
            ch <- i
        }
        close(ch)
    }()
    // one consumer
    var done = make(chan struct{})
    go func() {
        for i := range ch {
            fmt.Println(i)
        }
        close(done)
    }()
    <-done
}
```

完整示例请见此处：[`play.golang.org/p/hNgehu62kjv`](https://play.golang.org/p/hNgehu62kjv)。

## 多个生产者 (N * 1)

使用等待组可以轻松处理多个生产者或消费者。在多个生产者的情况下，所有 goroutine 将共享同一个通道：

```
// three producer
var ch = make(chan string)
wg := sync.WaitGroup{}
wg.Add(3)
for i := 0; i < 3; i++ {
    go func(n int) {
        for i := 0; i < 100; i++ {
            ch <- fmt.Sprintln(n, i)
        }
        wg.Done()
    }(i)
}
go func() {
    wg.Wait()
    close(ch)
}()
```

完整示例请见此处：[`play.golang.org/p/4DqWKntl6sS`](https://play.golang.org/p/4DqWKntl6sS)。

它们将使用 `sync.WaitGroup` 在关闭通道之前等待每个生产者完成。

## 多个消费者 (1 * M)

同样的推理适用于多个消费者——它们都在不同的 goroutine 中从同一个通道接收：

```
func main() {
    // three consumers
    wg := sync.WaitGroup{}
    wg.Add(3)
    var ch = make(chan string)

    for i := 0; i < 3; i++ {
        go func(n int) {
            for i := range ch {
                fmt.Println(n, i)
            }
            wg.Done()
        }(i)
    }

    // one producer
    go func() {
        for i := 0; i < 10; i++ {
            ch <- fmt.Sprintln("prod-", i)
        }
        close(ch)
    }()

    wg.Wait()
}
```

完整示例请见此处：[`play.golang.org/p/_SWtw54ITFn`](https://play.golang.org/p/_SWtw54ITFn)。

在这种情况下，`sync.WaitGroup` 用于等待应用程序结束。

## 多个消费者和生产者 (N*M)

最后一个场景是我们有任意数量的生产者 (`N`) 和另一个任意数量的消费者 (`M`)。

在这种情况下，我们需要两个等待组：一个用于生产者，另一个用于消费者：

```
const (
    N = 3
    M = 5
)
wg1 := sync.WaitGroup{}
wg1.Add(N)
wg2 := sync.WaitGroup{}
wg2.Add(M)
var ch = make(chan string)
```

随后是一系列生产者和消费者，每个都在自己的 goroutine 中：

```
for i := 0; i < N; i++ {
    go func(n int) {
        for i := 0; i < 10; i++ {
            ch <- fmt.Sprintf("src-%d[%d]", n, i)
        }
        wg1.Done()
    }(i)
}

for i := 0; i < M; i++ {
    go func(n int) {
        for i := range ch {
            fmt.Printf("cons-%d, msg %q\n", n, i)
        }
        wg2.Done()
    }(i)
}
```

最后一步是等待生产者的 `WaitGroup` 完成其工作以关闭通道。

然后，我们可以等待消费者通道，让所有消息被消费者处理：

```
wg1.Wait()
close(ch)
wg2.Wait()
```

# 其他模式

到目前为止，我们已经研究了最常见的并发模式。现在，我们将关注一些不太常见但值得一提的模式。

## 错误组

`sync.WaitGroup` 的强大之处在于它允许我们等待同时运行的 goroutine 完成其工作。我们已经研究了如何共享上下文，如果使用得当，可以让 goroutine 提前退出。第一个并发操作（如从通道发送或接收）位于 `select` 块中，与上下文完成通道在一起：

```
func main() {
    ctx, canc := context.WithTimeout(context.Background(), time.Second)
    defer canc()
    wg := sync.WaitGroup{}
    wg.Add(10)
    var ch = make(chan int)
    for i := 0; i < 10; i++ {
        go func(ctx context.Context, i int) {
            defer wg.Done()
            d := time.Duration(rand.Intn(2000)) * time.Millisecond
            time.Sleep(d)
            select {
            case <-ctx.Done():
                fmt.Println(i, "early exit after", d)
                return
            case ch <- i:
                fmt.Println(i, "normal exit after", d)
            }
        }(ctx, i)
    }
    go func() {
        wg.Wait()
        close(ch)
    }()
    for range ch {
    }
}
```

实验性的 `golang.org/x/sync/errgroup` 包为此场景提供了一种改进。

内置的 goroutine 始终是 `func()` 类型，但此包允许我们并发执行 `func() error` 并返回从各个 goroutine 接收到的第一个错误。

这在你一起启动多个 goroutine 并接收第一个错误的场景中非常有用。`errgroup.Group` 类型可以作为零值使用，其 `Do` 方法接受 `func() error` 作为参数并并发启动该函数。

`Wait` 方法要么等待所有函数成功完成并返回 `nil`，要么返回来自任意函数的第一个错误。

让我们创建一个定义 URL 访问器的示例，即一个获取 URL 字符串并返回 `func() error` 的函数，该函数进行调用：





# 错误组模式

## 基础用法

```
func visitor(url string) func() error {
    return func() (err error) {
        s := time.Now()
        defer func() {
            log.Println(url, time.Since(s), err)
        }()
        var resp *http.Response
        if resp, err = http.Get(url); err != nil {
            return
        }
        return resp.Body.Close()
    }
}
```

我们可以直接将其与 `Go` 方法结合使用并等待。这将返回由无效 URL 导致的错误：

```
func main() {
    eg := errgroup.Group{}
    var urlList = []string{
        "http://www.golang.org/",
        "http://invalidwebsite.hey/",
        "http://www.google.com/",
    }
    for _, url := range urlList {
        eg.Go(visitor(url))
    }
    if err := eg.Wait(); err != nil {
        log.Fatalln("Error:", err)
    }
}
```

## 带上下文

错误组还允许我们通过 `WithContext` 函数创建一个组及其关联的上下文。当接收到第一个错误时，该上下文会被取消。上下文的取消使 `Wait` 方法能够立即返回，但它也允许在你的函数中的 goroutine 实现提前退出。

我们可以创建一个类似的 `func() error` 创建器，它会向通道发送值，直到上下文关闭。我们将引入一个很小的概率（1%）来触发错误：

```
func sender(ctx context.Context, ch chan<- string, n int) func() error {
    return func() (err error) {
        for i := 0; ; i++ {
            if rand.Intn(100) == 42 {
                return errors.New("the answer")
            }
            select {
            case ch <- fmt.Sprintf("[%d]%d", n, i):
            case <-ctx.Done():
                return nil
            }
        }
    }
}
```

我们将使用专用函数生成一个错误组和一个上下文，并用它来启动该函数的多个实例。我们将在一个单独的 goroutine 中接收这些值，同时等待该组。等待结束后，我们将通过额外等待一秒来确保没有更多值被发送到通道（这会导致 panic）：

```
func main() {
    eg, ctx := errgroup.WithContext(context.Background())
    ch := make(chan string)
    for i := 0; i < 10; i++ {
        eg.Go(sender(ctx, ch, i))
    }
    go func() {
        for s := range ch {
            log.Println(s)
        }
    }()
    if err := eg.Wait(); err != nil {
        log.Println("Error:", err)
    }
    close(ch)
    log.Println("waiting...")
    time.Sleep(time.Second)
}
```

正如预期的那样，得益于上下文中的 `select` 语句，应用程序能够无缝运行且不会发生 panic。

# 漏桶算法

在前面的章节中，我们看到了如何使用 ticker 构建速率限制器：通过使用 `time.Ticker` 强制客户端等待轮到自己以获得服务。服务和库的速率限制还有另一种方法，称为**漏桶算法**。这个名字唤起了一幅有几个洞的桶的图景。如果你在注水，必须小心不要注入太多水，否则水会溢出。在加更多水之前，你需要等待水位下降——下降的速度取决于桶的大小和洞的数量。通过查看以下类比，我们可以轻松理解这一并发模式的作用：

- 穿过洞口的水代表已完成的请求。
- 从桶中溢出的水代表被丢弃的请求。

桶由两个属性定义：

- **速率**：如果请求频率低于该速率，则单位时间内的理想请求量。
- **容量**：在资源暂时变得无响应之前，可以同时处理的请求数量。

桶有一个最大容量，因此当请求的频率高于指定速率时，这个容量开始下降，就像你倒水太多导致桶开始溢出一样。如果频率为零或低于速率，桶将慢慢恢复其容量，水也会慢慢排出。

漏桶的数据结构将包含一个容量和一个可用请求的计数器。该计数器在创建时与容量相同，每次执行请求时都会减少。速率指定了将状态重置为容量的频率：

```
type bucket struct {
    capacity uint64
    status   uint64
}
```

## 实现

创建新桶时，我们还应处理状态重置。我们可以为此使用一个 goroutine，并使用上下文来正确终止它。我们可以使用速率创建一个 ticker，然后利用这些 tick 来重置状态。我们需要使用 `atomic` 包来确保线程安全：

```
func newBucket(ctx context.Context, cap uint64, rate time.Duration) *bucket {
    b := bucket{capacity: cap, status: cap}
    go func() {
        t := time.NewTicker(rate)
        for {
            select {
            case <-t.C:
                atomic.StoreUint64(&b.status, b.capacity)
            case <-ctx.Done():
                t.Stop()
                return
            }
        }
    }()
    return &b
}
```

当我们向桶中添加内容时，可以检查状态并采取相应措施：

- 如果状态为 `0`，我们无法添加任何内容。
- 如果要添加的数量高于可用量，我们添加我们能添加的最大量。
- 否则添加全部数量：

```
func (b *bucket) Add(n uint64) uint64 {
    for {
        r := atomic.LoadUint64(&b.status)
        if r == 0 {
            return 0
        }
        if n > r {
            n = r
        }
        if !atomic.CompareAndSwapUint64(&b.status, r, r-n) {
            continue
        }
        return n
    }
}
```

我们使用循环尝试原子交换操作，直到成功，以确保我们通过 `Load` 操作获得的值在执行**比较并交换**（**CAS**）时不会改变。

## 用法

桶可以用于一个客户端中，该客户端将尝试向桶中添加随机数量并记录其结果：

```
type client struct {
    name  string
    max   int
    b     *bucket
    sleep time.Duration
}

func (c client) Run(ctx context.Context, start time.Time) {
    for {
        select {
        case <-ctx.Done():
            return
        default:
            n := 1 + rand.Intn(c.max-1)
            time.Sleep(c.sleep)
            e := time.Since(start).Seconds()
            a := c.b.Add(uint64(n))
            log.Printf("%s tries to take %d after %.02fs, takes %d", c.name, n, e, a)
        }
    }
}
```

我们可以并发使用多个客户端，这样对资源的并发访问将产生以下结果：

- 有些 goroutine 将向桶中添加它们预期的数量。
- 一个 goroutine 最终将通过添加等于剩余容量的数量来填满桶，即使它们试图添加的数量更高。
- 其他 goroutine 将无法向桶中添加内容，直到容量被重置：

```
func main() {
    ctx, canc := context.WithTimeout(context.Background(), time.Second)
    defer canc()
    start := time.Now()
    b := newBucket(ctx, 10, time.Second/5)
    t := time.Second / 10
    for i := 0; i < 5; i++ {
        c := client{
            name:  fmt.Sprint(i),
            b:     b,
            sleep: t,
            max:   5,
        }
        go c.Run(ctx, start)
    }
    <-ctx.Done()
}
```

# 排序

在涉及多个 goroutine 的并发场景中，我们可能需要在 goroutine 之间进行同步，例如在每个 goroutine 发送后需要等待轮到自己的场景中。

此场景的一个用例可能是基于回合的应用程序，其中不同的 goroutine 向同一个通道发送消息，并且每个 goroutine 都必须等待所有其他 goroutine 完成后才能再次发送。





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
