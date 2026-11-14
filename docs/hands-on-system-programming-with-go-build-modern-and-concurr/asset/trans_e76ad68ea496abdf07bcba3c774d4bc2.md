

```go
func main() {
    var a interface{} = "hello"
    s, ok := a.(string) // true
    fmt.Println(s, ok)
    i, ok := a.(int) // false
    fmt.Println(i, ok)
}
```

完整示例请见此处：[https://play.golang.org/p/BIba2ywkNF_j](https://play.golang.org/p/BIba2ywkNF_j)。

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

完整示例请见此处：[https://play.golang.org/p/bX2rnw5pRXJ](https://play.golang.org/p/bX2rnw5pRXJ)。

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

完整示例请见此处：[https://play.golang.org/p/hUEsDYHFE7i](https://play.golang.org/p/hUEsDYHFE7i)。

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

完整示例请见此处：[https://play.golang.org/p/tmYuMc4AF1T](https://play.golang.org/p/tmYuMc4AF1T)。

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

完整示例请见此处：[https://play.golang.org/p/qjiouk88INn](https://play.golang.org/p/qjiouk88INn)。

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

完整示例请见此处：[https://play.golang.org/p/-uJjZvTuzVf](https://play.golang.org/p/-uJjZvTuzVf)。

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

完整示例请见此处：[https://play.golang.org/p/1942Dhm5sap](https://play.golang.org/p/1942Dhm5sap)。

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

完整示例请见此处：[https://play.golang.org/p/hKn8qNtn0gN](https://play.golang.org/p/hKn8qNtn0gN)。

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

完整示例请见此处：[https://play.golang.org/p/-X5JsBrlr4Q](https://play.golang.org/p/-X5JsBrlr4Q)。

### 创建新值

`reflect` 包还允许我们使用类型创建新值。有几个函数允许我们创建值：

*   `MakeChan()` 创建一个新的通道值
*   `MakeFunc()` 创建一个新的函数值
*   `MakeMap()` 和 `MakeMapWithSize()` 创建一个新的映射值

