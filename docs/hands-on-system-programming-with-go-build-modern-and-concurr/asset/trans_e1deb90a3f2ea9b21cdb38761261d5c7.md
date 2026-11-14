
# 切片

切片允许您使用 `Len` 方法获取其大小，并使用 `Index` 方法访问其元素。让我们在以下代码中看到其实际运行情况：

```go
func main() {
    m := []int{10, 20, 100}
    v := reflect.ValueOf(m)

    for i := 0; i < v.Len(); i++ {
        fmt.Println(i, v.Index(i))
    }
}
```

完整示例请见此处：[https://play.golang.org/p/ifq0O6bFIZc](https://play.golang.org/p/ifq0O6bFIZc)。

由于总是可以获取切片元素的地址，因此也可以使用 `reflect.Value` 来更改切片中相应元素的内容：

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

完整示例请见此处：[https://play.golang.org/p/onuIvWyQ7GY](https://play.golang.org/p/onuIvWyQ7GY)。

也可以使用 `reflect` 包向切片追加元素。如果该值是从指向切片的指针获取的，则此操作的结果也可用于替换原始切片：

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

完整示例请见此处：[https://play.golang.org/p/2hXRg7Ih9wk](https://play.golang.org/p/2hXRg7Ih9wk)。

# 函数

使用反射处理方法和函数，允许您收集有关特定条目签名的信息，并调用它。

## 分析函数

包中有几个 `reflect.Type` 的方法会返回关于函数的信息。这些方法如下：

- `NumIn`：返回函数的输入参数数量
- `In`：返回选定的输入参数
- `IsVariadic`：告诉您函数的最后一个参数是否为可变参数
- `NumOut`：返回函数返回的输出值数量
- `Out`：返回选定输出的 `Type` 值

请注意，如果 `reflect.Type` 的种类不是 `Func`，所有这些方法都会 panic。我们可以通过定义一系列函数来测试这些方法：

```go
func Foo() {}

func Bar(a int, b string) {}

func Baz(a int, b string) (int, error) { return 0, nil }

func Qux(a int, b ...string) (int, error) { return 0, nil }
```

现在我们可以使用 `reflect.Type` 的方法来获取关于它们的信息：

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

完整示例请见此处：[https://play.golang.org/p/LAjjhw8Et60](https://play.golang.org/p/LAjjhw8Et60)。

为了获取函数名称，我们使用 `runtime.FuncForPC` 函数，它返回包含方法的 `runtime.Func`，这些方法将暴露关于函数的运行时信息——`name`、`file` 和 `line`。该函数接受 `uintptr` 作为参数，可以通过函数的 `reflect.Value` 及其 `Pointer` 方法获得。

## 调用函数

虽然函数的类型显示了关于它的信息，但为了调用函数，我们需要使用它的值。

我们将传递一组参数值给函数，并获得函数调用返回的值：

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

完整示例请见此处：[https://play.golang.org/p/jPxO_G7YP2I](https://play.golang.org/p/jPxO_G7YP2I)。

# 通道

反射允许我们创建通道、发送和接收数据，以及使用 `select` 语句。

## 创建通道

可以通过 `reflect.MakeChan` 函数创建一个新通道，该函数需要一个 `reflect.Type` 接口值和一个大小：

```go
func main() {
    t := reflect.ChanOf(reflect.BothDir, reflect.TypeOf(""))
    v := reflect.MakeChan(t, 0)
    fmt.Printf("%T\n", v.Interface())
}
```

完整示例请见此处：[https://play.golang.org/p/7_RLtzjuTcz](https://play.golang.org/p/7_RLtzjuTcz)。

## 发送、接收和关闭

`reflect.Value` 类型提供了一些必须专门用于通道的方法：用于发送和接收的 `Send` 和 `Recv`，以及用于关闭通道的 `Close`。让我们看看这些函数和方法的一个示例用例：

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

完整示例请见此处：[https://play.golang.org/p/Gp8JJmDbLIL](https://play.golang.org/p/Gp8JJmDbLIL)。

## Select 语句

`select` 语句可以通过 `reflect.Select` 函数执行。每个 case 由一个数据结构表示：

```go
type SelectCase struct {
    Dir  SelectDir // direction of case
    Chan Value     // channel to use (for send or receive)
    Send Value     // value to send (for send)
}
```

它包含操作的方向、通道和值（用于发送操作）。方向可以是发送、接收或 none（用于 default 语句）：

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

完整示例请见此处：[https://play.golang.org/p/_DgSYRIBkJA](https://play.golang.org/p/_DgSYRIBkJA)。

# 反思反射
