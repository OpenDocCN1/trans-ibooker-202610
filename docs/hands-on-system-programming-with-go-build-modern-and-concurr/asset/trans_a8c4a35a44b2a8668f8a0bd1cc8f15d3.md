

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

