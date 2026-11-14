

# 构建伪终端

## 问题

5. 字符串构建器（string builder）和字节缓冲区（bytes buffer）有什么区别？

6. 为什么读取器和写入器的实现应该接受接口作为输入？

7. 管道与 `TeeReader` 有何不同？

## 技术要求

本章要求已安装 Go 并设置好你喜欢的编辑器。更多信息请参阅 [第 3 章](#602a92d5-25f7-46b8-83d4-10c6af1c6750.xhtml) *Go 概览*。

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

