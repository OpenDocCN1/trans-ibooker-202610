## 处理进程和守护进程

本章将介绍如何使用 Go 标准库处理当前进程的属性，以及如何更改它们。我们还将重点介绍如何创建子进程，并概述 `os/exec` 包。

最后，我们将解释什么是守护进程，它们具有哪些属性，以及如何使用标准库创建它们。

本章将涵盖以下主题：

*   理解进程
*   子进程
*   守护进程入门
*   创建服务

### 技术要求

本章要求安装 Go 并设置好你喜欢的编辑器。有关更多信息，请参阅 第 3 章 *Go 概览*。

### 理解进程

我们已经了解了进程在 Unix 操作系统中的重要性，现在我们将研究如何获取当前进程的信息，以及如何创建和处理子进程。

#### 当前进程

Go 标准库允许我们获取当前进程的信息。这是通过使用 `os` 包中可用的一系列函数来完成的。

#### 标准输入

程序可能想知道的前两件事是其标识符和父标识符，即 PID 和 PPID。这实际上非常简单——`os.Getpid()` 和 `os.Getppid()` 函数都返回一个整数值，包含这两个标识符，如下面的代码所示：

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    fmt.Println("Current PID:", os.Getpid())
    fmt.Println("Current Parent PID:", os.Getppid())
}
```

完整示例请访问 <https://play.golang.org/p/ng0m9y4LcD5>。

#### 用户和组 ID

另一个有用的信息是当前用户以及进程所属的组。一个典型的用例可能是将它们与特定文件的权限进行比较。

`os` 包提供了以下函数：

*   `os.Getuid()`：返回进程所有者的用户 ID
*   `os.Getgid()`：返回进程所有者的组 ID
*   `os.Getgroups()`：返回进程所有者的附加组 ID

我们可以看到这三个函数都以数字形式返回 ID：

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    fmt.Println("User ID:", os.Getuid())
    fmt.Println("Group ID:", os.Getgid())
    groups, err := os.Getgroups()
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println("Group IDs:", groups)
}
```

完整示例请访问 <https://play.golang.org/p/EqmonEEc_ZI>。




## 用户和组查找

为了获取用户和组的名称，`os/user` 包中提供了一些辅助函数。这些函数（名字非常直观）如下：

*   `func LookupGroupId(gid string) (*Group, error)`
*   `func LookupId(uid string) (*User, error)`

即使用户 ID 是整数，它也接受字符串作为参数，因此需要进行转换。最简单的方法是使用 `strconv` 包，它提供了一系列实用程序，用于将字符串转换为其他基本数据类型，反之亦然。

我们可以在以下示例中看到它们的实际应用：

```go
package main

import (
    "fmt"
    "os"
    "os/user"
    "strconv"
)

func main() {
    uid := os.Getuid()
    u, err := user.LookupId(strconv.Itoa(uid))
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    fmt.Printf("User: %s (uid %d)\n", u.Username, uid)
    gid := os.Getgid()
    group, err := user.LookupGroupId(strconv.Itoa(gid))
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    fmt.Printf("Group: %s (uid %d)\n", group.Name, uid)
}
```

完整示例请访问 [`play.golang.org/p/C6EWF2c50DT`](https://play.golang.org/p/C6EWF2c50DT)。

# 工作目录

进程可以提供给我们的另一个非常有用的信息是工作目录，以便我们可以更改它。在第 4 章 Working with the Filesystem *使用文件系统* 中，我们了解了可以使用哪些工具 —— `os.Getwd` 和 `os.Chdir`。

在下面的实践示例中，我们将探讨如何使用这些函数来操作工作目录：

1.  首先，我们将获取当前工作目录并用它来获取二进制文件的路径。
2.  然后，我们将把工作目录与另一个路径连接起来，并用它来创建一个目录。
3.  最后，我们将使用刚才创建的目录的路径来更改当前工作目录。

请查看以下代码：

```go
// obtain working directory
wd, err := os.Getwd()
if err != nil {
    fmt.Println("Error:", err)
    return
}
fmt.Println("Working Directory:", wd)
fmt.Println("Application:", filepath.Join(wd, os.Args[0]))

// create a new directory
d := filepath.Join(wd, "test")
if err := os.Mkdir(d, 0755); err != nil {
    fmt.Println("Error:", err)
    return
}
fmt.Println("Created", d)

// change the current directory
if err := os.Chdir(d); err != nil {
    fmt.Println("Error:", err)
    return
}
fmt.Println("New Working Directory:", d)
```

完整示例请访问 [`play.golang.org/p/UXAer5nGBtm`](https://play.golang.org/p/UXAer5nGBtm)。

# 子进程

Go 应用程序可以与操作系统交互以创建其他进程。`os` 的另一个子包提供了创建和运行新进程的功能。在 `os/exec` 包中，有一个 `Cmd` 类型，它代表命令执行：

```go
type Cmd struct {
    Path         string        // 要运行的命令。
    Args         []string      // 命令行参数（包括命令本身）
    Env          []string      // 进程的环境变量
    Dir          string        // 工作目录
    Stdin        io.Reader     // 标准输入
    Stdout       io.Writer     // 标准输出
    Stderr       io.Writer     // 标准错误
    ExtraFiles   []*os.File    // 额外的打开文件
    SysProcAttr  *syscall.SysProcAttr // 特定于操作系统的属性
    Process      *os.Process   // 底层进程
    ProcessState *os.ProcessState // 已退出进程的信息
}
```

创建新命令最简单的方法是使用 `exec.Command` 函数，它接受可执行文件路径和一系列参数。让我们看一个带有 `echo` 命令和一些参数的简单示例：

```go
package main

import (
    "fmt"
    "os/exec"
)

func main() {
    cmd := exec.Command("echo", "A", "sample", "command")
    fmt.Println(cmd.Path, cmd.Args[1:]) // echo [A sample command]
}
```

完整示例请访问 [`play.golang.org/p/dBIAUteJbxI`](https://play.golang.org/p/dBIAUteJbxI)。

一个非常重要的细节是标准输入、输出和错误的本质 —— 它们都是我们已经熟悉的接口：

*   输入是一个 `io.Reader`，可以是 `bytes.Reader`、`bytes.Buffer`、`strings.Reader`、`os.File` 或任何其他实现。
*   输出和错误是 `io.Writer`，也可以是 `os.File` 或 `bytes.Buffer`，还可以是 `strings.Builder` 或任何其他 writer 实现。

根据父应用程序的需求，有不同的方式来启动进程：

*   `Cmd.Run`：执行命令，如果子进程执行正确则返回 `nil` 错误。
*   `Cmd.Start`：异步执行命令并让父进程继续其流程。为了等待子进程完成执行，还有另一个方法 `Cmd.Wait`。
*   `Cmd.Output`：执行命令并返回其标准输出，如果 `Stderr` 未定义但标准错误产生了输出，则返回错误。
*   `Cmd.CombinedOutput`：执行命令并返回组合的标准错误和输出，当需要检查或保存子进程的全部输出（标准输出加标准错误）时非常有用。

# 访问子进程属性

一旦命令开始执行（无论是同步还是异步），底层的 `os.Process` 就会被填充，并且可以看到它的 PID，如下例所示：

```go
package main

import (
    "fmt"
    "os/exec"
)

func main() {
    cmd := exec.Command("ls", "-l")
    if err := cmd.Start(); err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println("Cmd: ", cmd.Args[0])
    fmt.Println("Args:", cmd.Args[1:])
    fmt.Println("PID: ", cmd.Process.Pid)
    cmd.Wait()
}
```

# 标准输入

标准输入可用于将一些数据从应用程序发送到子进程。可以使用缓冲区来存储数据并让命令读取它，如下例所示：

```go
package main

import (
    "bytes"
    "fmt"
    "os"
    "os/exec"
)

func main() {
    b := bytes.NewBuffer(nil)
    cmd := exec.Command("cat")
    cmd.Stdin = b
    cmd.Stdout = os.Stdout
    fmt.Fprintf(b, "Hello World! I'm using this memory address: %p", b)
    if err := cmd.Start(); err != nil {
        fmt.Println(err)
        return
    }
    cmd.Wait()
}
```

# 守护进程入门

在 Unix 中，所有在后台运行的程序都称为 **守护进程**。它们通常有一个以字母 *d* 结尾的名称，如 `sshd` 或 `syslogd`，并且提供了操作系统的许多功能。

# 操作系统支持

在 macOS、Unix 和 Linux 中，如果一个进程在其父进程的生命周期结束后仍然存活，即当父进程终止执行而子进程继续存活时，该进程就是一个守护进程。这是因为进程的父进程被更改为 `init` 进程，一个没有父进程的特殊守护进程，PID 为 1，它随操作系统启动和终止。在进一步深入之前，让我们介绍两个非常重要的概念 —— *会话* 和 *进程组*：

*   进程组是一组共享信号处理的进程的集合。组中的第一个进程称为 **组长**。有一个 Unix 系统调用 `setpgid`，能够更改进程的组，但它有一些限制。进程可以更改自己的进程组，或者在对其执行 `exec` 系统调用之前更改其子进程的组。当进程组更改时，会话组也需要相应地更改，目标组的组长也是如此。





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





# 使用 init.d 实现守护进程

```go
const initdFile = "/etc/init.d/mydaemon"

func installApp() error {
    _, err := os.Stat(initdFile)
    if err == nil {
        return errors.New("Already installed")
    }
    f, err := os.OpenFile(initdFile, os.O_CREATE|os.O_WRONLY, 0755)
    if err != nil {
        if !os.IsPermission(err) {
            return err
        }
        return ErrSudo
    }
    defer f.Close()
    if _, err = fmt.Fprintf(f, "#!/bin/sh\n\"%s\" $1", bin); err != nil {
        return err
    }
    fmt.Println("Daemon", bin, "installed")
    return nil
}

func uninstallApp() error {
    _, err := os.Stat(initdFile)
    if err != nil && os.IsNotExist(err) {
        return errors.New("not installed")
    }
    if err = os.Remove(initdFile); err != nil {
        if err != nil {
            if !os.IsPermission(err) {
                return err
            }
            return ErrSudo
        }
    }
    fmt.Println("Daemon", bin, "removed")
    return err
}
```

文件创建后，我们可以使用 `mydaemon install` 命令将应用安装为服务，并使用 `mydaemon uninstall` 将其移除。

守护进程安装完成后，我们可以使用 `sudo service mydaemon [start|stop|status]` 来控制守护进程。现在，我们只需实现这些操作即可：

*   `status` 将查找 `pid` 文件，读取它，并向进程发送信号以检查其是否正在运行。
*   `start` 将使用 `run` 命令运行应用程序并写入 `pid` 文件。
*   `stop` 将获取 `pid` 文件，找到进程，终止它，然后移除 `pid` 文件。

## Status 命令实现

让我们看看 `status` 命令是如何实现的。请注意，`0` 信号在 Unix 中不存在，不会触发操作系统或应用程序的任何动作，但如果进程未运行，该操作将会失败。这告诉我们进程是否存活：

```go
func statusApp() (err error) {
    var pid int
    defer func() {
        if pid == 0 {
            fmt.Println("status: not active")
            return
        }
        fmt.Println("status: active - pid", pid)
    }()
    pid, err = getPid()
    if err != nil {
        if os.IsNotExist(err) {
            return nil
        }
        return err
    }
    p, err := os.FindProcess(pid)
    if err != nil {
        return nil
    }
    if err = p.Signal(syscall.Signal(0)); err != nil {
        fmt.Println(pid, "not found - removing PID file...")
        os.Remove(filepath.Join(varDir, pidFile))
        pid = 0
    }
    return nil
}
```

## Start 命令实现

在 `start` 命令中，我们将按照*操作系统支持*部分介绍的步骤创建守护进程：

1.  使用文件作为标准输出和输入
2.  将工作目录设置为根目录
3.  异步启动命令

除了这些操作外，`start` 命令还会将进程的 PID 值保存在特定文件中，该文件将用于查看进程是否存活：

```go
func startApp() (err error) {
    const perm = os.O_CREATE | os.O_APPEND | os.O_WRONLY
    if err = os.MkdirAll(varDir, 0755); err != nil {
        if !os.IsPermission(err) {
            return err
        }
        return ErrSudo
    }
    cmd := exec.Command(bin, "run")
    cmd.Stdout, err = os.OpenFile(filepath.Join(varDir, outFile), perm, 0644)
    if err != nil {
        return err
    }
    cmd.Stderr, err = os.OpenFile(filepath.Join(varDir, errFile), perm, 0644)
    if err != nil {
        return err
    }
    cmd.Dir = "/"
    if err = cmd.Start(); err != nil {
        return err
    }
    if err := writePid(cmd.Process.Pid); err != nil {
        if err := cmd.Process.Kill(); err != nil {
            fmt.Println("Cannot kill process", cmd.Process.Pid, err)
        }
        return err
    }
    fmt.Println("Started with PID", cmd.Process.Pid)
    return nil
}
```

## Stop 命令实现

最后，如果 PID 文件存在，`stopApp` 将终止由该文件标识的进程：

```go
func stopApp() (err error) {
    pid, err := getPid()
    if err != nil {
        if os.IsNotExist(err) {
            return nil
        }
        return err
    }
    p, err := os.FindProcess(pid)
    if err != nil {
        return nil
    }
    if err = p.Signal(os.Kill); err != nil {
        return err
    }
    if err := os.Remove(filepath.Join(varDir, pidFile)); err != nil {
        return err
    }
    fmt.Println("Stopped PID", pid)
    return nil
}
```

## 主应用循环

现在，应用程序控制所需的所有部分都已就绪，唯一缺少的就是主应用程序部分，它应该是一个循环，以便守护进程保持存活：

```go
func runApp() error {
    fmt.Println("RUN")
    for {
        time.Sleep(time.Second)
    }
    return nil
}
```

在本例中，它只是在循环迭代之间休眠固定的时间。这在主循环中通常是个好主意，因为空的 `for` 循环会无故消耗大量资源。假设你的应用程序在 `for` 循环中检查某个条件。如果满足该条件，持续检查将消耗大量资源。增加几毫秒的空闲休眠有助于将空闲 CPU 消耗减少 90-95%，因此在设计守护进程时请记住这一点！

# 第三方包

到目前为止，我们已经了解了如何使用 `init.d` 服务从头实现守护进程。我们的实现非常简单且有限。虽然可以改进，但已经有许多包提供相同的功能。它们支持不同的提供者，如 `init.d` 和 `systemd`，其中一些也适用于非 Unix 操作系统，如 Windows。

较著名的包之一（GitHub 上超过 1,000 颗星）是 `kardianos/service`，它支持所有主流平台——Linux、macOS 和 Windows。

它定义了一个代表守护进程的主接口，并拥有两个方法——一个用于启动守护进程，另一个用于停止它。两者都是非阻塞的：

```go
type Interface interface {
    // Start 提供了一个启动服务的位置。服务直到此函数返回后才会发出启动完成的信号，因此
    // Start 函数最多不应超过几秒钟。
    Start(s Service) error

    // Stop 提供了一个在程序终止前清理执行环境的位置。
    // 执行时间不应超过几秒钟。
    // Stop 不应在函数中直接调用 os.Exit。
    Stop(s Service) error
}
```

该包在示例目录（[`github.com/kardianos/service/tree/master/example`](https://github.com/kardianos/service/tree/master/example)）中已经提供了一些用例，从简单到更复杂的都有。最佳实践是启动一个带有主活动循环的 goroutine。`Start` 方法可用于打开和准备必要的资源，而 `Stop` 应用于释放它们，以及执行其他延迟活动，如缓冲区刷新。

一些其他包仅提供与 Unix 系统的兼容性，例如 `takama/daemon`（[`github.com/takama/daemon`](https://github.com/takama/daemon)），其工作方式类似。它也提供了一些使用示例。

# 总结

在本章中，我们回顾了如何获取与当前进程相关的信息，包括 PID 和 PPID、UID 和 GID 以及工作目录。然后，我们了解了 `os/exec` 包如何允许我们创建子进程，以及如何像读取当前进程一样读取它们的属性。

接下来，我们探讨了什么是守护进程以及各种操作系统如何支持它们。我们验证了使用 `os/exec` 的 `Cmd.Run` 执行一个比其父进程存活时间更长的进程是多么简单。

然后，我们介绍了 Unix 可用的自动化守护进程管理系统，并逐步创建了一个能够与 `service` 配合运行的应用程序。




在下一章中，我们将通过查看如何使用退出代码以及如何管理和发送信号，来改进我们对子进程的控制。

## 问题

1.  Go 应用程序中，当前进程有哪些可用信息？
2.  如何创建子进程？
3.  如何确保子进程在其父进程终止后仍然存活？
4.  能否访问子进程属性？如何使用它们？
5.  Linux 中什么是守护进程？它们是如何被处理的？
