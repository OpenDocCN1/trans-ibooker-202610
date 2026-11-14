# 处理流

本章处理数据流，将输入和输出接口扩展到文件系统之外，以及如何实现自定义读取器和写入器以服务于任何目的。

它还专注于以多种不同方式组合它们的输入和输出工具的缺失部分，目标是完全控制传入和传出的数据。

本章将涵盖以下主题：

*   流
*   自定义读取器
*   自定义写入器
*   工具

# 技术要求




本章要求已安装 Go 并设置好您喜爱的编辑器。更多信息请参阅 第 3 章 *Go 概览*。

# 流

写入器和读取器不仅用于文件；它们是接口，用于抽象单向或双向的数据流。这些流通常被称为 `streams`，是大多数应用程序的重要组成部分。

# 输入和读取器

如果应用程序无法控制数据流，传入的数据流将被视为 `io.Reader` 接口，并将等待错误以结束进程。在最理想的情况下，会接收到 `io.EOF` 值，这是一个特殊的错误，表示没有更多内容可读；否则将是其他错误。另一种选择是读取器也能终止流。在这种情况下，正确的表示是 `io.ReadCloser` 接口。

除了 `os.File`，标准库中分布着多个读取器的实现。

# 字节读取器

`bytes` 包包含一个有用的结构，它将字节切片视为 `io.Reader` 接口，并实现了更多 I/O 接口：

*   `io.Reader`：可作为常规读取器
*   `io.ReaderAt`：使得从特定位置开始读取成为可能
*   `io.WriterTo`：使得带偏移量写入内容成为可能
*   `io.Seeker`：可自由移动读取器的光标
*   `io.ByteScanner`：可对每个字节单独执行读取操作
*   `io.RuneScanner`：可对由多个字节组成的字符执行相同操作

通过以下示例可以阐明 rune 和字节的区别，其中有一个字符串由一个 rune `⌘` 组成，它由三个字节 `e28c98` 表示：

```
func main() {
    const a = `⌘`

    fmt.Printf("plain string: %s\n", a)
    fmt.Printf("quoted string: %q\n",a)

    fmt.Printf("hex bytes: ")
    for i := 0; i < len(a); i++ {
        fmt.Printf("%x ", a[i])
    }
    fmt.Printf("\n")
}
```

完整示例请访问 [`play.golang.org/p/gVZOufSmlq1`](https://play.golang.org/p/gVZOufSmlq1)。

还有 `bytes.Buffer`，它在 `bytes.Reader` 的基础上增加了写入功能，并使得访问底层切片或将内容作为字符串获取成为可能。

`Buffer.String` 方法将字节转换为字符串，在 Go 中这种类型转换是通过复制字节来完成的，因为字符串是不可变的。这意味着在复制之后对缓冲区的最终更改不会传播到字符串。

# 字符串读取器

`strings` 包包含另一个与 `io.Reader` 接口非常相似的结构，称为 `strings.Reader`。它的工作方式完全相同，但底层值是字符串而不是字节切片。

当处理需要被读取的字符串时，使用字符串而非字节读取器的主要优势之一，是在初始化时避免了数据复制。这种细微的差别有助于提升性能和内存使用，因为它减少了分配，且不需要 `Garbage Collector` (`GC`) 来清理副本。

# 定义读取器

任何 Go 应用程序都可以定义 `io.Reader` 接口的自定义实现。实现接口时的一个好原则是接受接口、返回具体类型，避免不必要的抽象。

让我们看一个实际例子。我们想实现一个自定义读取器，从另一个读取器获取内容并将其转换为大写；我们可以称之为 `AngryReader`：

```
func NewAngryReader(r io.Reader) *AngryReader {
    return &AngryReader{r: r}
}

type AngryReader struct {
    r io.Reader
}

func (a *AngryReader) Read(b []byte) (int, error) {
    n, err := a.r.Read(b)
    for r, i, w := rune(0), 0, 0; i < n; i += w {
        // 读取一个 rune
        r, w = utf8.DecodeRune(b[i:])
        // 如果不是字母则跳过
        if !unicode.IsLetter(r) {
            continue
        }
        // rune 的大写版本
        ru := unicode.ToUpper(r)
        // 编码 rune 并期望长度相同
        if wu := utf8.EncodeRune(b[i:], ru); w != wu {
            return n, fmt.Errorf("%c->%c, size mismatch %d->%d", r, ru, w, wu)
        }
    }
    return n, err
}
```

这是一个相当直接的示例，使用 `unicode` 和 `unicode/utf8` 来实现目标：

*   `utf8.DecodeRune` 用于获取第一个 rune 及其宽度，作为读取切片的一部分
*   `unicode.IsLetter` 判断 rune 是否为字母
*   `unicode.ToUpper` 将文本转换为大写
*   `utf8.EncodeRune` 用必要的字节写入新字母
*   字母及其大写版本应具有相同的宽度

完整示例请访问 [`play.golang.org/p/PhdSsbzXcbE`](https://play.golang.org/p/PhdSsbzXcbE)。

# 输出和写入器

适用于传入流的推理同样适用于传出流。我们有 `io.Writer` 接口，应用程序只能发送数据；以及 `io.WriteCloser` 接口，应用程序也能关闭连接。

# 字节写入器

我们已经看到 `bytes` 包提供了 `Buffer`，它同时具备读取和写入功能。它实现了 `ByteReader` 接口的所有方法，以及不止一个 `Writer` 接口：

*   `io.Writer`：可作为常规写入器
*   `io.WriterAt`：使得从特定位置开始写入成为可能
*   `io.ByteWriter`：使得写入单个字节成为可能

`bytes.Buffer` 是一个非常灵活的结构，因为它同时适用于 `Writer` 和 `ByteWriter`，且由于 `Reset` 和 `Truncate` 方法，重用时效果最佳。与其将用过的缓冲区留给 `GC` 回收并创建新缓冲区，不如重置现有缓冲区，保留底层数组并将切片长度设为 `0`。

在上一章中，我们看到了一个缓冲区用法的好例子：

```
    bookList := []book{
        {Author: grr, Title: "A Game of Thrones", Year: 1996},
        {Author: grr, Title: "A Clash of Kings", Year: 1998},
        {Author: grr, Title: "A Storm of Swords", Year: 2000},
        {Author: grr, Title: "A Feast for Crows", Year: 2005},
        {Author: grr, Title: "A Dance with Dragons", Year: 2011},
        {Author: grr, Title: "The Winds of Winter"},
        {Author: grr, Title: "A Dream of Spring"},
    }
    b := bytes.NewBuffer(make([]byte, 0, 16))
    for _, v := range bookList {
        // 将格式化后的消息及参数打印到 writer
        fmt.Fprintf(b, "%s - %s", v.Title, v.Author)
        if v.Year > 0 { // 如果年份不存在则不打印
            fmt.Fprintf(b, " (%d)", v.Year)
        }
        b.WriteRune('\n')
        if _, err := b.WriteTo(dst); true { // 复制字节，清空缓冲区
            fmt.Println("Error:", err)
            return
        }
    }
```

缓冲区并非用于拼接字符串值。因此，当调用 `String` 方法时，字节会被转换为字符串，字符串是不可变的，这与切片不同。通过这种方式创建的新字符串是当前切片的副本，对切片的更改不会影响字符串。这既不是限制也不是特性；它是一个属性，如果使用不当可能导致错误。以下是重置缓冲区并使用 `String` 方法的效果示例：

```
package main

import (
    "bytes"
    "fmt"
)

func main() {
    b := bytes.NewBuffer(nil)
    b.WriteString("One")
    s1 := b.String()
    b.WriteString("Two")
    s2 := b.String()
    b.Reset()
    b.WriteString("Hey!")    // 不会改变 s1 或 s2
    s3 := b.String()
    fmt.Println(s1, s2, s3)  // 打印 "One OneTwo Hey!"
}
```

完整示例请访问 [`play.golang.org/p/zBjGPMC4sfF`](https://play.golang.org/p/zBjGPMC4sfF)




# 字符串写入器

字节缓冲区通过复制字节来生成字符串。这就是为什么在 1.10 版本中 `strings.Builder` 应运而生。它共享缓冲区的所有写入相关方法，但不允许通过 `Bytes` 方法访问底层切片。获取最终字符串的唯一方法是 `String` 方法，它在底层使用 `unsafe` 包将切片转换为字符串，而无需复制底层数据。

其主要后果是该结构体强烈不建议复制——因为被复制切片的底层切片指向同一个数组，在副本中写入会影响另一个。由此产生的操作会 panic：

```
package main

import (
    "strings"
)

func main() {
    b := strings.Builder{}
    b.WriteString("One")
    c := b
    c.WriteString("Hey!") // panic: strings: illegal use of non-zero Builder copied by value
}
```

# 定义写入器

应用程序中可以定义任何写入器的自定义实现。一个非常常见的情况是装饰器，它是一个包装另一个写入器并改变或扩展原始写入器行为的写入器。就读取器而言，拥有一个接受另一个写入器并可能将其包装的构造函数是一个好习惯，以使其兼容许多标准库结构，例如：

*   `*os.File`
*   `*bytes.Buffer`
*   `*strings.Builder`

让我们看一个真实的用例——我们想生成一些文本，其中每个单词的字母都被打乱，以测试何时开始变得人类无法阅读。我们将创建一个可配置的写入器，在写入目标写入器之前打乱字母，并创建一个接受文件并生成其打乱版本的二进制程序。我们将使用 `math/rand` 包来随机化打乱过程。

让我们定义结构体及其构造函数。它将接受另一个写入器、一个随机数生成器和一个打乱 `chance`（概率）：

```
func NewScrambleWriter(w io.Writer, r *rand.Rand, chance float64) *ScrambleWriter {
    return &ScrambleWriter{w: w, r: r, c: chance}
}

type ScrambleWriter struct {
    w io.Writer
    r *rand.Rand
    c float64
}
```

`Write` 方法需要原样执行非字母字节，并打乱字母序列。它将迭代 rune，使用我们之前看到的 `utf8.DecodeRune` 函数，打印非字母的内容，并堆叠它能找到的所有字母序列：

```
func (s *ScrambleWriter) Write(b []byte) (n int, err error) {
    var runes = make([]rune, 0, 10)
    for r, i, w := rune(0), 0, 0; i < len(b); i += w {
        r, w = utf8.DecodeRune(b[i:])
        if unicode.IsLetter(r) {
            runes = append(runes, r)
            continue
        }
        v, err := s.shambleWrite(runes, r)
        if err != nil {
            return n, err
        }
        n += v
        runes = runes[:0]
    }
    if len(runes) != 0 {
        v, err := s.shambleWrite(runes, 0)
        if err != nil {
            return n, err
        }
        n += v
    }
    return
}
```

当序列结束时，将由 `shambleWrite` 方法处理，该方法将实际执行打乱并写入打乱后的 rune：

```
func (s *ScrambleWriter) shambleWrite(runes []rune, sep rune) (n int, err error) {
    //scramble after first letter
    for i := 1; i < len(runes)-1; i++ {
        if s.r.Float64() > s.c {
            continue
        }
        j := s.r.Intn(len(runes)-1) + 1
        runes[i], runes[j] = runes[j], runes[i]
    }
    if sep != 0 {
        runes = append(runes, sep)
    }
    var b = make([]byte, 10)
    for _, r := range runes {
        v, err := s.w.Write(b[:utf8.EncodeRune(b, r)])
        if err != nil {
            return n, err
        }
        n += v
    }
    return
}
```

完整示例请访问 [`play.golang.org/p/0Xez--6P7nj`](https://play.golang.org/p/0Xez--6P7nj)。

# 内置工具

`io` 和 `io/ioutil` 包中还有许多其他函数有助于管理读取器、写入器等。了解所有可用工具将帮助你避免编写不必要的代码，并指导你为工作选择最佳工具。

## 从一个流复制到另一个流

`io` 包中有三个主要函数可以将数据从写入器传输到读取器。这是一个非常常见的场景；例如，你可能正在将打开用于读取的文件内容写入另一个打开用于写入的文件，或者清空缓冲区并将其内容作为标准输出写入。

我们已经在第 4 章 *Working with the Filesystem* 中看到了如何在文件上使用 `io.Copy` 函数来模拟 `cp` 命令的行为。这种行为可以扩展到任何类型的读取器和写入器实现，从缓冲区到网络连接。

如果写入器也是 `io.WriterTo` 接口，复制将调用 `WriteTo` 方法。如果不是，它将使用固定大小（32 KB）的缓冲区执行一系列写入。如果操作以 `io.EOF` 值结束，则不返回错误。一个常见的场景是 `bytes.Buffer` 结构体，它能够将其内容写入另一个写入器并表现相应的行为。或者，如果目标是 `io.ReaderFrom` 接口，则执行 `ReadFrom` 方法。

如果接口是简单的 `io.Writer` 接口，此方法使用一个临时缓冲区，之后将被清理。为了避免在垃圾回收上浪费计算能力，并可能重用相同的缓冲区，还有另一个函数——`io.CopyBuffer` 函数。它有一个额外的参数，仅当该额外参数为 `nil` 时才会分配新缓冲区。

最后一个函数是 `io.CopyN`，它的工作原理与 `io.Copy` 完全相同，但允许指定要写入的字节数限制作为额外参数。如果读取器也是 `io.Seeker`，它对于写入部分内容很有用——查找器首先将光标移动到正确的偏移量，然后写入一定数量的字节。

让我们举一个一次复制 `n` 字节的例子：

```
func CopyNOffset(dst io.Writer, src io.ReadSeeker, offset, length int64) (int64, error) {
  if _, err := src.Seek(offset, io.SeekStart); err != nil {
    return 0, err
  }
  return io.CopyN(dst, src, length)
}
```

完整示例请访问 [`play.golang.org/p/8wCqGXp5mSZ`](https://play.golang.org/p/8wCqGXp5mSZ)。

## 连接的读取器和写入器

`io.Pipe` 函数创建一对连接的读取器和写入器。这意味着发送给写入器的任何内容都将从读取器接收。如果上一次操作仍有数据悬而未决，写入操作将被阻塞；只有当读取器消费完已发送的内容后，新操作才会结束。

对于非并发应用程序，这不是一个重要工具，因为它们更可能使用通道等并发工具，但当读取器和写入器在不同的 goroutine 上执行时，这可以成为一种出色的同步机制，如下面的程序所示：

```
pr, pw := io.Pipe()
go func(w io.WriteCloser) {
    for _, s := range []string{"a string", "another string", 
       "last one"} {
            fmt.Printf("-> writing %q\n", s)
            fmt.Fprint(w, s)
    }
    w.Close()
}(pw)
var err error
for n, b := 0, make([]byte, 100); err == nil; {
    fmt.Println("<- waiting...")
    n, err = pr.Read(b)
    if err == nil {
        fmt.Printf("<- received %q\n", string(b[:n]))
    }
}
if err != nil && err != io.EOF {
    fmt.Println("error:", err)
}
```

完整示例请访问 [`play.golang.org/p/0YpRK25wFw_c`](https://play.golang.org/p/0YpRK25wFw_c)。

## 扩展读取器





# 传入流函数

说到传入流，标准库中有许多可用的函数来增强读取器的功能。最简单的例子之一是 `ioutil.NopCloser`，它接受一个读取器并返回 `io.ReadCloser`，什么也不做。如果某个函数负责释放资源，但所用的读取器不是 `io.Closer`（如 `bytes.Buffer`），这非常有用。

## 限制读取字节数

有两个工具可以限制读取的字节数。`ReadAtLeast` 函数定义了要读取的最小字节数。只有当没有字节可读时，结果才会是 `EOF`；否则，如果在 `EOF` 前读取的字节数较少，将返回 `ErrUnexpectedEOF`。如果字节缓冲区比请求的字节数短（这说不通），将会出现 `ErrShortBuffer`。如果发生读取错误，该函数仍会设法读取至少所需数量的字节，并丢弃该错误。

然后是 `ReadFull`，它预期会填满缓冲区，否则将返回 `ErrUnexpectedEOF`。

## 限制读取器

另一个限制函数是 `LimitReader`。该函数是一个装饰器，它获取一个读取器并返回另一个读取器，一旦读取了所需的字节数，后者将返回 `EOF`。这可用于预览实际读取器的内容，如下例所示：

```go
s := strings.NewReader(`Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged.`)
io.Copy(os.Stdout, io.LimitReader(s, 25)) // 将打印 "Lorem Ipsum is simply dum"
```

完整示例请访问 [`play.golang.org/p/LllOdWg9uyU`](https://play.golang.org/p/LllOdWg9uyU)。

## 组合读取器

可以使用 `MultiReader` 函数将多个读取器按顺序组合在一起，它将依次读取每一部分直到遇到 `EOF`，然后跳转到下一个。

## 连接读取器和写入器

可以将一个读取器和一个写入器连接起来，以便从读取器传来的任何内容都被复制到写入器——这与 `io.Pipe` 发生的情况相反。这是通过 `io.TeeReader` 完成的。

## 示例：文件系统搜索写入器

让我们尝试用它来创建一个充当文件系统搜索引擎的写入器，仅打印与请求的查询匹配的行。我们想要一个执行以下操作的程序：

*   从参数中读取目录路径和要搜索的字符串
*   获取所选路径下的文件列表
*   读取每个文件，并将包含所选字符串的行传递给另一个写入器
*   这个其他的写入器将注入颜色字符以高亮显示该字符串，并将其内容复制到标准输出

### 颜色注入

让我们从颜色注入开始。在 Unix shell 中，彩色输出通过以下序列获得：

*   `\x1b[`：一个转义字符
*   `\[`：一个左括号
*   `39`：一个数字
*   `m`：字母 `m`

该数字决定了背景色和前景色。在本例中，我们将使用 `31`（红色）和 `39`（默认色）。

### 查询写入器实现

我们正在创建一个写入器，它将打印匹配的行并高亮显示文本：

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
            b[:i], // 匹配前的内容
            []byte("\x1b[31m"), // 开始红色
            b[i : i+l], // 匹配内容
            []byte("\x1b[39m"), // 默认颜色
            b[i+l:], // 剩余内容
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

### 主函数

这将配合 `TeeReader` 和一个打开的文件使用，以便读取文件时会自动写入 `queryWriter`：

```go
func main() {
    if len(os.Args) < 3 {
        fmt.Println("Please specify a path and a search string.")
        return
    }
    root, err := filepath.Abs(os.Args[1]) // 获取绝对路径
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

正如你所见，无需显式写入；从文件读取会自动写入连接到标准输出的查询写入器。

# 写入器和装饰器

有大量工具可用于增强、装饰读取器并供其使用，但同样的情况不适用于写入器。

还有 `io.WriteString` 函数，它避免了从字符串到字节的不必要转换。首先，它检查写入器是否支持字符串写入，尝试转型为 `io.stringWriter`（一个仅包含 `WriteString` 方法的未导出接口），如果成功则写入字符串，否则将其转换为字节。

还有 `io.MultiWriter` 函数，它创建一个写入器，将信息复制到一系列其他写入器中，这些写入器在创建时接收。一个实际的例子是在向标准输出显示内容的同时写入内容，如下例所示：

```go
r := strings.NewReader("let's read this message\n")
b := bytes.NewBuffer(nil)
w := io.MultiWriter(b, os.Stdout)
io.Copy(w, r) // 打印到标准输出
fmt.Println(b.String()) // 缓冲区现在也包含字符串
```

完整示例请访问 [`play.golang.org/p/ZWDF2vCDfsM`](https://play.golang.org/p/ZWDF2vCDfsM)。

还有一个有用的变量 `ioutil.Discard`，它是一个写入到 `/dev/null`（空设备）的写入器。这意味着写入该变量会忽略数据。

# 总结

在本章中，我们介绍了流的概念，用于描述传入和传出的数据流。我们了解到读取器接口代表接收到的数据，而写入器代表发送的数据。

我们比较了标准包中可用的不同读取器。我们在上一章查看了文件，在本章中我们将字节和字符串读取器添加到了列表中。我们通过一个示例学习了如何实现自定义读取器，并了解到设计一个建立在另一个读取器之上的读取器总是好的做法。

然后，我们专注于写入器。我们发现如果正确打开，文件也是写入器，并且标准包中有几种写入器，包括字节缓冲区和字符串构建器。我们还实现了一个自定义写入器，并了解了如何使用 `utf8` 包处理字节和符文。

最后，我们探索了 `io` 和 `ioutil` 中剩余的功能，分析了复制数据、连接读取器和写入器的各种工具。我们还了解了可用于改进或更改读取器和写入器功能的装饰器。

在下一章中，我们将讨论伪终端应用程序，并利用所有这些知识来构建其中的一些。

# 问题

1.  什么是流？
2.  哪些接口抽象了传入流？
3.  哪些接口代表传出流？
4.  何时应使用字节读取器？何时应改用字符串读取器？





# 构建伪终端

## 问题

5. 字符串构建器（string builder）和字节缓冲区（bytes buffer）有什么区别？

6. 为什么读取器和写入器的实现应该接受接口作为输入？

7. 管道与 `TeeReader` 有何不同？

## 技术要求

本章要求已安装 Go 并设置好你喜欢的编辑器。更多信息请参阅 第 3 章 *Go 概览*。

## 理解伪终端

伪终端，或伪电传打字机，是在终端或电传打字机下运行并模拟其行为的应用程序。这是一种创建交互式软件的非常便捷的方式，该软件能够在没有任何图形界面的情况下在终端内运行。这是因为它利用终端本身来模拟一个终端。

## 从电传打字机开始

**电传打字机** 或电传印刷机是指通过串口控制的带有机电系统的打字机。它连接到一台能够发送信息给设备进行打印的计算机。数据由一系列有限符号（如 ASCII 字符）组成，采用固定字体。这些设备充当早期计算机的用户界面，因此从某种意义上说，它们是现代屏幕的前身。

当屏幕取代打印机作为输出设备时，其内容的组织方式类似：一个二维字符矩阵。在早期阶段，它们被称为玻璃 TTY，字符显示仍然是显示器本身的一部分，由其自身的逻辑电路控制。随着首批视频显示卡的出现，计算机能够拥有不依赖硬件的界面。

作为操作系统主要界面的纯文本控制台继承了 TTY 之名，被称为控制台。即使操作系统运行像现代操作系统那样的图形环境，用户也始终可以访问一定数量的虚拟控制台，它们作为 **命令行界面 (CLI)** 工作，通常被称为 Shell。

## 伪电传打字机

许多应用程序被设计为在 Shell 内工作，但其中一些模仿 Shell 的行为。图形界面拥有终端模拟器，专门用于执行 Shell。这类应用程序被称为 **伪电传打字机 (PTY)**。为了被视为 PTY，应用程序需要具备以下能力：

*   接受用户输入
*   将输入发送给控制台并接收输出
*   将此输出展示给用户

现有的 Linux 实用工具中已有一些例子，**screen** 是最著名的一个。它是一个伪终端应用程序，允许用户使用多个 Shell 并控制它们。它可以打开和关闭新的 Shell，并在所有已打开的 Shell 之间切换。它允许用户为会话命名，以便如果因任何意外原因被终止，用户可以恢复会话。

## 创建基础 PTY

我们将从创建一个简单版本的伪终端开始，先创建输入管理器，然后创建命令选择器，最后创建命令执行。

### 输入管理

标准输入可用于接收用户命令。我们可以从使用缓冲输入来读取行并打印它们开始。为了读取一行，有一个有用的命令 `bufio.Scanner`，它已经提供了行读取器。代码将类似于以下代码片段：

```
s := bufio.NewScanner(os.Stdin)
w := os.Stdout
fmt.Fprint(w, "Some welcome message\n")
for {
    s.Scan() // get next the token
    fmt.Fprint(w, "You wrote \"")
    w.Write(s.Bytes())
    fmt.Fprintln(w, "\"\n") // writing back the text
}
```

由于这段代码没有退出点，我们可以首先创建 `exit` 命令，它将终止 Shell 的执行。我们可以对代码做一个小改动来实现这一点，如下所示：

```
s := bufio.NewScanner(os.Stdin)
w := os.Stdout
fmt.Fprint(w, "Some welcome message\n")
for {
    s.Scan() // get next the token
    msg := string(s.Bytes())
    if msg == "exit" {
        return
    }
    fmt.Fprintf(w, "You wrote %q\n", msg) // writing back the text
}
```

现在应用程序有了一个除 `kill` 命令之外的退出点。目前，它除了 `exit` 之外不实现任何命令，只是将你输入的任何内容原样打印回来。

### 选择器

为了能够正确解释命令，消息需要被分割成参数。这与操作系统应用于传递给进程的参数的逻辑相同。`strings.Split` 函数可以完成这项工作，通过指定空格作为第二个参数并将字符串分割成单词，如下面的代码所示：

```
args := strings.Split(string(s.Bytes()), " ")
cmd := args[0]
args = args[1:]
```

可以对 `cmd` 执行任何类型的检查，例如以下 `switch` 语句：

```
switch cmd {
case "exit":
    return
case "someCommand":
    someCommand(w, args)
case "anotherCommand":
    anotherCommand(w, args)
}
```

这允许用户通过定义一个函数并向 `switch` 语句添加新的 `case` 来添加新命令。

### 命令执行

现在一切已就绪，只剩下定义各个命令实际要做什么。我们可以定义执行命令的函数类型以及 switch 的行为：

```
var cmdFunc func(w io.Writer, args []string) (exit bool)
switch cmd {
case "exit":
    cmdFunc = exitCmd
}
if cmdFunc == nil {
    fmt.Fprintf(w, "%q not found\n", cmd)
    continue
}
if cmdFunc(w, args) { // execute and exit if true
    return
}
```

返回值告诉应用程序是否需要终止，并允许我们轻松定义 `exit` 函数，而无需将其作为特殊情况处理：

```
func exitCmd(w io.Writer, args []string) bool {
    fmt.Fprintf(w, "Goodbye! :)")
    return true
}
```

根据我们应用程序的范围，我们现在可以实现任何类型的命令。让我们创建一个 `shuffle` 命令，它将借助 `math/rand` 包以打乱的顺序打印参数：

```
func shuffle(w io.Writer, args ...string) bool {
    rand.Shuffle(len(args), func(i, j int) {
        args[i], args[j] = args[j], args[i]
    })
    for i := range args {
        if i > 0 {
            fmt.Fprint(w, " ")
        }
        fmt.Fprintf(w, "%s", args[i])
    }
    fmt.Fprintln(w)
    return false
}
```

我们可以通过创建一个 `print` 命令来与文件系统和文件交互，该命令将在输出中显示文件的内容：

```
func print(w io.Writer, args ...string) bool {
    if len(args) != 1 {
        fmt.Fprintln(w, "Please specify one file!")
        return false
    }
    f, err := os.Open(args[0])
    if err != nil {
        fmt.Fprintf(w, "Cannot open %s: %s\n", args[0], err)
    }
    defer f.Close()
    if _, err := io.Copy(w, f); err != nil {
        fmt.Fprintf(w, "Cannot print %s: %s\n", args[0], err)
    }
    fmt.Fprintln(w)
    return false
}
```

### 一些重构

当前版本的伪终端应用程序可以通过少量重构来改进。我们可以从将命令定义为自定义类型开始，并添加几个描述其行为的方法：





## 命令结构

```go
type cmd struct {
    Name string // 命令名称
    Help string // 描述字符串
    Action func(w io.Writer, args ...string) bool
}

func (c cmd) Match(s string) bool {
    return c.Name == s
}

func (c cmd) Run(w io.Writer, args ...string) bool {
    return c.Action(w, args...)
}
```

所有关于每个命令的信息都可以封装在一个结构体中。我们也可以开始定义依赖其他命令的命令，例如 `help` 命令。如果在包中某处定义了命令的切片或映射 `var cmds []cmd`，`help` 命令将如下所示：

```go
help := cmd{
    Name: "help",
    Help: "显示可用命令",
    Action: func(w io.Writer, args ...string) bool {
        fmt.Fprintln(w, "可用命令:")
        for _, c := range cmds {
            fmt.Fprintf(w, " - %-15s %s\n", c.Name, c.Help)
        }
        return false
    },
}
```

主循环中选择正确命令的部分将略有不同；它需要在切片中找到匹配项并执行它：

```go
for i := range cmds {
    if !cmds[i].Match(args[0]) {
        continue
    }
    idx = i
    break
}
if idx == -1 {
    fmt.Fprintf(w, "%q 未找到。使用 `help` 查看可用命令\n", args[0])
    continue
}
if cmds[idx].Run(w, args[1:]...) {
    fmt.Fprintln(w)
    return
}
```

现在有了一个显示可用命令列表的 `help` 命令，我们可以提倡在用户指定不存在的命令时使用它——就像我们目前在检查索引是否已从其默认值 `-1` 更改时所做的那样。

## 改进 PTY

既然我们已经了解了如何创建基本的伪终端，我们将看到如何通过一些额外功能来改进它。

## 多行输入

首先可以改进的是参数与空格之间的关系，通过添加对引用字符串的支持来实现。这可以通过带有自定义分割函数的 `bufio.Scanner` 来完成，该函数的行为类似于 `bufio.ScanWords`，不同之处在于它能识别引号。以下代码演示了这一点：

```go
func ScanArgs(data []byte, atEOF bool) (advance int, token []byte, err error) {
    // first space
    start, first := 0, rune(0)
    for width := 0; start < len(data); start += width {
        first, width = utf8.DecodeRune(data[start:])
        if !unicode.IsSpace(first) {
            break
        }
    }
    // skip quote
    if isQuote(first) {
        start++
    }
```

该函数的第一个块跳过空格并找到第一个非空格字符；如果该字符是引号，则跳过它。然后，它查找终止参数的第一个字符，对于普通参数是空格，对于其他参数则是相应的引号：

```go
    // loop until arg end character
    for width, i := 0, start; i < len(data); i += width {
        var r rune
        r, width = utf8.DecodeRune(data[i:])
        if ok := isQuote(first); !ok && unicode.IsSpace(r) || ok && r == first {
            return i + width, data[start:i], nil
        }
    }
```

如果在引用上下文中到达文件末尾，则返回部分字符串；否则不跳过引号并请求更多数据：

```go
    // token from EOF
    if atEOF && len(data) > start {
        return len(data), data[start:], nil
    }
    if isQuote(first) {
        start--
    }
    return start, nil, nil
}
```

完整示例可在以下地址获取：<https://play.golang.org/p/CodJjcpzlLx>。

现在我们可以使用它作为解析参数的行，同时使用定义如下的辅助结构 `argsScanner`：

```go
type argsScanner []string

func (a *argsScanner) Reset() { *a = (*a)[0:0] }

func (a *argsScanner) Parse(r io.Reader) (extra string) {
    s := bufio.NewScanner(r)
    s.Split(ScanArgs)
    for s.Scan() {
        *a = append(*a, s.Text())
    }
    if len(*a) == 0 {
        return ""
    }
    lastArg := (*a)[len(*a)-1]
    if !isQuote(rune(lastArg[0])) {
        return ""
    }
    *a = (*a)[:len(*a)-1]
    return lastArg + "\n"
}
```

这个自定义切片将允许我们通过改变循环的工作方式来接收带有引号和引号之间换行的行：

```go
func main() {
    s := bufio.NewScanner(os.Stdin)
    w := os.Stdout
    a := argsScanner{}
    b := bytes.Buffer{}
    for {
        // prompt message
        a.Reset()
        b.Reset()
        for {
            s.Scan()
            b.Write(s.Bytes())
            extra := a.Parse(&b)
            if extra == "" {
                break
            }
            b.WriteString(extra)
        }
        // a contains the split arguments
    }
}
```

## 为伪终端提供颜色支持

可以通过提供彩色输出来改进伪终端。我们已经看到，在 Unix 中，有可以更改背景和前景颜色的转义序列。让我们首先定义一个自定义类型：

```go
type color int

func (c color) Start(w io.Writer) {
    fmt.Fprintf(w, "\x1b[%dm", c)
}

func (c color) End(w io.Writer) {
    fmt.Fprintf(w, "\x1b[%dm", Reset)
}

func (c color) Sprintf(w io.Writer, format string, args ...interface{}) {
    c.Start(w)
    fmt.Fprintf(w, format, args...)
    c.End(w)
}

// List of colors
const (
    Reset color = 0
    Red color = 31
    Green color = 32
    Yellow color = 33
    Blue color = 34
    Magenta color = 35
    Cyan color = 36
    White color = 37
)
```

这个新类型可用于增强带有彩色输出的命令。例如，让我们获取 `shuffle` 命令，并使用交替颜色来区分字符串，现在我们支持带空格的参数：

```go
func shuffle(w io.Writer, args ...string) bool {
    rand.Shuffle(len(args), func(i, j int) {
        args[i], args[j] = args[j], args[i]
    })
    for i := range args {
        if i > 0 {
            fmt.Fprint(w, " ")
        }
        var f func(w io.Writer, format string, args ...interface{})
        if i%2 == 0 {
            f = Red.Fprintf
        } else {
            f = Green.Fprintf
        }
        f(w, "%s", args[i])
    }
    fmt.Fprintln(w)
    return false
}
```

## 建议命令

当指定的命令不存在时，我们可以建议一些相似的命令。为此，我们可以使用 Levenshtein 距离公式，它通过计算从一个字符串到另一个字符串所需的删除、插入和替换次数来衡量字符串之间的相似度。

在以下代码中，我们将使用 `agnivade/levenshtein` 包，该包将通过 `go get` 命令获得：

```bash
go get github.com/agnivade/levenshtein/...
```

然后，我们定义一个在没有与现有命令匹配时调用的新函数：

```go
func commandNotFound(w io.Writer, cmd string) {
    var list []string
    for _, c := range cmds {
        d := levenshtein.ComputeDistance(c.Name, cmd)
        if d < 3 {
            list = append(list, c.Name)
        }
    }
    fmt.Fprintf(w, "命令 %q 未找到。", cmd)
    if len(list) == 0 {
        return
    }
    fmt.Fprint(w, " 你是指: ")
    for i := range list {
        if i > 0 {
            fmt.Fprint(w, ", ")
        }
        fmt.Fprintf(w, "%s", list[i])
    }
}
```

## 可扩展命令

我们伪终端目前的局限性是其可扩展性。如果需要添加新命令，必须直接添加到主包中。我们可以考虑一种将命令与主包分离并允许其他用户使用其命令扩展功能的方法：

1. 第一步是创建一个导出的命令。让我们使用接口来定义命令，以便用户可以实现自己的：

```go
// Command 表示一个终端命令
type Command interface {
    GetName() string
    GetHelp() string
    Run(input io.Reader, output io.Writer, args ...string) (exit bool)
}
```




## 2. 命令注册与列表管理

现在我们可以指定一个命令列表，并提供一个函数供其他包添加命令：

```go
// ErrDuplicateCommand 当两个命令同名时返回
var ErrDuplicateCommand = errors.New("Duplicate command")

var commands []Command

// Register 将 Command 添加到命令列表
func Register(command Command) error {
    name := command.GetName()
    for i, c := range commands {
        // 按字母顺序保持唯一命令
        switch strings.Compare(c.GetName(), name) {
        case 0:
            return ErrDuplicateCommand
        case 1:
            commands = append(commands, nil)
            copy(commands[i+1:], commands[i:])
            commands[i] = command
            return nil
        case -1:
            continue
        }
    }
    commands = append(commands, command)
    return nil
}
```

## 3. 基础命令实现

我们可以提供一个基础命令实现，用于执行简单函数：

```go
// Base 是运行闭包的基本 Command
type Base struct {
    Name, Help string
    Action     func(input io.Reader, output io.Writer, args ...string) bool
}

func (b Base) String() string { return b.Name }

// GetName 返回 Name
func (b Base) GetName() string { return b.Name }

// GetHelp 返回 Help
func (b Base) GetHelp() string { return b.Help }

// Run 调用闭包
func (b Base) Run(input io.Reader, output io.Writer, args ...string) bool {
    return b.Action(input, output, args...)
}
```

## 4. 命令查找

我们可以提供一个函数，根据名称匹配命令：

```go
// GetCommand 返回给定名称的命令
func GetCommand(name string) Command {
    for _, c := range commands {
        if c.GetName() == name {
            return c
        }
    }
    return suggest
}
```

## 5. 建议命令

我们可以使用前一个示例的逻辑，使该函数返回建议命令，定义如下：

```go
var suggest = Base{
    Action: func(in io.Reader, w io.Writer, args ...string) bool {
        var list []string
        for _, c := range commands {
            name := c.GetName()
            d := levenshtein.ComputeDistance(name, args[0])
            if d < 3 {
                list = append(list, name)
            }
        }
        fmt.Fprintf(w, "Command %q not found.", args[0])
        if len(list) == 0 {
            return false
        }
        fmt.Fprint(w, " Maybe you meant: ")
        for i := range list {
            if i > 0 {
                fmt.Fprint(w, ", ")
            }
            fmt.Fprintf(w, "%s", list[i])
        }
        return false
    },
}
```

## 6. 注册内置命令

我们已经可以在 `exit` 和 `help` 包中注册一些命令。只有 `help` 可以在此定义，因为命令列表是私有的：

```go
func init() {
    Register(Base{Name: "help", Help: "...", Action: helpAction})
    Register(Base{Name: "exit", Help: "...", Action: exitAction})
}

func helpAction(in io.Reader, w io.Writer, args ...string) bool {
    fmt.Fprintln(w, "Available commands:")
    for _, c := range commands {
        n := c.GetName()
        fmt.Fprintf(w, " - %-15s %s\n", n, c.GetHelp())
    }
    return false
}

func exitAction(in io.Reader, w io.Writer, args ...string) bool {
    fmt.Fprintf(w, "Goodbye! :)\n")
    return true
}
```

这种方法允许用户使用 `Base` 结构体创建简单命令，或嵌入它、使用自定义结构体（如带状态的命令）：

```go
// 嵌入未命名字段（继承方法）
type MyCmd struct {
    Base
    MyField string
}

// 自定义实现
type MyImpl struct{}

func (MyImpl) GetName() string { return "myimpl" }
func (MyImpl) GetHelp() string { return "help string" }
func (MyImpl) Run(input io.Reader, output io.Writer, args ...string) bool {
    // do something
    return true
}
```

`MyCmd` 结构体和 `MyImpl` 结构体的区别在于：前者可作为另一个命令的装饰器，后者是完全不同的实现，无法与其他命令交互。

# 带状态的命令

到目前为止，我们创建的命令没有内部状态。但某些命令可以保持内部状态并据此改变行为。状态可能仅限于会话本身，也可能跨多个会话共享。最明显的例子是终端中的命令历史，所有执行的命令都被存储并在会话间保留。

## 易失性状态

最容易实现的是非持久化状态，应用退出时丢失。我们只需创建一个承载状态并满足命令接口的自定义数据结构。方法必须属于类型指针，否则无法修改数据。

下面示例创建一个基础内存存储，作为栈（先进后出）工作，带参数。先实现 `push` 和 `pop` 功能：

```go
type Stack struct {
    data []string
}

func (s *Stack) push(values ...string) {
    s.data = append(s.data, values...)
}

func (s *Stack) pop() (string, bool) {
    if len(s.data) == 0 {
        return "", false
    }
    v := s.data[len(s.data)-1]
    s.data = s.data[:len(s.data)-1]
    return v, true
}
```

栈中存储的字符串代表命令状态。现在实现命令接口方法，从简单的开始：

```go
func (s *Stack) GetName() string {
    return "stack"
}

func (s *Stack) GetHelp() string {
    return "a stack-like memory storage"
}
```

现在决定内部工作方式。有两个子命令：

* `push`，后跟一个或多个参数，将值压入栈。
* `pop` 取出栈顶元素，不需要参数。

定义辅助方法 `isValid` 检查参数是否有效：

```go
func (s *Stack) isValid(cmd string, args []string) bool {
    switch cmd {
    case "pop":
        return len(args) == 0
    case "push":
        return len(args) > 0
    default:
        return false
    }
}
```

现在实现命令执行方法，使用有效性检查。通过则执行选定命令，否则显示帮助信息：

```go
func (s *Stack) Run(r io.Reader, w io.Writer, args ...string) (exit bool) {
    if l := len(args); l < 2 || !s.isValid(args[1], args[2:]) {
        fmt.Fprintf(w, "Use `stack push <something>` or `stack pop`\n")
        return false
    }
    if args[1] == "push" {
        s.push(args[2:]...)
        return false
    }
    if v, ok := s.pop(); !ok {
        fmt.Fprintf(w, "Empty!\n")
    } else {
        fmt.Fprintf(w, "Got: `%s`\n", v)
    }
    return false
}
```

## 持久化状态

下一步是跨会话持久化状态，这需要应用启动时执行某操作，退出时执行另一操作。这些新行为可通过修改命令接口集成：

```go
type Command interface {
    Startup() error
    Shutdown() error
    GetName() string
    GetHelp() string
    Run(r io.Reader, w io.Writer, args ...string) (exit bool)
}
```

`Startup()` 方法负责状态加载，应用启动时调用；`Shutdown()` 方法需在 `exit` 前将当前状态保存到磁盘。我们可以用这些方法更新 `Base` 结构体，但它们不做任何事，因为没有状态：

```go
// Startup 什么都不做
func (b Base) Startup() error { return nil }

// Shutdown 什么都不做
func (b Base) Shutdown() error { return nil }
```

命令列表未导出，是未导出变量 `commands`。我们可以添加两个函数与该列表交互，并确保在所有可用命令上执行 `Startup` 和 `Shutdown` 方法：



# 升级 Stack 命令

我们希望之前定义的命令 `Stack` 能够在会话之间保存其状态。最简单的解决方案是将堆栈的内容保存为文本文件，每行一个元素。我们可以使用 `os/user` 包将该文件放在用户的 `home` 目录中，使其对每个用户唯一：

```go
func (s *Stack) getPath() (string, error) {
    u, err := user.Current()
    if err != nil {
        return "", err
    }
    return filepath.Join(u.HomeDir, ".stack"), nil
}
```

让我们开始编写；我们将创建并截断文件（使用 `TRUNC` 标志将其大小设置为 `0`）并写入以下行：

```go
func (s *Stack) Shutdown(w io.Writer) error {
    path, err := s.getPath()
    if err != nil {
        return err
    }
    f, err := os.OpenFile(path, os.O_CREATE|os.O_WRONLY|os.O_TRUNC, 0600)
    if err != nil {
        return err
    }
    defer f.Close()
    for _, v := range s.data {
        if _, err := fmt.Fprintln(f, v); err != nil {
            return err
        }
    }
    return nil
}
```

关机时使用的方法将逐行读取文件并将元素添加到堆栈中。我们可以使用 `bufio.Scanner`，正如我们在前一章中所见，轻松实现这一点：

```go
func (s *Stack) Startup(w io.Writer) error {
    path, err := s.getPath()
    if err != nil {
        return err
    }
    f, err := os.Open(path)
    if err != nil {
        if os.IsNotExist(err) {
            return nil
        }
        return err
    }
    defer f.Close()
    s.data = s.data[:0]
    scanner := bufio.NewScanner(f)
    for scanner.Scan() {
        s.push(string(scanner.Bytes()))
    }
    return nil
}
```

# 总结

在本章中，我们浏览了一些术语，以了解现代终端应用程序为何存在以及它们是如何演变的。

然后，我们专注于如何实现一个基本的伪终端。第一步是创建一个处理输入管理的循环，然后需要创建一个命令选择器，最后是一个执行器。选择器可以在包中定义的一系列函数之间进行选择，我们创建了一个特殊的命令来退出应用程序。通过一些重构，我们从函数转向了包含名称和动作的结构体。

我们看到了以各种方式改进应用程序的方法。首先，我们创建了对多行输入的支持（为扫描器使用自定义分割函数），该支持支持带换行的引用字符串。然后，我们创建了一些工具为我们的函数添加彩色输出，并在之前定义的命令之一中使用了它们。我们还使用莱文斯坦距离在用户指定不存在的命令时建议相似的命令。

最后，我们将命令从主应用程序中分离出来，并创建了一种从外部注册新命令的方法。我们使用了接口，因为这允许更好的扩展和定制，并提供了接口的基本实现。

在下一章中，我们将开始讨论进程属性和子进程。

# 问题

1. 什么是终端，什么是伪终端？
2. 伪终端应该能够做什么？
3. 我们使用了哪些 Go 工具来模拟终端？
4. 我的应用程序如何从标准输入获取指令？
5. 使用接口作为命令有什么优势？
6. 什么是莱文斯坦距离？为什么它在伪终端中很有用？
