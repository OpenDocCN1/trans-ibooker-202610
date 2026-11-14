

会话是一组进程组的集合，它允许我们对进程组和其他操作施加一系列限制。会话不允许进程组迁移到另一个会话，并阻止进程在不同会话中创建进程组。如果进程不是进程组领导者，`setsid` 系统调用允许我们将进程会话更改为新会话。此外，第一个进程组 ID 设置会话 ID。如果此 ID 与正在运行的进程的 ID 相同，则该进程被称为 `session leader`（会话领导者）。

既然我们已经解释了这两个属性，就可以看看创建守护进程所需的标准操作，通常如下：

*   清理环境以移除不必要的变量。
*   创建一个分叉，以便主进程可以正常终止进程。
*   使用 `setsid` 系统调用，它完成以下三个步骤：
    1.  从分叉进程中移除 `PPID`，使其被 `init` 进程收养。
    2.  为分叉创建一个新会话，该分叉将成为 `session leader`（会话领导者）。
    3.  将进程设置为组领导者。
*   将分叉的当前目录设置为根目录，以避免占用其他目录，并关闭父进程打开的所有文件（子进程在需要时会打开它们）。
*   将标准输入设置为 `/dev/null`，并使用一些日志文件作为标准输出和错误输出。
*   可选地，可以再次分叉，然后退出。第一个分叉将是组领导者，第二个分叉将拥有相同的组，从而允许我们拥有一个不是组领导者的分叉。

这适用于基于 Unix 的操作系统，尽管 Windows 也支持永久后台进程，称为 `services`（服务）。服务可以在启动时自动启动，或使用称为 `Service Control Manager`（`SCM`，服务控制管理器）的可视化应用程序手动启动和停止。它们也可以从命令行控制，在常规提示符下使用 `sc` 命令，以及在 PowerShell 中通过 `Start-Service` 和 `Stop-Service` cmdlet。

# 守护进程实战

现在我们了解了什么是守护进程以及它是如何工作的，我们可以尝试使用 Go 标准库来创建一个。Go 应用程序是多线程的，不允许我们直接调用 `fork` 系统调用。

我们已经了解到，`os/exec` 包中的 `Cmd.Start` 方法允许我们异步启动一个进程。第二步是使用 `release` 方法关闭当前进程的所有资源。

以下示例展示了如何做到这一点：

```
package main

import (
    "fmt"
    "os"
    "os/exec"
    "time"
)

var pid = os.Getpid()

func main() {
    fmt.Printf("[%d] Start\n", pid)
    fmt.Printf("[%d] PPID: %d\n", pid, os.Getppid())
    defer fmt.Printf("[%d] Exit\n\n", pid)
    if len(os.Args) != 1 {
        runDaemon()
        return
    }
    if err := forkProcess(); err != nil {
        fmt.Printf("[%d] Fork error: %s\n", pid, err)
        return
    }
    if err := releaseResources(); err != nil {
        fmt.Printf("[%d] Release error: %s\n", pid, err)
        return
    }
}
```

让我们看看 `forkProcess` 函数的作用，创建另一个进程并启动它：

1.  首先，进程工作目录被设置为根目录，输出和错误流被设置为标准流：

```
func forkProcess() error {
    cmd := exec.Command(os.Args[0], "daemon")
    cmd.Stdout, cmd.Stderr, cmd.Dir = os.Stdout, os.Stderr, "/"
    return cmd.Start()
}
```

2.  然后，我们可以释放资源——但首先，我们需要找到当前进程。然后，我们可以调用 `os.Process` 方法 `Release`，以确保主进程释放其资源：

```
func releaseResources() error {
    p, err := os.FindProcess(pid)
    if err != nil {
        return err
    }
    return p.Release()
}
```

3.  `main` 函数将包含守护进程逻辑，在本例中非常简单——它只是每隔几秒打印一次正在运行的内容：

```
func runDaemon() {
    for {
        fmt.Printf("[%d] Daemon mode\n", pid)
        time.Sleep(time.Second * 10)
    }
}
```

# 服务

我们已经看到，从启动到操作系统关闭存活的第一个进程被称为 `init` 或 `init.d`，因为它是一个守护进程。该进程负责处理其他守护进程，并将其配置存储在 `/etc/init.d` 目录中。

每个 Linux 发行版使用自己的守护进程控制进程版本，如 Chrome OS 的 `upstart` 或 Arch Linux 的 `systemd`。它们都服务于相同的目的，并具有相似的行为。

每个守护进程都有一个控制脚本或应用程序，位于 `/etc/init.d` 内部，并且应该能够将一系列命令作为第一个参数进行解释，例如 `status`、`start`、`stop` 和 `restart`。在大多数情况下，`init.d` 文件是一个脚本，它对参数执行 switch 操作并相应地表现。

# 创建服务

一些应用程序能够自动处理其服务文件，这正是我们将尝试逐步实现的目标。让我们从一个 `init.d` 脚本开始：

```
#!/bin/sh

"/path/to/mydaemon" $1
```

这是一个示例脚本，将第一个参数传递给守护进程。二进制文件的路径将取决于文件所在的位置。这需要在运行时定义：

```
// ErrSudo 是一个建议以超级用户身份执行命令的错误
// 它将用于因权限而失败的函数
var ErrSudo error

var (
    bin string
    cmd string
)

func init() {
    p, err := filepath.Abs(filepath.Dir(os.Args[0]))
    if err != nil {
        panic(err)
    }
    bin = p
    if len(os.Args) != 1 {
        cmd = os.Args[1]
    }
    ErrSudo = fmt.Errorf("try `sudo %s %s`", bin, cmd)
}
```

`main` 函数将处理不同的命令，如下所示：

```
func main() {
    var err error
    switch cmd {
    case "run":
        err = runApp()
    case "install":
        err = installApp()
    case "uninstall":
        err = uninstallApp()
    case "status":
        err = statusApp()
    case "start":
        err = startApp()
    case "stop":
        err = stopApp()
    default:
        helpApp()
    }
    if err != nil {
        fmt.Println(cmd, "error:", err)
    }
}
```

我们如何确保我们的应用正在运行？一个非常可靠的策略是使用 `PID` 文件，这是一个包含正在运行的进程的当前 PID 的文本文件。让我们定义几个辅助函数来实现这一点：

```
const (
    varDir = "/var/mydaemon/"
    pidFile = "mydaemon.pid"
)

func writePid(pid int) (err error) {
    f, err := os.OpenFile(filepath.Join(varDir, pidFile), os.O_CREATE|os.O_WRONLY, 0644)
    if err != nil {
        return err
    }
    defer f.Close()
    if _, err = fmt.Fprintf(f, "%d", pid); err != nil {
        return err
    }
    return nil
}

func getPid() (pid int, err error) {
    b, err := ioutil.ReadFile(filepath.Join(varDir, pidFile))
    if err != nil {
        return 0, err
    }
    if pid, err = strconv.Atoi(string(b)); err != nil {
        return 0, fmt.Errorf("Invalid PID value: %s", string(b))
    }
    return pid, nil
}
```

`install` 和 `uninstall` 函数将负责添加或移除位于 `/etc/init.d/mydaemon` 的服务文件，并且要求我们以 root 权限启动应用程序，因为文件位置的原因。

