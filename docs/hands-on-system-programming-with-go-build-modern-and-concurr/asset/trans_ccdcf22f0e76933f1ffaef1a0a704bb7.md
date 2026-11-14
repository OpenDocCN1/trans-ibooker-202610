
# Go 网络编程示例

## TCP 连接

### handleConn 函数

```go
func handleConn(conn net.Conn) {
    r := bufio.NewReader(conn)
    time.Sleep(time.Second / 2)
    for {
        msg, err := r.ReadString('\n')
        if err != nil {
            log.Println("<- Message error:", err)
            continue
        }
        switch msg = strings.TrimSpace(msg); msg {
        case `\q`:
            log.Println("Exiting...")
            if err := conn.Close(); err != nil {
                log.Println("<- Close:", err)
            }
            time.Sleep(time.Second / 2)
            return
        case `\x`:
            log.Println("<- Special message `\\x` received!")
        default:
            log.Println("<- Message Received:", msg)
        }
    }
}
```

### TCP 服务端与客户端

以下代码示例在一个应用程序中同时创建了客户端和服务端，但可以轻松拆分为两个应用程序——一个服务端（能够同时处理更多连接）和一个客户端，后者建立到服务端的单个连接。服务端将有一个 `Accept()` 循环，在单独的 goroutine 中处理接收到的连接。`handleConn()` 函数与我们之前定义的相同：

#### 服务端主函数

```go
func main() {
    if len(os.Args) != 2 {
        log.Fatalln("Please specify an address.")
    }
    addr, err := net.ResolveTCPAddr("tcp", os.Args[1])
    if err != nil {
        log.Fatalln("Invalid address:", os.Args[1], err)
    }
    listener, err := net.ListenTCP("tcp", addr)
    if err != nil {
        log.Fatalln("Listener:", os.Args[1], err)
    }
    for {
        time.Sleep(time.Millisecond * 100)
        conn, err := listener.AcceptTCP()
        if err != nil {
            log.Fatalln("<- Accept:", os.Args[1], err)
        }
        go handleConn(conn)
    }
}
```

#### 客户端主函数

客户端将创建连接并发送消息。`createConn()` 与我们之前定义的相同：

```go
func main() {
    if len(os.Args) != 2 {
        log.Fatalln("Please specify an address.")
    }
    addr, err := net.ResolveTCPAddr("tcp", os.Args[1])
    if err != nil {
        log.Fatalln("Invalid address:", os.Args[1], err)
    }
    createConn(addr)
}
```

在分离的客户端和服务端中，可以测试当客户端或服务端关闭连接时会发生什么。

## UDP 连接

UDP 是互联网上另一种广泛使用的协议。它专注于低延迟，这也是它不如 TCP 可靠的原因。它有许多应用场景，从在线游戏、媒体流传输到 `Voice over Internet Protocol`（`VoIP`，网络电话协议）。在 UDP 中，如果数据包未被接收，它就会丢失，不会像在 TCP 中那样被重新发送。想象一下 `VoIP` 通话，如果出现连接问题，你会丢失部分对话内容，但恢复后，你仍能近乎实时地继续通信。若将 TCP 用于此类应用，可能会导致每次丢包都累积延迟——从而使对话变得不可能。

在下面的示例中，我们将创建一个客户端和一个服务端应用程序。服务端将充当一种回显程序，将接收到的客户端消息发送回去，但也会反转消息内容。

客户端将与 TCP 客户端非常相似，但有几个例外——它将使用 `net.ResolveUDPAddr()` 函数获取地址，并使用 `net.DialUDP()` 获取连接：

### UDP 客户端

```go
func main() {
    if len(os.Args) != 2 {
        log.Fatalln("Please specify an address.")
    }
    addr, err := net.ResolveUDPAddr("udp", os.Args[1])
    if err != nil {
        log.Fatalln("Invalid address:", os.Args[1], err)
    }
    conn, err := net.DialUDP("udp", nil, addr)
    if err != nil {
        log.Fatalln("-> Connection:", err)
    }
    log.Println("-> Connection to", addr)
    r := bufio.NewReader(os.Stdin)
    b := make([]byte, 1024)
    for {
        fmt.Print("# ")
        msg, err := r.ReadBytes('\n')
        if err != nil {
            log.Println("-> Message error:", err)
        }
        if _, err := conn.Write(msg); err != nil {
            log.Println("-> Connection:", err)
            return
        }
        n, err := conn.Read(b)
        if err != nil {
            log.Println("<- Receive error:", err)
        }
        msg = bytes.TrimSpace(b[:n])
        log.Printf("<- %q", msg)
    }
}
```

### UDP 服务端

服务端将与 TCP 服务端有很大不同。主要区别在于，TCP 有一个监听器接受不同的连接并分别处理；而 UDP 监听器本身就是一个连接。它可以盲目接收数据，或使用 `ReceiveFrom()` 方法，该方法也会返回发送方的地址。这可以用在 `WriteTo()` 方法中进行回复，如下面的代码所示：

```go
func main() {
    if len(os.Args) != 2 {
        log.Fatalln("Please specify an address.")
    }
    addr, err := net.ResolveUDPAddr("udp", os.Args[1])
    if err != nil {
        log.Fatalln("Invalid address:", os.Args[1], err)
    }
    conn, err := net.ListenUDP("udp", addr)
    if err != nil {
        log.Fatalln("Listener:", os.Args[1], err)
    }

    b := make([]byte, 1024)
    for {
        n, addr, err := conn.ReadFromUDP(b)
        if err != nil {
            log.Println("<-", addr, "Message error:", err)
            continue
        }
        msg := bytes.TrimSpace(b[:n])
        log.Printf("<- %q from %s", msg, addr)
        for i, l := 0, len(msg); i < l/2; i++ {
            msg[i], msg[l-1-i] = msg[l-1-i], msg[i]
        }
        msg = append(msg, '\n')
        if _, err := conn.WriteTo(b[:n], addr); err != nil {
            log.Println("->", addr, "Send error:", err)
        }
    }
}
```

## 编码与校验和

在客户端和服务端之间设置某种形式的编码是一种良好的实践，如果编码包含校验和以验证数据完整性，则是更好的实践。我们可以用一个同时实现编码和校验和的自定义协议来改进上一节的示例。让我们首先定义编码函数，给定一条消息，它将返回以下字节序列：

| 功能 | 字节序列 |
| --- | --- |
| 前四个字节将遵循一个序列 | `2A 00 2A 00` |
| 两个字节将按小端序（最低有效字节在前）存储消息长度 | `08 00` |
| 四个字节用于数据校验和 | `00 00 00 00` |
| 后跟原始消息 | `0F 1D 3A FF ...` |
| 以相同的起始序列结束 | `2A 00 2A 00` |

`Checksum()` 函数将通过对消息内容求和来计算，使用五字节一组的小端序（最低有效字节在前），逐个添加剩余的任何字节，然后取总和的前四个字节作为小端序：

```go
func Checksum(b []byte) []byte {
    var sum uint64
    for len(b) >= 5 {
        for i := range b[:5] {
            v := uint64(b[i])
            for j := 0; j < i; j++ {
                v = v * 256
            }
            sum += v
        }
        b = b[5:]
    }
    for _, v := range b {
        sum += uint64(v)
    }
    s := make([]byte, 8)
    binary.LittleEndian.PutUint64(s, sum)
    return s[:4]
}
```

现在，让我们创建一个函数，使用我们定义的协议来封装消息：

```go
var ErrLength = errors.New("message too long")

func CreateMessage(content []byte) ([]byte, error) {
    if len(content) > 65535 {
        return nil, ErrLength
    }
    data := make([]byte, 0, len(content)+14)
    data = append(data, Sequence...)
    data = append(data, byte(len(content)/256), byte(len(content)%256))
    data = append(data, Checksum(content)...)
    data = append(data, content...)
    data = append(data, Sequence...)
    return data, nil
}
```

我们还需要另一个函数来执行相反的操作，检查消息是否有效并提取其内容。
