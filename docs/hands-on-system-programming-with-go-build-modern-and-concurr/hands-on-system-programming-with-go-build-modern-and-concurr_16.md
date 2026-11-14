# 使用 sync 和 atomic 进行同步

本章将继续深入 Go 并发，介绍 `sync` 和 `atomic` 包，它们是另外几个用于编排 `goroutine` 间同步的工具。这将使得编写优雅简洁的代码成为可能，这些代码允许并发使用资源并管理 `goroutine` 的生命周期。`sync` 包含高级同步原语，而 `atomic` 包含低级原语。

本章将涵盖以下主题：

*   锁
*   等待组
*   其他 `sync` 组件
*   `atomic` 包

## 技术要求

本章要求已安装 Go 并设置好你喜欢的编辑器。有关更多信息，请参阅 第 3 章 *Go 概览*。

## 同步原语

我们了解了 `channel` 如何专注于 `goroutine` 间的通信，现在我们将关注 `sync` 包提供的工具，其中包含用于 `goroutine` 间同步的基本原语。首先我们将看到如何使用锁实现对同一资源的并发访问。

### 并发访问与锁

Go 为可加锁和解锁的对象提供了一个通用接口。加锁意味着获取其控制权，而解锁则释放它供他人使用。该接口为每个操作暴露了一个方法。以下是代码示例：

```go
type Locker interface {
    Lock()
    Unlock()
}
```

#### 互斥锁

锁最简单的实现是 `sync.Mutex`。由于其方法使用指针接收者，因此不应复制或按值传递。如果可能，`Lock()` 方法会获取 `mutex` 的控制权，否则阻塞 `goroutine` 直到 `mutex` 可用。`Unlock()` 方法释放 `mutex`，如果在未加锁的 `mutex` 上调用，会返回运行时错误。

这里有一个简单的示例，我们启动一堆 `goroutine` 使用锁来查看哪个先执行：

```go
func main() {
    var m sync.Mutex
    done := make(chan struct{}, 10)
    for i := 0; i < cap(done); i++ {
        go func(i int, l sync.Locker) {
            l.Lock()
            defer l.Unlock()
            fmt.Println(i)
            time.Sleep(time.Millisecond * 10)
            done <- struct{}{}
        }(i, &m)
    }
    for i := 0; i < cap(done); i++ {
        <-done
    }
}
```

完整示例请访问：<https://play.golang.org/p/resVh7LImLf>

我们使用 `channel` 在任务完成时向主 `goroutine` 发信号，并退出应用程序。让我们创建一个外部计数器并使用 `goroutine` 并发地增加它。

在不同 `goroutine` 上执行的操作不是线程安全的，正如我们从以下示例中看到的：

```go
done := make(chan struct{}, 10000)
var a = 0
for i := 0; i < cap(done); i++ {
    go func(i int) {
        if i%2 == 0 {
            a++
        } else {
            a--
        }
        done <- struct{}{}
    }(i)
}
for i := 0; i < cap(done); i++ {
    <-done
}
fmt.Println(a)
```

我们期望有 5000 次加一和 5000 次减一，最终指令打印 `0`。然而，每次运行应用程序得到的值都不同。之所以发生这种情况，是因为这类操作不是线程安全的，所以两个或更多操作可能同时发生，最后一个操作会覆盖其他操作。这种现象称为 `race condition`（竞态条件）；即多个操作尝试写入同一结果。

这意味着如果没有任何同步，结果是不可预测的；如果我们检查前面的示例并使用锁来避免 `race condition`，整数的值将为零——这是我们期望的结果：





# 数据结构中的互斥锁嵌入

```
m := sync.Mutex{}
for i := 0; i < cap(done); i++ {
    go func(l sync.Locker, i int) {
        l.Lock()
        defer l.Unlock()
        if i%2 == 0 {
            a++
        } else {
            a--
        }
        done <- struct{}{}
    }(&m, i)
    fmt.Println(a)
}
```

一种非常常见的做法是在数据结构中嵌入互斥锁，以表明该容器是你想要加锁的对象。之前的计数器变量可以表示如下：

```
type counter struct {
    m     sync.Mutex
    value int
}
```

计数器执行的操作可以是方法，它们在主要操作前处理加锁，在之后处理解锁，如下面的代码块所示：

```
func (c *counter) Incr() {
    c.m.Lock()
    c.value++
    c.m.Unlock()
}

func (c *counter) Decr() {
    c.m.Lock()
    c.value--
    c.m.Unlock()
}

func (c *counter) Value() int {
    c.m.Lock()
    a := c.value
    c.m.Unlock()
    return a
}
```

这将简化 goroutine 循环，从而得到更清晰的代码：

```
var a = counter{}
for i := 0; i < cap(done); i++ {
    go func(i int) {
        if i%2 == 0 {
            a.Incr()
        } else {
            a.Decr()
        }
        done <- struct{}{}
    }(i)
}
// ...
fmt.Println(a.Value())
```

# RWMutex

竞态条件的问题是由并发写入引起的，而非读取操作。实现了 locker 接口的另一种数据结构 `sync.RWMutex`，旨在同时支持这两种操作，拥有独占且与读锁互斥的写锁。这意味着互斥锁可以被单个写锁锁定，也可以被一个或多个读锁锁定。当读取者锁定互斥锁时，其他试图锁定它的读取者不会被阻塞。它们通常被称为*共享-独占*锁。这允许读操作同时发生，而无需等待时间。

写锁操作通过 locker 接口的 `Lock` 和 `Unlock` 方法完成。读取操作通过另外两个方法执行：`RLock` 和 `RUnlock`。还有一个方法 `RLocker`，它返回一个用于读取操作的 locker。

我们可以通过创建一个并发字符串列表来快速演示它们的用法：

```
type list struct {
    m     sync.RWMutex
    value []string
}
```

我们可以遍历切片以查找选定的值，并使用读锁在读取时延迟写入：

```
func (l *list) contains(v string) bool {
    for _, s := range l.value {
        if s == v {
            return true
        }
    }
    return false
}

func (l *list) Contains(v string) bool {
    l.m.RLock()
    found := l.contains(v)
    l.m.RUnlock()
    return found
}
```

我们可以在添加新元素时使用写锁：

```
func (l *list) Add(v string) bool {
    l.m.Lock()
    defer l.m.Unlock()
    if l.contains(v) {
        return false
    }
    l.value = append(l.value, v)
    return true
}
```

然后我们可以尝试使用多个 goroutine 在列表上执行相同的操作：

```
var src = []string{
    "Ryu", "Ken", "E. Honda", "Guile",
    "Chun-Li", "Blanka", "Zangief", "Dhalsim",
}
var l list
for i := 0; i < 10; i++ {
    go func(i int) {
        for _, s := range src {
            go func(s string) {
                if !l.Contains(s) {
                    if l.Add(s) {
                        fmt.Println(i, "add", s)
                    } else {
                        fmt.Println(i, "too slow", s)
                    }
                }
            }(s)
        }
    }(i)
}
time.Sleep(500 * time.Millisecond)
```

我们首先检查名称是否包含在锁中，然后尝试添加元素。这导致多个 routine 尝试添加新元素，但由于写锁是独占的，只有一个会成功。

# 写饥饿

在设计应用程序时，这种互斥锁并不总是显而易见的选择，因为在读锁数量较多而写锁较少的场景中，互斥锁在第一个读锁之后会接受更多传入的读锁，让写操作等待一个没有活动读锁的时刻。这种现象被称为**写饥饿**。

为了验证这一点，我们可以定义一个同时具有写操作和读操作的类型，它们都需要一些时间，如下面的代码所示：

```
type counter struct {
    m     sync.RWMutex
    value int
}

func (c *counter) Write(i int) {
    c.m.Lock()
    time.Sleep(time.Millisecond * 100)
    c.value = i
    c.m.Unlock()
}

func (c *counter) Value() int {
    c.m.RLock()
    time.Sleep(time.Millisecond * 100)
    a := c.value
    c.m.RUnlock()
    return a
}
```

我们可以尝试在单独的 goroutine 中以相同的频率执行写和读操作，使用的持续时间低于方法的执行时间（50 ms 对比 100 ms）。我们还将检查它们在锁定状态下花费了多少时间：

```
var c counter
t1 := time.NewTicker(time.Millisecond * 50)
time.AfterFunc(time.Second*2, t1.Stop)
for {
    select {
    case <-t1.C:
        go func() {
            t := time.Now()
            c.Value()
            fmt.Println("val", time.Since(t))
        }()
        go func() {
            t := time.Now()
            c.Write(0)
            fmt.Println("inc", time.Since(t))
        }()
    case <-time.After(time.Millisecond * 200):
        return
    }
}
```

如果我们执行该应用程序，会看到每个写操作对应执行了多个读操作，且每次后续调用花费的时间都比前一次长，都在等待锁。读操作则不然，它可以同时发生，因此一旦某个读取者设法锁定了资源，所有其他等待的读取者也会这样做。用 `Mutex` 替换 `RWMutex` 将使两种操作具有相同的优先级，就像前面的例子一样。

# 加锁陷阱

在加锁和解锁互斥锁时必须小心，以避免应用程序中出现意外行为和死锁。请看下面的片段：

```
for condition {
    mu.Lock()
    defer mu.Unlock()
    action()
}
```

乍一看这段代码似乎没问题，但它必然会阻塞 goroutine。这是因为 `defer` 语句不是在每次循环迭代结束时执行，而是在函数返回时执行。所以第一次尝试会加锁而不释放，第二次尝试就会卡住。

稍微重构一下可以修复这个问题，如下面的片段所示：

```
for condition {
    func() {
        mu.Lock()
        defer mu.Unlock()
        action()
    }()
}
```

我们可以使用闭包来确保即使 `action` 发生 panic，延迟的 `Unlock` 也会被执行。

如果在互斥锁上执行的操作类型不会导致 panic，那么放弃 defer 并在执行操作后直接使用解锁可能是个好主意，如下所示：

```
for condition {
    mu.Lock()
    action()
    mu.Unlock()
}
```

`defer` 是有开销的，所以在不必要时（例如进行简单的变量读取或赋值时）最好避免使用它。

# 同步 Goroutines

直到现在，为了等待 goroutine 完成，我们使用了一个空结构体通道，并在最后一个操作中通过通道发送一个值，如下所示：

```
ch := make(chan struct{})
for i := 0; i < n; n++ {
    go func() {
        // do something
        ch <- struct{}{}
    }()
}
for i := 0; i < n; n++ {
    <-ch
}
```

这种策略可行，但不是完成任务的首选方式。从语义上讲这是不正确的，因为我们使用通道（一种通信工具）来发送空数据。这个用例关乎同步而非通信。这就是为什么有 `sync.WaitGroup` 数据结构，它涵盖了此类情况。它有一个主要状态，称为计数器，代表等待的元素数量：





# WaitGroup 结构与用法

## 结构定义

```go
type WaitGroup struct {
    noCopy noCopy
    state1 [3]uint32
}
```

`noCopy` 字段通过 `panic` 防止结构体被值拷贝。`state1` 是一个由三个 `uint32` 组成的数组，但仅使用首尾两个元素；中间的元素用于编译器优化。

## 核心方法

`WaitGroup` 提供三个方法来实现同步控制：

*   **`Add(delta int)`**：按给定值（可为负）修改计数器。若计数器小于零，程序将 `panic`。
*   **`Done()`**：`Add(-1)` 的简写。通常在 goroutine 完成工作时调用，将计数器减 1。
*   **`Wait()`**：阻塞当前 goroutine，直到计数器归零。

使用 `WaitGroup` 可使代码更清晰、可读性更强，如下例所示：

```go
func main() {
    wg := sync.WaitGroup{}
    wg.Add(10)
    for i := 1; i <= 10; i++ {
        go func(a int) {
            for i := 1; i <= 10; i++ {
                fmt.Printf("%dx%d=%d\n", a, i, a*i)
            }
            wg.Done()
        }(i)
    }
    wg.Wait()
}
```

向 `WaitGroup` 添加的 `delta` 等于预先启动的 goroutine 数量。每个 goroutine 中调用 `Done` 方法递减计数。若 goroutine 数量未知，可在启动每个 goroutine 前执行 `Add(1)`，如下所示：

```go
func main() {
    wg := sync.WaitGroup{}
    for i := 1; rand.Intn(10) != 0; i++ {
        wg.Add(1)
        go func(a int) {
            for i := 1; i <= 10; i++ {
                fmt.Printf("%dx%d=%d\n", a, i, a*i)
            }
            wg.Done()
        }(i)
    }
    wg.Wait()
}
```

上例中，`for` 循环每次迭代有 10% 概率结束，因此在启动 goroutine 前向组中加 1。

## 常见错误

一个典型错误是在 goroutine 内部调用 `Add`，这通常导致程序过早退出且无任何 goroutine 执行。因为应用创建 goroutine 并执行 `Wait` 时，goroutine 尚未启动并调用 `Add`，如下例所示：

```go
func main() {
    wg := sync.WaitGroup{}
    for i := 1; i < 10; i++ {
        go func(a int) {
            wg.Add(1)
            for i := 1; i <= 10; i++ {
                fmt.Printf("%dx%d=%d\n", a, i, a*i)
            }
            wg.Done()
        }(i)
    }
    wg.Wait()
}
```

该程序无任何输出，因为它在 goroutine 启动并调用 `Add` 之前就到达了 `Wait` 语句。

# Go 中的单例模式

单例模式是软件开发中常用的策略，将某类型的实例数限制为一，在整个应用中共享同一实例。简单实现如下：

```go
type obj struct {}

var instance *obj

func Get() *obj {
    if instance == nil {
        instance = &obj{}
    }
    return instance
}
```

这在串行场景下无问题，但在并发场景（如许多 Go 应用）中非线程安全，可能产生竞态条件。

## 使用互斥锁保证线程安全

通过添加锁可避免竞态条件：

```go
type obj struct {}

var (
    instance *obj
    lock     sync.Mutex
)

func Get() *obj {
    lock.Lock()
    defer lock.Unlock()
    if instance == nil {
        instance = &obj{}
    }
    return instance
}
```

此法虽安全，但较慢，因为每次请求实例时 `Mutex` 都会同步。

## 使用 `sync.Once` 的最佳实践

最佳方案是使用 `sync.Once`，它结合 `Mutex` 和 `atomic` 读取（详见本章第二部分）确保函数仅执行一次：

```go
type obj struct {}

var (
    instance *obj
    once     sync.Once
)

func Get() *obj {
    once.Do(func() {
        instance = &obj{}
    })
    return instance
}
```

该代码符合惯用风格、清晰明了，且性能优于互斥锁方案。因操作仅执行一次，可省去前例中的 `nil` 检查。

# `sync.Once` 与重置

`sync.Once` 旨在仅执行一次函数。第三方库 `github.com/matryer/resync` 提供 `Reset` 方法以重置单例状态。

典型用例包括：遇特定错误需重新初始化，如重新获取 API 密钥或连接中断后重新拨号。

# 资源回收

上一章已介绍使用带缓冲通道的工作池实现资源回收。主要包含两个方法：

*   **`Get`**：尝试从通道接收消息，或返回新实例。
*   **`Put`**：尝试将实例放回通道，或丢弃它。

以下是基于通道的简单池实现：

```go
type A struct{}

type Pool chan *A

func (p Pool) Get() *A {
    select {
    case a := <-p:
        return a
    default:
        return new(A)
    }
}

func (p Pool) Put(a *A) {
    select {
    case p <- a:
    default:
    }
}
```

可使用 `sync.Pool` 改进，它实现了线程安全的对象集合，支持保存和获取。只需定义创建新对象时的行为：

```go
type Pool struct {
    // New 可选，指定当 Get 否则返回 nil 时生成值的函数。
    // 不得与 Get 调用并发修改。
    New func() interface{}
    // contains filtered or unexported fields
}
```

`sync.Pool` 提供 `Get` 和 `Put` 两个方法，分别从池中获取对象（或创建新对象）和将对象放回池中。因 `Get` 返回 `interface{}`，使用前需类型断言。我们已详细讨论缓冲回收，下面用 `sync.Pool` 实现一个示例。

需定义池及获取/释放缓冲区的函数。缓冲区初始容量 4 KB，`Put` 函数在放回池前确保重置缓冲区：

```go
var pool = sync.Pool{
    New: func() interface{} {
        return bytes.NewBuffer(make([]byte, 0, 4096))
    },
}

func Get() *bytes.Buffer {
    return pool.Get().(*bytes.Buffer)
}

func Put(b *bytes.Buffer) {
    b.Reset()
    pool.Put(b)
}
```

现在创建一系列 goroutine，使用 `WaitGroup` 标记完成，并执行以下操作：

*   等待一定时间（1-5 秒）。
*   获取缓冲区。
*   向缓冲区写入信息。
*   将内容复制到标准输出。
*   释放缓冲区。

睡眠时间为 `1` 秒，加上每 `4` 次循环迭代增加 `1` 秒，最多 `5` 秒：

```go
start := time.Now()
wg := sync.WaitGroup{}
wg.Add(20)
for i := 0; i < 20; i++ {
    go func(v int) {
        time.Sleep(time.Second * time.Duration(1+v/4))
        b := Get()
        defer func() {
            Put(b)
            wg.Done()
        }()
        fmt.Fprintf(b, "Goroutine %2d using %p, after %.0fs\n", v, b, time.Since(start).Seconds())
        fmt.Printf("%s", b.Bytes())
    }(i)
}
wg.Wait()
```





打印输出的信息中也包含缓冲区的内存地址。这将帮助我们确认缓冲区始终是同一个，且没有创建新的缓冲区。

# 切片回收问题

对于具有底层字节切片的数据结构（如 `bytes.Buffer`），在将它们与 `sync.Pool` 或类似的回收机制结合使用时应当小心。让我们修改前面的示例，收集缓冲区的字节而不是将它们打印到标准输出。以下是实现此功能的示例代码：

```go
var (
    list = make([][]byte, 20)
    m sync.Mutex
)
for i := 0; i < 20; i++ {
    go func(v int) {
        time.Sleep(time.Second * time.Duration(1+v/4))
        b := Get()
        defer func() {
            Put(b)
            wg.Done()
        }()
        fmt.Fprintf(b, "Goroutine %2d using %p, after %.0fs\n", v, b, time.Since(start).Seconds())
        m.Lock()
        list[v] = b.Bytes()
        m.Unlock()
    }(i)
}
wg.Wait()
```

那么，当我们打印字节切片列表时会发生什么？我们可以在以下示例中看到：

```go
for i := range list {
    fmt.Printf("%d - %s", i, list[i])
}
```

我们得到了一个意外的结果，因为缓冲区已被覆盖。这是因为缓冲区复用了同一个底层切片，并且每次新用法都会覆盖内容。

解决此问题的方法通常是执行字节拷贝，而不仅仅是赋值：

```go
m.Lock()
list[v] = make([]byte, b.Len())
copy(list[v], b.Bytes())
m.Unlock()
```

# 条件变量

在并发编程中，条件变量是一种同步机制，包含等待同一条件验证的线程。在 Go 中，这意味着有一些 goroutine 在等待某事发生。我们已经使用通道实现了这一点，仅有一个 goroutine 在等待，如下例所示：

```go
ch := make(chan struct{})
go func() {
    // do something
    ch <- struct{}{}
}()
go func() {
    // wait for condition
    <-ch
    // do something else
}
```

这种方法仅限于单个 goroutine，但可以通过从发送消息切换为关闭通道来改进，以支持更多监听器：

```go
go func() {
    // do something
    close(ch)
}()
for i := 0; i < n; i++ {
    go func() {
        // wait for condition
        <-ch
        // do something else
    }()
}
```

关闭通道适用于多个监听器，但不允许它们在通道关闭后继续使用该通道。

`sync.Cond` 类型是一种工具，它使以更好的方式处理所有这些行为成为可能。它在实现中使用了一个锁，并暴露了三个方法：

*   `Broadcast`：唤醒所有等待条件的 goroutine。
*   `Signal`：如果至少有一个 goroutine 在等待条件，则唤醒其中一个。
*   `Wait`：解锁锁，挂起 goroutine 的执行，稍后恢复执行并再次加锁，等待 `Broadcast` 或 `Signal`。

虽然不是必须的，但 `Broadcast` 和 `Signal` 操作可以在持有锁时进行，在之前加锁，之后释放。`Wait` 方法要求调用前持有锁，并在条件被使用后解锁。

让我们创建一个使用 `sync.Cond` 来编排多个 goroutine 的并发应用程序。我们将从命令行获得一个提示，每条记录将被写入一系列文件。我们将有一个主结构体来保存所有数据：

```go
type record struct {
    sync.Mutex
    buf string
    cond *sync.Cond
    writers []io.Writer
}
```

我们将监控的条件是 `buf` 字段的变化。在 `Run` 方法中，`record` 结构体将为每个 writer 启动多个 goroutine。每个 goroutine 将等待条件触发并写入其文件：

```go
func (r *record) Run() {
    for i := range r.writers {
        go func(i int) {
            for {
                r.Lock()
                r.cond.Wait()
                fmt.Fprintf(r.writers[i], "%s\n", r.buf)
                r.Unlock()
            }
        }(i)
    }
}
```

我们可以看到，我们在使用 `Wait` 前加锁，在使用完条件所指的值后解锁。主函数将根据提供的命令行参数创建一个 record 和一系列文件：

```go
// let's make sure we have at least a file argument
if len(os.Args) < 2 {
    log.Fatal("Please specify at least a file")
}
r := record{
    writers: make([]io.Writer, len(os.Args)-1),
}
r.cond = sync.NewCond(&r)
for i, v := range os.Args[1:] {
    f, err := os.Create(v)
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()
    r.writers[i] = f
}
r.Run()
```

然后我们将使用 `bufio.Scanner` 读取行并广播 `buf` 字段的变化。我们还将接受一个特殊值 `\q` 作为退出命令：

```go
scanner := bufio.NewScanner(os.Stdin)
for {
    fmt.Printf(":> ")
    if !scanner.Scan() {
        break
    }
    r.Lock()
    r.buf = scanner.Text()
    r.Unlock()
    switch {
    case r.buf == `\q`:
        return
    default:
        r.cond.Broadcast()
    }
}
```

我们可以看到，`buf` 的更改是在持有锁时进行的，随后调用 `Broadcast`，这会唤醒所有等待条件的 goroutine。

# 同步 Map

Go 内置的 map 不是线程安全的，因此尝试从不同的 goroutine 写入可能会导致运行时错误：`concurrent map writes`。我们可以使用一个尝试并发进行更改的简单程序来验证这一点：

```go
func main() {
    var m = map[int]int{}
    wg := sync.WaitGroup{}
    wg.Add(10)
    for i := 0; i < 10; i++ {
        go func(i int) {
            m[i%5]++
            fmt.Println(m)
            wg.Done()
        }(i)
    }
    wg.Wait()
}
```

在写入时读取也是一个运行时错误，`concurrent map iteration and map write`，我们可以通过运行以下示例看到这一点：

```go
func main() {
    var m = map[int]int{}
    var done = make(chan struct{})
    go func() {
        for i := 0; i < 100; i++ {
            time.Sleep(time.Nanosecond)
            m[i]++
        }
        close(done)
    }()
    for {
        time.Sleep(time.Nanosecond)
        fmt.Println(len(m), m)
        select {
        case <-done:
            return
        default:
        }
    }
}
```

有时，尝试迭代 map（正如 `Print` 语句所做的那样）可能会导致 panic，例如 `index out of range`，因为内部切片可能已被分配在别处。

一种使 map 并发安全的非常简单的策略是将其与 `sync.Mutex` 或 `sync.RWMutex` 结合使用。这使得在执行操作时可以锁定 map：

```go
type m struct {
    sync.Mutex
    m map[int]int
}
```

我们使用 map 来获取或设置值，例如如下所示：

```go
func (m *m) Get(key int) int {
    m.Lock()
    a := m.m[key]
    m.Unlock()
    return a
}

func (m *m) Put(key, value int) {
    m.Lock()
    m.m[key] = value
    m.Unlock()
}
```

我们还可以传递一个接受键值对并为每个元组执行的函数，同时锁定 map：

```go
func (m *m) Range(f func(k, v int)) {
    m.Lock()
    for k, v := range m.m {
        f(k, v)
    }
    m.Unlock()
}
```

Go 1.9 引入了一个名为 `sync.Map` 的结构，它正是做这件事的。它是一个非常通用的 `map[interface{}]interface{}`，使得可以使用以下方法执行线程安全操作：

*   `Load`：获取给定键在 map 中的值。
*   `Store`：在 map 中为给定键设置值。
*   `Delete`：从 map 中移除给定键的条目。
*   `LoadOrStore`：如果键存在则返回其值，否则返回存储的值。
*   `Range`：为 map 中的每个键值对调用一个返回布尔值的函数。如果返回 `false` 则停止迭代。





## sync.Map 示例

我们可以在以下片段中看到其工作原理，其中我们尝试同时进行多次写入：

```go
func main() {
    var m = sync.Map{}
    var wg = sync.WaitGroup{}
    wg.Add(1000)
    for i := 0; i < 1000; i++ {
        go func(i int) {
            m.LoadOrStore(i, i)
            wg.Done()
        }(i)
    }
    wg.Wait()
    i := 0
    m.Range(func(k, v interface{}) bool {
        i++
        return true
    })
    fmt.Println(i)
}
```

该应用程序与使用普通 `Map` 的版本不同，不会崩溃并能执行所有操作。

# 信号量

在上一章中，我们了解了如何使用通道创建加权信号量。在实验性 `sync` 包中有一个更好的实现。可在以下位置找到：`golang.org/x/sync/semaphore`。

该实现允许创建新的信号量，并通过 `semaphore.NewWeighted` 指定权重。

可以使用 `Acquire` 方法获取配额，指定要获取的配额数量。可以使用 `Release` 方法释放配额，如下例所示：

```go
func main() {
    s := semaphore.NewWeighted(int64(10))
    ctx := context.Background()
    for i := 0; i < 20; i++ {
        if err := s.Acquire(ctx, 1); err != nil {
            log.Fatal(err)
        }
        go func(i int) {
            fmt.Println(i)
            s.Release(1)
        }(i)
    }
    time.Sleep(time.Second)
}
```

获取配额除了需要数量参数外，还需要 `context.Context` 参数。这是 Go 中另一个并发工具，我们将在下一章中学习如何使用。

# 原子操作

`sync` 包提供同步原语，其底层使用线程安全的整数和指针操作。我们可以在另一个名为 `sync/atomic` 的包中找到这些功能，该包可用于创建针对用户用例的特定工具，具有更好的性能和更少的内存使用。

## 整数操作

针对不同整数类型的指针，有一系列函数：

*   `int32`
*   `int64`
*   `uint32`
*   `uint64`
*   `uintptr`

这包括一种表示指针的特定整数类型 `uintptr`。这些类型可用的操作如下：

*   `Load`：从指针检索整数值
*   `Store`：将整数值存储到指针
*   `Add`：将指定的增量添加到指针值
*   `Swap`：在指针中存储新值并返回旧值
*   `CompareAndSwap`：仅当旧值与指定值相同时，才用新值替换旧值

## Clicker 计数器

此函数有助于轻松定义线程安全组件。一个明显的例子是简单的整数计数器，它使用 `Add` 修改计数器，`Load` 检索当前值，`Store` 重置它：

```go
type clicker int32

func (c *clicker) Click() int32 {
    return atomic.AddInt32((*int32)(c), 1)
}

func (c *clicker) Reset() {
    atomic.StoreInt32((*int32)(c), 0)
}

func (c *clicker) Value() int32 {
    return atomic.LoadInt32((*int32)(c))
}
```

我们可以在一个简单的程序中看到它的实际运行，该程序尝试并发读取、写入和重置计数器。

我们定义 `clicker` 和 `WaitGroup`，并向等待组添加正确数量的元素：

```go
c := clicker(0)
wg := sync.WaitGroup{}
// 2*iteration + reset at 5
wg.Add(21)
```

我们可以启动一组执行不同操作的 goroutine，例如：10 次读取、10 次增加和一次重置：

```go
for i := 0; i < 10; i++ {
    go func() {
        c.Click()
        fmt.Println("click")
        wg.Done()
    }()
    go func() {
        fmt.Println("load", c.Value())
        wg.Done()
    }()
    if i == 0 || i%5 != 0 {
        continue
    }
    go func() {
        c.Reset()
        fmt.Println("reset")
        wg.Done()
    }()
}
wg.Wait()
```

我们将看到 `clicker` 按预期工作，在没有竞态条件的情况下执行并发求和。

## 线程安全浮点数

`atomic` 包仅提供整数原语，但由于 `float32` 和 `float64` 与 `int32` 和 `int64` 使用相同的数据结构存储，我们可以利用它们创建原子浮点值。

技巧是使用 `math.Float64bits` 函数获取浮点数的无符号整数表示，并使用 `math.Float64frombits` 函数将无符号整数转换为浮点数。让我们看看 `float64` 的工作原理：

```go
type f64 uint64

func uf(u uint64) (f float64) { return math.Float64frombits(u) }
func fu(f float64) (u uint64) { return math.Float64bits(f) }

func newF64(f float64) *f64 {
    v := f64(fu(f))
    return &v
}

func (f *f64) Load() float64 {
    return uf(atomic.LoadUint64((*uint64)(f)))
}

func (f *f64) Store(s float64) {
    atomic.StoreUint64((*uint64)(f), fu(s))
}
```

创建 `Add` 函数稍微复杂一些。我们需要用 `Load` 获取值，然后进行比较并交换。由于此操作可能会失败，因为 `Load` 是原子操作而 **比较并交换** (**CAS**) 是另一个原子操作，我们在循环中不断尝试直到成功：

```go
func (f *f64) Add(s float64) float64 {
    for {
        old := f.Load()
        new := old + s
        if f.CompareAndSwap(old, new) {
            return new
        }
    }
}

func (f *f64) CompareAndSwap(old, new float64) bool {
    return atomic.CompareAndSwapUint64((*uint64)(f), fu(old), fu(new))
}
```

## 线程安全布尔值

我们也可以使用 `int32` 表示布尔值。整数 `0` 表示 `false`，`1` 表示 `true`，从而创建线程安全的布尔条件：

```go
type cond int32

func (c *cond) Set(v bool) {
    a := int32(0)
    if v {
        a++
    }
    atomic.StoreInt32((*int32)(c), a)
}

func (c *cond) Value() bool {
    return atomic.LoadInt32((*int32)(c)) != 0
}
```

这将允许我们将 `cond` 类型用作线程安全的布尔值。

## 指针操作

Go 中的指针变量存储在 `intptr` 变量中，这是足以容纳内存地址的整数。`atomic` 包使得为其他整数类型执行相同操作成为可能。有一个包允许不安全指针操作，提供用于原子操作的 `unsafe.Pointer` 类型。

在以下示例中，我们定义两个整数变量及其相应的整数指针。然后执行第一个指针与第二个指针的交换：

```go
v1, v2 := 10, 100
p1, p2 := &v1, &v2
log.Printf("P1: %v, P2: %v", *p1, *p2)
atomic.SwapPointer((*unsafe.Pointer)(unsafe.Pointer(&p1)), unsafe.Pointer(p2))
log.Printf("P1: %v, P2: %v", *p1, *p2)
v1 = -10
log.Printf("P1: %v, P2: %v", *p1, *p2)
v2 = 3
log.Printf("P1: %v, P2: %v", *p1, *p2)
```

交换后，两个指针现在都指向第二个变量；对第一个值的任何更改都不会影响指针。更改第二个变量会改变指针所引用的值。

## atomic.Value

我们可以使用的最简单工具是 `atomic.Value`。它持有 `interface{}` 并使其能够以线程安全的方式读写。它暴露两个方法 `Store` 和 `Load`，用于设置或检索值。与其他线程安全工具一样，`atomic.Value` 在首次使用后不得复制。

我们可以尝试让多个 goroutine 设置和读取同一个值。每次加载操作都能获取最新存储的值，并发不会引发错误：

```go
func main() {
    var (
        v atomic.Value
        wg sync.WaitGroup
    )
    wg.Add(20)
    for i := 0; i < 10; i++ {
        go func(i int) {
            fmt.Println("load", v.Load())
            wg.Done()
        }(i)
        go func(i int) {
            v.Store(i)
            fmt.Println("store", i)
            wg.Done()
        }(i)
    }
    wg.Wait()
}
```




这是一个非常通用的容器；它可以用于任何类型的 `variable`，且 `variable` 类型应当能够从一种变为另一种。如果具体类型发生变化，会导致 `method` `panic()`；同样的情况也适用于 `nil` `empty interface`。

## 内部原理

`sync.Value` 类型将其数据存储在一个非导出的 `interface{}` 中，如源代码所示：

```go
type Value struct {
    v interface{}
}
```

它使用 `unsafe` 包中的一个类型将该结构转换为另一个结构，后者与 `interface{}` 具有相同的数据结构：

```go
type ifaceWords struct {
    typ  unsafe.Pointer
    data unsafe.Pointer
}
```

具有完全相同内存布局的两种类型可以通过这种方式相互转换，从而绕过 Go 的类型安全。这使得可以对指针使用 `atomic` 操作，并执行线程安全的 `Store()` 和 `Load()` 操作。

为了获取写入值的锁，`atomic.Value` 在类型字段中使用带有 `unsafe.Pointer(^uintptr(0))` 值（即 `0xffffffff`）的比较并交换操作；它会更改值并将类型替换为正确的类型。

同样地，加载操作会循环等待，直到类型不再是 `0xffffffff`，然后尝试读取值。

利用这种权宜之计，`atomic.Value` 能够使用其他 `atomic` 操作来存储和加载任意值。

## 总结

在本章中，我们了解了 Go 标准包中用于同步的工具。它们位于两个包中：`sync` 提供高级工具（如互斥锁），而 `sync/atomic` 执行低级操作。

首先，我们了解了如何使用锁来同步数据。我们看到了如何使用 `sync.Mutex` 来锁定资源，而不论操作类型；以及如何使用 `sync.RWMutex` 允许并发读取并阻塞写入。使用后者时应当小心，因为连续的读取可能会延迟写入。

接下来，我们了解了如何跟踪正在运行的操作，以便等待一系列 goroutine 结束，使用的是 `sync.WaitGroup`。它充当当前 goroutine 的线程安全计数器，并通过 `Wait()` 方法使当前 goroutine 休眠，直到计数器归零。

此外，我们还检查了用于执行一次性功能的 `sync.Once` 结构，它允许实现线程安全的单例模式，例如。然后我们使用 `sync.Pool` 来复用实例，而不是在可能的情况下创建新实例。池所需的只是一个返回新实例的函数。

`sync.Cond` 结构体表示一个特定条件，并使用一个锁来更改它，允许 goroutine 等待该条件的变化。可以使用 `Signal()` 将变化通知单个 goroutine，或使用 `Broadcast()` 通知所有 goroutine。该包还提供了线程安全的 `sync.Map`。

最后，我们了解了 `atomic` 的功能，它们主要是整数的线程安全操作：加载、保存、添加、交换和 CAS。我们还看到了 `atomic.Value`，它使得可以并发地更改 `interface{}` 的值，并且不允许在首次更改后改变其类型。

下一章将介绍 Go 并发中引入的最新元素：`Context`，它是一个处理截止时间、取消信号等功能的接口。

## 问题

1.  什么是竞态条件？
2.  当尝试对 map 并发执行读写操作时会发生什么？
3.  `Mutex` 和 `RWMutex` 有什么区别？
4.  为什么等待组很有用？
5.  `Once` 的主要用途是什么？
6.  如何使用 `Pool`？
7.  使用原子操作有什么优势？
