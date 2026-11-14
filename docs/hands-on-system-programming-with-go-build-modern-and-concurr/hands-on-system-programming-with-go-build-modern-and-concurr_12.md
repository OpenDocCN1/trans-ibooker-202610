# 网络编程

本章将涵盖网络编程。这将使我们的应用程序能够与在任何远程计算机、同一局域网，甚至互联网上运行的其他程序进行通信。

我们将从一些关于网络和架构的理论开始。然后我们将讨论套接字级别的通信，并解释如何创建 Web 服务器。最后，我们将讨论 Go 内置模板引擎的工作原理。

本章将涵盖以下主题：

*   网络
*   套接字编程
*   Web 服务器
*   模板引擎

## 技术要求

本章要求安装 `Go` 并设置好你喜欢的编辑器。更多信息请参阅 第 3 章 *Go 概览*。

此外，还要求在你的机器上安装 `OpenSSL`。许多 Linux 发行版已经预装了某些版本的 OpenSSL。它也可以在 Windows 上通过官方安装程序或第三方包管理器（如 `Chocolatey` 或 `Scoop`）进行安装。

## 通过网络通信

应用程序之间的通信可以通过网络进行，即使应用程序位于同一台机器上。为了传输信息，它们需要建立一个通用协议，该协议规定了从应用程序到线路各层面会发生什么。

## OSI 模型

`开放系统互连` (`OSI`) 模型是一个可以追溯到 1970 年代初的理论模型。它定义了一套通信标准，无论网络的物理或技术结构如何，该标准均适用，其目标是为不同网络提供互操作性。

该模型定义了七个不同的层，编号为一到七，每一层比前一层具有更高的抽象级别。前三层通常被称为 `媒体层`，而其余四层被称为 `主机层`。让我们在以下章节中逐一进行探讨。

### 第 1 层 – 物理层

OSI 模型的第一层是物理层，负责从设备（类似于以太网端口）传输未经处理的数据及其传输介质（如以太网电缆）。该层定义了与连接的物理/材料本质相关的所有特性——尺寸、形状、连接器电压、频率和时序。

物理层定义的另一个方面是传输方向，可以是以下之一：

*   `单工`：通信是单向的。
*   `半双工`：通信是双向的，但同一时间只能在一个方向上进行。
*   `全双工`：双向通信，双方可以同时通信。

许多知名技术（包括蓝牙和以太网）都包含了它们所使用的物理层定义。

### 第 2 层 – 数据链路层

下一层是数据链路层，它定义了直接连接的两个节点之间应如何进行数据传输。它负责以下事项：

*   检测第一层的通信错误
*   纠正物理错误
*   控制节点间的流量/传输速率
*   连接终止

数据链路层定义的一些现实例子包括以太网 (802.3) 和 Wi-Fi (802.11) 的定义。

### 第 3 层 – 网络层

下一层是网络层，它关注称为数据包的数据序列，数据包可以具有可变长度。数据包从一个节点传输到另一个节点，这两个节点可能位于同一网络上，也可能位于不同网络上。

该层将网络定义为一系列通过前两层识别的连接到同一介质的节点。网络仅凭目标地址就能传递消息。

### 第 4 层 – 传输层

第四层是传输层，它确保数据包从发送方到达接收方。这是通过来自目标的 `确认` (`ACK`) 和 `否定确认` (`NACK`) 消息来实现的，这些消息可以触发消息的重复发送，直到正确接收为止。还有其他机制在起作用，例如将消息拆分为块以进行传输（分段）、将各部分重新组装成单个消息（重组）以及检查数据是否成功发送和接收（错误控制）。

OSI 模型指定了五种不同的传输协议——TP0、TP1、TP2、TP3 和 TP4。TP0 是最简单的，仅执行消息的分段和重组。其他类别在其基础上增加了其他功能——例如，重传或超时。

### 第 5 层 – 会话层




## 第 5 层 – 会话层

第五层引入了会话的概念，即两台计算机之间临时的交互式信息交换。它负责创建连接并终止连接（同时跟踪会话），并允许设置检查点和恢复。

# 第 6 层 – 表示层

倒数第二层是表示层，它通过处理复杂的数据表示，负责应用程序之间的语法和语义。它使最后一层独立于用于表示数据的编码。OSI 模型的表示层使用 `ASN.1` 编码，但有各种不同的表示协议被广泛使用，例如 `XML` 和 `JSON`。

# 第 7 层 – 应用层

最后一层，应用层，是直接与应用程序通信的层。应用程序不被视为 OSI 模型的一部分，该层负责定义应用程序使用的接口。它包括 `FTP`、`DNS` 和 `SMTP` 等协议。

# TCP/IP – 互联网协议套件

**传输控制协议/互联网协议** (**TCP/IP**)，或互联网协议套件，是另一个由比 OSI 更少层数组成的模型，也是一个被广泛采用的模型。

# 第 1 层 – 链路层

第一层是链路层，是 OSI 物理层和数据链路层的结合，它通过指定协议（如 `MAC`，包含 `Ethernet` 和 `Wi-Fi`）来定义本地网络通信将如何发生。

# 第 2 层 – 网际层

网际层是第二层，可与 OSI 的网络层相比较。它定义了一个通用接口，允许不同网络在无需感知彼此底层拓扑结构的情况下有效通信。该层负责 **局域网** (**LAN**) 内节点之间的通信，以及构成互联网的全球互联网络之间的通信。

# 第 3 层 – 传输层

第三层类似于 OSI 的第四层。它处理两个设备之间的端到端通信，并负责错误检查和恢复，使上层无需知晓数据的复杂性。它定义了两个主要协议——`TCP`，它通过确认系统允许接收方按正确顺序获取数据；以及 **用户数据报协议** (**UDP**)，它不应用错误控制，也不要求接收方确认。

# 第 4 层 – 应用层

最后一层，应用层，汇总了 OSI 的后三层——会话层、表示层和应用层。该层定义了应用程序使用的架构，如对等网或客户端/服务器，以及应用程序使用的协议，如 `SSH`、`HTTP` 或 `SMTP`。每个进程都通过一个称为 `port` 的虚拟通信端点进行寻址。

# 理解套接字编程

Go 标准库允许我们轻松地与传输层交互，同时使用 `TCP` 和 `UDP` 连接。在本节中，我们将探讨如何使用套接字暴露服务，以及如何从另一个应用程序中查找并使用它。

# net 包

创建和处理 `TCP` 连接所需的工具位于 `net` 包内。该包的主要接口是 `Conn`，它代表一个连接。

它有四种实现：

*   `IPConn`：使用 `IP` 协议的原始连接，`TCP` 和 `UDP` 连接构建于此
*   `TCPConn`：使用 `TCP` 协议的 `IP` 连接
*   `UDPConn`：使用 `UDP` 协议的 `IP` 连接
*   `UnixConn`：Unix 域套接字，用于同一台机器上的进程间连接

在以下章节中，我们将探讨如何分别使用 `TCP` 和 `UDP`，以及如何使用 `IPConn` 实现自定义通信协议。

# TCP 连接

`TCP` 是互联网上使用最广泛的协议，它支持有序交付数据（字节）。该协议的主要关注点是可靠性，通过建立双向通信来实现，接收方每成功接收一个数据报就会发送一个确认信号。

可以使用 `net.Dial` 函数创建新连接。它是一个通用函数，接受不同的网络类型，例如：

*   `tcp`、`tcp4` (仅 IPv4)、`tcp6` (仅 IPv6)
*   `udp`、`udp4` (仅 IPv4)、`udp6` (仅 IPv6)
*   `ip`、`ip4` (仅 IPv4)、`ip6` (仅 IPv6)
*   `unix` (流式套接字)、`unixgram` (数据报套接字)、`unixpacket` (数据包套接字)

可以通过指定 `tcp` 协议以及主机和端口来创建 `TCP` 连接：

```
conn, err := net.Dial("tcp", "localhost:8080")
```

一种更直接的创建连接的方式是 `net.DialTCP`，它允许你同时指定本地和远程地址。它要求你创建一个 `net.TCPAddr` 以便使用：

```
addr, err := net.ResolveTCPAddr("tcp", "localhost:8080")
if err != nil {
    // handle error
}
conn, err := net.DialTCP("tcp", nil, addr)
if err != nil {
    // handle error
}
```

为了接收和处理连接，还有另一个接口 `net.Listener`，它有四种不同的实现——每种连接类型一种。对于连接，有一个通用的 `net.Listen` 函数和一个特定的 `net.ListenTCP` 函数。

我们可以尝试构建一个简单的应用程序，创建一个 `TCP` 监听器并连接到它，发送来自标准输入的任何内容。该应用应创建一个监听器以在后台启动连接，将标准输入发送到连接，然后接受并处理它。我们将使用换行符作为消息的分隔符。这在以下代码中展示：

```
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
    log.Println("<- Listening on", addr)

    go createConn(addr)

    conn, err := listener.AcceptTCP()
    if err != nil {
        log.Fatalln("<- Accept:", os.Args[1], err)
    }
    handleConn(conn)
}
```

连接创建非常简单。它创建连接，从标准输入读取消息，并通过写入将它们转发给连接：

```
func createConn(addr *net.TCPAddr) {
    defer log.Println("-> Closing")
    conn, err := net.DialTCP("tcp", nil, addr)
    if err != nil {
        log.Fatalln("-> Connection:", err)
    }
    log.Println("-> Connection to", addr)
    r := bufio.NewReader(os.Stdin)
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
    }
}
```

在我们的用例中，发送数据的连接将通过一条特殊消息 `\\q` 关闭，该消息将被解释为命令。监听器接受连接会创建另一个连接，代表由拨号操作获得的连接。由监听器创建的连接将接收来自拨号连接的消息并做出相应处理。它将解释特定消息（如 `\\q`）并执行特定操作；否则，它只会在屏幕上打印消息，如下面的代码所示：




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



# Go 中的 Web 服务器

## 消息内容验证

```go
func MessageContent(b []byte) ([]byte, error) {
    n := len(b)
    if n < 14 {
        return nil, fmt.Errorf("Too short")
    }
    if open := b[:4]; !bytes.Equal(open, Sequence) {
        return nil, fmt.Errorf("Wrong opening sequence %x", open)
    }
    if length := int(b[4])*256 + int(b[5]); n-14 != length {
        return nil, fmt.Errorf("Wrong length: %d (expected %d)", length, n-14)
    }
    if close := b[n-4:n]; !bytes.Equal(close, Sequence) {
        return nil, fmt.Errorf("Wrong closing sequence %x", close)
    }
    content := b[10 : n-4]
    if !bytes.Equal(Checksum(content), b[6:10]) {
        return nil, fmt.Errorf("Wrong checksum")
    }
    return content, nil
}
```

我们现在可以使用它们对消息进行编码和解码。例如，我们可以改进上一节中的 UDP 客户端和服务器，在发送时进行编码：

```go
// Send
data, err := common.CreateMessage(msg)
if err != nil {
    log.Println("->", addr, "Encode error:", err)
    continue
}
if _, err := conn.WriteTo(data, addr); err != nil {
    log.Println("->", addr, "Send error:", err)
}
```

我们可以对接收到的传入消息的字节进行解码，以提取内容：

```go
// Receive
n, addr, err := conn.ReadFromUDP(b)
if err != nil {
    log.Println("<-", addr, "Message error:", err)
    continue
}
msg, err := common.MessageContent(b[:n])
if err != nil {
    log.Println("<-", addr, "Decode error:", err)
    continue
}
log.Printf("<- %q from %s", msg, addr)
```

为了验证我们接收到的内容是否有效，我们使用前面定义的 `MessageContent` 实用函数。这将检查头部、长度和校验和。它将仅提取组成消息的字节。

## Web 服务器

Web 服务器应用程序是一种软件，它可以通过 TCP/IP 网络使用 HTTP 协议（以及其他一些相关协议）来提供内容。有许多知名的 Web 服务器应用程序，例如 Apache、NGINX 和 Microsoft IIS。常见的服务器用例场景包括以下内容：

*   **提供静态文件，例如网站和相关资源**：HTML 页面、图片、样式表和脚本。
*   **暴露 Web 应用程序**：一种在服务器上运行、具有基于 HTML 界面的应用程序，需要通过浏览器访问。
*   **暴露 Web API**：不由用户直接使用、而是由其他应用程序使用的远程接口。更多详情请参阅第 1 章 *系统编程简介*。

## HTTP 协议

HTTP 协议是 Web 服务器的基石。其设计始于 1989 年。HTTP 的主要用法是请求-响应范式，即客户端发送请求，服务器向客户端返回响应。

**统一资源定位符 (URL)** 是 HTTP 请求的唯一标识符，其结构如下：

| 部分 | 示例 |
| --- | --- |
| 协议 | `http` |
| `://` | `://` |
| 主机 | `www.website.com` |
| 路径 | `/path/to/some-resource` |
| `?` | `?` |
| 查询（可选） | `query=string&with=values` |

从前面的表格中，我们可以得出以下结论：

*   除了 HTTP 及其加密版本 (HTTPS) 之外，还有其他几种不同的协议，例如 **文件传输协议 (FTP)** 及其安全对应协议 **SSH 文件传输协议 (SFTP)**。
*   主机可以是实际的 IP 地址，也可以是主机名。当选择主机名时，还有另一个参与者，**域名服务器 (DNS)**，它充当主机名和物理地址之间的电话簿。DNS 将主机名转换为 IP。
*   路径是服务器上期望的资源，且始终是绝对路径。
*   查询字符串是添加在问号后面的路径内容。它是一系列键值对，形式为 `key=value`，它们由 `&` 符号分隔。

HTTP 是一种文本协议，它包含 URL 的一些元素以及其他信息——方法、头部和主体。

请求主体是发送到服务器的信息，例如表单值或上传的文件。

头部是与请求相关的元数据，每行一个，采用 `Key: Value; extra data` 形式。有一系列具有特定功能的已定义头部，例如 `Authorization`、`User-Agent` 和 `Content-Type`。

某些方法表示对资源执行的操作。以下是最常用的方法：

*   `GET`：获取选定资源的表示
*   `HEAD`：类似于 `GET`，但没有响应主体
*   `POST`：向服务器提交资源，通常是新资源
*   `PUT`：提交资源的新版本
*   `DELETE`：删除资源
*   `PATCH`：请求对资源进行特定更改

HTTP 请求如下所示：

```
POST /resource/ HTTP/1.1
User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)
Host: www.website.com
Content-Length: 1024
Accept-Language: en-us

the actual request content
that is optional
```

从前面的代码中，我们可以看到以下内容：

*   第一行是一个以空格分隔的三元组：方法—路径—协议。
*   其后每行一个头部。
*   一个空行作为分隔符。
*   可选的请求主体。

对于每个请求，都有一个响应，其结构与 HTTP 请求非常相似。唯一不同的部分是第一行，它包含一个不同的以空格分隔的三元组：HTTP 版本—状态码—原因短语。

状态码是一个表示请求结果的整数。主要有四大状态类别：

*   `100`：信息/请求已接收，将继续处理
*   `200`：请求成功；例如 `OK 200` 或 `Created 201`
*   `300`：重定向到另一个 URL，临时或永久
*   `400`：客户端错误，例如 `Not Found 404` 或 `Conflict 409`
*   `500`：服务端错误，例如 `Internal Server Error 503`

HTTP 响应如下所示：

```
HTTP/1.1 200 OK
Content-Length: 88
Content-Type: text/html

<html>
  <body>
    <h1>Sample Page</h1>
  </body>
</html>
```

## HTTP/2 和 Go

最常用的 HTTP 版本是 HTTP/1.1，发布于 1997 年。2009 年，Google 启动了一个新项目，旨在创建一个比 HTTP/1.1 更快的继任者，名为 SPDY。该协议最终成为了现在 **超文本传输协议** 的 2.0 版本，**HTTP/2**。

它的构建方式使得现有 Web 应用程序无需修改即可工作，但为使用新协议的应用程序提供了新功能，包括更快的通信速度。一些差异包括以下内容：

*   它是二进制的（HTTP/1.1 是文本的）。
*   它是完全多路复用的，可以并行请求数据，仅使用一个 TCP 连接。
*   它使用头部压缩来减少开销。
*   服务器可以向客户端推送响应，而不是由客户端定期轮询。
*   它具有更快的协议协商——得益于 **应用层协议协商 (ALPN)** 扩展。

所有主流现代浏览器都支持 HTTP/2。Go 1.6 版本包含了对 HTTP/2 的透明支持，1.8 版本引入了从服务器向客户端推送响应的能力。

## 使用标准包

我们现在将了解如何使用标准包在 Go 中创建 Web 服务器。所有内容都包含在 `net/http` 包中，该包暴露了一系列用于发起 HTTP 请求和创建 HTTP 服务器的函数。

### 发起 HTTP 请求



# HTTP 客户端

该包暴露了一个 `http.Client` 类型，可用于发起请求。如果请求是简单的 `GET` 或 `POST`，则有专门的方法。该包还提供了一个同名函数，但它只是 `DefaultClient` 实例相应方法的简写。请查看以下代码：

```
resp, err := http.Get("http://example.com/")
resp, err := client.Get("http://example.com/")
...
resp, err := http.Post("http://example.com/upload", "image/jpeg", &buf)
resp, err := client.Post("http://example.com/upload", "image/jpeg", &buf)
...
values := url.Values{"key": {"Value"}, "id": {"123"}}
resp, err := http.PostForm("http://example.com/form", values)
resp, err := client.PostForm("http://example.com/form", values)
```

对于任何其他类型的需求，`Do` 方法允许我们执行特定的 `http.Request`。`NewRequest` 函数允许我们指定任意 `io.Reader`：

```
req, err := http.NewRequest("GET", "http://example.com", nil)
// ...
req.Header.Add("Content-Type", "text/html")
resp, err := client.Do(req)
// ...
```

`http.Client` 有多个字段，其中许多是接口，允许我们使用默认实现或自定义实现。第一个是 `CookieJar`，它允许客户端存储和复用 Web Cookie。Cookie 是一段数据，浏览器将其发送给客户端，客户端可以将其发回服务器以替换头部，例如认证。默认客户端不使用 Cookie Jar。另一个接口是 `RoundTripper`，它只有一个方法 `RoundTrip`，该方法接收一个请求并返回一个响应。如果未指定值，则会使用 `DefaultTransport` 值，它也可用于组合自定义的 `RoundTripper` 实现。客户端返回的 `http.Response` 也有一个 Body，它是 `io.ReadCloser`，关闭它是应用程序的责任。因此，建议在获得响应后立即使用延迟的 `Close` 语句。在下面的示例中，我们将实现一个自定义 Transport，它在执行标准 RoundTripper 之前记录请求的 URL 并修改一个头部：

```
type logTripper struct {
    http.RoundTripper
}

func (l logTripper) RoundTrip(r *http.Request) (*http.Response,  
    error) {
        log.Println(r.URL)
        r.Header.Set("X-Log-Time", time.Now().String())
        return l.RoundTripper.RoundTrip(r)
}
```

我们将在一个客户端中使用这个 Transport 来发起一个简单的请求：

```
func main() {
    client := http.Client{Transport: logTripper{http.DefaultTransport}}
    req, err := http.NewRequest("GET", "https://www.google.com/search?q=golang+net+http", nil)
    if err != nil {
        log.Fatal(err)
    }
    resp, err := client.Do(req)
    if err != nil {
        log.Fatal(err)
    }
    defer resp.Body.Close()
    log.Println("Status code:", resp.StatusCode)
}
```

# 创建简单的服务器

该包提供的另一个功能是创建服务器。该包的主要接口是 `Handle`，它有一个方法 `ServeHTTP`，该方法使用请求来写入响应。其最简单的实现是 `HandlerFunc`，它是一个与 `ServeHTTP` 具有相同签名的函数，并通过执行自身来实现 `Handler`。

`ListenAndServe` 函数使用给定的地址和处理器启动一个 HTTP 服务器。如果未指定处理器，它将使用 `DefaultServeMux` 变量。`ServeMux` 是一种特殊类型的 `Handler`，它根据请求的 URL 路径管理不同处理器的执行。它有两个方法，`Handle` 和 `HandleFunc`，允许用户指定路径和相应的处理器。该包还提供了通用的处理器函数，它们类似于我们在 `Client` 中看到的，它们将调用默认 `ServerMux` 中同名的方法。

在下面的示例中，我们将创建一个 `customHandler` 并创建一个带有一些端点的简单服务器，包括自定义端点：

```
type customHandler int

func (c *customHandler) ServeHTTP(w http.ResponseWriter, r  
    *http.Request) {
        fmt.Fprintf(w, "%d", *c)
        *c++
}

func main() {
    mux := http.NewServeMux()
    mux.HandleFunc("/hello", func(w http.ResponseWriter, r 
        *http.Request) {
            fmt.Fprintf(w, "Hello!")
    })
    mux.HandleFunc("/bye", func(w http.ResponseWriter, r 
        *http.Request) {
            fmt.Fprintf(w, "Goodbye!")
    })
    mux.HandleFunc("/error", func(w http.ResponseWriter, r 
        *http.Request) {
            w.WriteHeader(http.StatusInternalServerError)
            fmt.Fprintf(w, "An error occurred!")
    })
    mux.Handle("/custom", new(customHandler))
    if err := http.ListenAndServe(":3000", mux); err != nil {
        log.Fatal(err)
    }
}
```

# 服务文件系统

Go 标准包允许我们使用 `http.FileServer` 函数轻松服务文件系统中的某个目录，该函数在给定 `http.FileSystem` 接口时，返回一个服务该目录的 `Handler`。默认实现是 `http.Dir`，它是一个代表系统中目录的自定义字符串。`FileServer` 函数已经内置了保护机制，防止我们使用相对路径（如 `../../../dir`）来访问服务目录之外的目录。

以下是一个示例文件服务器，它使用作为参数提供的目录作为服务文件的根目录：

```
func main() {
    if len(os.Args) != 2 {
        log.Fatalln("Please specify a directory")
    }
    s, err := os.Stat(os.Args[1])
    if err == nil && !s.IsDir() {
        err = errors.New("not a directory")
    }
    if err != nil {
        log.Fatalln("Invalid path:", err)
    }
    http.Handle("/", http.FileServer(http.Dir(os.Args[1])))
    if err := http.ListenAndServe(":3000", nil); err != nil {
        log.Fatal(err)
    }
}
```

# 遍历路由和方法

使用的 HTTP 方法存储在 `Request.Method` 字段中。该字段可以在处理器内部使用，以便为每种支持的方法提供不同的行为：

```
switch r.Method {
case http.MethodGet:
    // GET implementation
case http.MethodPost:
    // POST implementation
default:
    http.NotFound(w, r)
}
```

`http.Handler` 接口的优势在于我们可以定义自定义类型。这可以使代码更具可读性，并能泛化这种特定于方法的行为：

```
type methodHandler map[string]http.Handler

func (m methodHandler) ServeHTTP(w http.ResponseWriter, r 
        *http.Request) {
            h, ok := m[strings.ToUpper(r.Method)]
            if !ok {
                http.NotFound(w, r)
                return
            }
    h.ServeHTTP(w, r)
}
```

这将使代码更具可读性，并且可针对不同路径复用：

```
func main() {
    http.HandleFunc("/path1", methodHandler{
        http.MethodGet: http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
            fmt.Fprint(w, "Showing record")
        }),
        http.MethodPost: http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
            fmt.Fprint(w, "Updated record")
        }),
    })
    if err := http.ListenAndServe(":3000", nil); err != nil {
        log.Fatal(err)
    }
}
```

# Multipart 请求和文件

请求体是一个 `io.ReadCloser`。这意味着关闭它是服务器的责任。对于文件上传，Body 不是直接的文件内容，而是通常是一个 multipart 请求，这是一种在头部指定分隔符并在 Body 内部使用它将消息分成多个部分的请求。

这是一个 multipart 消息的示例：



## MIME 多部分消息示例

```
MIME-Version: 1.0
Content-Type: multipart/mixed; boundary=xxxx

This part before boundary is ignored
--xxxx
Content-Type: text/plain

First part of the message. The next part is binary data encoded in base64
--xxxx
Content-Type: application/octet-stream
Content-Transfer-Encoding: base64

PGh0bWw+CiAgPGhlYWQ+CiAgPC9oZWFkPgogIDxib2R5PgogICAgPHA+VGhpcyBpcyB0aGUg
Ym9keSBvZiB0aGUgbWVzc2FnZS48L3A+CiAgPC9ib2R5Pgo8L2h0bWw+Cg==
--xxxx--
```

我们可以看到，`boundary` 以两个连字符作为前缀并后跟换行符，最终的 `boundary` 也以两个连字符作为后缀。在下面的示例中，服务器将处理文件上传，并提供一个小表单用于从浏览器发送请求。

让我们定义一些在处理程序中将使用的常量：

```go
const (
    param    = "file"
    endpoint = "/upload"
    content  = `<html><body>` +
        `<form enctype="multipart/form-data" action="%s" method="POST">` +
        `<input type="file" name="%s"/><input type="submit" value="Upload"/>` +
        `</form></html></body>`
)
```

现在，我们可以定义 `handler` 函数。如果方法是 `GET`，第一部分应显示模板，因为它在 `POST` 上执行上传，否则返回未找到状态：

```go
mux.HandleFunc(endpoint, func(w http.ResponseWriter, r *http.Request) {
    if r.Method == "GET" {
        fmt.Fprintf(w, content, endpoint, param)
        return
    } else if r.Method != "POST" {
        http.NotFound(w, r)
        return
    }

    path, err := upload(r)
    if err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)
        return
    }
    fmt.Fprintf(w, "Uploaded to %s", path)
})
```

`upload` 函数将使用 `Request.FormFile` 方法，该方法返回文件及其元数据：

```go
func upload(r *http.Request) (string, error) {
    f, h, err := r.FormFile(param)
    if err != nil {
        return "", err
    }
    defer f.Close()

    p := filepath.Join(os.TempDir(), h.Filename)
    fw, err := os.OpenFile(p, os.O_WRONLY|os.O_CREATE, 0666)
    if err != nil {
        return "", err
    }
    defer fw.Close()

    if _, err = io.Copy(fw, f); err != nil {
        return "", err
    }
    return p, nil
}
```

# HTTPS

如果您希望 Web 服务器使用 HTTPS，而不依赖外部应用程序（如 `NGINX`），如果您已经拥有有效证书，则可以轻松实现。如果没有，可以使用 `OpenSSL` 创建一个：

```bash
> openssl genrsa -out server.key 2048

> openssl req -new -x509 -sha256 -key server.key -out server.crt -days 3650
```

第一条命令生成私钥，第二条命令创建服务器所需的公共证书。第二条命令还需要大量额外信息来创建证书，从国家名称到电子邮件地址。

一切准备就绪后，为了创建 HTTPS 服务器，需要将 `http.ListenAndServe` 函数替换为其安全对应函数 `http.ListenAndServeTLS`：

```go
func main() {
    http.HandleFunc("/hello", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprint(w, "Hello!")
    })
    err := http.ListenAndServeTLS(":443", "server.crt", "server.key", nil)
    if err != nil {
        log.Fatal("ListenAndServe: ", err)
    }
}
```

# 第三方包

Go 开源社区开发了许多与 `net/http` 集成的包，实现了 `Handler` 接口，但提供了一组独特的功能，使 Web 服务器的开发更加容易。

## gorilla/mux

`github.com/gorilla/mux` 包包含 `Handler` 的另一种实现，增强了标准 `ServeMux` 的功能：

*   使用 URL 中的任何元素（包括模式、方法、主机或查询值）更好地将 URL 与处理程序匹配。
*   URL 元素（如主机、路径和查询键）可以包含占位符（也可以使用正则表达式）。
*   在子路由的帮助下，路由可以分层定义，嵌套路由测试路径的一部分。
*   处理程序也可以用作中间件，在主处理程序之前，针对所有路径或路径子集。

让我们从使用其他路径元素进行匹配的示例开始：

```go
r := mux.NewRouter()
// 仅限本地请求
r.Host("localhost:3000")
// 仅当存在某个标头时
r.Headers("X-Requested-With", "XMLHttpRequest")
// 仅当指定了查询参数时
r.Queries("access_key", "0x20")
```

变量是另一个非常有用的功能，允许我们指定占位符，并通过辅助函数 `mux.Vars` 获取其值，如下例所示：

```go
r := mux.NewRouter()
r.HandleFunc("/products/", ProductsHandler)
r.HandleFunc("/products/{key}/", ProductHandler)
r.HandleFunc("/products/{key}/details", ProductDetailsHandler)
...
// 在处理程序内部
vars := mux.Vars(request)
key := vars["key"]
```

`Subrouter` 是另一个有用的函数，用于分组相同前缀的路由。这允许我们将前面的代码简化为以下代码片段：

```go
r := mux.NewRouter()
s := r.PathPrefix("/products").Subrouter()
s.HandleFunc("/", ProductsHandler)
s.HandleFunc("/{key}/", ProductHandler)
s.HandleFunc("/{key}/details", ProductDetailsHandler)
```

中间件在与子路由结合使用时也非常有用，可执行一些常见任务，如身份验证和验证：

```go
r := mux.NewRouter()
pub := r.PathPrefix("/public").Subrouter()
pub.HandleFunc("/login", LoginHandler)
priv := r.PathPrefix("/private").Subrouter()
priv.Use(AuthHandler)
priv.HandleFunc("/profile", ProfileHandler)
priv.HandleFunc("/logout", LogoutHandler)
```

## gin-gonic/gin

`github.com/gin-gonic/gin` 包是另一个 Go Web 框架，通过许多简写和辅助函数扩展了 Go HTTP 服务器的功能。其功能包括：

*   **速度**：其路由速度快，内存占用极小。
*   **中间件**：允许定义和使用中间处理程序，并完全控制其流程。
*   **无恐慌**：自带可从恐慌中恢复的中间件。
*   **分组**：可以将具有相同前缀的路由分组在一起。
*   **错误**：管理和收集请求期间发生的错误。
*   **渲染**：开箱即支持大多数 Web 格式的渲染器（JSON、XML、HTML）。

该包的核心是 `gin.Engine`，它也是一个 `http.Handler`。`gin.Default` 函数返回一个使用两个中间件的引擎——`Logger`（打印每个接收到的 HTTP 请求的结果）和 `Recovery`（从恐慌中恢复）。另一个选择是使用 `gin.New` 函数，它返回一个没有中间件的引擎。

它允许我们使用以其 HTTP 对应项命名的一系列引擎方法，将处理程序绑定到单个 HTTP 方法：

*   `DELETE`
*   `GET`
*   `HEAD`
*   `OPTIONS`
*   `PATCH`
*   `POST`
*   `PUT`
*   `Any`（捕获任何 HTTP 方法的所有情况）

还有一个 `group` 方法，返回所选路径的路由分组，该分组暴露上述所有方法：

```go
router := gin.Default()

router.GET("/resource", getResource)
router.POST("/resource", createResource)
router.PUT("/resource", updateResoure)
router.DELETE("/resource", deleteResource)
// 使用分组
g := router.Group("/resource")
g.GET("", getResource)
g.POST("", createResource)
g.PUT("", updateResoure)
g.DELETE("", deleteResource)
```

此框架中的处理程序具有不同的签名。它不使用响应写入器和请求作为参数，而是使用 `gin.Context`，这是一个包装两者的结构，并提供许多简写和实用程序。例如，该包提供在 URL 中使用占位符的可能性，上下文允许读取这些参数：

```go
router := gin.Default()
router.GET("/hello/:name", func(c *gin.Context) {
    c.String(http.StatusOK, "Hello %s!", c.Param("name"))
})
```



我们在示例中还可以看到，context 提供了一个 `String` 方法，使我们能够用一行代码写入 HTTP 状态和响应内容。

# 其他功能

Web 服务器还有其他功能。其中一些已由标准库支持（如 HTTP/2 Pusher），而另一些则可通过实验性包或第三方库获得（如 WebSockets）。

## HTTP/2 Pusher

我们已经讨论过，Go 从 1.8 版本开始支持 HTTP/2 服务端推送功能。让我们看看如何在应用程序中使用它。它的用法非常简单；如果响应写入器可以类型断言为 `http.Pusher` 接口，就可以用它在主请求中推送额外的请求。在本例中，我们用它来随页面一起旁路加载一张 SVG 图片：

```go
func main() {
    const imgPath = "/image.svg"
    http.HandleFunc("/", func(w http.ResponseWriter, r 
        *http.Request) {
            pusher, ok := w.(http.Pusher)
            if ok {
                fmt.Println("Push /image")
                pusher.Push(imgPath, nil)
            }
        w.Header().Add("Content-Type", "text/html")
        fmt.Fprintf(w, `<html><body><img src="%s"/>`+
            `</body></html>`, imgPath)
    })
    http.HandleFunc(imgPath, func(w http.ResponseWriter, r 
        *http.Request) {
            w.Header().Add("Content-Type", "image/svg+xml")
            fmt.Fprint(w, `<?xml version="1.0" standalone="no"?>
<svg xmlns="http://www.w3.org/2000/svg">
  <rect width="150" height="150" style="fill:blue"/>
</svg>`)
    })
    if err := http.ListenAndServe(":3000", nil); err != nil {
        fmt.Println(err)
    }
}
```

对于 HTTP/1，这将导致两个单独的请求；而对于 HTTP/2，则是一个单独的请求，其中第二个请求是利用浏览器的推送能力获得的。

## WebSocket 协议

HTTP 协议只能实现单向通信，而 WebSocket 协议则是在客户端和服务器之间进行全双工通信。Go 实验库通过 `golang.org/x/net/websocket` 包提供 WebSocket 支持，Gorilla 也有自己的实现 `github.com/gorilla/websocket`。

第二个实现功能更全面，它被 `github.com/olahol/melody` 包使用，该包实现了一个用于简易 WebSocket 通信的框架。每个包都提供了不同的 WebSocket 服务端和客户端配对的工作示例。

# 开始使用模板引擎

另一个非常强大的工具是 Go 模板引擎，位于 `text/template` 中。其功能在 `html/template` 包中得到了复制和扩展，这构成了 Go 语言 Web 开发的另一个强大工具。

## 语法和基本用法

template 包使我们能够使用文本文件和数据结构将展示层与数据分离。模板引擎定义了两个分隔符——左和右——用于包裹表示数据求值的动作。默认分隔符是 `{{` 和 `}}`，模板只对包含在这些分隔符内的内容求值，其余部分保持原样。

通常绑定到模板的数据是一个结构体或映射，可通过 `$` 变量在模板的任何位置访问。无论是映射还是结构体，字段总是以相同的方式访问，使用 `.Field` 语法。如果省略了美元符号，则该值指代当前上下文，如果不在特殊语句（如循环）中，当前上下文就是 `$`。除这些例外情况外，`{{$.Field}}` 和 `{{.Field}}` 语句是等价的。

模板中的流程由条件语句 `{{if}}` 和循环语句 `{{range}}` 控制，两者均以 `{{end}}` 语句结束。条件语句还提供了链式 `{{else if}}` 语句来指定另一个条件，类似于 switch，以及 `{{else}}` 语句，可视为 switch 的 default 分支。`{{else}}` 可与 `range` 语句一起使用，当 range 的参数为 `nil` 或长度为零时执行。

## 创建、解析和执行模板

`template.Template` 类型是一个或多个模板的集合，可以通过多种方式初始化。`template.New` 函数创建一个具有给定名称的新空模板，可用于调用 `Parse` 方法，该方法使用字符串创建模板。请看下面的代码：

```go
var data = struct {
    Question string
    Answer int
}{
    Question: "Answer to the Ultimate Question of Life, " +
        "the Universe, and Everything",
    Answer: 42,
}
tpl, err := template.New("question-answer").Parse(`
    <p>Question: {{.Question}}</p>
    <p>Answer: {{.Answer}}</p>
`)
if err != nil {
    log.Fatalln("Error:", err)
}
if err = tpl.Execute(os.Stdout, data); err != nil {
    log.Fatalln("Error:", err)
}
```

完整示例见此处：[`play.golang.org/p/k-t0Ns1b2Mv.`](https://play.golang.org/p/k-t0Ns1b2Mv)

模板也可以从文件系统加载和解析，使用 `template.ParseFiles`（接受文件列表）和 `template.ParseGlob`（使用 Unix `glob` 命令语法选择文件列表）。让我们创建一个包含以下内容的模板文件：

```html
<html>
    <body>
        <h1>{{.name}}</h1>
        <ul>
            <li>First appearance: {{.appearance}}</li>
            <li>Style: {{.style}}</li>
        </ul>
    </body>
</html>
```

我们可以使用这两个函数中的任意一个来加载它，并用一些示例数据执行它：

```go
func main() {
    tpl, err := template.ParseGlob("ch9/template/parse/*.html")
    if err != nil {
        log.Fatal("Error:", err)
    }
    data := map[string]string{
        "name": "Jin Kazama",
        "style": "Karate",
        "appearance": "Tekken 3",
    }
    if err := tpl.Execute(os.Stdout, data); err != nil {
        log.Fatal("Error:", err)
    }
}
```

当加载多个模板时，`Execute` 方法将使用最后一个。如果需要选择特定模板，还有另一个方法 `ExecuteTemplate`，它也接收模板名称作为参数，以指定使用哪个模板。

## 条件和循环

`range` 语句可以用不同的方式使用——最简单的方式就是调用 `range` 后跟上你想要迭代的切片或映射。

或者，你可以指定值，或索引和值：

```go
var a = []int{1, 2, 3, 4}
`{{ range . }} {{.}} {{ end }}` // simple
`{{ range $v := . }} {{$v}} {{ end }}` // value
`{{ range $i, $v := . }} {{$v}} {{ end }}` // index and value
```

在循环中，`{{.}}` 变量取当前迭代元素的值。下面的例子循环一个项的切片：

```go
var data = []struct {
    Question, Answer string
}{{
    Question: "Answer to the Ultimate Question of Life, " +
        "the Universe, and Everything",
    Answer: "42",
}, {
    Question: "Who you gonna call?",
    Answer: "Ghostbusters",
}}
tpl, err := template.New("question-answer").Parse(`{{range .}}
Question: {{.Question}}
Answer: {{.Answer}}
{{end}}`)
if err != nil {
    log.Fatalln("Error:", err)
}
if err = tpl.Execute(os.Stdout, data); err != nil {
    log.Fatalln("Error:", err)
}
```

完整示例见此处：[`play.golang.org/p/MtU_d9CsFb-.`](https://play.golang.org/p/MtU_d9CsFb-)

下一个例子是条件语句的一个用例，也用到了 `lt` 函数：




```go
var data = []struct {
    Name  string
    Score int
}{
    {"Michelangelo", 30},
    {"Donatello", 50},
    {"Leonardo", 80},
    {"Raffaello", 100},
}
tpl, err := template.New("question-answer").Parse(`{{range .}}
{{.Name}} scored {{.Score}}. He did {{if lt .Score 50}}bad{{else if lt .Score 75}}okay{{else if lt .Score 90}}good{{else}}great{{end}}
{{end}}`)
if err != nil {
    log.Fatalln("Error:", err)
}
if err = tpl.Execute(os.Stdout, data); err != nil {
    log.Fatalln("Error:", err)
}
```

完整示例请见此处：[`play.golang.org/p/eBKDcJ47rPU`](https://play.golang.org/p/eBKDcJ47rPU)。

我们将在下一节中更详细地探讨函数。

# 模板函数

函数是模板引擎的重要组成部分，有许多内置函数，例如比较函数（`eq`、`lt`、`gt`、`le`、`ge`）或逻辑函数（`AND`、`OR`、`NOT`）。函数通过其名称调用，后跟使用空格作为分隔符的参数。上一个示例中使用的函数 `lt a b` 表示 `lt(a,b)`。当函数嵌套更深时，需要用括号将函数和参数括起来。例如，`not lt a b` 语句意味着 X 函数有三个参数，即 `not(lt, a, b)`。正确的版本是 `not (lt a b)`，这会告知模板先求解括号内的元素。

创建模板时，可以使用 `Funcs` 方法为其分配自定义函数，并在模板中使用它们。这非常有用，正如以下示例所示：

```go
var data = struct {
    Name, Surname, Occupation, City string
}{
    "Bojack", "Horseman", "Actor", "Los Angeles",
}
tpl, err := template.New("question-answer").Funcs(template.FuncMap{
    "upper": func(s string) string { return strings.ToUpper(s) },
    "lower": func(s string) string { return strings.ToLower(s) },
}).Parse(`{{.Name}} {{.Surname}} - {{lower .Occupation}} from {{upper .City}}`)
if err != nil {
    log.Fatalln("Error:", err)
}
if err = tpl.Execute(os.Stdout, data); err != nil {
    log.Fatalln("Error:", err)
}
```

完整示例请见此处：[`play.golang.org/p/DdoKEOixDDB`](https://play.golang.org/p/DdoKEOixDDB)。

`|` 运算符可用于将一个语句的输出链接到另一个语句的输入，这类似于 Unix Shell 中的管道操作。例如，语句 `{{"put" | printf "%s%s" "out" | printf "%q"}}` 将产生 `"output"`。

# RPC 服务器

**远程过程调用 (RPC)** 是一种从另一个系统调用应用程序功能执行的方法，使用 TCP 协议。Go 原生支持 RPC 服务器。

## 定义服务

Go RPC 服务器允许我们注册任意 Go 类型及其方法。这通过 RPC 协议暴露这些方法，使我们能够从远程客户端按名称调用它们。让我们创建一个辅助工具，用于在阅读时跟踪书籍的进度：

```go
// Book represents a book entry
type Book struct {
    ISBN   string
    Title  string
    Author string
    Year   int
    Pages  int
}

// ReadingList keeps tracks of books and pages read
type ReadingList struct {
    Books    []Book
    Progress []int
}
```

首先，让我们定义一个名为 `bookIndex` 的小辅助方法，它使用书籍的标识符（ISBN）返回书籍的索引：

```go
func (r *ReadingList) bookIndex(isbn string) int {
    for i := range r.Books {
        if isbn == r.Books[i].ISBN {
            return i
        }
    }
    return -1
}
```

现在，我们可以定义 `ReadingList` 将具备的操作。它应该能够添加和移除书籍：

```go
// AddBook checks if the book is not present and adds it
func (r *ReadingList) AddBook(b Book) error {
    if b.ISBN == "" {
        return ErrISBN
    }
    if r.bookIndex(b.ISBN) != -1 {
        return ErrDuplicate
    }
    r.Books = append(r.Books, b)
    r.Progress = append(r.Progress, 0)
    return nil
}

// RemoveBook removes the book from list and forgets its progress
func (r *ReadingList) RemoveBook(isbn string) error {
    if isbn == "" {
        return ErrISBN
    }
    i := r.bookIndex(isbn)
    if i == -1 {
        return ErrMissing
    }
    // replace the deleted book with the last of the list
    r.Books[i] = r.Books[len(r.Books)-1]
    r.Progress[i] = r.Progress[len(r.Progress)-1]
    // shrink the list of 1 element to remove the duplicate
    r.Books = r.Books[:len(r.Books)-1]
    r.Progress = r.Progress[:len(r.Progress)-1]
    return nil
}
```

它还应该能够读取和修改书籍的进度：

```go
// GetProgress returns the progress of a book
func (r *ReadingList) GetProgress(isbn string) (int, error) {
    if isbn == "" {
        return -1, ErrISBN
    }
    i := r.bookIndex(isbn)
    if i == -1 {
        return -1, ErrMissing
    }
    return r.Progress[i], nil
}
```

然后，`SetProgress` 如下所示更改书籍的进度：

```go
func (r *ReadingList) SetProgress(isbn string, pages int) error {
    if isbn == "" {
        return ErrISBN
    }
    i := r.bookIndex(isbn)
    if i == -1 {
        return ErrMissing
    }
    if p := r.Books[i].Pages; pages > p {
        pages = p
    }
    r.Progress[i] = pages
    return nil
}
```

`AdvanceProgress` 向书籍的进度添加页数：

```go
func (r *ReadingList) AdvanceProgress(isbn string, pages int) error {
    if isbn == "" {
        return ErrISBN
    }
    i := r.bookIndex(isbn)
    if i == -1 {
        return ErrMissing
    }
    if p := r.Books[i].Pages - r.Progress[i]; p < pages {
        pages = p
    }
    r.Progress[i] += pages
    return nil
}
```

这些函数中使用的错误变量定义如下：

```go
// List of errors
var (
    ErrISBN      = fmt.Errorf("missing ISBN")
    ErrDuplicate = fmt.Errorf("duplicate book")
    ErrMissing   = fmt.Errorf("missing book")
)
```

## 创建服务器

现在我们有了服务，可以非常轻松地创建一个 RPC 服务器。然而，所使用的类型必须遵守一些规则，以使其方法可用：

*   方法的类型和方法本身都必须是导出的。
*   方法有两个参数，且都必须是导出的。
*   第二个参数必须是指针。
*   方法必须返回一个 error。

方法看起来应该像这样：`func (t *T) Method(in T1, out *T2) error.`

下一步是为 `ReadingList` 创建一个满足这些规则的包装器：

```go
// ReadingService adapts ReadingList for RPC
type ReadingService struct {
    ReadingList
}

// sets the success pointer value from error
func setSuccess(err error, b *bool) error {
    *b = err == nil
    return err
}
```

我们可以使用 `Book`（这是一个导出类型）和内置类型重新定义书籍、添加和移除函数：

```go
func (r *ReadingService) AddBook(b Book, success *bool) error {
    return setSuccess(r.ReadingList.AddBook(b), success)
}

func (r *ReadingService) RemoveBook(isbn string, success *bool) error {
    return setSuccess(r.ReadingList.RemoveBook(isbn), success)
}
```

对于进度操作，我们有两个输入（`isbn` 和 `pages`），因此必须定义一个包含两者的结构体，因为输入必须是单个参数：

```go
func (r *ReadingService) GetProgress(isbn string, pages *int) (err error) {
    *pages, err = r.ReadingList.GetProgress(isbn)
    return err
}

type Progress struct {
    ISBN  string
    Pages int
}

func (r *ReadingService) SetProgress(p Progress, success *bool) error {
    return setSuccess(r.ReadingList.SetProgress(p.ISBN, p.Pages), success)
}

func (r *ReadingService) AdvanceProgress(p Progress, success *bool) error {
    return setSuccess(r.ReadingList.AdvanceProgress(p.ISBN, p.Pages), success)
}
```

定义的类型可以在 RPC 服务器中注册和使用，后者将使用 `rpc.HandleHTTP` 为传入的 RPC 消息注册 HTTP 处理器。




```go
if len(os.Args) != 2 {
    log.Fatalln("Please specify an address.")
}
if err := rpc.Register(&common.ReadingService{}); err != nil {
    log.Fatalln(err)
}
rpc.HandleHTTP()

l, err := net.Listen("tcp", os.Args[1])
if err != nil {
    log.Fatalln(err)
}
log.Println("Server Started")
if err := http.Serve(l, nil); err != nil {
    log.Fatal(err)
}
```

## 创建客户端

可以使用 RPC 包的 `rpc.DialHTTP` 函数创建客户端，使用相同的主机端口来获取客户端：

```go
if len(os.Args) != 2 {
    log.Fatalln("Please specify an address.")
}
client, err := rpc.DialHTTP("tcp", os.Args[1])
if err != nil {
    log.Fatalln(err)
}
defer client.Close()
```

然后，我们定义一个将在示例中使用的书籍列表：

```go
const hp = "H.P. Lovecraft"
var books = []common.Book{
    {ISBN: "1540335534", Author: hp, Title: "The Call of Cthulhu", Pages: 36},
    {ISBN: "1980722803", Author: hp, Title: "The Dunwich Horror ", Pages: 53},
    {ISBN: "197620299X", Author: hp, Title: "The Shadow Over Innsmouth", Pages: 40},
    {ISBN: "1540335534", Author: hp, Title: "The Case of Charles Dexter Ward", Pages: 176},
}
```

考虑到 fmt 包会打印指向内置类型的指针的地址，我们将定义一个辅助函数来显示指针的内容：

```go
func callClient(client *rpc.Client, method string, in, out interface{}) {
    var r interface{}
    if err := client.Call(method, in, out); err != nil {
        out = err
    }
    switch v := out.(type) {
    case error:
        r = v
    case *int:
        r = *v
    case *bool:
        r = *v
    }
    log.Printf("%s: [%+v] -> %+v", method, in, r)
}
```

客户端以 `type.method` 的形式获取要执行的操作，因此我们将按如下方式使用该函数：

```go
callClient(client, "ReadingService.GetProgress", books[0].ISBN, new(int))
callClient(client, "ReadingService.AddBook", books[0], new(bool))
callClient(client, "ReadingService.AddBook", books[0], new(bool))
callClient(client, "ReadingService.GetProgress", books[0].ISBN, new(int))
callClient(client, "ReadingService.AddBook", books[1], new(bool))
callClient(client, "ReadingService.AddBook", books[2], new(bool))
callClient(client, "ReadingService.AddBook", books[3], new(bool))
callClient(client, "ReadingService.SetProgress", common.Progress{
    ISBN: books[3].ISBN,
    Pages: 10,
}, new(bool))
callClient(client, "ReadingService.GetProgress", books[3].ISBN, new(int))
callClient(client, "ReadingService.AdvanceProgress", common.Progress{
    ISBN: books[3].ISBN,
    Pages: 40,
}, new(bool))
callClient(client, "ReadingService.GetProgress", books[3].ISBN, new(int))
```

这将输出每个操作及其结果。

## 总结

在本章中，我们探讨了 Go 中如何处理网络连接。我们从一些网络标准开始。首先，我们讨论了 OSI 模型，然后是 TCP/IP。

然后，我们检查了 `net` 包，并学习了如何使用它来创建和管理 TCP 连接。这包括特殊命令的处理以及如何从服务器端终止连接。接下来，我们看到了如何用 UDP 做同样的事情，并了解了如何实现带有校验和控制的自定义编码。

然后，我们讨论了 HTTP 协议，解释了第一个版本的工作原理，然后谈论了 HTTP/2 的差异和改进。接着，我们学习了如何使用 Go 发起 HTTP 请求，随后是如何设置 Web 服务器。我们探讨了如何服务现有文件，如何将不同的操作关联到不同的 HTTP 方法，以及如何处理多部分请求和文件上传。我们轻松设置了 HTTPS 服务器，然后了解了一些第三方 Web 服务器库提供的优势。最后，我们演示了 Go 中模板引擎的工作原理，以及如何轻松构建 RPC 客户端/服务器。

在下一章中，我们将介绍如何使用主要的数据交换格式，如 JSON 和 XML，它们也可以用来创建 Web 服务器。

## 问题

1.  使用通信模型有什么优势？
2.  TCP 连接和 UDP 连接有什么区别？
3.  发送请求时谁关闭请求体？
4.  在服务器接收请求时谁关闭主体？
