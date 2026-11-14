
# HTTP Client

The package exposes a `http.Client` type that can be used to make requests. If the requests are simple `GET` or `POST`, then there are dedicated methods. The package also offers a function with the same name, but it's just a shorthand to the respective methods for the `DefaultClient` instance. Check the following code:

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

For any other kind of requirement, the `Do` method allows us to execute a specific `http.Request`. The `NewRequest` function allows us to specify any `io.Reader`:

```
req, err := http.NewRequest("GET", "http://example.com", nil)
// ...
req.Header.Add("Content-Type", "text/html")
resp, err := client.Do(req)
// ...
```

`http.Client` has several fields and many of these are interfaces that allows us to use the default implementation or a custom one. The first is `CookieJar` that allows the client to store and reuse web cookies. A cookie is a piece of data that the browser sends to the client and the client can send back to the server to replace headers, such as authentication. The default client does not use a cookie jar. The other interface is `RoundTripper`, which has only one method, `RoundTrip`, which gets a request and returns a response. There is a `DefaultTransport` value used if no value is specified that can also be used to compose a custom implementation of `RoundTripper`. `http.Response` returned by the client has also a body, which is `io.ReadCloser`, and its closure is the responsibility of the application. That's why it's recommended to use a deferred `Close` statement as soon as the response is obtained. In the following example, we will implement a custom transport that logs the requested URL and modifies one header before executing the standard round tripper:

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

We will use this transport in a client that we will use to make a simple request:

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

# Creating a Simple Server

The other functionality offered by the package is the server creation. The main interface of the package is `Handle`, which has one method, `ServeHTTP`, that uses the request to write a response. Its simplest implementation is `HandlerFunc`, which is a function with the same signature of `ServeHTTP` and implements `Handler` by executing itself.

The `ListenAndServe` function starts a HTTP server using the given address and handler. If no handler is specified, it uses the `DefaultServeMux` variable. `ServeMux` is special type of `Handler` that manages the execution of different handlers, depending on the URL path requested. It has two methods, `Handle` and `HandleFunc`, that allow the user to specify the path and the respective handler. The package also offers generic handler functions that are similar to what we have seen for the `Client`, they will call the methods with same name from the default `ServerMux`.

In the following example, we will create a `customHandler` and create a simple server with some endpoints, including the custom one:

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

# Serving Filesystem

The Go standard package allows us to easily serve a certain directory in the filesystem, using the `http.FileServer` function, which, when given the `http.FileSystem` interface, returns a `Handler` that serves that directory. The default implementation is `http.Dir`, which is a custom string that represents a directory in the system. The `FileServer` function already has a protection mechanism that prevents us to use a relative path, such as `../../../dir`, to access directories outside the served one.

The following is a sample file server that uses the directory provided as an argument, as the root of the file served:

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

# Navigating Through Routes and Methods

The HTTP method used is stored in the `Request.Method` field. This field can be used inside a handler to have different behaviors for each supported method:

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

The advantage of the `http.Handler` interface is that we can define our custom types. This can make the code more readable and can generalize this method-specific behavior:

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

This will make the code much more readable, and it will be reusable for different paths:

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

# Multipart Request and Files

The request body is an `io.ReadCloser`. This means that it is the server's responsibility to close it. For a file upload, the body is not content of the file directly, but it is usually a multipart request, which is a request that specifies a boundary in the header and uses it inside the body to separate the message in parts.

This is a sample multipart message:
