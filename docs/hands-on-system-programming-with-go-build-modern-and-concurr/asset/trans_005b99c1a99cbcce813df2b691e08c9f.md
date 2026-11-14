
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
