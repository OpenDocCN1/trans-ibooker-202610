
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
