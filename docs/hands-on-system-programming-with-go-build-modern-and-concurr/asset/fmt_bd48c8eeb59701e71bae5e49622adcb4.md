
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

## Creating the client

The client can be created using the RPC package's `rpc.DialHTTP` function, using the same host-port to obtain a client:

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

Then, we define a list of books that we are going to use for our example:

```go
const hp = "H.P. Lovecraft"
var books = []common.Book{
    {ISBN: "1540335534", Author: hp, Title: "The Call of Cthulhu", Pages: 36},
    {ISBN: "1980722803", Author: hp, Title: "The Dunwich Horror ", Pages: 53},
    {ISBN: "197620299X", Author: hp, Title: "The Shadow Over Innsmouth", Pages: 40},
    {ISBN: "1540335534", Author: hp, Title: "The Case of Charles Dexter Ward", Pages: 176},
}
```

Considering that the format package prints the address of pointers to built-in types, we are going to define a helper function to show the pointer's content:

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

The client gets the operation to execute in the form of `type.method`, so we will be using the function like this:

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

This will output each operation with its result.

## Summary

In this chapter, we examined how network connections are handled in Go. We started with some network standards. First, we discussed the OSI model, and then TCP/IP.

Then, we checked the `net` package and learned how to use it to create and manage TCP connections. This included the handling of special commands and how to terminate the connection from the server side. Next, we saw how to do the same with UDP, and we have seen how to implement a custom encoding with checksum control.

Then, we discussed the HTTP protocol, explained how the first version works, and then talked about the differences and improvements of HTTP/2. Then, we learned how to make an HTTP request using Go, followed by how to set up a web server. We explored how to serve existing files, how to associate different actions to different HTTP methods, and how to handle multipart requests and file uploads. We set up an HTTPS server easily, and then we learned what advantages are offered by some third-party libraries for web servers. Finally, we demonstrated how the template engine works in Go, and how to build an RPC client/server easily.

In the next chapter, we are going to cover how to use the main data interchange format such as JSON and XML, which can also be used to create web servers.

## Questions

1.  What's the advantage of using communication models?
2.  What's the difference between a TCP and a UDP connection?
3.  Who closes the request body when sending requests?
4.  Who closes the body when receiving them in the server?

# Data Encoding Using Go

This chapter will show you how to use the more common encoding for exchanging data in an application. Encoding is the process of transforming data, and it can be used when an application has to communicate with another—using the same encoding will allow the two programs to understand each other. This chapter will explain how to handle text-based protocols such as `JSON`, first of all, and then how to use binary protocols such as `gob`.

The following topics will be covered in this chapter:

*   Using text-based encoding such as `JSON` and `XML`
*   Learning about binary encoding such as `gob` and `protobuf`

## Technical requirements

This chapter requires Go to be installed and your favorite editor to be set up. For more information, please refer to [Chapter 3](#602a92d5-25f7-46b8-83d4-10c6af1c6750.xhtml), *An Overview of Go*.

In order to use the protocol buffer, you will need to install the `protobuf` library. Instructions are available at <https://github.com/golang/protobuf>.

## Understanding text-based encoding

The most human-readable data serialization format is the text-based format. In this section, we will analyze some of the most used text-based encoding—`CSV`, `JSON`, `XML`, and `YAML`.

### CSV

**Comma-separated values** (**CSV**) is a type of text encoding that stores data in a tabular form. Each line is a table entry and values of a line are separated by a special character, which is usually a comma—hence, the name CSV. Each record of the CSV file must have the same value count and the first record can be used as a header to describe each record field:

```
name,age,country
```

String values can be quoted in order to permit the use of the comma.

### Decoding values

Go allows users to create a `CSV` reader from any `io.Reader`. It is possible to read records one by one using the `Read` method:

```go
func main() {
    r := csv.NewReader(strings.NewReader("a,b,c\ne,f,g\n1,2,3"))
    for {
        r, err := r.Read()
        if err != nil {
            log.Fatal(err)
        }
        log.Println(r)
    }
}
```

A full example of the preceding code is available at <https://play.golang.org/p/wZgVzMqAN_K>.

Note that each record is a string slice and the reader is expecting the length of each row to be consistent. If a row has more or fewer entries than the first, this will result in an error. It is also possible to read all records at once using `ReadAll`. The same example from before using such a method will look like this:

```go
func main() {
    r := csv.NewReader(strings.NewReader("a,b,c\ne,f,g\n1,2,3"))
    records, err := r.ReadAll()
    if err != nil {
        log.Fatal(err)
    }
    for _, r := range records {
        log.Println(r)
    }
}
```

A full example of the preceding code is available at <https://play.golang.org/p/RJ-wxBB5fs6>.

### Encoding values

A `CSV` writer can be created using any `io.Writer`. The resulting writer will be buffered, so, in order to not lose data, it is necessary to call its method, `Flush`: this will ensure that the buffer gets drained and all content goes to the writer.

The `Write` method receives a string slice and encodes it in `CSV` format. Let's see how it works in the following example:
