
# Slices

A slice allows you to get its size with the `Len` method and to access its elements using the `Index` method. Let's see that in action in the following code:

```go
func main() {
    m := []int{10, 20, 100}
    v := reflect.ValueOf(m)

    for i := 0; i < v.Len(); i++ {
        fmt.Println(i, v.Index(i))
    }
}
```

A full example is available here: [https://play.golang.org/p/ifq0O6bFIZc](https://play.golang.org/p/ifq0O6bFIZc).

Since it is always possible to get the address of a slice element, it is also possible to use `reflect.Value` to change the content of the respective element in the slice:

```go
func main() {
    m := []int64{10, 20, 100}
    v := reflect.ValueOf(m)

    for i := 0; i < v.Len(); i++ {
        v.Index(i).SetInt(v.Index(i).Interface().(int64) * 2)
    }
    fmt.Println(m)
}
```

A full example is available here: [https://play.golang.org/p/onuIvWyQ7GY](https://play.golang.org/p/onuIvWyQ7GY).

It is also possible to append to a slice using the `reflect` package. If the value is obtained from the pointer to the slice, the result of this operation can also be used to replace the original slice:

```go
func main() {
    var s = []int{1, 2}
    fmt.Println(s)

    v := reflect.ValueOf(s)
    // same as append(s, 3)
    v2 := reflect.Append(v, reflect.ValueOf(3))
    // s can't and does not change
    fmt.Println(v.CanSet(), v, v2)
    
    // using the pointer allows change
    v = reflect.ValueOf(&s).Elem()
    v.Set(v2)
    fmt.Println(v.CanSet(), v, v2)
}
```

A full example is available here: [https://play.golang.org/p/2hXRg7Ih9wk](https://play.golang.org/p/2hXRg7Ih9wk).

# Functions

Method and function handling with reflection allow you to gather information about the signature of a certain entry and also to invoke it.

## Analyzing a function

There are a few methods of `reflect.Type` in the package that will return information about a function. These methods are as follows:

- `NumIn`: Returns the number of input arguments of the function
- `In`: Returns the selected input argument
- `IsVariadic`: Tells you if the last argument of the function is variadic
- `NumOut`: Returns the number of output values returned by the function
- `Out`: Returns the `Type` value of the select output

Note that all these methods will panic if the kind of `reflect.Type` is not `Func`. We can test these methods by defining a series of functions:

```go
func Foo() {}

func Bar(a int, b string) {}

func Baz(a int, b string) (int, error) { return 0, nil }

func Qux(a int, b ...string) (int, error) { return 0, nil }
```

Now we can use the method from `reflect.Type` to obtain information about them:

```go
func main() {
    for _, f := range []interface{}{Foo, Bar, Baz, Qux} {
        t := reflect.TypeOf(f)
        name := runtime.FuncForPC(reflect.ValueOf(f).Pointer()).Name()
        in := make([]reflect.Type, t.NumIn())
        for i := range in {
            in[i] = t.In(i)
        }
        out := make([]reflect.Type, t.NumOut())
        for i := range out {
            out[i] = t.Out(i)
        }
        fmt.Printf("%q %v %v %v\n", name, in, out, t.IsVariadic())
    }
}
```

A full example is available here: [https://play.golang.org/p/LAjjhw8Et60](https://play.golang.org/p/LAjjhw8Et60).

In order to obtain the name of the functions, we use the `runtime.FuncForPC` function, which returns `runtime.Func` containing methods that will expose runtime information about the function—`name`, `file`, and `line`. The function takes `uintptr` as an argument, which can be obtained with `reflect.Value` of the function and its `Pointer` method.

## Invoking a function

While the type of the function shows information about it, in order to call a function, we need to use its value.

We will pass the function a list of argument values and get back the ones returned by the function call:

```go
func main() {
    for _, f := range []interface{}{Foo, Bar, Baz, Qux} {
        v, t := reflect.ValueOf(f), reflect.TypeOf(f)
        name := runtime.FuncForPC(v.Pointer()).Name()
        in := make([]reflect.Value, t.NumIn())
        for i := range in {
            switch a := t.In(i); a.Kind() {
            case reflect.Int:
                in[i] = reflect.ValueOf(42)
            case reflect.String:
                in[i] = reflect.ValueOf("42")
            case reflect.Slice:
                switch a.Elem().Kind() {
                case reflect.Int:
                    in[i] = reflect.ValueOf(21)
                case reflect.String:
                    in[i] = reflect.ValueOf("21")
                }
            }
        }
        out := v.Call(in)
        fmt.Printf("%q %v%v\n", name, in, out)
    }
}
```

A full example is available here: [https://play.golang.org/p/jPxO_G7YP2I](https://play.golang.org/p/jPxO_G7YP2I).

# Channels

Reflection allows us to create channels, send and receive data, and also to use `select` statements.

## Creating channels

A new channel can be created via the `reflect.MakeChan` function, which requires a `reflect.Type` interface value and a size:

```go
func main() {
    t := reflect.ChanOf(reflect.BothDir, reflect.TypeOf(""))
    v := reflect.MakeChan(t, 0)
    fmt.Printf("%T\n", v.Interface())
}
```

A full example is available here: [https://play.golang.org/p/7_RLtzjuTcz](https://play.golang.org/p/7_RLtzjuTcz).

## Sending, receiving, and closing

The `reflect.Value` type offers a few methods that have to be used exclusively with channels: `Send` and `Recv` for sending and receiving, and `Close` for closing channels. Let's take a look at a sample use case of these functions and methods:

```go
func main() {
    t := reflect.ChanOf(reflect.BothDir, reflect.TypeOf(""))
    v := reflect.MakeChan(t, 0)
    go func() {
        for i := 0; i < 10; i++ {
            v.Send(reflect.ValueOf(fmt.Sprintf("msg-%d", i)))
        }
        v.Close()
    }()
    for msg, ok := v.Recv(); ok; msg, ok = v.Recv() {
        fmt.Println(msg)
    }
}
```

A full example is available here: [https://play.golang.org/p/Gp8JJmDbLIL](https://play.golang.org/p/Gp8JJmDbLIL).

## Select statement

A `select` statement can be executed with the `reflect.Select` function. Each case is represented by a data structure:

```go
type SelectCase struct {
    Dir  SelectDir // direction of case
    Chan Value     // channel to use (for send or receive)
    Send Value     // value to send (for send)
}
```

It contains the direction of the operation and both the channel and the value (for send operations). The direction can be either send, receive, or none (for default statements):

```go
func main() {
    v := reflect.ValueOf(make(chan string, 1))
    fmt.Println("sending", v.TrySend(reflect.ValueOf("message"))) // true 1 1
    branches := []reflect.SelectCase{
        {Dir: reflect.SelectRecv, Chan: v, Send: reflect.Value{}},
        {Dir: reflect.SelectSend, Chan: v, Send: reflect.ValueOf("send")},
        {Dir: reflect.SelectDefault},
    }

    // send, receive and default
    i, recv, closed := reflect.Select(branches)
    fmt.Println("select", i, recv, closed)

    v.Close()
    // just default and receive
    i, _, closed = reflect.Select(branches[:2])
    fmt.Println("select", i, closed) // 1 false
}
```

A full example is available here: [https://play.golang.org/p/_DgSYRIBkJA](https://play.golang.org/p/_DgSYRIBkJA).

# Reflecting on reflection
