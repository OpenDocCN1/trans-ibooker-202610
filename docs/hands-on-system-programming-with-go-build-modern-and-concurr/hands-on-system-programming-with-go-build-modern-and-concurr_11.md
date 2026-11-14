# 退出代码、信号和管道

本章将延续上一章的内容，演示父进程和子进程之间的通信。特别是，本章将向您展示如何通过正确使用退出代码、自定义信号处理以及使用管道连接进程来管理通信。这些通信形式将用于让我们的应用程序能够与操作系统和其他进程进行有效通信。

本章将涵盖以下主题：

*   返回退出代码
*   读取退出代码
*   拦截信号
*   发送信号
*   使用管道
*   使用其他流工具

## 技术要求

本章要求已安装 Go 并设置好您喜欢的编辑器。更多信息请参阅 第 3 章 *Go 概览*。

## 使用退出代码

退出代码，或称退出状态，是一个进程在退出时传递给其父进程的小整数。它是通知您应用程序执行结果的最简单方式。在 第 2 章 *Unix 操作系统组件* 中，我们简要提到了退出代码。现在我们将学习如何在应用程序内部使用它们，以及如何解读子进程的退出代码。

### 发送退出代码

退出代码是进程在终止后通知其父进程其状态的方式。要从当前进程返回任何退出状态，有一个函数能以直接的方式完成这项工作：`os.Exit`。

该函数接受一个参数，即一个整数，代表将返回给父进程的退出代码。可以使用一个简单的程序来验证这一点，如下面的代码所示：

```go
package main

import (
   "fmt"
    "os"
)

func main() {
    fmt.Println("Hello, playground")
    os.Exit(1)
}
```

完整示例请访问 <https://play.golang.org/p/-6GIY7EaVD_V>。

退出代码 `0` 表示应用程序成功执行。任何其他退出代码都象征着执行期间可能发生的某种错误。当 `main` 函数正常结束时，它返回 `0`；当 panic 未被恢复时，它返回 `2`。

### Bash 中的退出代码

每当在 Shell 中执行一条命令，生成的退出代码都会存储在一个变量中。最后执行的命令的状态存储在 `$?` 变量中，可以按如下方式打印：

```bash
> echo $? # 将打印 1
```

重要的是要注意，退出代码仅在您运行通过 `go build` 或 `go install` 获得的二进制文件时才有效。如果您使用 `go run`，对于任何非 `0` 的代码，它都将返回 `1`。

### 退出值的位大小

退出状态是一个 8 位整数；这意味着即使 Go 函数的参数是整数，返回的状态也将是传递值与 `256` 进行取模运算的结果。

让我们看看下面的程序：

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    fmt.Println("Hello, playground")
    os.Exit(-1)
}
```

完整示例请访问 <https://play.golang.org/p/vzwI1kDiGrP>。

即使函数参数是 `-1`，其退出状态也将是 `255`，因为 `(-1)%256=255`。之所以发生这种情况，是因为退出代码是一个 8 位数字（`0` 到 `255`）。

### Exit 与延迟函数

关于此函数用法的一个重要注意事项是：**延迟函数不会被执行**。

以下示例将没有任何输出：

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    defer fmt.Println("Hello, playground")
    os.Exit(0)
}
```

完整示例请访问 <https://play.golang.org/p/2zbczc_ckgb>。

### Panic 与退出代码

如果应用程序因未恢复的 panic 而终止，则延迟函数**将会**被执行，但退出代码将是 `2`：

```go
package main

import (
    "fmt"
)

func main() {
    defer fmt.Println("Hello, playground")
    panic("panic")
}
```

完整示例请访问 <https://play.golang.org/p/mjOMb0KsM3e>。

### 退出代码与 Goroutine

如果 `os.Exit` 函数在 goroutine 中被调用，所有 goroutine（包括主 goroutine）将立即终止，且不执行任何延迟调用，如下所示：

```go
package main

import (
    "fmt"
    "os"
    "time"
)

func main() {
    go func() {
        defer fmt.Println("go end (deferred)")
        fmt.Println("go start")
        os.Exit(1)
    }()
    fmt.Println("main end (deferred)")
    fmt.Println("main start")
    time.Sleep(time.Second)
    fmt.Println("main end")
}
```

完整示例请访问 <https://play.golang.org/p/JVEB5MTcEoa>。

您必须谨慎使用 `os.Exit`，因为所有延迟操作都不会被执行，这可能导致资源泄漏或错误，例如未刷新缓冲区、未将所有内容写入文件。

### 读取子进程退出代码

我们在上一章探讨了如何创建子进程。Go 使您能够轻松检查子进程的退出代码，但这并不直接，因为 `exec.Cmd` 结构体中有一个字段拥有 `os.ProcessState` 属性。

`os.ProcessState` 属性有一个 `Sys` 方法，返回一个接口。在 Unix 系统中，其值为 `syscall.WaitStatus` 结构体，这使得可以使用 `ExitCode` 方法访问退出代码。以下代码演示了这一点：

```go
package main

import (
    "fmt"
    "os"
    "os/exec"
    "syscall"
)

func exitStatus(state *os.ProcessState) int {
    status, ok := state.Sys().(syscall.WaitStatus)
    if !ok {
        return -1
    }
    return status.ExitStatus()
}

func main() {
    cmd := exec.Command("ls", "__a__")
    if err := cmd.Run(); err != nil {
        if status := exitStatus(cmd.ProcessState); status == -1 {
            fmt.Println(err)
        } else {
            fmt.Println("Status:", status)
        }
    }
}
```

如果无法访问命令变量，则返回的错误为 `exec.ExitError`，它包装了 `os.ProcessState` 属性，如下所示：

```go
func processState(e error) *os.ProcessState {
    err, ok := e.(*exec.ExitError)
    if !ok {
        return nil
    }
    return err.ProcessState
}
```

我们可以看到，获取退出代码并不直接，需要一些类型转换。

## 处理信号

信号是 Unix 操作系统提供的另一种进程间通信工具。它们是可以从一个进程发送到另一个进程的整数值，赋予我们的应用程序与父进程以外的更多进程通信的能力。通过这样做，应用程序能够解读传入的信号，也可以向其他进程发送信号。

### 处理传入信号



Go 应用程序的正常行为是通过终止应用程序来处理某些传入信号，包括 `SIGHUP`、`SIGINT` 和 `SIGABRT`。我们可以用自定义行为替换这种标准行为，该行为拦截所有或部分信号并做出相应处理。

# signal 包

自定义行为是通过 `os/signal` 包实现的，该包暴露了必要的函数。

例如，如果应用程序不需要拦截某个信号，`signal.Ignore` 函数允许您将信号添加到忽略列表中。`signal.Ignored` 函数还允许您验证某个信号是否已被忽略。

为了通过通道拦截信号，您可以使用核心函数 `signal.Notify`。这使得指定通道并选择哪些信号应发送到该通道成为可能。然后应用程序可以在任何 goroutine 中使用该通道，通过自定义行为处理信号。请注意，如果未指定信号，则该通道将接收发送给应用程序的所有信号，如下所示：

```
signal.Notify(ch, signalList...)
```

`signal.Stop` 函数用于停止从特定通道接收信号，而 `signal.Reset` 函数停止向所有通道拦截一个或多个信号。为了重置所有信号，无需向 `Reset` 传递任何参数。

# 优雅关闭

当应用程序在终止前等待任务完成并清理其所有资源时，即执行优雅关闭。使用自定义信号处理是一种非常好的实践，因为它给了我们时间来释放仍在打开状态的资源。在关闭之前，我们可以执行退出应用程序前应完成的任何其他任务；例如，保存当前状态。

现在我们了解了退出代码的工作原理，可以介绍 `log` 包了。从现在起，它将用于向标准输出打印语句，而不是使用 `fmt`。这使得执行 `Print` 语句和 `Fatal` 语句成为可能，后者等同于打印并执行 `os.Exit(1)`。`log` 包还允许用户定义日志标志，以在每次打印时输出日期、时间和/或文件/行号。

我们可以从一个非常基础的示例开始，处理所有信号，如下所示：

```
package main

import (
    "log"
    "os"
    "os/signal"
    "syscall"
)

func main() {
    log.Println("Start application...")
    c := make(chan os.Signal)
    signal.Notify(c)
    s := <-c
    log.Println("Exit with signal:", s)
}
```

为了测试此应用程序，您可以使用两个不同的终端。首先，您可以在第一个终端中启动应用程序，并使用另一个终端执行 `ps` 命令以查找应用程序的 PID，以便使用 `kill` 命令向其发送信号。

第二种方法仅使用一个终端，在后台启动应用程序。这将在屏幕上显示 PID，并将在 `kill` 命令中使用，如下所示：

```
$ go build -o "signal" ch8/signal/base/base.go

$ ./signal &
[1] 265
[Log] Start application...

$ kill -6 265
[Log] Exit with signal: aborted
```

请注意，如果您使用的是 macOS，您将获得 `abort trap` 信号名称。

# 退出清理与资源释放

整洁关闭的一个更实用且常见的示例是资源清理。当使用退出语句时，`bufio.Writer` 结构体的 `Flush` 等延迟函数不会被执行。这可能导致信息丢失，如下例所示：

```
package main

import (
    "bufio"
    "fmt"
    "log"
    "os"
    "time"
)

func main() {
    f, err := os.OpenFile("file.txt", os.O_CREATE|os.O_TRUNC|os.O_WRONLY, 0644)
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()
    w := bufio.NewWriter(f)
    defer w.Flush()
    for i := 0; i < 3; i++ {
        fmt.Fprintln(w, "hello")
        log.Println(i)
        time.Sleep(time.Second)
    }
}
```

如果在该应用程序完成之前发送 `TERM` 信号，文件将被创建并截断，但 flush 永远不会执行——导致文件为空。

这或许可能是预期行为，但这种情况很少见。最好在信号处理部分进行任何清理，如下例所示：

```
func main() {
    c := make(chan os.Signal, syscall.SIGTERM)
    signal.Notify(c)
    f, err := os.OpenFile("file.txt", os.O_CREATE|os.O_TRUNC|os.O_WRONLY, 0644)
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()
    w := bufio.NewWriter(f)
    go func() {
        <-c
        w.Flush()
        os.Exit(0)
    }()
    for i := 0; i < 3; i++ {
        fmt.Fprintln(w, "hello")
        log.Println(i)
        time.Sleep(time.Second)
    }
}
```

在这种情况下，我们结合使用 goroutine 和信号通道，在退出前刷新 writer。这将确保写入缓冲区的任何内容都持久化到文件中。

# 配置重载

信号的用途不仅限于终止应用程序。应用程序可以对每个信号做出不同的反应，从而可用于实现不同的功能，使得控制应用程序流程成为可能。

下一个示例将在文本文件中存储一些设置。设置将为 `time.Duration` 类型，存储为其字符串版本。持续时间是一个 `int64` 值，其字符串版本采用人类可读的格式，例如 `2m10s`，并且拥有许多有用的方法。这用于 `time` 包的不同函数中。

应用程序将执行某项操作，其频率取决于当前的设置值。信号可能的操作包括以下内容：

*   `SIGHUP (1)`：从设置文件加载间隔。
*   `SIGTERM (2)`：保存当前间隔值，并退出应用程序。
*   `SIGQUIT (6)`：不保存直接退出。
*   `SIGUSR1 (10)`：将间隔加倍（增加）。
*   `SIGUSR2 (11)`：将间隔减半（减少）。
*   `SIGALRM (14)`：保存当前间隔值。

通过 `signal.Notify` 函数捕获这些信号，该函数针对所有不同的信号被调用。从通道接收的值需要一个条件语句，即类型开关，以允许应用程序根据值执行不同的操作：

```
func main() {
    c := make(chan os.Signal, 1)
    d := time.Second * 4
    signal.Notify(c,
        syscall.SIGHUP, syscall.SIGINT, syscall.SIGQUIT,
        syscall.SIGUSR1, syscall.SIGUSR2, syscall.SIGALRM)
    // initial load
    if err := handleSignal(syscall.SIGHUP, &d); err != nil && 
        !os.IsNotExist(err) {
            log.Fatal(err)
    }

    for {
        select {
        case s := <-c:
            if err := handleSignal(s, &d); err != nil {
                log.Printf("Error handling %s: %s", s, err)
                continue
            }
        default:
            time.Sleep(d)
            log.Println("After", d, "Executing action!")
        }
    }
}
```

`handleSignal` 函数将包含针对信号的 `switch` 语句：

```
func handleSignal(s os.Signal, d *time.Duration) error {
    switch s {
    case syscall.SIGHUP:
        return loadSettings(d)
    case syscall.SIGALRM:
        return saveSettings(d)
    case syscall.SIGINT:
        if err := saveSettings(d); err != nil {
            log.Println("Cannot save:", err)
            os.Exit(1)
        }
        fallthrough
    case syscall.SIGQUIT:
        os.Exit(0)
    case syscall.SIGUSR1:
        changeSettings(d, (*d)*2)
        return nil
    case syscall.SIGUSR2:
        changeSettings(d, (*d)/2)
        return nil
    }
    return nil
}
```

以下描述了将在信号处理函数中实现的不同行为：

*   更改值将仅使用持续时间指针来存储新值。




## 加载和保存设置

加载将尝试将文件内容（如果存在）扫描为持续时间并更改设置值。

保存会将持续时间写入文件，并使用其字符串格式。以下代码演示了这一点：

```go
func changeSettings(d *time.Duration, v time.Duration) {
    *d = v
    log.Println("Changed", v)
}

func loadSettings(d *time.Duration) error {
    b, err := ioutil.ReadFile(cfgPath)
    if err != nil {
        return err
    }
    var v time.Duration
    if v, err = time.ParseDuration(string(b)); err != nil {
        return err
    }
    *d = v
    log.Println("Loaded", v)
    return nil
}

func saveSettings(d *time.Duration) error {
    f, err := os.OpenFile(cfgPath,
        os.O_WRONLY|os.O_CREATE|os.O_TRUNC, 0644)
    if err != nil {
        return err
    }
    defer f.Close()

    if _, err = fmt.Fprint(f, d); err != nil {
        return err
    }
    log.Println("Saved", *d)
    return nil
}
```

我们将在 `init` 函数中获取用户主目录的路径，并用它来组合 `settings` 文件的路径，如下所示：

```go
var cfgPath string

func init() {
    u, err := user.Current()
    if err != nil {
        log.Fatalln("user:", err)
    }
    cfgPath = filepath.Join(u.HomeDir, ".multi")
}
```

我们可以在一个终端中启动应用程序，并使用另一个终端发送信号，如下所示：

| **终端 1** | **终端 2** |
| --- | --- |
| ```bash\n$ go run ch08/signal/multi/multi.go\nLoaded 1s\nAfter 1s Executing action!\n\nChanged 2s\nAfter 2s Executing action!\n\nChanged 4s\nAfter 4s Executing action!\n\nChanged 2s\nAfter 2s Executing action!\n\nSaved 1s\n\n$\n``` | ```bash\n$ kill -SIGUSR1 $(pgrep multi)\n\n$ kill -SIGUSR1 $(pgrep multi)\n\n$ kill -SIGUSR2 $(pgrep multi)\n\n$ kill -SIGINT $(pgrep multi)\n``` |

在左栏中，我们可以看到应用程序的输出；在右栏中，我们可以看到触发它们的命令。为了获取正在运行的应用程序的 PID，我们使用 `pgrep` 命令并将其嵌套在 `kill` 中。

## 向其他进程发送信号

在了解了处理传入信号的方式后，让我们看看如何以编程方式向其他进程发送信号。`os.Process` 结构是我们所需的唯一工具——其 `Signal` 方法使得向进程发送信号成为可能。就这么简单！

不那么简单的部分是获取进程。有两种用例，如下所示：

*   该进程是子进程，并且我们已经拥有进程值，这是 `os.StartProcess` 或 `exec.Command` 结构体的结果。
*   该进程已存在但我们没有它，所以我们需要使用其 PID 来搜索它。

第一种用例更简单，因为我们已经将进程作为变量，或作为 `exec.Cmd` 变量的属性，并且可以直接调用该方法。

另一种用例需要使用 `os.FindProcess` 方法按 PID 搜索进程，如下所示：

```go
p, err := os.FindProcess(pid)
if err != nil {
    panic(err)
}
```

一旦我们有了 `os.Process`，就可以使用其 `Signal` 方法向其发送特定信号，如下所示：

```go
if err = p.Signal(syscall.SIGTERM); err != nil {
    panic(err)
}
```

我们将发送给进程的信号类型取决于目标进程和我们想要建议的行为，例如中断或终止。

## 连接流

Go 中的流是一种抽象，它使得将任何类型的通信或数据流视为一系列读取器和写入器成为可能。我们已经了解到流是 Go 的一个重要元素。现在我们将学习如何利用我们已知的输入和输出知识来控制与进程相关的流——标准输入、标准输出和标准错误。

### 管道

管道是以同步方式连接输入和输出的最佳方式之一，允许进程间通信。

#### 匿名管道

使用 Shell 时，不同的命令可以按顺序链接在一起，使一个命令的输出成为下一个命令的输入。例如，考虑以下命令：

```bash
cat book_list.txt | grep "Game" | wc -l
```

这里，我们显示一个文件，使用前一个命令过滤包含特定字符串的行，最后使用过滤后的输出来计算行数。

当进程在应用程序内部创建时，这可以在 Go 中以编程方式完成。

`io.Pipe` 函数返回一对已连接的读取器/写入器；写入管道写入器的任何内容都将被管道读取器读取。写入操作是阻塞的，这意味着在执行新的写入操作之前，所有写入的数据都必须被读取。

我们已经看到 `exec.Cmd` 允许其输出和输入使用通用流——这使我们能够使用 `io.Pipe` 函数返回的值将一个进程连接到另一个进程。

首先，我们定义三个命令，如下所示：

*   索引为 `0` 的 `cat`
*   索引为 `1` 的 `grep`
*   索引为 `2` 的 `wc`

然后，我们可以定义所需的两个管道，如下面的代码所示：

```go
r1, w1 := io.Pipe()
r2, w2 := io.Pipe()

var cmds = []*exec.Cmd{
   exec.Command("cat", "book_list.txt"),
   exec.Command("grep", "Game"),
   exec.Command("wc", "-l"),
}
```

接下来，我们链接输入和输出流。我们将 `cat`（命令 `0`）的输出和 `grep`（命令 `1`）的输入连接起来，对 `grep` 的输出和 `wc` 的输入做同样的操作：

```go
cmds[1].Stdin, cmds[0].Stdout = r1, w1
cmds[2].Stdin, cmds[1].Stdout = r2, w2
cmds[2].Stdout = os.Stdout
```

然后我们启动命令，如下所示：

```go
for i := range cmds {
    if err := cmds[i].Start(); err != nil {
        log.Fatalln("Start", i, err)
    }
}
```

我们等待每个命令执行结束并关闭相应的管道写入器；否则，下一个命令的读取器将挂起。为了方便起见，每个管道写入器都是切片中的一个元素，并且每个写入器的索引与其链接的命令相同。最后一个是 `nil`，因为最后一个命令没有通过管道链接：

```go
for i, closer := range []io.Closer{w1, w2, nil} {
    if err := cmds[i].Wait(); err != nil {
        log.Fatalln("Wait", i, err)
    }
    if closer == nil {
        continue
    }
    if err := closer.Close(); err != nil {
        log.Fatalln("Close", i, err)
    }
}
```

`io` 包还提供了其他工具，可以帮助简化某些操作。

#### 标准输入和输出管道

`io.MultiWriter` 函数使得将相同内容写入多个读取器成为可能。当命令的输出需要自动广播到一系列不同的命令时，这将非常有用。

假设我们想做之前做的事（即在文件中查找一个词），但针对不同的词。我们可以使用 `MultiWriter` 函数将输出复制到一系列 `grep` 命令，每个命令都将连接到其自己的 `wc` 命令。

我们将在示例中使用 `exec.Command` 的两个辅助方法：

*   `Cmd.StdinPipe`：这返回一个将连接到命令标准输入的 `PipeWriter` 结构体。
*   `Cmd.StdoutPipe`：这返回一个将连接到命令标准输出的 `PipeReader` 结构体。

让我们首先定义一个搜索词列表：一个用于命令元组（`grep` 和 `wc`），一个用于连接到第一个命令的写入器，一个用于每个命令链的最终输出：

```go
var (
    words = []string{"Game", "Feast", "Dragons", "of"}
    cmds = make([][2]*exec.Cmd, len(words))
    writers = make([]io.Writer, len(words))
    buffers = make([]bytes.Buffer, len(words))
    err error
)
```

现在让我们定义命令及其连接——每个 `grep` 命令的一侧将使用带有管道写入器的 `MultiWriter` 函数连接到 `cat`，另一侧连接到 `wc` 命令的输入：




```
for i := range words {
    cmds[i][0] = exec.Command("grep", words[i])
    if writers[i], err = cmds[i][0].StdinPipe(); err != nil {
        log.Fatal("in pipe", i, err)
    }
    cmds[i][1] = exec.Command("wc", "-l")
    if cmds[i][1].Stdin, err = cmds[i][0].StdoutPipe(); err != nil {
        log.Fatal("in pipe", i, err)
    }
    cmds[i][1].Stdout = &buffers[i]
}

cat := exec.Command("cat", "book_list.txt")
cat.Stdout = io.MultiWriter(writers...)
```

我们可以运行主 `cat` 命令，当它完成后，我们可以关闭第一组写入管道，以便 `grep` 命令能够终止，如下所示：

```
for i := range cmds {
    if err := writers[i].(io.Closer).Close(); err != nil {
        log.Fatalln("close 0", i, err)
    }
}

for i := range cmds {
    if err := cmds[i][0].Wait(); err != nil {
        log.Fatalln("grep wait", i, err)
    }
}
```

然后我们可以等待另一个命令完成并显示结果，如下所示：

```
for i := range cmds {
    if err := cmds[i][1].Wait(); err != nil {
        log.Fatalln("wc wait", i, err)
    }
    count := bytes.TrimSpace(buffers[i].Bytes())
    log.Printf("%10q %s entries", cmds[i][0].Args[1], count)
}
```

请注意，当使用 `StdinPipe` 方法时，生成的 writer 必须被关闭，但使用 `StdoutPipe` 方法时则不需要。

# 总结

在本章中，我们学习了如何使用三大核心功能处理进程间通信：退出码、信号和管道。

退出码是进程返回给其父进程的 `0` 到 `255` 之间的 8 位值。退出码为 `0` 表示应用程序执行成功。在 Go 中返回退出码非常容易，但使用 `os.Exit` 函数会跳过延迟函数的执行。当发生 panic 时，所有延迟函数都会被执行，并返回代码 `2`。从子进程获取退出码相对棘手，因为它依赖于操作系统；不过，在 Unix 系统中，可以通过一系列类型断言来实现。

信号用于与任意进程通信。它们是 1 到 64 之间的 6 位值，通过系统调用从一个进程发送到另一个进程。接收信号可以使用通道和 `signal.Notify` 函数来完成。发送信号则可以通过 `Process.Signal` 方法轻松实现。

管道是一对同步连接的输入和输出流元组。它们用于将一个进程的输入连接到另一个进程的输出。我们了解了如何以终端相同的方式连接多个命令，并学习了如何使用 `io.MultiReader` 将一个命令的输出广播给多个命令。

在下一章中，我们将深入网络编程，从 TCP 一直到 HTTP 服务器。

# 问题

1. 什么是退出码？谁会使用它？

2. 当应用程序发生 panic 时会发生什么？会返回哪个退出码？

3. Go 应用程序接收所有信号时的默认行为是什么？

4. 如何拦截信号并决定应用程序必须如何表现？

5. 可以向其他进程发送信号吗？如果可以，如何发送？

6. 什么是管道？为什么它们很重要？
