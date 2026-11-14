# 使用反射

本章讲述**反射**，这是一种允许应用程序检查自身代码的工具，克服了 Go 静态类型及其缺乏泛型所施加的一些限制。这非常有用，例如，用于生成能够处理接收到的任何类型输入的包。

本章将涵盖以下主题：

*   理解接口和类型断言
*   了解与基本类型的交互
*   在复杂类型上使用反射
*   评估反射的成本
*   学习反射使用的最佳实践

## 技术要求

本章要求已安装 Go 并设置好您喜欢的编辑器。更多信息请参阅**第 3 章**，*Go 概览*。

## 什么是反射？

反射是一项非常强大的功能，它允许**元编程**，即应用程序检查自身结构的能力。它在运行时分析应用程序中的类型非常有用，并被许多编码包（如 `JSON` 和 `XML`）所使用。

## 类型断言

我们在**第 3 章**，*Go 概览*中简要提到了类型断言的工作原理。类型断言是一种允许我们从接口转换到具体类型，反之亦然的操作。它采用以下形式：

```go
// 不安全的断言
v := SomeVar.(SomeType)

// 安全的断言
v, ok := SomeVar.(SomeType)
```

第一个版本是不安全的，它将值赋给单个变量。

将断言作为函数的参数使用也算作不安全。如果断言错误，此类操作会引发 panic：

```go
func main() {
    var a interface{} = "hello"
    fmt.Println(a.(string)) // 正常
    fmt.Println(a.(int))    // panic!
}
```

完整示例请见此处：<https://play.golang.org/p/hNN87SuprGR>。

第二个版本使用布尔值 `ok` 作为第二个返回值，它将显示操作是否成功。如果断言不可能，第一个值将始终是断言类型的零值。





```go
func main() {
    var a interface{} = "hello"
    s, ok := a.(string) // true
    fmt.Println(s, ok)
    i, ok := a.(int) // false
    fmt.Println(i, ok)
}
```

完整示例请见此处：[`play.golang.org/p/BIba2ywkNF_j`](https://play.golang.org/p/BIba2ywkNF_j)。

# 接口断言

断言也可以从一个接口到另一个接口进行。假设有两个不同的接口：

```go
type Fooer interface {
    Foo()
}

type Barer interface {
    Bar()
}
```

让我们定义一个实现了其中一个接口的类型，以及一个同时实现了两个接口的类型：

```go
type A int

func (A) Foo() {}

type B int

func (B) Bar() {}
func (B) Foo() {}
```

如果我们为第一个接口定义一个新变量，只有当底层值的类型同时实现了两个接口时，向第二个接口的断言才会成功；否则将会失败：

```go
func main() {
    var a Fooer
  
    a = A(0)
    v, ok := a.(Barer)
    fmt.Println(v, ok)

    a = B(0) 
    v, ok = a.(Barer)
    fmt.Println(v, ok)
}
```

完整示例请见此处：[`play.golang.org/p/bX2rnw5pRXJ`](https://play.golang.org/p/bX2rnw5pRXJ)。

一个用例场景可能是拥有 `io.Reader` 接口，检查它是否也是 `io.Closer` 接口，如果不是，则使用 `ioutil.NopCloser` 函数（返回 `io.ReadCloser` 接口）进行包装：

```go
func Closer(r io.Reader) io.ReadCloser {
    if rc, ok := r.(io.ReadCloser); ok {
        return rc
    }
    return ioutil.NopCloser(r)
}

func main() {
    log.Printf("%T", Closer(&bytes.Buffer{}))
    log.Printf("%T", Closer(&os.File{}))
}
```

完整示例请见此处：[`play.golang.org/p/hUEsDYHFE7i`](https://play.golang.org/p/hUEsDYHFE7i)。

在进入反射主题之前，我们需要强调接口的一个重要方面——它的表示始终是一个接口-值元组，其中值是具体类型，不能是另一个接口。

## 理解基本机制

`reflect` 包允许你从任何 `interface{}` 变量中提取类型和值。这可以通过以下方式完成：

*   使用 `reflect.TypeOf()` 以 `reflect.Type` 变量的形式返回接口的类型。
*   `reflect.ValueOf()` 函数使用 `reflect.Value` 变量返回接口的值。

## Value 和 Type 方法

`reflect.Value` 类型也携带类型信息，可以通过 `Type()` 方法检索：

```go
func main() {
    var a interface{} = int64(23)
    fmt.Println(reflect.TypeOf(a).String())
    // int64
    fmt.Println(reflect.ValueOf(a).String())
    // <int64 Value>
    fmt.Println(reflect.ValueOf(a).Type().String())
    // int64
}
```

完整示例请见此处：[`play.golang.org/p/tmYuMc4AF1T`](https://play.golang.org/p/tmYuMc4AF1T)。

## Kind

`reflect.Type` 的另一个重要属性是 `Kind`，它是基本类型和通用复合类型的枚举。`reflect.Kind` 和 `reflect.Type` 之间的主要关系是，前者代表后者的内存表示。

对于内置类型，`Kind` 和 `Type` 是相同的，但对于自定义类型它们会有所不同——`Type` 值将是预期的类型，但 `Kind` 值将是自定义类型所基于的内置类型之一：

```go
func main() {
    var a interface{}

    a = "" // 内置 string
    t := reflect.TypeOf(a)
    fmt.Println(t.String(), t.Kind())

    type A string // 自定义类型
    a = A("")
    t = reflect.TypeOf(a)
    fmt.Println(t.String(), t.Kind())
}
```

完整示例请见此处：[`play.golang.org/p/qjiouk88INn`](https://play.golang.org/p/qjiouk88INn)。

对于复合类型，它将只反映主类型而非底层类型。这意味着指向结构体的指针或指向整数的指针具有相同的 kind，即 `reflect.Pointer`：

```go
func main() {
    var a interface{}

    a = new(int) // int 指针
    t := reflect.TypeOf(a)
    fmt.Println(t.String(), t.Kind())

    a = new(struct{}) // 结构体指针
    t = reflect.TypeOf(a)
    fmt.Println(t.String(), t.Kind())
}
```

完整示例请见此处：[`play.golang.org/p/-uJjZvTuzVf`](https://play.golang.org/p/-uJjZvTuzVf)。

同样的推理适用于所有其他复合类型，如数组、切片、映射和通道。

## Value 转 interface

正如我们可以从任何 `interface{}` 值获取 `reflect.Value` 一样，我们也可以执行反向操作，从 `reflect.Value` 获取 `interface{}`。这是通过反射值的 `Interface()` 方法完成的，如有必要可转换为具体类型。如果相关方法或函数接受空接口（如 `json.Marshal` 或 `fmt.Println`），则返回值可以直接传递，无需任何转换：

```go
func main() {
    var a interface{} = int(12)
    v := reflect.ValueOf(a)
    fmt.Println(v.String())
    fmt.Printf("%v", v.Interface())
}
```

完整示例请见此处：[`play.golang.org/p/1942Dhm5sap`](https://play.golang.org/p/1942Dhm5sap)。

## 操作值

如果值本身不能被改变，那么在反射中转换值并返回值就没有多大用处。这就是为什么我们的下一步是了解如何使用 `reflect` 包来更改它们。

### 更改值

`reflect.Value` 类型有一系列方法允许你更改底层值：

*   `Set()`：使用另一个 `reflect.Value`
*   `SetBool()`：布尔值
*   `SetBytes()`：字节切片
*   `SetComplex()`：任意复数类型
*   `SetFloat()`：任意浮点类型
*   `SetInt()`：任意有符号整数类型
*   `SetPointer()`：指针
*   `SetString()`：字符串
*   `SetUint()`：任意无符号整数类型

为了设置值，它必须是可编辑的，这仅在特定条件下发生。为了验证这一点，有一个方法 `CanSet()`，如果值可以被更改则返回 `true`。如果值不能被更改但仍调用了 `Set()` 方法，应用程序将会 panic：

```go
func main() {
    var a = int64(12)
    v := reflect.ValueOf(a)
    fmt.Println(v.String(), v.CanSet())
    v.SetInt(24)
}
```

完整示例请见此处：[`play.golang.org/p/hKn8qNtn0gN`](https://play.golang.org/p/hKn8qNtn0gN)。

为了被更改，值必须是可寻址的。如果可以修改对象保存的实际存储位置，则该值是可寻址的。当使用基本内置类型（如 `string`）创建新值时，传递给函数的是 `interface{}`，它包含字符串的一个副本。

更改此副本将导致副本的变化，而对原始变量没有任何影响。这将令人难以置信地困惑，并会让反射这一实用工具的使用变得更加困难。这就是为什么 `reflect` 包选择 panic 而不是这种无用的行为——这是一个设计选择。这解释了为什么最后一个示例会 panic。

我们可以使用指向要更改的值的指针来创建 `reflect.Value`，并使用 `Elem()` 方法访问该值。这将给我们一个可寻址的值，因为我们复制的是指针而不是值，所以反射值仍然是指向变量的指针：

```go
func main() {
    var a = int64(12)
    v := reflect.ValueOf(&a)
    fmt.Println(v.String(), v.CanSet())
    e := v.Elem()
    fmt.Println(e.String(), e.CanSet())
    e.SetInt(24)
    fmt.Println(a)
}
```

完整示例请见此处：[`play.golang.org/p/-X5JsBrlr4Q`](https://play.golang.org/p/-X5JsBrlr4Q)。

### 创建新值

`reflect` 包还允许我们使用类型创建新值。有几个函数允许我们创建值：

*   `MakeChan()` 创建一个新的通道值
*   `MakeFunc()` 创建一个新的函数值
*   `MakeMap()` 和 `MakeMapWithSize()` 创建一个新的映射值




# 创建新值

*   `MakeSlice` 创建一个新的切片值
*   `New` 创建一个指向该类型的新指针
*   `NewAt` 使用选定的地址创建一个指向该类型的新指针
*   `Zero` 创建所选类型的零值

以下代码展示了如何以几种不同的方式创建新值：

```
func main() {
    t := reflect.TypeOf(int64(100))
    // 零值
    fmt.Printf("%#v\n", reflect.Zero(t))
    // 指向 int 的指针
    fmt.Printf("%#v\n", reflect.New(t))
}
```

完整示例请见此处：[`play.golang.org/p/wCTILSK1F1C`](https://play.golang.org/p/wCTILSK1F1C)。

# 处理复杂类型

在了解如何处理反射基础知识后，我们现在将看到如何使用反射来处理结构体和映射等复杂数据类型。

## 数据结构

就可变性而言，结构体的工作方式与基本类型完全相同；我们需要获取指针的反射对象，然后访问其元素，以便能够更改值，因为直接使用结构体会生成其副本，并在更改值时发生 panic。

在获取新值的反射对象后，我们可以使用 `Set` 方法替换整个结构体的值：

```
func main() {
    type X struct {
        A, B int
        c string
    }
    var a = X{10, 100, "apple"}
    fmt.Println(a)
    e := reflect.ValueOf(&a).Elem()
    fmt.Println(e.String(), e.CanSet())
    e.Set(reflect.ValueOf(X{1, 2, "banana"}))
    fmt.Println(a)
}
```

完整示例请见此处：[`play.golang.org/p/mjb3gJw5CeA`](https://play.golang.org/p/mjb3gJw5CeA)。

## 更改字段

单个字段也可以使用 `Field` 方法进行修改：

*   `Field` 使用索引返回一个字段
*   `FieldByIndex` 使用一系列索引返回一个嵌套字段
*   `FieldByName` 使用名称返回一个字段
*   `FieldByNameFunc` 使用 `func(string) bool` 在名称中返回一个字段

让我们定义一个结构体来更改字段的值，同时使用简单和复杂类型，并至少包含一个未导出字段：

```
type A struct {
    B
    x int
    Y int
    Z int
}

type B struct {
    F string
    G string
}
```

现在我们有了结构体，可以尝试以不同方式访问字段：

```
func main() {
    var a A
    v := reflect.ValueOf(&a)
    func() {
        // 尝试从指针获取字段会 panic
        defer func() {
            log.Println("panic:", recover())
        }()
        log.Printf("%s", v.Field(1).String())
    }()
    v = v.Elem()
    // 按索引更改字段
    for i := 0; i < 4; i++ {
        f := v.Field(i)
        if f.CanSet() && f.Type().Kind() == reflect.Int {
            f.SetInt(42)
        }
    }
    // 按索引更改嵌套字段
    v.FieldByIndex([]int{0, 1}).SetString("banana")

    // 按名称获取字段
    v.FieldByName("B").FieldByName("F").SetString("apple")

    log.Printf("%+v", a)
}
```

完整示例请见此处：[`play.golang.org/p/z5slFkIU5UE`](https://play.golang.org/p/z5slFkIU5UE)。

当使用 `reflect.Value` 和结构体字段时，你得到的是其他值，它们与结构体本身无法区分。而当处理 `reflect.Type` 时，你会得到一个 `reflect.StructField` 结构体，这是另一种携带字段所有信息的类型。

## 使用标签

结构体字段携带大量信息，从字段名和索引到其标签：

```
type StructField struct {
    Name string
    PkgPath string

    Type Type      // 字段类型
    Tag StructTag  // 字段标签字符串
    Offset uintptr // 结构体内的偏移量，以字节为单位
    Index []int    // Type.FieldByIndex 的索引序列
    Anonymous bool // 是否为嵌入字段
}
```

可以使用 `reflect.Type` 方法获取 `reflect.StructField` 值：

*   `Field`
*   `FieldByName`
*   `FieldByIndex`

它们与 `reflect.Value` 使用的方法相同，但返回不同的类型。`NumField` 方法返回结构体的字段总数，允许我们执行迭代：

```
type Person struct {
    Name string `json:"name,omitempty" xml:"-"`
    Surname string `json:"surname,omitempty" xml:"-"`
}

func main() {
    v := reflect.ValueOf(Person{"Micheal", "Scott"})
    t := v.Type()
    fmt.Println("Type:", t)
    for i := 0; i < t.NumField(); i++ {
       fmt.Printf("%v: %v\n", t.Field(i).Name, v.Field(i))
    }
}
```

完整示例请见此处：[`play.golang.org/p/nkEADg77zFC`](https://play.golang.org/p/nkEADg77zFC)。

标签对于反射至关重要，因为它们可以存储关于字段的额外信息以及其他包如何处理它。要向字段添加标签，需要在字段名和类型后插入一个字符串，该字符串应具有 `key:"value"` 结构。一个字段的标签中可以有多个元组，每对之间用空格分隔。让我们看一个实际例子：

```
type A struct {
    Name    string `json:"name,omitempty" xml:"-"`
    Surname string `json:"surname,omitempty" xml:"-"`
}
```

该结构体有两个字段，都带有标签，且每个标签有两对键值。`Get` 方法返回特定键的值：

```
func main() {
    t := reflect.TypeOf(A{})
    fmt.Println(t)
    for i := 0; i < t.NumField(); i++ {
        f := t.Field(i)
        fmt.Printf("%s JSON=%s XML=%s\n", f.Name, f.Tag.Get("json"), f.Tag.Get("xml"))
    }
}
```

完整示例请见此处：[`play.golang.org/p/P-Te8O1Hyyn`](https://play.golang.org/p/P-Te8O1Hyyn)。

# 映射和切片

你可以轻松使用反射来读取和操作映射和切片。由于它们是编写应用程序的重要工具，让我们看看如何使用反射执行操作。

## 映射

`map` 类型允许你使用 `Key` 和 `Elem` 方法获取键和值的类型：

```
func main() {
    maps := []interface{}{
        make(map[string]struct{}),
        make(map[int]rune),
        make(map[float64][]byte),
        make(map[int32]chan bool),
        make(map[[2]string]interface{}),
    }
    for _, m := range maps {
        t := reflect.TypeOf(m)
        fmt.Printf("%s k:%-10s v:%-10s\n", m, t.Key(), t.Elem())
    }
}
```

完整示例请见此处：[`play.golang.org/p/j__1jtgy-56`](https://play.golang.org/p/j__1jtgy-56)。

值可以通过映射正常访问的所有方式来访问：

*   通过键获取值
*   遍历键
*   遍历值

让我们在一个实际例子中看看它是如何工作的：

```
func main() {
    m := map[string]int64{
        "a": 10,
        "b": 20,
        "c": 100,
        "d": 42,
    }

    v := reflect.ValueOf(m)

    // 访问一个字段
    fmt.Println("a", v.MapIndex(reflect.ValueOf("a")))
    fmt.Println()

    // 遍历键
    for _, k := range v.MapKeys() {
        fmt.Println(k, v.MapIndex(k))
    }
    fmt.Println()

    // 遍历键和值
    i := v.MapRange()
    for i.Next() {
        fmt.Println(i.Key(), i.Value())
    }
}
```

请注意，我们不需要传递指向映射的指针使其可寻址，因为映射本身就是指针。

每种方法都非常直观，具体取决于你需要哪种类型的映射访问。设置值也是可能的，并且应该总是可行的，因为映射是按引用传递的。以下片段展示了一个实际例子：

```
func main() {
    m := map[string]int64{}
    v := reflect.ValueOf(m)

    // 设置一个字段
    v.SetMapIndex(reflect.ValueOf("key"), reflect.ValueOf(int64(1000)))

    fmt.Println(m)
}
```

完整示例请见此处：[`play.golang.org/p/JxK_8VPoWU0`](https://play.golang.org/p/JxK_8VPoWU0).

也可以使用此方法取消设置变量，就像我们使用 `reflect.Value` 的零值作为第二个参数调用 `delete` 函数时所做的那样：



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

完整示例请见此处：[`play.golang.org/p/ifq0O6bFIZc`](https://play.golang.org/p/ifq0O6bFIZc)。

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

完整示例请见此处：[`play.golang.org/p/onuIvWyQ7GY`](https://play.golang.org/p/onuIvWyQ7GY)。

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

完整示例请见此处：[`play.golang.org/p/2hXRg7Ih9wk`](https://play.golang.org/p/2hXRg7Ih9wk)。

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

完整示例请见此处：[`play.golang.org/p/LAjjhw8Et60`](https://play.golang.org/p/LAjjhw8Et60)。

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

完整示例请见此处：[`play.golang.org/p/jPxO_G7YP2I`](https://play.golang.org/p/jPxO_G7YP2I)。

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

完整示例请见此处：[`play.golang.org/p/7_RLtzjuTcz`](https://play.golang.org/p/7_RLtzjuTcz)。

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

完整示例请见此处：[`play.golang.org/p/Gp8JJmDbLIL`](https://play.golang.org/p/Gp8JJmDbLIL)。

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

完整示例请见此处：[`play.golang.org/p/_DgSYRIBkJA`](https://play.golang.org/p/_DgSYRIBkJA)。

# 反思反射




# 性能开销

反射允许代码具有灵活性，并通过分析内存表示来处理未知数据类型。这并非零成本，除了复杂性之外，反射影响的另一个方面是性能。

我们可以创建几个示例来演示使用反射时，某些简单操作会慢得多。我们可以创建一个超时机制，并在 goroutine 中不断重复这些操作。当超时到期时，两个例程都将终止，然后我们对比结果：

```go
func baseTest(fn1, fn2 func(int)) {
    ctx, canc := context.WithTimeout(context.Background(), time.Second)
    defer canc()
    go func() {
        for i := 0; ; i++ {
            select {
            case <-ctx.Done():
                return
            default:
                fn1(i)
            }
        }
    }()
    go func() {
        for i := 0; ; i++ {
            select {
            case <-ctx.Done():
                return
            default:
                fn2(i)
            }
        }
    }()
    <-ctx.Done()
}
```

我们可以将普通的 map 写入与使用反射完成的相同操作进行比较：

```go
func testMap() {
    m1, m2 := make(map[int]int), make(map[int]int)
    m := reflect.ValueOf(m2)
    baseTest(func(i int) { m1[i] = i }, func(i int) {
        v := reflect.ValueOf(i)
        m.SetMapIndex(v, v)
    })
    fmt.Printf("normal %d\n", len(m1))
    fmt.Printf("reflect %d\n", len(m2))
}
```

我们还可以测试结构体字段读取和设置的速度，对比使用反射与不使用反射的情况：

```go
func testStruct() {
    type T struct {
        Field int
    }
    var m1, m2 T
    m := reflect.ValueOf(&m2).Elem()
    baseTest(func(i int) { m1.Field++ }, func(i int) {
        f := m.Field(0)
        f.SetInt(int64(f.Interface().(int) + 1))
    })
    fmt.Printf("normal %d\n", m1.Field)
    fmt.Printf("reflect %d\n", m2.Field)
}
```

与标准的静态方式相比，通过反射执行操作时性能至少下降 **50%**。当性能是应用程序的**最高优先级**时，这种下降可能非常关键，但如果不是这种情况，那么使用反射可能是一个合理的选择。

# 标准库中的用法

标准库中有许多不同的包使用 `reflect` 包：

- `archive/tar`
- `context`
- `database/sql`
- `encoding/asn1`
- `encoding/binary`
- `encoding/gob`
- `encoding/json`
- `encoding/xml`
- `fmt`
- `html/template`
- `net/http`
- `net/rpc`
- `sort/slice`
- `text/template`

我们可以通过以编码包为例，来推断它们使用反射的方式。这些包中的每一个都提供用于编码和解码的接口，例如 `encoding/json` 包。我们定义了以下接口：

```go
type Marshaler interface {
    MarshalJSON() ([]byte, error)
}

type Unmarshaler interface {
    UnmarshalJSON([]byte) error
}
```

在解码或编码时，该包首先检查未知类型是否实现了该接口，如果没有，则使用反射。我们可以将反射视为该包使用的**最后手段**。甚至 `sort` 包也有一个通用的 `slice` 方法，它使用反射接受任意切片并设置值，还有一个避免使用反射的排序接口。

还有其他包，例如 `text/template` 和 `html/template`，它们读取包含有关访问或使用哪个方法或字段的指令的运行时文本文件。在这种情况下，除了反射别无他法，也没有接口可以避免它。

# 在包中使用反射

在了解反射的工作原理及其给代码带来的复杂性之后，我们可以考虑在我们编写的包中使用它。Go 语言的一句谚语，来自其创始人 Rob Pike，给出了指引：

> **清晰胜过聪明。反射从不清晰。**

反射的威力巨大，但也以使代码更复杂、更隐式为代价。只有在极其必要时（如模板场景）才应使用它，在其他任何情况下都应避免，或者至少提供一个接口来避免它（如编码包所做的那样）。

# 属性文件

我们可以尝试使用反射来创建一个读取属性文件的包。

我们可以使用反射来创建一个读取属性文件的包：

1.  首先要做的是定义一个避免使用反射的接口：

```go
type Unmarshaller interface {
    UnmarshalProp([]byte) error
}
```

2.  然后，我们可以定义一个解码器结构体，它将作用于一个 `io.Reader` 实例，使用行扫描器来读取单个属性：

```go
type Decoder struct {
    scanner *bufio.Scanner
}

func NewDecoder(r io.Reader) *Decoder {
    return &Decoder{scanner: bufio.NewScanner(r)}
}
```

3.  解码器也将被 `Unmarshal` 方法使用：

```go
func Unmarshal(data []byte, v interface{}) error {
    return NewDecoder(bytes.NewReader(data)).Decode(v)
}
```

4.  我们可以通过构建字段名称和索引的缓存来减少反射的使用次数。这会有所帮助，因为在反射中，字段的值只能通过索引访问，而不能通过名称访问：

```go
var cache = make(map[reflect.Type]map[string]int)

func findIndex(t reflect.Type, k string) (int, bool) {
    if v, ok := cache[t]; ok {
        n, ok := v[k]
        return n, ok
    }
    m := make(map[string]int)
    for i := 0; i < t.NumField(); i++ {
        f := t.Field(i)
        if s := f.Name[:1]; strings.ToLower(s) == s {
            continue
        }
        name := strings.ToLower(f.Name)
        if tag := f.Tag.Get("prop"); tag != "" {
            name = tag
        }
        m[name] = i
    }
    cache[t] = m
    return findIndex(t, k)
}
```

5.  下一步是定义 `Decode` 方法。它将接收一个指向结构体的指针，然后处理来自扫描器的行并填充结构体字段：

```go
func (d *Decoder) Decode(v interface{}) error {
    val := reflect.ValueOf(v)
    t := val.Type()
    if t.Kind() != reflect.Ptr && t.Elem().Kind() != reflect.Struct {
        return fmt.Errorf("%v not a struct pointer", t)
    }
    val = val.Elem()
    t = t.Elem()
    line := 0
    for d.scanner.Scan() {
        line++
        b := d.scanner.Bytes()
        if len(b) == 0 || b[0] == '#' {
            continue
        }
        parts := bytes.SplitN(b, []byte{':'}, 2)
        if len(parts) != 2 {
            return decodeError{line: line, err: errNoSep}
        }
        index, ok := findIndex(t, string(parts[0]))
        if !ok {
            continue
        }
        value := bytes.TrimSpace(parts[1])
        if err := d.decodeValue(val.Field(index), value); err != nil {
            return decodeError{line: line, err: err}
        }
    }
    return d.scanner.Err()
}
```

最重要的工作将由私有的 `decodeValue` 方法完成。首先要验证是否满足 `Unmarshaller` 接口，如果满足则使用它。否则，该方法将使用反射来正确解码接收到的值。对于每种类型，它将使用 `reflection.Value` 中不同的 `Set` 方法，如果遇到未知类型则返回错误。





# 使用包

为了测试包的行为是否符合预期，我们可以创建一个满足 `Unmarshaller` 接口的自定义类型。该类型在解码时将字符串转换为大写：

```
type UpperString string

func (u *UpperString) UnmarshalProp(b []byte) error {
    *u = UpperString(strings.ToUpper(string(b)))
    return nil
}
```

现在我们可以将该类型用作结构体字段，并验证它在 `decode` 操作中是否被正确转换：

```
func main() {
    r := strings.NewReader(
        "\n# comment, ignore\nkey1: 10.5\nkey2: some string" +
            "\nkey3: 42\nkey4: false\nspecial: another string\n")
    var v struct {
        Key1 float32
        Key2 string
        Key3 uint64
        Key4 bool
        Key5 UpperString `prop:"special"`
        key6 int
    }
    if err := prop.NewDecoder(r).Decode(&v); err != nil {
        log.Fatal(r)
    }
    log.Printf("%+v", v)
}
```

# 总结

在本章中，我们详细回顾了 Go 语言中接口的内存模型，重点强调了接口始终包含一个具体类型。我们利用这些信息深入了解了类型转换，并理解了当接口被转换为另一个接口时会发生什么。

然后，我们介绍了反射的基本机制，从 `Type` 和 `Value` 开始，它们是该包的两个主要类型。它们分别代表变量的类型和值。`Value` 允许你读取变量的内容，如果变量是可寻址的，还允许写入它。为了使变量可寻址，需要通过其地址访问该变量，例如使用指针。

我们还看到了如何使用反射处理复杂数据类型，了解了如何访问结构体字段的值。结构体的数据类型可用于获取关于字段的元数据，包括名称和标签，这些在编码包和其他第三方库中被大量使用。

我们了解了如何创建和操作映射，包括添加、设置和删除值。对于切片，我们看到了如何编辑其值以及如何执行追加操作。我们还展示了如何使用通道发送和接收数据，甚至如何以静态类型编程相同的方式使用 `select` 语句。

最后，我们列出了标准库中使用反射的地方，并快速分析了其计算成本。我们以一些关于如何以及何时在库或你正在编写的任何应用程序中使用反射的提示作为本章的结尾。

下一章是本书的最后一章，它解释了如何使用 CGO 在 Go 中利用现有的 C 库。

# 问题

1.  Go 中接口的内存表示是什么？
2.  当接口类型转换为另一个接口类型时会发生什么？
3.  反射中的 `Value`、`Type` 和 `Kind` 是什么？
4.  如果一个值是可寻址的意味着什么？
5.  为什么结构体字段标签在 Go 中很重要？
6.  反射的一般权衡是什么？
7.  你能描述一种使用反射的良好方法吗？
