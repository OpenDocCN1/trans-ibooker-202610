
# Incoming Stream Functions

When it comes to incoming streams, there are a lot of functions available in the standard library to improve the capabilities of readers. One of the easiest examples is `ioutil.NopCloser`, which takes a reader and returns `io.ReadCloser`, which does nothing. This is useful if a function is in charge of releasing a resource, but the reader used is not `io.Closer` (like in `bytes.Buffer`).

## Constraining Bytes Read

There are two tools that constrain the number of bytes read. The `ReadAtLeast` function defines a minimum number of bytes to read. The result will be `EOF` only if there are no bytes to read; otherwise, if a smaller number of bytes is read before `EOF`, `ErrUnexpectedEOF` will be returned. If the bytes buffer is shorter than the bytes requested, which does not make sense, there will be a `ErrShortBuffer`. In the event of a reading error, the function manages to read at least the desired number of bytes, and that error is dropped.

There is then `ReadFull`, which is expected to fill the buffer and will return `ErrUnexpectedEOF` otherwise.

## Limiting Readers

The other constraining function is `LimitReader`. This function is a decorator that gets a reader and returns another reader that will return `EOF` once the desired bytes are read. This could be used for a preview of the content of an actual reader, as in the following example:

```go
s := strings.NewReader(`Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged.`)
io.Copy(os.Stdout, io.LimitReader(s, 25)) // will print "Lorem Ipsum is simply dum"
```

The full example is available at [https://play.golang.org/p/LllOdWg9uyU](https://play.golang.org/p/LllOdWg9uyU).

## Combining Readers

More readers can be combined in a sequence with the `MultiReader` function will read each part sequentially until it reaches `EOF`, and then jump to the next one.

## Connecting Readers and Writers

One reader and one writer can be connected so that whatever comes from the reader is copied to the writer—the opposite of what happens with `io.Pipe`. This is done via `io.TeeReader`.

## Example: Filesystem Search Writer

Let's try to use it to create a writer that acts as a search engine in the filesystem, printing only the rows with a match to the query requested. We want a program that does the following:

*   Reads a directory path and a string to search from the arguments
*   Gets a list of files in the selected path
*   Reads each file and passes the lines that contain the selected string to another writer
*   This other writer will inject color characters to highlight the string and copy its content to the standard output

### Color Injection

Let's start with color injection. In a Unix shell, colored output is obtained with the following sequence:

*   `\x1b[`: An escape character
*   `\[`: An opening bracket
*   `39`: A number
*   `m`: The letter `m`

The number determines both the background and foreground color. For this example, we'll use `31` (red) and `39` (default).

### Query Writer Implementation

We are creating a writer that will print the rows with a match and highlight the text:

```go
type queryWriter struct {
    Query []byte
    io.Writer
}

func (q queryWriter) Write(b []byte) (n int, err error) {
    lines := bytes.Split(b, []byte{'\n'})
    l := len(q.Query)
    for _, b := range lines {
        i := bytes.Index(b, q.Query)
        if i == -1 {
            continue
        }
        for _, s := range [][]byte{
            b[:i], // what's before the match
            []byte("\x1b[31m"), // start red color
            b[i : i+l], // match
            []byte("\x1b[39m"), // default color
            b[i+l:], // whatever is left
        } {
            v, err := q.Writer.Write(s)
            n += v
            if err != nil {
                return 0, err
            }
        }
        fmt.Fprintln(q.Writer)
    }
    return len(b), nil
}
```

### Main Function

This will be used with `TeeReader` with an open file, so that reading the file will write to `queryWriter`:

```go
func main() {
    if len(os.Args) < 3 {
        fmt.Println("Please specify a path and a search string.")
        return
    }
    root, err := filepath.Abs(os.Args[1]) // get absolute path
    if err != nil {
        fmt.Println("Cannot get absolute path:", err)
        return
    }
    q := []byte(strings.Join(os.Args[2:], " "))
    fmt.Printf("Searching for %q in %s...\n", query, root)
    err = filepath.Walk(root, func(path string, info os.FileInfo,
        err error) error {
            if info.IsDir() {
                return nil
            }
            fmt.Println(path)
            f, err := os.Open(path)
            if err != nil {
                return err
            }
            defer f.Close()

            _, err = ioutil.ReadAll(io.TeeReader(f, queryWriter{q, os.Stdout}))
            return err
    })
    if err != nil {
        fmt.Println(err)
    }
}
```

As you can see, there is no need to write; reading from the file automatically writes to the query writer that is connected to the standard output.

# Writers and Decorators

There are a plethora of tools to enhance, decorate, and use for readers, but the same thing does not apply to writers.

There is also the `io.WriteString` function, which prevents unnecessary conversions from strings to bytes. First, it checks whether the writer supports string writing, attempting a cast to `io.stringWriter`, an unexported interface with just the `WriteString` method, then writes the string if successful, or converts it into bytes otherwise.

There is the `io.MultiWriter` function, which creates a writer that replicates the information to a series of other writers, which it receives upon creation. A practical example is writing some content while showing it on the standard output, as in the following example:

```go
r := strings.NewReader("let's read this message\n")
b := bytes.NewBuffer(nil)
w := io.MultiWriter(b, os.Stdout)
io.Copy(w, r) // prints to the standard output
fmt.Println(b.String()) // buffer also contains string now
```

The full example is available at [https://play.golang.org/p/ZWDF2vCDfsM](https://play.golang.org/p/ZWDF2vCDfsM).

There is also a useful variable, `ioutil.Discard`, which is a writer that writes to `/dev/null`, a null device. This means that writing to this variable ignores the data.

# Summary

In this chapter, we introduced the concept of streams for describing incoming and outgoing flows of data. We saw that the reader interface represents the data received, while the writer is the sent data.

We compared the different readers that are available in the standard package. We looked at files in the previous chapter, and in this one we added byte and string readers to the list. We learned how to implement custom readers with an example, and saw that it's always good to design a reader to be built on top of another.

Then, we focused on writers. We discovered that files are also writers if opened correctly and that there are several writers in the standard package, including the byte buffer and the string builder. We also implemented a custom writer and saw how to handle bytes and runes with the `utf8` package.

Finally, we explored the remaining functionality in `io` and `ioutil`, analyzing the various tools offered for copying data, and connecting readers and writers. We also saw which decorators are available for improving or changing readers' and writers' capabilities.

In the next chapter, we will talk about pseudo terminal applications, and we will use all that knowledge to build some of them.

# Questions

1.  What's a stream?
2.  What interfaces abstract incoming streams?
3.  Which interfaces represent outgoing streams?
4.  When should a bytes reader be used? When should a string reader be used instead?
