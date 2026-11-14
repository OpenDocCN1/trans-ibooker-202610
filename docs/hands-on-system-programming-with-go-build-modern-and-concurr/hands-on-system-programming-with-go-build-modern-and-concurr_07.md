# 与文件系统交互

本章专门讲解与 Unix 文件系统的交互。我们将从基本的读写操作，到更高级的缓冲操作，如令牌扫描和文件监控。

在 Unix 中，所有用户或系统信息都以文件形式存储，因此要与系统和用户数据交互，必须与文件系统交互。

本章将展示执行读写操作的不同方式，以及每种方式在代码简洁性、应用内存使用和性能、执行速度方面的侧重点。

本章将涵盖以下主题：

*   文件路径操作
*   读取文件
*   写入文件
*   其他文件系统操作
*   第三方包

## 技术要求

本章要求已安装 Go 并设置好常用编辑器。更多信息请参阅 第 3 章, *Go 概览*。

## 处理路径

Go 提供了一系列函数，使得以平台无关方式操作文件路径成为可能，这些函数主要包含在 `path/filepath` 和 `os` 包中。

## 工作目录

每个进程都有一个关联的目录，称为**工作目录**，通常从父进程继承。这使得指定相对路径成为可能——即不以根文件夹开头的路径。在 Unix 和 macOS 上根目录为 `/`，在 Windows 上为 `C:\\`（或其他驱动器盘符）。

绝对/完整路径以根目录开头，表示文件系统中的同一位置，例如 `/usr/local`。

相对路径不以根目录开头，路径从当前工作目录开始，例如 `documents`。

操作系统将这些路径解释为相对于当前目录，因此其绝对版本是工作目录与相对路径的拼接。请看以下示例：

```
user:~ $ cd documents

user:~/documents $ cd ../videos

user:~/videos $
```

用户位于其主文件夹 `~`。用户指定切换到 `documents` 目录，`cd` 命令自动将工作目录作为前缀添加并移动到 `~/documents`。

在进行第二个命令之前，先介绍两个在所有操作系统的所有目录中都可用的特殊文件：

*   `.`：单点引用当前目录。如果是路径的第一个元素，它是进程工作目录；否则引用它之前的路径元素（例如在 `~/./documents` 中，`.` 指代 `~`）。
*   `..`：双点引用当前目录的父目录（如果是路径的第一个元素），或引用它之前目录的父目录（例如在 `~/images/../documents` 中，`..` 指代 `~/images` 的父目录 `~`）。

了解这一点，我们可以轻松推断：第二个路径先拼接为 `~/documents/../videos`，父目录元素 `..` 被解析，最终得到路径 `~/videos`。

## 获取和设置工作目录

我们可以使用 `os` 包的 `func Getwd() (dir string, err error)` 函数来查找代表当前工作目录的路径。

更改工作目录使用同一包的另一个函数：`func Chdir(dir string) error`，如下所示：

```go
wd, err := os.Getwd()
if err != nil {
    fmt.Println(err)
    return
}
fmt.Println("starting dir:", wd)

if err := os.Chdir("/"); err != nil {
    fmt.Println(err)
    return
}

if wd, err = os.Getwd(); err != nil {
    fmt.Println(err)
    return
}
fmt.Println("final dir:", wd)
```

## 路径操作

`filepath` 包包含不到 20 个函数，与标准库其他包相比数量较少，用于操作路径。让我们快速浏览这些函数：

*   `func Abs(path string) (string, error)`：返回传入路径的绝对版本（如果尚非绝对路径，则将其与当前工作目录拼接），并进行清理。
*   `func Base(path string) string`：返回路径的最后一个元素（基名）。例如 `path/to/some/file` 返回 `file`。注意如果路径为空，此函数返回 `.`（点）路径。
*   `func Clean(path string) string`：通过应用一系列定义规则返回路径的最短版本。执行替换 `.` 和 `..`、移除尾部分隔符等操作。
*   `func Dir(path string) string`：获取去掉最后一个元素的路径。通常返回该元素的父目录。
*   `func EvalSymlinks(path string) (string, error)`：评估符号链接后返回路径。如果提供的路径是相对路径且不包含绝对路径的符号链接，则返回相对路径。
*   `func Ext(path string) string`：获取路径的文件扩展名，即最后一个元素中最后一个点开头的后缀；如果没有点则返回空字符串（例如 `docs/file.txt` 返回 `.txt`）。



# Filepath 包常用函数

*   `func FromSlash(path string) string`：将路径中所有的 `/`（斜杠）替换为操作系统的路径分隔符。如果操作系统是 Windows，此函数不执行任何操作；在 Unix 或 macOS 下执行替换。
*   `func Glob(pattern string) (matches []string, err error)`：查找所有匹配指定模式的文件。如果没有匹配的文件，结果为 `nil`。它不报告路径探索过程中发生的错误。其语法与 `Match` 共享。
*   `func HasPrefix(p, prefix string) bool`：此函数已弃用。
*   `func IsAbs(path string) bool`：判断路径是否为绝对路径。
*   `func Join(elem ...string) string`：通过文件路径分隔符连接多个路径元素。注意，这也会对结果调用 `Clean`。
*   `func Match(pattern, name string) (matched bool, err error)`：验证给定名称是否匹配模式，允许使用通配符字符 `\*` 和 `?`，以及使用方括号的字符组或序列。
*   `func Rel(basepath, targpath string) (string, error)`：返回目标路径相对于基础路径的相对路径，如果不可能则返回错误。此函数会对结果调用 `Clean`。
*   `func Split(path string) (dir, file string)`：使用最后一个尾随斜杠将路径分为两部分。结果通常是输入路径的父路径和文件名。如果没有分隔符，`dir` 将为空，文件将是路径本身。
*   `func SplitList(path string) []string`：返回路径列表，使用列表分隔符字符分隔，Unix 和 macOS 中为 `:`，Windows 中为 `;`。
*   `func ToSlash(path string) string`：执行与 `FromSlash` 函数相反的替换，将每个路径分隔符改为 `/`，在 Unix 和 macOS 上不执行任何操作，在 Windows 上执行替换。
*   `func VolumeName(path string) string`：在非 Windows 平台上不执行任何操作。返回引用卷的路径组件。这适用于本地路径和网络资源。
*   `func Walk(root string, walkFn WalkFunc) error`：从根目录开始，递归遍历文件树，为树中的每个条目执行遍历函数。如果遍历函数返回错误，遍历停止并返回该错误。函数定义如下：

```
type WalkFunc func(path string, info os.FileInfo, err error) error
```

## 介绍 `os.Args`

在进入下一个示例之前，让我们介绍一个重要变量：`os.Args`。该变量至少包含一个值，即调用当前进程的路径。随后可能跟随在同一次调用中指定的参数。

我们想要实现一个小型应用程序，列出并统计目录中的文件数量。我们可以使用刚才看到的一些工具来实现这一点。

列出并统计文件的示例代码如下：

```
package main

import (
    "fmt"
    "os"
    "path/filepath"
)

func main() {
    if len(os.Args) != 2 { // 确保指定了路径
        fmt.Println("Please specify a path.")
        return
    }
    root, err := filepath.Abs(os.Args[1]) // 获取绝对路径
    if err != nil {
        fmt.Println("Cannot get absolute path:", err)
        return
    }
    fmt.Println("Listing files in", root)
    var c struct {
        files int
        dirs  int
    }
    filepath.Walk(root, func(path string, info os.FileInfo, err error) error {
        // 遍历树以统计文件和文件夹
        if info.IsDir() {
            c.dirs++
        } else {
            c.files++
        }
        fmt.Println("-", path)
        return nil
    })
    fmt.Printf("Total: %d files in %d directories", c.files, c.dirs)
}
```

# 从文件读取

获取文件内容可以使用 `io/ioutil` 包中的辅助函数，以及 `ReadFile` 函数，它一次性打开、读取并关闭文件。这使用一个小缓冲区（512 字节）并将全部内容加载到内存中。如果文件非常大、大小未知，或者文件内容可以分部分处理，这不是一个好主意。

一次性从磁盘读取巨大文件意味着将文件的所有内容复制到主内存中，而主内存是有限资源。这可能导致内存短缺以及运行时错误。一次读取文件的一个块可以帮助读取大文件内容，而不会导致巨大的内存使用。这是因为读取下一个块时将重复使用内存的同一部分。

一次性读取所有内容的示例代码如下：

```
package main

import (
    "fmt"
    "io/ioutil"
    "os"
)

func main() {
    if len(os.Args) != 2 {
        fmt.Println("Please specify a path.")
        return
    }
    b, err := ioutil.ReadFile(os.Args[1])
    if err != nil {
        fmt.Println("Error:", err)
    }
    fmt.Println(string(b))
}
```

# Reader 接口

对于所有从磁盘读取的操作，有一个至关重要的接口：

```
type Reader interface {
    Read(p []byte) (n int, err error)
}
```

它的工作非常简单——用读取的内容填充给定的字节切片，并返回已读取的字节数和错误（如果发生）。`io` 包定义了一个特殊的错误变量，称为 `EOF`（**文件结束**），当没有更多输入可用时应返回该错误。

Reader 使得按块处理数据成为可能（块的大小由切片决定），如果后续操作重复使用同一个切片，生成的程序将始终更加节省内存，因为它使用分配切片的同一有限内存部分。

# 文件结构

`os.File` 类型满足 Reader 接口，是用于与文件内容交互的主要角色。用于读取目的获取实例的最常用方法是 `os.Open` 函数。记住在使用完文件后关闭它非常重要——对于短生命周期的程序这可能不明显，但如果应用程序持续打开文件而不关闭已完成的文件，应用程序将达到操作系统施加的打开文件限制，并开始导致打开操作失败。

Shell 提供了一些实用工具，如下：

*   获取打开文件限制——`ulimit -n`
*   检查某个进程打开了多少文件——`lsof -p PID`

前面的示例打开文件只是为了将其内容显示到标准输出，它通过将所有内容加载到内存中来实现。这可以使用我们刚才提到的工具轻松优化。在下面的示例中，我们使用一个小缓冲区，在其内容被下一次读取覆盖之前打印其内容，使用小缓冲区将内存使用量保持在最低限度。

使用字节数组作为缓冲区的示例代码如下：

```
func main() {
    if len(os.Args) != 2 {
        fmt.Println("Please specify a file")
        return
    }
    f, err := os.Open(os.Args[1])
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    defer f.Close() // 确保关闭以避免泄漏

    var (
        b = make([]byte, 16)
    )
    for n := 0; err == nil; {
        n, err = f.Read(b)
        if err == nil {
            fmt.Print(string(b[:n])) // 只打印已读取的内容
        }
    }
    if err != nil && err != io.EOF { // 我们期望一个 EOF
        fmt.Println("\n\nError:", err)
    }
}
```

如果一切按预期工作，读取循环将继续执行读取操作，直到文件内容结束。在这种情况下，读取循环将返回 `io.EOF` 错误，表明没有更多内容可用。

# 使用缓冲区




# 数据缓冲区

**数据缓冲区**，或简称缓冲区，是内存的一部分，用于在数据移动时存储临时数据。字节缓冲区在 `bytes` 包中实现，它们由底层切片实现，该切片能够在每次需要存储的数据量无法容纳时进行增长。

如果每次都分配新的缓冲区，旧的缓冲区最终将由 GC 本身清理，这并非最优解。重用缓冲区总比分配新缓冲区要好。这是因为它们可以在保持容量不变的情况下重置切片（底层数组不会被清理或被 GC 回收）。

缓冲区还提供了两个函数来显示其底层的长度和容量。在下面的示例中，我们可以看到如何使用 `Buffer.Reset` 重用缓冲区以及如何跟踪其容量。

## 缓冲区重用与容量

下面的代码展示了缓冲区重用及其底层容量的示例：

```go
package main

import (
    "bytes"
    "fmt"
)

func main() {
    var b = bytes.NewBuffer(make([]byte, 26))
    var texts = []string{
        `As he came into the window`,
        `It was the sound of a crescendo
He came into her apartment`,
        `He left the bloodstains on the carpet`,
        `She ran underneath the table
He could see she was unable
So she ran into the bedroom
She was struck down, it was her doom`,
    }
    for i := range texts {
        b.Reset()
        b.WriteString(texts[i])
        fmt.Println("Length:", b.Len(), "\tCapacity:", b.Cap())
    }
}
```

# 预览内容

在前面的示例中，我们固定了字节数，以便在每次读取打印之前存储内容。`bufio` 包提供了一些功能，使得可以使用一个不直接由用户控制的底层缓冲区，并使得执行一项名为 **预览** 的重要操作成为可能。

**预览** 是指在不移动读取器光标的情况下读取内容的能力。在这里，预览的数据在底层存储在缓冲区中。每次读取操作都会检查此缓冲区中是否有数据，如果有，则返回该数据并将其从缓冲区中移除。这就像一个队列（先进先出）。

这一简单操作开启的可能性是无限的，它们都源于不断预览直到找到所需的数据序列，然后实际读取感兴趣的数据块。该操作最常见的用途包括以下内容：

*   缓冲区持续从读取器读取，直到找到换行符（一次读取一行）。
*   同理，该操作用于直到找到空格（一次读取一个单词）。

## Scanner 与 SplitFunc

允许应用程序实现此行为的结构是 `bufio.Scanner`。这使得可以定义分割函数，其类型如下：

```go
type SplitFunc func(data []byte, atEOF bool) (advance int, token []byte, err error)
```

当返回错误时，此函数停止；否则，它返回要在内容中前进的字节数，以及最终的一个标记。包中实现的函数如下：

*   `ScanBytes`：字节标记
*   `ScanRunes`：Rune 标记
*   `ScanWords`：单词标记
*   `ScanLines`：行标记

我们可以实现一个仅使用读取器即可统计行数的文件读取器。生成的程序将尝试模拟 Unix `wc -l` 命令的功能。

### 示例：统计文件行数

下面的代码展示了打印文件并统计行数的示例：

```go
func main() {
    if len(os.Args) != 2 {
        fmt.Println("Please specify a path.")
        return
    }
    f, err := os.Open(os.Args[1])
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    defer f.Close()
    r := bufio.NewReader(f) // 用缓冲读取器包装读取器
    var rowCount int
    for err == nil {
        var b []byte
        for moar := true; err == nil && moar; {
            b, moar, err = r.ReadLine()
            if err == nil {
                fmt.Print(string(b))
            }
        }
        // 每当 moar 为 false 时，表示一行已完全读取
        if err == nil {
            fmt.Println()
            rowCount++
        }
    }
    if err != nil && err != io.EOF {
        fmt.Println("\nError:", err)
        return
    }
    fmt.Println("\nRow count:", rowCount)
}
```

# Closer 与 Seeker

还有两个与读取器相关的接口：`io.Closer` 和 `io.Seeker`：

```go
type Closer interface {
    Close() error
}

type Seeker interface {
    Seek(offset int64, whence int) (int64, error)
}
```

这些通常与 `io.Reader` 结合，生成的接口如下：

```go
type ReadCloser interface {
    Reader
    Closer
}

type ReadSeeker interface {
    Reader
    Seeker
}
```

`Close` 方法确保资源被释放并避免泄漏，而 `Seek` 方法使得可以将当前对象（例如 `Writer`）的光标移动到距离文件开头/结尾或当前位置的期望偏移量处。

`os.File` 结构体实现了这些方法，因此满足所有列出的接口。可以在操作完成后关闭文件，或者根据需要移动当前光标。

# 写入文件

正如我们在读取时所见，写入文件有不同的方式，每种方式都有其自身的优缺点。例如，在 `ioutil` 包中，我们有另一个名为 `WriteFile` 的函数，它允许我们在一行代码中执行整个操作。这包括打开文件、写入内容，然后关闭它。

## 一次性写入所有内容

下面的代码展示了一次性写入文件所有内容的示例：

```go
package main

import (
    "fmt"
    "io/ioutil"
    "os"
)

func main() {
    if len(os.Args) != 3 {
        fmt.Println("Please specify a path and some content")
        return
    }
    // 第二个参数（内容）需要转换为字节切片
    if err := ioutil.WriteFile(os.Args[1], []byte(os.Args[2]), 0644); err != nil {
        fmt.Println("Error:", err)
    }
}
```

此示例在单次操作中一次性写入所有内容。这要求我们使用字节切片将所有内容分配在内存中。如果内容太大，内存使用可能会成为操作系统的问题，从而可能终止我们应用程序的进程。

如果内容大小不是很大且应用程序生命周期短，将内容加载到内存并通过单次操作写入并不是问题。但对于长期运行、对许多不同文件执行读写的应用程序来说，这并非最佳实践。它们必须将所有内容分配在内存中，而这些内存将在某个时刻由 GC 释放——此操作并非零成本，这意味着它在内存使用和性能方面存在劣势。

# Writer 接口

适用于读取的相同原则也适用于写入——`io` 包中有一个接口定义了写入行为，如下所示：

```go
type Writer interface {
    Write(p []byte) (n int, err error)
}
```

`io.Writer` 接口定义了一个方法，给定一个字节切片，返回已写入的字节数和/或是否发生了错误。写入器使得可以一次写入一块数据，而无需一次性拥有所有数据。`os.File` 结构体碰巧也是一个写入器，可以按这种方式使用。

我们可以使用字节切片作为缓冲区，逐块写入信息。在下面的示例中，我们将尝试结合前一节的读取与写入，利用 `io.Seeker` 的能力在写入前反转其内容。

## 示例：反转文件内容

下面的代码展示了反转文件内容的示例：




```
// 让我们省略参数检查和文件打开，直接获取 src 和 dst
cur, err := src.Seek(0, os.SEEK_END) // 让我们移动到文件末尾
if err != nil {
    fmt.Println("Error:", err)
    return
}
b := make([]byte, 16)
```

移动到文件末尾并定义一个字节缓冲区后，我们进入一个循环，该循环在文件中向后移动一小段距离，然后读取一部分内容，如下面的代码所示：

```
for step, r, w := int64(16), 0, 0; cur != 0; {
    if cur < step { // 确保游标最多回到 0
        b, step = b[:cur], cur
    }
    cur = cur - step
    _, err = src.Seek(cur, os.SEEK_SET) // 向后移动
    if err != nil {
        break
    }
    if r, err = src.Read(b); err != nil || r != len(b) {
        if err == nil { // 应读取完整个缓冲区
            err = fmt.Errorf("read: expected %d bytes, got %d", len(b), r)
        }
        break
    }
```

然后，我们反转内容并将其写入目标文件，如下面的代码所示：

```
    for i, j := 0, len(b)-1; i < j; i, j = i+1, j-1 {
        switch { // 交换 (\r\n) 以便它们恢复原位
        case b[i] == '\r' && b[i+1] == '\n':
            b[i], b[i+1] = b[i+1], b[i]
        case j != len(b)-1 && b[j-1] == '\r' && b[j] == '\n':
            b[j], b[j-1] = b[j-1], b[j]
        }
        b[i], b[j] = b[j], b[i] // 交换字节
    }
    if w, err = dst.Write(b); err != nil || w != len(b) {
        if err != nil {
            err = fmt.Errorf("write: expected %d bytes, got %d", len(b), w)
        }
    }
}
if err != nil && err != io.EOF { // 我们预期会遇到 EOF
    fmt.Println("\n\nError:", err)
}
```

# 缓冲区与格式

在上一节中，我们了解了如何使用 `bytes.Buffer` 临时存储数据，以及它如何通过追加底层切片来处理自身的增长。`fmt` 包大量使用缓冲区来执行其操作；出于依赖原因，这些缓冲区并非 `bytes` 包中的那些。这种方法内在于 Go 语言的一句谚语中：

> "A little copy is better than a little dependency."（少量复制胜过少量依赖。）

如果你必须导入一个包仅为了使用一个函数或类型，你应该考虑直接将必要的代码复制到你自己的包中。如果一个包包含的内容远超你的需求，复制代码可以减小最终二进制文件的体积。你也可以自定义代码并根据需要进行调整。

缓冲区的另一个用途是在写入前组装消息。让我们编写一些代码，以便使用缓冲区来格式化书籍列表：

```
const grr = "G.R.R. Martin"

type book struct {
    Author, Title string
    Year int
}

func main() {
    dst, err := os.OpenFile("book_list.txt", os.O_CREATE|os.O_WRONLY, 0666)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    defer dst.Close()
    bookList := []book{
        {Author: grr, Title: "A Game of Thrones", Year: 1996},
        {Author: grr, Title: "A Clash of Kings", Year: 1998},
        {Author: grr, Title: "A Storm of Swords", Year: 2000},
        {Author: grr, Title: "A Feast for Crows", Year: 2005},
        {Author: grr, Title: "A Dance with Dragons", Year: 2011},
        // 如果省略 year，则默认为零值
        {Author: grr, Title: "The Winds of Winter"},
        {Author: grr, Title: "A Dream of Spring"},
    }
    b := bytes.NewBuffer(make([]byte, 0, 16))
    for _, v := range bookList {
        // 将格式化后的消息连同参数打印到 writer
        fmt.Fprintf(b, "%s - %s", v.Title, v.Author)
        if v.Year > 0 { 
            // 如果年份不存在则不打印
            fmt.Fprintf(b, " (%d)", v.Year)
        }
        b.WriteRune('\n')
        if _, err := b.WriteTo(dst); true { // 复制字节，清空缓冲区
            fmt.Println("Error:", err)
            return
        }
    }
}
```

缓冲区用于组装书籍描述，如果年份不存在则将其省略。在处理字节时这非常高效，如果每次都重用缓冲区则效果更好。如果此类操作的输出应为字符串，`strings` 包中有一个非常相似的结构体叫 `strings.Builder`，它拥有相同的写入方法，但有一些差异，如下所示：

*   `String()` 方法使用 `unsafe` 包将字节转换为字符串，而不是复制它们。
*   不允许复制 `strings.Builder` 然后向副本写入数据，因为这会导致 `panic`。

## 高效写入

每次执行 `os.File` 方法（即 `Write`）时，都会转化为一次系统调用，这是一种带有开销的操作。一般来说，通过一次写入更多数据来最小化操作次数是个好主意，从而减少在此类调用上花费的时间。

`bufio.Writer` 结构体是一个包装了另一个 writer（如 `os.File`）的 writer，它仅在缓冲区满时执行写入操作。这使得可以使用 `Flush` 方法执行强制写入，该方法通常保留到写入过程结束时才调用。使用缓冲区的一个良好模式如下：

```
var w io.WriteCloser
// 初始化 writer
defer w.Close()
b := bufio.NewWriter(w)
defer b.Flush()
// 写入操作
```

`defer` 语句会在当前函数返回前按逆序执行，因此第一个 `Flush` 确保缓冲区上残留的任何内容都被写入，然后 `Close` 实际关闭文件。如果这两个操作按相反顺序执行，flush 会尝试写入一个已关闭的文件，返回错误，并导致最后一块信息写入失败。

## 文件模式

我们看到 `os.OpenFile` 函数可以通过文件模式选择如何打开文件，文件模式是一个 `uint32`，其中每一位都有特定含义（类似于 Unix 文件和文件夹权限）。`os` 包提供了一系列值，每个值指定一种模式，组合它们的正确方式是使用 `|`（按位或）。

以下代码展示了可用的模式，直接摘自 Go 的源代码：

```
// 必须恰好指定 O_RDONLY、O_WRONLY 或 O_RDWR 之一。
O_RDONLY int = syscall.O_RDONLY // 以只读模式打开文件。
O_WRONLY int = syscall.O_WRONLY // 以只写模式打开文件。
O_RDWR int = syscall.O_RDWR // 以读写模式打开文件。
// 其余值可通过或运算组合以控制行为。
O_APPEND int = syscall.O_APPEND // 写入时将数据追加到文件末尾。
O_CREATE int = syscall.O_CREAT // 如果文件不存在则创建新文件。
O_EXCL int = syscall.O_EXCL // 与 O_CREATE 连用，文件必须不存在。
O_SYNC int = syscall.O_SYNC // 以同步 I/O 方式打开。
O_TRUNC int = syscall.O_TRUNC // 如果可能，打开时截断文件。
```

前三个代表允许的操作（读、写或两者皆可），其他的如下：

*   `O_APPEND`：每次写入前，文件偏移量定位到文件末尾。
*   `O_CREATE`：如果文件不存在则创建它。
*   `O_EXCL`：如果与 create 连用，文件已存在则失败（独占创建）。
*   `O_SYNC`：执行读/写操作并验证其完成情况。
*   `O_TRUNC`：如果文件存在，将其大小截断为 `0`。

## 其他操作

读和写并非文件上唯一可执行的操作。在下一节中，我们将了解如何使用 `os` 包来使用它们。

### 创建

为了创建一个空文件，我们可以调用一个名为 `Create` 的辅助函数，它以 `0666` 权限打开一个新文件，如果文件不存在则将其截断。或者，我们可以使用带有 `O_CREATE|O_TRUNC` 模式的 `OpenFile` 来指定自定义权限，如下面的代码所示：

```
package main

import "os"

func main() {
    f, err := os.Create("file.txt")
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    f.Close()
}
```

### 截断




要在特定尺寸下截断文件内容，如果文件较小则保持不变，可以使用 `os.Truncate` 方法。它的用法非常简单，如下面的代码所示：

```go
package main

import "os"

func main() {
    // 让我们保持在 4kB 以下
    if err := os.Truncate("file.txt", 4096); err != nil {
        fmt.Println("Error:", err)
    }
}
```

# 截断

## 删除

要删除文件，有一个简单的函数，称为 `os.Remove`，如下面的代码所示：

```go
package main

import "os"

func main() {
    if err := os.Remove("file.txt"); err != nil {
        fmt.Println("Error:", err)
    }
}
```

## 移动

`os.Rename` 函数可以更改文件名及/或其目录。请注意，如果目标文件已存在，此操作会替换它。

更改文件名或其目录的代码如下：

```go
package main

import "os"

func main() {
    if err := os.Rename("file.txt", "../file.txt"); err != nil {
        fmt.Println("Error:", err)
    }
}
```

## 复制

没有单一的函数可以复制文件，但可以使用读取器和写入器配合 `io.Copy` 函数轻松完成。以下示例展示了如何使用它将内容从一个文件复制到另一个文件：

```go
func CopyFile(from, to string) (int64, error) {
    src, err := os.Open(from)
    if err != nil {
        return 0, err
    }
    defer src.Close()
    dst, err := os.OpenFile(to, os.O_WRONLY|os.O_CREATE, 0644)
    if err != nil {
        return 0, err
    }
    defer dst.Close()
    return io.Copy(dst, src)
}
```

## 统计信息

`os` 包提供了 `FileInfo` 接口，它返回文件的元数据，如下面的代码所示：

```go
type FileInfo interface {
    Name() string       // 文件的基本名称
    Size() int64        // 常规文件的长度（字节）；其他文件取决于系统
    Mode() FileMode     // 文件模式位
    ModTime() time.Time // 修改时间
    IsDir() bool        // Mode().IsDir() 的简写
    Sys() interface{}   // 底层数据源（可返回 nil）
}
```

`os.Stat` 函数返回指定路径文件的信息。

## 更改属性

为了与文件系统交互并更改这些属性，有三个可用的函数：

*   `func Chmod(name string, mode FileMode) error`：更改文件的权限
*   `func Chown(name string, uid, gid int) error`：更改文件的所有者和组
*   `func Chtimes(name string, atime time.Time, mtime time.Time) error`：更改文件的访问时间和修改时间

# 第三方包

社区提供了许多完成各种任务的包。我们将在本节中快速浏览其中一些。

## 虚拟文件系统

在 Go 中，文件是一个结构体，一种具体类型，并且没有围绕它的抽象，而文件的信息由 `os.FileInfo` 表示，它是一个接口。这略显不一致，因此曾有许多尝试创建一个完整且一致的文件系统抽象，通常称为*虚拟文件系统*。

最常用的两个包如下：

*   `vfs`：[github.com/blang/vfs](https://github.com/blang/vfs)
*   `afero`：[github.com/spf13/afero](https://github.com/spf13/afero)

即使它们是分别开发的，但它们做的事情是一样的——它们定义了一个包含 `os.File` 所有方法的接口，然后定义了一个实现 `os` 包中可用函数的接口，如创建、打开和删除文件等。

它们提供了一个基于 `os.File` 的版本，该版本使用标准包实现，但也有一个内存版本，它使用模拟文件系统的数据结构。这对于为任何包构建测试非常有用。

## 文件系统事件

Go 在 `golang.org/x/` 包中有一些实验性功能，这些功能位于 Go 的 GitHub 组织下（[`github.com/golang/`](https://github.com/golang/)）。`golang.org/x/sys` 包是此列表的一部分，并包含一个专门用于 Unix 系统事件的子包。这被用来构建一个 Go 文件功能中缺失且非常有用的特性——观察某个路径以获取文件创建、删除和更新等事件。

最著名的两个实现如下：

*   `notify`：[github.com/rjeczalik/notify](https://github.com/rjeczalik/notify)
*   `fsnotify`：[github.com/fsnotify/fsnotify](https://github.com/fsnotify/fsnotify)

这两个包都暴露了一个允许创建监视器的函数。监视器是包含通道的结构体，负责传递文件事件。它们还暴露了另一个负责终止/关闭监视器和底层通道的函数。

# 总结

在本章中，我们概述了如何在 Go 中执行文件操作。为了定位文件，`filepath` 包提供了大量函数。这些函数可以帮助你执行各种操作，从组合路径到从中提取元素。

我们还研究了如何使用各种方法执行读取操作，从位于 `io/ioutil` 包中最简单但内存效率较低的方法，到需要 `io.Writer` 实现来读取固定字节块的方法。`bufio` 包中实现的预读内容的能力，允许进行一整套操作，如读单词或读行，这些操作在找到标记时停止读取。还有其他由文件满足的非常有用的接口；例如，`io.Closer` 确保资源被释放，`io.Seeker` 用于移动读取光标，而无需实际读取文件并丢弃输出。

将字节切片写入文件可以通过不同方式实现——`io/ioutil` 包使得通过一个函数调用即可完成，而对于更复杂或更节省内存的操作，则有 `io.Writer` 接口。这使得一次写入一个字节切片成为可能，并可被 `fmt` 包用于打印格式化数据。缓冲写入用于减少实际磁盘写入次数。这是通过一个收集内容的缓冲区完成的，每当缓冲区满时，它就会将内容传输到磁盘。

最后，我们了解了如何在文件系统上完成其他文件操作（创建、删除、复制/移动和更改文件属性），并查看了一些与文件系统相关的第三方包，即虚拟文件系统抽象和文件系统事件通知。

下一章将介绍流，并将重点关注所有与文件系统无关的读取器和写入器实例。

# 问题

1.  绝对路径和相对路径有什么区别？
2.  如何获取或更改当前工作目录？
3.  使用 `ioutil.ReadAll` 的优缺点是什么？
4.  为什么缓冲区对读取操作很重要？
5.  何时应该使用 `ioutil.WriteFile`？
6.  使用允许预读的缓冲读取器时，有哪些可用操作？
7.  何时最好使用字节缓冲区读取内容？
8.  缓冲区如何用于写入？使用它们有什么优势？
