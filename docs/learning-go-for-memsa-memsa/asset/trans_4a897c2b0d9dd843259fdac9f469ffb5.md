

```
func TestDoMath(t *testing.T) {
    result, err := DoMath(2, 2, "+")
    if result != 4 {
        t.Error("Should have been 4, got", result)
    }
    if err != nil {
        t.Error("Should have been nil error, got", err)
    }
    result2, err2 := DoMath(2, 2, "-")
    if result2 != 0 {
        t.Error("Should have been 0, got", result2)
    }
    if err2 != nil {
        t.Error("Should have been nil error, got", err2)
    }
    // and so on...
}
```

让我们用表格测试来替换这种重复。首先，声明一个匿名结构体切片。该结构体包含测试名称、输入参数和返回值字段。切片中的每个条目代表另一个测试：

```
data := []struct {
    name     string
    num1     int
    num2     int
    op       string
    expected int
    err      error
}{
    {"addition", 2, 2, "+", 4, nil},
    {"subtraction", 2, 2, "-", 0, nil},
    {"multiplication", 2, 2, "*", 4, nil},
    {"division", 2, 2, "/", 1, nil},
    {"bad_division", 2, 0, "/", 0, ErrDivZero},
}
```

接下来，遍历 `data` 中的每个测试用例，每次调用 `Run` 方法。这就是实现魔法的那一行。你向 `Run` 传递两个参数，一个子测试的名称和一个带有单个 `*testing.T` 类型参数的函数。在函数内部，你使用 `data` 中当前条目的字段调用 `DoMath`，反复使用相同的逻辑。当你运行这些测试时，你会发现它们不仅通过了，而且当你使用 `-v` 标志时，每个子测试现在也有一个名称：

```
for _, d := range data {
    t.Run(d.name, func(t *testing.T) {
        result, err := DoMath(d.num1, d.num2, d.op)
        if result != d.expected {
            t.Errorf("Expected %d, got %d", d.expected, result)
        }
        if !errors.Is(err, d.err) {
            t.Errorf("expected %v, got %v", d.err, err)
        }
    })
}
```

### Tip

在测试中比较错误消息很脆弱，因为消息文本可能没有兼容性保证。如果你正在测试使用 `errors.New` 和 `fmt.Errorf` 创建错误的代码，你有两个选择：重写你的代码以使用自定义错误类型和哨兵错误，或比较消息并祈祷最好。最好重写你的代码。

# Running Tests Concurrently

默认情况下，单元测试是顺序运行的。由于每个单元测试都 supposed to 与其他每个单元测试独立，它们是并发的理想候选者。要使单元测试与其他测试并发运行，请在测试中的第一行对 `*testing.T` 调用 `Parallel` 方法：

```
func TestMyCode(t *testing.T) {
    t.Parallel()
    // rest of test goes here
}
```

标记为并行的测试会与其他标记为并行的测试并发运行。你也可以在表格测试内部调用 `Parallel`：

```
func TestParallelTable(t *testing.T) {
    data := []struct {
        name   string
        input  int
        output int
    }{
        {"a", 10, 20},
        {"b", 30, 40},
        {"c", 50, 60},
    }
    for _, d := range data {
        t.Run(d.name, func(t *testing.T) {
            t.Parallel()
            t.Log(d.input, d.output)
            out := toTest(d.input)
            if out != d.output {
                t.Error("didn't match", out, d.output)
            }
        })
    }
}
```

你可以在 [The Go Playground](https://oreil.ly/b0S4n) 上运行此代码，或在 [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/parallel) 的 `sample_code/parallel` 目录中运行。

并行测试的优点是它们可以加快长时间运行的测试套件的速度。也有一些缺点。如果你有多个依赖相同共享可变状态的测试，不要将它们标记为并行，因为你会得到不一致的结果。 （但在我所有的警告之后，你的应用程序中没有任何共享可变状态，对吧？）另外请注意，如果你将测试标记为并行并在测试函数中使用 `Setenv` 方法，你的测试将 panic。

### Note

Go 1.22（于 2024 年 2 月发布）更改了 `for` 循环中索引和值变量的生命周期。如果索引和值变量有任何可能被闭包捕获的机会，编译器现在会在每次迭代时创建一个新的副本。这消除了使用共享索引和值状态的并行测试中的错误。如果你仍在使用 Go 1.21 或更早版本（或在 `go.mod` 文件中的 `go` 指令中将 Go 版本设置为 1.2.1 或更早版本），我强烈建议升级以利用这种改进的行为。

现在你有了一种运行大量测试的方法，你将学习代码覆盖率以了解你的测试正在测试什么。

# Exercises

这些练习帮助你练习 Go 测试的基础知识。它们位于 [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/exercises) 的 `exercises` 目录中。解决方案位于 [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/exercise-solutions) 的 `exercise-solutions` 目录中。

1. 在 `ex1` [directory](https://github.com/learning-go-book-3e/ch15/tree/main/exercises/ex1) 中，有一个名为 `calculator.go` 的文件，其中有一个名为 `Average` 的函数。创建一个 `calculator_test.go` 文件，其中包含一个用于测试 `Average` 的函数。如果你发现函数中的 bug，请修复 bug 并确认测试通过。

2. 在 `ex2` [directory](https://github.com/learning-go-book-3e/ch15/tree/main/exercises/ex2) 中，有一个名为 `temperature.go` 的文件，其中有一个名为 `Convert` 的函数。创建一个 `temperature_test.go` 文件，其中包含一个使用表格测试来验证所有输入和可能错误的函数。如果你发现函数中的 bug，请修复 bug 并确认测试通过。

3. 在 `ex3` [directory](https://github.com/learning-go-book-3e/ch15/tree/main/exercises/ex3) 中，有一个名为 `counter.go` 的文件，其中有一个名为 `TotalFile` 的函数。创建一个 `counter_test.go` 文件，其中包含一个用于测试 `TotalFile` 的函数。使用辅助函数创建好的和坏的示例数据。

# Checking Your Code Coverage

代码覆盖率是一个非常有用的工具，可以知道你是否遗漏了任何明显的情况。然而，达到 100% 的代码覆盖率并不能保证对于某些输入你的代码中没有 bug。首先你将看到 `go test` 如何显示代码覆盖率，然后你将查看仅依赖代码覆盖率的局限性。

向 `go test` 命令添加 `-cover` 标志会计算覆盖率信息并在测试输出中包含摘要。如果你包含第二个标志 `-coverprofile`，你可以将覆盖率信息保存到文件中。让我们回到 [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/table) 中 `sample_code/table` 目录并收集代码覆盖率信息：

```
$ go test -v -cover -coverprofile=c.out
```

如果你使用代码覆盖率运行表格测试，测试输出现在包含一行指示测试代码覆盖率的金额，87.5%。知道这一点很好，但如果你能看到你遗漏了什么会更有用。Go 随附的 `cover` 工具生成源代码的 HTML 表示，其中包含该信息：

```
$ go tool cover -html=c.out
```

当你运行它时，你的网络浏览器会打开并显示一个页面，其中使用深色模式配色方案显示代码覆盖率信息。每个被测试的文件都会出现在左上角的组合框中。源代码有三种颜色之一。灰色用于不可测试的代码行，绿色用于已被测试覆盖的代码，红色用于未被测试的代码。这种对颜色的依赖对于红绿色盲的人来说是不幸的（以及那些出版商不想打印大块黑色墨水的人）。为了解决这个问题，我创建了一个 [fork of the cover tool](https://github.com/jonbodner/coverlight)。使用以下方法安装并运行它：

