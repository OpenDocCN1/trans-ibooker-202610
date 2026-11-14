## 使用 Context 进行协调

本章介绍相对较新的 `context` 包及其在并发编程中的用法。它通过定义一个独特的接口，成为一个非常强大的工具，该接口在标准库的许多不同位置以及许多第三方包中被使用。

本章将涵盖以下主题：

*   理解什么是 context
*   学习其在标准库中的用法
*   创建一个使用 context 的包

## 技术要求

本章要求安装 Go 并设置好你喜欢的编辑器。更多信息请参阅 第 3 章 *Go 概览*。

## 理解 context

Context 是一个相对较新的组件，在 1.7 版本加入标准库。它是用于 goroutine 间同步的接口，最初由 Go 团队内部使用，最终成为语言的核心部分。

### 接口

包中的主要实体是 `Context` 本身，它是一个接口。它只有四个方法：

```go
type Context interface {
    Deadline() (deadline time.Time, ok bool)
    Done() <-chan struct{}
    Err() error
    Value(key interface{}) interface{}
}
```

让我们来了解这四个方法：

*   `Deadline()`：返回 context 应该被取消的时间，以及一个布尔值，当没有截止时间时该值为 `false`
*   `Done()`：返回一个只接收的空结构体通道，用于发出 context 应该被取消的信号
*   `Err()`：当 `done` 通道打开时返回 `nil`；否则返回 context 取消的原因
*   `Value()`：返回当前 context 中与键关联的值，如果该键没有值则返回 `nil`

`Context` 拥有的方法比标准库中的其他接口多，后者通常只有一两个方法。其中三个方法紧密相关：

*   `Deadline()` 是取消的时间
*   `Done()` 发出 context 完成的信号
*   `Err()` 返回取消的原因

最后一个方法 `Value()` 返回与特定键关联的值。包的其余部分是一系列函数，允许你创建不同类型的 context。让我们浏览包中的各种函数，看看创建和装饰 context 的各种工具。

### 默认 context

`TODO()` 和 `Background()` 函数无需任何输入参数即可返回 `context.Context`。返回的值是一个空 context，尽管它们的区别仅在于语义上。

#### Background

`Background()` 是一个空 context，它不会被取消，没有截止时间，也不持有任何值。它主要被 `main()` 函数用作根 context，或用于测试目的。以下是该 context 的示例代码：

```go
func main() {
    ctx := context.Background()
    done := ctx.Done()
    for i := 0; ; i++ {
        select {
        case <-done:
            return
        case <-time.After(time.Second):
            fmt.Println("tick", i)
        }
    }
}
```

完整示例请见：<https://play.golang.org/p/y_3ip7sdPnx>。

我们可以看到，在示例中，循环会无限进行下去，因为 context 永远不会完成。

#### TODO

`TODO()` 是另一个空 context，当 context 的作用域不明确或 context 类型尚不可用时应使用它。它的使用方式与 `Background()` 完全相同。事实上，在内部实现上，它们是同一个东西；区别仅在于语义。如果我们查看源代码，它们具有完全相同的定义：

```go
var (
    background = new(emptyCtx)
    todo       = new(emptyCtx)
)
```

该代码的来源可在 <https://golang.org/pkg/context/?m=all#pkg-variables> 找到。

这些基本 context 可以使用包中的其他函数进行扩展。它们将作为装饰器，为 context 添加更多功能。

### 取消、超时和截止时间

我们刚才看到的 context 永远不会被取消，但该包提供了不同的选项来添加此功能。

#### 取消



# Go Context 包

`context.WithCancel` 装饰器函数接收一个 context，并返回另一个 context 和一个名为 `cancel` 的函数。返回的 context 将是该 context 的副本，但拥有一个不同的 `done` 通道（标记当前 context 是否完成的通道），该通道会在父 context 关闭时或调用 `cancel` 函数时关闭——以先发生者为准。

在下面的示例中，我们可以看到我们在调用 `cancel` 函数前等待了几秒钟，程序正确终止。`Err` 的值是 `context.Canceled` 变量：

```go
func main() {
    ctx, cancel := context.WithCancel(context.Background())
    time.AfterFunc(time.Second*5, cancel)
    done := ctx.Done()
    for i := 0; ; i++ {
        select {
        case <-done:
            fmt.Println("exit", ctx.Err())
            return
        case <-time.After(time.Second):
            fmt.Println("tick", i)
        }
    }
}
```

完整示例请见此处：[`play.golang.org/p/fNHLIZL8e0L`](https://play.golang.org/p/fNHLIZL8e0L)。

## 截止时间

`context.WithDeadline` 是另一个装饰器，它指定一个 `time.Time` 类型的截止时间，并将其应用到另一个 context 上。如果已经存在一个截止时间且早于提供的截止时间，则忽略指定的截止时间。如果在截止时间到达时 `done` 通道仍然开放，它会自动关闭。

在下面的示例中，我们将截止时间设置为 5 秒后，并在 10 秒后调用 `cancel`。截止时间先于取消到达，`Err` 返回一个 `context.DeadlineExceeded` 错误：

```go
func main() {
    ctx, cancel := context.WithDeadline(context.Background(), 
         time.Now().Add(5*time.Second))
    time.AfterFunc(time.Second*10, cancel)
    done := ctx.Done()
    for i := 0; ; i++ {
        select {
        case <-done:
            fmt.Println("exit", ctx.Err())
            return
        case <-time.After(time.Second):
            fmt.Println("tick", i)
        }
    }
}
```

完整示例请见此处：[`play.golang.org/p/iyuOmd__CGH`](https://play.golang.org/p/iyuOmd__CGH)。

我们可以看到前面的示例表现完全符合预期。它会每秒打印几次 `tick` 语句，直到截止时间到达并返回错误。

## 超时

最后一个与取消相关的装饰器是 `context.WithTimeout`，它允许你指定一个 `time.Duration` 以及 context，并在超时过后自动关闭 `done` 通道。

如果存在活动的截止时间，新值仅在早于父级时才生效。我们可以看一个几乎相同的示例，除了 context 定义不同，并得到与截止时间示例相同的结果：

```go
func main() {
    ctx, cancel := context.WithTimeout(context.Background(),5*time.Second)
    time.AfterFunc(time.Second*10, cancel)
    done := ctx.Done()
    for i := 0; ; i++ {
        select {
        case <-done:
            fmt.Println("exit", ctx.Err())
            return
        case <-time.After(time.Second):
            fmt.Println("tick", i)
        }
    }
}
```

完整示例请见此处：[`play.golang.org/p/-Zp63_e0zYD`](https://play.golang.org/p/-Zp63_e0zYD)。

## 键和值

`context.WithValue` 函数创建父 context 的副本，该副本将给定的键与指定的值关联。其作用域保存与单个请求相关的值，这些值在请求处理期间有效，不应用于其他作用域，例如可选函数参数。

键应该是可比较的，最好避免使用 `string` 值，因为使用 context 的两个不同包可能会覆盖彼此的值。建议使用用户定义的具体类型，例如 `struct{}`。

这里，我们可以看到一个示例，我们获取一个基础 context 并为每个 goroutine 添加不同的值，使用空结构体作为键：

```go
type key struct{}

func main() {
    ctx, canc := context.WithCancel(context.Background())
    wg := sync.WaitGroup{}
    wg.Add(5)
    for i := 0; i < 5; i++ {
        go func(ctx context.Context) {
            v := ctx.Value(key{})
            fmt.Println("key", v)
            wg.Done()
            <-ctx.Done()
            fmt.Println(ctx.Err(), v)
        }(context.WithValue(ctx, key{}, i))
    }
    wg.Wait()
    canc()
    time.Sleep(time.Second)
}
```

完整示例请见此处：[`play.golang.org/p/lM61u_QKEW1`](https://play.golang.org/p/lM61u_QKEW1)。

我们还可以看到，取消父 context 会取消其他 context。另一种有效的键类型可以是导出的指针值，即使底层数据相同，它们也不会相同：

```go
type key *int

func main() {
    k := new(key)
    ctx, canc := context.WithCancel(context.Background())
    wg := sync.WaitGroup{}
    wg.Add(5)
    for i := 0; i < 5; i++ {
        go func(ctx context.Context) {
            v := ctx.Value(k)
            fmt.Println("key", v, ctx.Value(new(key)))
            wg.Done()
            <-ctx.Done()
            fmt.Println(ctx.Err(), v)
        }(context.WithValue(ctx, k, i))
    }
    wg.Wait()
    canc()
    time.Sleep(time.Second)
}
```

完整示例请见此处：[`play.golang.org/p/05XJwWF0-0n`](https://play.golang.org/p/05XJwWF0-0n)。

我们可以看到，定义一个具有相同底层值的键指针不会返回预期的值。

## 标准库中的 Context

现在我们已经介绍了该包的内容，我们将看看如何在标准包或应用程序中使用它们。Context 用于标准包中的一些函数和方法，主要是网络包：

- `http.Server` 在 `Shutdown` 方法中使用它，以便完全控制超时或取消操作。
- `http.Request` 允许你使用 `WithContext` 方法设置 context。它也允许你使用 `Context` 获取当前 context。
- 在 `net` 包中，`Listen`、`Dial` 和 `Lookup` 都有使用 `Context` 来控制截止时间和超时的版本。
- 在 `database/sql` 包中，context 用于停止或设置许多不同操作的超时。

## HTTP 请求

在官方包引入之前，每个 HTTP 相关框架都使用自己的 context 版本来存储与 HTTP 请求相关的数据。这导致了碎片化，如果不重写中间件或任何特定的绑定代码，就无法复用处理器和中间件。

## 传递作用域值

`http.Request` 中引入 `context.Context` 试图通过定义一个可以赋值、恢复并在各种处理器中使用的单一接口来解决这个问题。

缺点是 context 不会自动分配给请求，且 context 值不能被回收。不应该有充分的理由这样做，因为 context 应该存储特定于某个包或作用域的数据，包本身应该是唯一能够与它们交互的实体。

一个好的模式是使用唯一的非导出键类型，结合辅助函数来获取或设置某个值：

```go
type keyType struct{}

var key = &keyType{}

func WithKey(ctx context.Context, value string) context.Context {
    return context.WithValue(ctx, key, value)
}

func GetKey(ctx context.Context) (string, bool) {
    v := ctx.Value(key)
    if v == nil {
        return "", false
    }
    return v.(string), true
}
```

Context 请求是标准库中唯一一种将其存储在数据结构中（使用 `WithContext` 方法）并通过 `Context` 方法访问的情况。这样做是为了不破坏现有代码，并保持 Go 1 的兼容性承诺。

完整示例请见此处：[`play.golang.org/p/W6gGp_InoMp`](https://play.golang.org/p/W6gGp_InoMp)。

## 请求取消



Context 的一个良好用例是在使用 `http.Client` 执行 HTTP 请求时进行取消和超时控制，它会根据上下文自动处理中断。以下示例正是演示了这一点：

```go
func main() {
    const addr = "localhost:8080"
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        time.Sleep(time.Second * 5)
    })
    go func() {
        if err := http.ListenAndServe(addr, nil); err != nil {
            log.Fatalln(err)
        }
    }()
    req, _ := http.NewRequest(http.MethodGet, "http://"+addr, nil)
    ctx, canc := context.WithTimeout(context.Background(), time.Second*2)
    defer canc()
    time.Sleep(time.Second)
    if _, err := http.DefaultClient.Do(req.WithContext(ctx)); err != nil {
        log.Fatalln(err)
    }
}
```

上下文取消方法也可用于中断传递给客户端的当前 HTTP 请求。在调用不同端点并返回最先接收到的结果的场景中，取消其他请求是个好主意。

让我们创建一个应用程序，它在不同的搜索引擎上运行查询，并返回响应最快的那个的结果，同时取消其他的请求。我们可以创建一个 Web 服务器，它有一个唯一的端点，在 0 到 10 秒内响应：

```go
const addr = "localhost:8080"
http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
    d := time.Second * time.Duration(rand.Intn(10))
    log.Println("wait", d)
    time.Sleep(d)
})
go func() {
    if err := http.ListenAndServe(addr, nil); err != nil {
        log.Fatalln(err)
    }
}()
```

我们可以为请求使用可取消的上下文，并结合等待组来同步请求的结束。每个 goroutine 将创建一个请求并尝试通过通道发送结果。既然我们只关心第一个结果，我们将使用 `sync.Once` 来限制发送：

```go
ctx, canc := context.WithCancel(context.Background())
ch, o, wg := make(chan int), sync.Once{}, sync.WaitGroup{}
wg.Add(10)
for i := 0; i < 10; i++ {
    go func(i int) {
        defer wg.Done()
        req, _ := http.NewRequest(http.MethodGet, "http://"+addr, nil)
        if _, err := http.DefaultClient.Do(req.WithContext(ctx)); err != nil {
            log.Println(i, err)
            return
        }
        o.Do(func() { ch <- i })
    }(i)
}
log.Println("received", <-ch)
canc()
log.Println("cancelling")
wg.Wait()
```

当该程序运行时，我们将看到其中一个请求成功完成并被发送到通道，而其他的请求则被取消或忽略。

# HTTP 服务器

`net/http` 包有多处使用 context 的地方，包括停止监听器或作为请求的一部分。

## 关闭

`http.Server` 允许我们为关闭操作传递一个上下文。这使我们能够利用上下文的一些功能，例如取消和超时。我们可以定义一个带有其 `mux` 和可取消上下文的新服务器：

```go
mux := http.NewServeMux()
server := http.Server{
    Addr: ":3000",
    Handler: mux,
}
ctx, canc := context.WithCancel(context.Background())
defer canc()
mux.HandleFunc("/shutdown", func(w http.ResponseWriter, r *http.Request) {
    w.Write([]byte("OK"))
    canc()
})
```

我们可以在一个单独的 goroutine 中启动服务器：

```go
go func() {
    if err := server.ListenAndServe(); err != nil {
        if err != http.ErrServerClosed {
            log.Fatal(err)
        }
    }
}()
```

当调用关闭端点并调用取消函数时，上下文将变为完成状态。我们可以等待该事件，然后使用另一个带有超时的上下文来调用关闭方法：

```go
select {
case <-ctx.Done():
    ctx, canc := context.WithTimeout(context.Background(), time.Second*5)
    defer canc()
    if err := server.Shutdown(ctx); err != nil {
        log.Fatalln("Shutdown:", err)
    } else {
        log.Println("Shutdown:", "ok")
    }
}
```

这将允许我们在超时内有效地终止服务器，超时后它将报错终止。

## 传递值

服务器中 context 的另一个用法是在不同的 HTTP 处理器之间传播值和取消信号。让我们看一个例子，其中每个请求都有一个唯一的整数键。我们将使用几个函数，它们类似于我们之前使用整数作为值的例子。新键的生成将使用 `atomic` 完成：

```go
type keyType struct{}

var key = &keyType{}

var counter int32

func WithKey(ctx context.Context) context.Context {
    return context.WithValue(ctx, key, atomic.AddInt32(&counter, 1))
}

func GetKey(ctx context.Context) (int32, bool) {
    v := ctx.Value(key)
    if v == nil {
        return 0, false
    }
    return v.(int32), true
}
```

现在，我们可以定义另一个函数，它接受任意 HTTP 处理器，并在必要时创建上下文，然后将键添加到其中：

```go
func AssignKeyHandler(h http.Handler) http.HandlerFunc {
    return func(w http.ResponseWriter, r *http.Request) {
        ctx := r.Context()
        if ctx == nil {
            ctx = context.Background()
        }
        if _, ok := GetKey(ctx); !ok {
            ctx = WithKey(ctx)
        }
        h.ServeHTTP(w, r.WithContext(ctx))
    }
}
```

通过这样做，我们可以定义一个非常简单的处理器，用于服务某个根目录下的文件。该函数将使用上下文中的键来正确记录日志信息。它还会在尝试服务文件之前检查文件是否存在：

```go
func ReadFileHandler(root string) http.HandlerFunc {
    root = filepath.Clean(root)
    return func(w http.ResponseWriter, r *http.Request) {
        k, _ := GetKey(r.Context())
        path := filepath.Join(root, r.URL.Path)
        log.Printf("[%d] requesting path %s", k, path)
        if !strings.HasPrefix(path, root) {
            http.Error(w, "not found", http.StatusNotFound)
            log.Printf("[%d] unauthorized %s", k, path)
            return
        }
        if stat, err := os.Stat(path); err != nil || stat.IsDir() {
            http.Error(w, "not found", http.StatusNotFound)
            log.Printf("[%d] not found %s", k, path)
            return
        }
        http.ServeFile(w, r, path)
        log.Printf("[%d] ok: %s", k, path)
    }
}
```

我们可以组合这些处理器来服务不同文件夹的内容，例如用户主目录或临时目录：

```go
home, err := os.UserHomeDir()
if err != nil {
    log.Fatal(err)
}
tmp := os.TempDir()
mux := http.NewServeMux()
server := http.Server{
    Addr: ":3000",
    Handler: mux,
}

mux.Handle("/tmp/", http.StripPrefix("/tmp/", AssignKeyHandler(ReadFileHandler(tmp))))
mux.Handle("/home/", http.StripPrefix("/home/", AssignKeyHandler(ReadFileHandler(home))))
if err := server.ListenAndServe(); err != nil {
    if err != http.ErrServerClosed {
        log.Fatal(err)
    }
}
```

我们使用 `http.StripPrefix` 来移除路径的第一部分并获取相对路径，然后将其传递给底层的处理器。生成的服务器将使用 context 在处理器之间传递键值——这允许我们创建另一个类似的处理器，并使用 `AssignKeyHandler` 函数包装处理器，以及使用 `GetKey(r.Context())` 在我们的处理器内部访问键。

# TCP 拨号

网络包提供了与上下文相关的功能，例如在拨号或监听传入连接时取消拨号。它允许我们在拨号连接时使用上下文的超时和取消功能。

## 取消连接

为了测试上下文在 TCP 连接中的用法，我们可以创建一个带有 TCP 服务器的 goroutine，该服务器会在启动监听器之前等待一段时间：



```
addr := os.Args[1]
go func() {
    time.Sleep(time.Second)
    listener, err := net.Listen("tcp", addr)
    if err != nil {
        log.Fatalln("Listener:", addr, err)
    }
    c, err := listener.Accept()
    if err != nil {
        log.Fatalln("Listener:", addr, err)
    }
    defer c.Close()
}()
```

我们可以使用一个超时时间低于服务器等待时间的 context。为了在拨号操作中使用 context，我们必须使用 `net.Dialer`：

```
ctx, canc := context.WithTimeout(context.Background(),   
    time.Millisecond*100)
defer canc()
conn, err := (&net.Dialer{}).DialContext(ctx, "tcp", os.Args[1])
if err != nil {
    log.Fatalln("-> Connection:", err)
}
log.Println("-> Connection to", os.Args[1])
conn.Close()
```

应用程序将尝试连接一小段时间，但最终会在 context 过期时放弃，并返回一个错误。

在您想要从一系列端点建立单个连接的情况下，context 取消将是一个完美的用例。所有连接尝试将共享同一个 context，第一个成功拨号的连接将调用取消函数，从而停止其他尝试。我们将创建一个监听我们将要尝试调用的地址之一的单个服务器：

```
list := []string{
    "localhost:9090",
    "localhost:9091",
    "localhost:9092",
}
go func() {
    listener, err := net.Listen("tcp", list[0])
    if err != nil {
        log.Fatalln("Listener:", list[0], err)
    }
    time.Sleep(time.Second * 5)
    c, err := listener.Accept()
    if err != nil {
        log.Fatalln("Listener:", list[0], err)
    }
    defer c.Close()
}()
```

然后，我们可以尝试拨号所有三个地址，并一旦有一个连接成功就取消 context。我们将使用 `WaitGroup` 来同步 goroutine 的结束：

```
ctx, canc := context.WithTimeout(context.Background(), time.Second*10)
defer canc()
wg := sync.WaitGroup{}
wg.Add(len(list))
for _, addr := range list {
    go func(addr string) {
        defer wg.Done()
        conn, err := (&net.Dialer{}).DialContext(ctx, "tcp", addr)
        if err != nil {
            log.Println("-> Connection:", err)
            return
        }
        log.Println("-> Connection to", addr, "cancelling context")
        canc()
        conn.Close()
    }(addr)
}
wg.Wait()
```

我们将在该程序的输出中看到一个连接成功，随后是其他尝试的取消错误。

# 数据库操作

本书不涉及 `sql/database` 包，但为了完整性，值得一提的是它也使用了 context。其大多数操作都有对应的带 context 版本，例如：

*   开启新事务
*   执行查询
*   Ping 数据库
*   准备查询

这结束了标准库中使用 context 的包的介绍。接下来，我们将尝试使用 context 来构建一个包，允许该包的用户取消请求。

# 实验性包

实验性包中使用 context 的一个显著例子是我们已经看过的 —— `semaphore`。现在我们对 context 的用途有了更好的理解，应该很清楚为什么 acquire 操作也接受一个 context 作为参数。

在创建应用程序时，我们可以提供一个带有超时或取消功能的 context 并做出相应反应：

```
func main() {
    s := semaphore.NewWeighted(int64(5))
    ctx, canc := context.WithTimeout(context.Background(), time.Second)
    defer canc()
    wg := sync.WaitGroup{}
    wg.Add(20)
    for i := 0; i < 20; i++ {
        go func(i int) {
            defer wg.Done()
            if err := s.Acquire(ctx, 1); err != nil {
                fmt.Println(i, err)
                return
            }
            go func(i int) {
                fmt.Println(i)
                time.Sleep(time.Second / 2)
                s.Release(1)
            }(i)
        }(i)
    }
    wg.Wait()
}
```

运行此应用程序将显示 `semaphore` 在第一秒内被获取，但在此之后 context 过期，所有剩余操作均失败。

# 在您的应用程序中使用 Context

如果您的包或应用程序具有可能耗时较长且用户可以取消的操作，或者它们应该有时间限制（如超时或截止时间），那么 `context.Context` 是集成到您的包或应用程序中的完美工具。

# 应避免的事项

尽管 Go 团队已经非常清楚地定义了 context 的作用域，但开发者们以各种方式使用它 —— 其中一些不太正统。让我们看看其中的一些用法，以及有哪些替代方案，而不是诉诸于 context。

# 键使用错误的类型

要避免的第一个做法是使用内置类型作为键。这很有问题，因为它们可能被覆盖，因为具有相同内置值的两个接口被认为是相同的，如下例所示：

```
func main() {
    var a interface{} = "request-id"
    var b interface{} = "request-id"
    fmt.Println(a == b)

    ctx := context.Background()
    ctx = context.WithValue(ctx, a, "a")
    ctx = context.WithValue(ctx, b, "b")
    fmt.Println(ctx.Value(a), ctx.Value(b))
}
```

完整示例请见此处：[`play.golang.org/p/2W3noYQP5eh`](https://play.golang.org/p/2W3noYQP5eh)。

第一条打印指令输出 `true`，并且由于键是按值比较的，第二次赋值覆盖了第一次，导致两个键的值相同。一个潜在的解决方案是使用空结构体自定义类型，或指向内置值的非导出指针。

# 传递参数

有时您可能需要穿过很长的函数调用栈。一个非常诱人的解决方案是使用 context 存储该值，并仅在需要它的函数中调用它。隐藏本应显式传递的必需参数通常不是个好主意。这会导致代码可读性降低，因为无法清楚地说明什么会影响某个函数的执行。

最好还是将函数参数向下传递。如果参数列表变得太长，可以将其分组为一个或多个结构体，以提高可读性。

让我们看看下面的函数：

```
func SomeFunc(ctx context.Context, 
    name, surname string, age int, 
    resourceID string, resourceName string) {}
```

参数可以按以下方式分组：

```
type User struct {
    Name string
    Surname string
    Age int
}

type Resource struct {
    ID string
    Name string
}

func SomeFunc(ctx context.Context, u User, r Resource) {}
```

# 可选参数

Context 应用于传递可选参数，也不应将其用作一种“万能包”，类似于 Python 的 `kwargs` 或 JavaScript 的 `arguments`。将 context 用作行为的替代品可能会非常有问题，因为它可能导致变量遮蔽，正如我们在 `context.WithValue` 示例中看到的那样。

这种方法的另一个主要缺点是隐藏了正在发生的事情，使代码更加晦涩。处理可选值的更好方法是使用指向结构体参数的指针 —— 这允许您通过 `nil` 完全避免传递结构体。

假设您有以下代码：

```
// 此函数有两个必需参数和 4 个可选参数
func SomeFunc(ctx context.Context, arg1, arg2 int, 
    opt1, opt2, opt3, opt4 string) {}
```

通过使用 `Optional`，您将得到类似这样的代码：

```
type Optional struct {
    Opt1 string
    Opt2 string
    Opt3 string
    Opt4 string
}

// 此函数有两个必需参数和 4 个可选参数
func SomeFunc(ctx context.Context, arg1, arg2 int, o *Optional) {}
```

# 全局变量



有些全局变量可以存储在 `context` 中，以便它们能在一系列函数调用中传递。这通常不是好的做法，因为全局变量在应用程序的每个点都可用，所以使用 `context` 来存储和回调它们是毫无意义的，也是对资源和性能的浪费。如果你的包有一些全局变量，你可以使用我们在 `Synchronization with sync and atomic` 中研究的单例模式，以允许从包或应用程序的任何位置访问它们。

# 使用 Context 构建服务

我们现在将重点关注如何创建支持 `context` 用法的包。这将帮助我们整合迄今为止所学的并发知识。我们将尝试创建一个并发文件搜索，它利用通道、Goroutine、同步和 `context`。

## 主接口和用法

包的签名将包含一个 `context`、根文件夹、搜索词以及几个可选参数：

*   **在内容中搜索**：将在文件内容中查找字符串，而不是文件名
*   **排除列表**：将不搜索具有选定名称/名称的文件

该函数看起来会像这样：

```
type Options struct {
    Contents bool
    Exclude []string
}

func FileSearch(ctx context.Context, root, term string, o *Options)
```

由于它应该是一个并发函数，返回类型可以是一个结果通道，该通道可以是错误，也可以是文件中的一系列匹配项。由于我们可以搜索名称或内容，后者可能有不止一个匹配：

```
type Result struct {
    Err error
    File string
    Matches []Match
}

type Match struct {
    Line int
    Text string
}
```

前面的函数将返回一个 `Result` 类型的只接收通道：

```
func FileSearch(ctx context.Context, root, term string, o *Options) <-chan Result
```

在这里，该函数将持续从通道接收值，直到通道关闭：

```
for r := range FileSearch(ctx, directory, searchTerm, options) {
    if r.Err != nil {
        fmt.Printf("%s - error: %s\n", r.File, r.Err)
        continue
    }
    if !options.Contents {
        fmt.Printf("%s - match\n", r.File)
        continue
    }
    fmt.Printf("%s - matches:\n", r.File)
    for _, m := range r.Matches {
        fmt.Printf("\t%d:%s\n", m.Line, m.Text)
    }
}
```

## 退出和入口点

结果通道应由 `context` 的取消或搜索结束来关闭。由于通道不能关闭两次，我们可以使用 `sync.Once` 来避免第二次关闭通道。为了跟踪正在运行的 Goroutine，我们可以使用 `sync.WaitGroup`：

```
ch, wg, once := make(chan Result), sync.WaitGroup{}, sync.Once{}
go func() {
    wg.Wait()
    fmt.Println("* Search done *")
    once.Do(func() {
        close(ch)
    })
}()
go func() {
    <-ctx.Done()
    fmt.Println("* Context done *")
    once.Do(func() {
        close(ch)
    })
}()
```

我们可以为每个文件启动一个 Goroutine，以便我们可以定义一个私有函数作为入口点，然后递归地用于子目录：

```
func fileSearch(ctx context.Context, ch chan<- Result, wg *sync.WaitGroup, file, term string, o *Options)
```

主导出函数将首先向等待组添加一个值。然后它将启动私有函数，将其作为异步进程启动：

```
wg.Add(1)
go fileSearch(ctx, ch, &wg, root, term, o)
```

每个 `fileSearch` 最后要做的事情是调用 `WaitGroup.Done` 以标记当前文件的结束。

## 排除列表

私有函数将在使用 `Done` 方法完成之前减少等待组计数器。除此之外，它首先要做的是检查文件名，以便如果文件名在排除列表中则跳过它：

```
defer wg.Done()
_, name := filepath.Split(file)
if o != nil {
    for _, e := range o.Exclude {
        if e == name {
            return
        }
    }
}
```

如果不是这种情况，我们可以使用 `os.Stat` 检查当前文件的信息，如果不成功则向通道发送错误。由于我们不能冒险向已关闭的通道发送数据导致 panic，我们可以检查 `context` 是否已完成，如果没有，则发送错误：

```
info, err := os.Stat(file)
if err != nil {
    select {
    case <-ctx.Done():
        return
    default:
        ch <- Result{File: file, Err: err}
    }
    return
}
```

## 处理目录

接收到的信息将告诉我们该文件是否为目录。如果是目录，我们可以获取文件列表并处理错误，就像我们之前用 `os.Stat` 做的那样。然后，如果 `context` 尚未完成，我们可以为每个文件启动另一系列搜索。以下代码总结了这些操作：

```
if info.IsDir() {
    files, err := ioutil.ReadDir(file)
    if err != nil {
        select {
        case <-ctx.Done():
            return
        default:
            ch <- Result{File: file, Err: err}
        }
        return
    }
    select {
    case <-ctx.Done():
    default:
        wg.Add(len(files))
        for _, f := range files {
            go fileSearch(ctx, ch, wg, filepath.Join(file, f.Name()), term, o)
        }
    }
    return
}
```

## 检查文件名和内容

如果文件是常规文件而不是目录，我们可以根据指定的选项比较文件名或其内容。检查文件名非常简单：

```
if o == nil || !o.Contents {
    if name == term {
        select {
        case <-ctx.Done():
        default:
            ch <- Result{File: file}
        }
    }
    return
}
```

如果我们要搜索内容，应该打开文件：

```
f, err := os.Open(file)
if err != nil {
    select {
    case <-ctx.Done():
    default:
        ch <- Result{File: file, Err: err}
    }
    return
}
defer f.Close()
```

然后，我们可以逐行读取文件以搜索选定的术语。如果在读取文件时 `context` 过期，我们将停止所有操作：

```
scanner, matches, line := bufio.NewScanner(f), []Match{}, 1
for scanner.Scan() {
    select {
    case <-ctx.Done():
        break
    default:
        if text := scanner.Text(); strings.Contains(text, term) {
            matches = append(matches, Match{Line: line, Text: text})
        }
        line++
    }
}
```

最后，我们可以检查扫描器的错误。如果没有错误且搜索有结果，我们可以将所有匹配项发送到输出通道：

```
select {
case <-ctx.Done():
    break
default:
    if err := scanner.Err(); err != nil {
        ch <- Result{File: file, Err: err}
        return
    }
    if len(matches) != 0 {
        ch <- Result{File: file, Matches: matches}
    }
}
```

在不到 200 行代码中，我们创建了一个并发文件搜索函数，它为每个文件使用一个 Goroutine。它利用通道发送结果，并利用同步原语来协调操作。

## 总结

在本章中，我们了解了较新的包之一 `context` 的全部内容。我们看到 `Context` 是一个有四个方法的简单接口，并且应该用作函数的第一个参数。它的主要作用是处理取消和截止日期，以同步并发操作，并为用户提供取消操作的功能。

我们看到默认的 context `Background` 和 `TODO` 不允许取消，但可以使用包的各种函数来扩展它们，以添加超时或取消功能。我们还讨论了 `context` 在保存值方面的能力，以及如何小心使用它以避免遮蔽和其他问题。



然后，我们深入标准库包以查看 context 已在何处被使用。这包括请求的 HTTP 功能，其中可将其用于值传递、取消和超时，以及服务器关闭操作。我们还了解了 TCP 包如何允许我们通过实际示例以类似方式使用它，并列出了 database 包中允许我们使用 context 取消操作的函数。

在使用 context 构建我们自己的功能之前，我们探讨了一些应避免的用法，从使用错误类型的键到使用 context 传递本应位于函数或方法签名中的值。接着，我们利用前三章学到的并发知识，创建了一个搜索文件和内容的函数。

下一章将通过展示最常见的 Go 并发模式及其用法，结束本书的并发部分。这将使我们能够将迄今为止所学的所有并发知识组合成一些非常常见且有效的配置。

## 问题

1. Go 中什么是 context？
2. 取消、截止时间和超时有什么区别？
3. 使用 context 传递值的最佳实践是什么？
4. 哪些标准库包已经使用了 context？
