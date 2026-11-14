
本章要求已安装 Go 并设置好您喜爱的编辑器。更多信息请参阅 [第 3 章](#602a92d5-25f7-46b8-83d4-10c6af1c6750.xhtml) *Go 概览*。

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

完整示例请访问 [https://play.golang.org/p/gVZOufSmlq1](https://play.golang.org/p/gVZOufSmlq1)。

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

完整示例请访问 [https://play.golang.org/p/PhdSsbzXcbE](https://play.golang.org/p/PhdSsbzXcbE)。

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

完整示例请访问 [https://play.golang.org/p/zBjGPMC4sfF](https://play.golang.org/p/zBjGPMC4sfF)
