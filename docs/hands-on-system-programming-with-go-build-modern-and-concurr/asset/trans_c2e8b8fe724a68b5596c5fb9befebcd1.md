
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
