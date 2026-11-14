
有些全局变量可以存储在 `context` 中，以便它们能在一系列函数调用中传递。这通常不是好的做法，因为全局变量在应用程序的每个点都可用，所以使用 `context` 来存储和回调它们是毫无意义的，也是对资源和性能的浪费。如果你的包有一些全局变量，你可以使用我们在 `Synchronization with sync and atomic` 中研究的单例模式，以允许从包或应用程序的任何位置访问它们。

# 使用 Context 构建服务

我们现在将重点关注如何创建支持 `context` 用法的包。这将帮助我们整合迄今为止所学的并发知识。我们将尝试创建一个并发文件搜索，它利用通道、Goroutine、同步和 `context`。

## 主接口和用法

包的签名将包含一个 `context`、根文件夹、搜索词以及几个可选参数：

*   **在内容中搜索**：将在文件内容中查找字符串，而不是文件名
*   **排除列表**：将不搜索具有选定名称/名称的文件

该函数看起来会像这样：

```
type Options struct {
    Contents bool
    Exclude []string
}

func FileSearch(ctx context.Context, root, term string, o *Options)
```

由于它应该是一个并发函数，返回类型可以是一个结果通道，该通道可以是错误，也可以是文件中的一系列匹配项。由于我们可以搜索名称或内容，后者可能有不止一个匹配：

```
type Result struct {
    Err error
    File string
    Matches []Match
}

type Match struct {
    Line int
    Text string
}
```

前面的函数将返回一个 `Result` 类型的只接收通道：

```
func FileSearch(ctx context.Context, root, term string, o *Options) <-chan Result
```

在这里，该函数将持续从通道接收值，直到通道关闭：

```
for r := range FileSearch(ctx, directory, searchTerm, options) {
    if r.Err != nil {
        fmt.Printf("%s - error: %s\n", r.File, r.Err)
        continue
    }
    if !options.Contents {
        fmt.Printf("%s - match\n", r.File)
        continue
    }
    fmt.Printf("%s - matches:\n", r.File)
    for _, m := range r.Matches {
        fmt.Printf("\t%d:%s\n", m.Line, m.Text)
    }
}
```

## 退出和入口点

结果通道应由 `context` 的取消或搜索结束来关闭。由于通道不能关闭两次，我们可以使用 `sync.Once` 来避免第二次关闭通道。为了跟踪正在运行的 Goroutine，我们可以使用 `sync.WaitGroup`：

```
ch, wg, once := make(chan Result), sync.WaitGroup{}, sync.Once{}
go func() {
    wg.Wait()
    fmt.Println("* Search done *")
    once.Do(func() {
        close(ch)
    })
}()
go func() {
    <-ctx.Done()
    fmt.Println("* Context done *")
    once.Do(func() {
        close(ch)
    })
}()
```

我们可以为每个文件启动一个 Goroutine，以便我们可以定义一个私有函数作为入口点，然后递归地用于子目录：

```
func fileSearch(ctx context.Context, ch chan<- Result, wg *sync.WaitGroup, file, term string, o *Options)
```

主导出函数将首先向等待组添加一个值。然后它将启动私有函数，将其作为异步进程启动：

```
wg.Add(1)
go fileSearch(ctx, ch, &wg, root, term, o)
```

每个 `fileSearch` 最后要做的事情是调用 `WaitGroup.Done` 以标记当前文件的结束。

## 排除列表

私有函数将在使用 `Done` 方法完成之前减少等待组计数器。除此之外，它首先要做的是检查文件名，以便如果文件名在排除列表中则跳过它：

```
defer wg.Done()
_, name := filepath.Split(file)
if o != nil {
    for _, e := range o.Exclude {
        if e == name {
            return
        }
    }
}
```

如果不是这种情况，我们可以使用 `os.Stat` 检查当前文件的信息，如果不成功则向通道发送错误。由于我们不能冒险向已关闭的通道发送数据导致 panic，我们可以检查 `context` 是否已完成，如果没有，则发送错误：

```
info, err := os.Stat(file)
if err != nil {
    select {
    case <-ctx.Done():
        return
    default:
        ch <- Result{File: file, Err: err}
    }
    return
}
```

## 处理目录

接收到的信息将告诉我们该文件是否为目录。如果是目录，我们可以获取文件列表并处理错误，就像我们之前用 `os.Stat` 做的那样。然后，如果 `context` 尚未完成，我们可以为每个文件启动另一系列搜索。以下代码总结了这些操作：

```
if info.IsDir() {
    files, err := ioutil.ReadDir(file)
    if err != nil {
        select {
        case <-ctx.Done():
            return
        default:
            ch <- Result{File: file, Err: err}
        }
        return
    }
    select {
    case <-ctx.Done():
    default:
        wg.Add(len(files))
        for _, f := range files {
            go fileSearch(ctx, ch, wg, filepath.Join(file, f.Name()), term, o)
        }
    }
    return
}
```

## 检查文件名和内容

如果文件是常规文件而不是目录，我们可以根据指定的选项比较文件名或其内容。检查文件名非常简单：

```
if o == nil || !o.Contents {
    if name == term {
        select {
        case <-ctx.Done():
        default:
            ch <- Result{File: file}
        }
    }
    return
}
```

如果我们要搜索内容，应该打开文件：

```
f, err := os.Open(file)
if err != nil {
    select {
    case <-ctx.Done():
    default:
        ch <- Result{File: file, Err: err}
    }
    return
}
defer f.Close()
```

然后，我们可以逐行读取文件以搜索选定的术语。如果在读取文件时 `context` 过期，我们将停止所有操作：

```
scanner, matches, line := bufio.NewScanner(f), []Match{}, 1
for scanner.Scan() {
    select {
    case <-ctx.Done():
        break
    default:
        if text := scanner.Text(); strings.Contains(text, term) {
            matches = append(matches, Match{Line: line, Text: text})
        }
        line++
    }
}
```

最后，我们可以检查扫描器的错误。如果没有错误且搜索有结果，我们可以将所有匹配项发送到输出通道：

```
select {
case <-ctx.Done():
    break
default:
    if err := scanner.Err(); err != nil {
        ch <- Result{File: file, Err: err}
        return
    }
    if len(matches) != 0 {
        ch <- Result{File: file, Matches: matches}
    }
}
```

在不到 200 行代码中，我们创建了一个并发文件搜索函数，它为每个文件使用一个 Goroutine。它利用通道发送结果，并利用同步原语来协调操作。

## 总结

在本章中，我们了解了较新的包之一 `context` 的全部内容。我们看到 `Context` 是一个有四个方法的简单接口，并且应该用作函数的第一个参数。它的主要作用是处理取消和截止日期，以同步并发操作，并为用户提供取消操作的功能。

我们看到默认的 context `Background` 和 `TODO` 不允许取消，但可以使用包的各种函数来扩展它们，以添加超时或取消功能。我们还讨论了 `context` 在保存值方面的能力，以及如何小心使用它以避免遮蔽和其他问题。
