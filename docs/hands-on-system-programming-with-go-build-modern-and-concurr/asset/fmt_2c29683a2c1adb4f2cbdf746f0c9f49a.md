
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
