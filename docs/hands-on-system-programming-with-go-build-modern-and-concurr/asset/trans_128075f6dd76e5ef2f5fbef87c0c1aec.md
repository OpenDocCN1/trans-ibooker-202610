
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
*   **暴露 Web API**：不由用户直接使用、而是由其他应用程序使用的远程接口。更多详情请参阅[第 1 章](#27b19b4d-7890-4fdc-9619-5204d0a7a60d.xhtml) *系统编程简介*。

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
