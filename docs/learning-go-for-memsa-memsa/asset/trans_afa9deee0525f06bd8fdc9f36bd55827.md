

如果只想测试包的公共 API，Go 有一种约定来指定这一点。你仍然将测试源代码保存在与生产源代码相同的目录中，但使用 `packagename_test` 作为包名。让我们使用导出的函数重新做初始测试用例。你可以在 [第 15 章仓库](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/pubadder) 的 `sample_code/pubadder` 目录中找到代码。在 `pubadder` 包的 `adder_public.go` 文件中，声明了以下导出函数：

```
func AddNumbers(x, y int) int {
    return x + y
}
```

你通过在 `pubadder` 包中名为 `adder_public_test.go` 的文件中使用以下代码来测试该函数作为公共 API：

```
package pubadder_test

import (
    "github.com/learning-go-book-3e/ch15/sample_code/pubadder"
    "testing"
)

func TestAddNumbers(t *testing.T) {
    result := pubadder.AddNumbers(2, 3)
    if result != 5 {
        t.Error("incorrect result: expected 5, got", result)
    }
}
```

注意测试文件的包名是 `pubadder_test`。即使文件在同一目录下，你也必须导入 `github.com/learning-go-book-3e/ch15/sample_code/pubadder`。为了遵循命名测试的约定，测试函数名与 `AddNumbers` 函数名相匹配。另请注意你使用 `pubadder.AddNumbers`，因为你正在调用另一个包中的导出函数。

就像你可以在包内部调用导出函数一样，你也可以从与源代码在同一包中的测试来测试你的公共 API。使用包名中 `_test` 后缀的优点是它让你将测试的包视为“黑盒”。你只能通过其导出的函数、方法、类型、常量和变量与之交互。

### Tip

一个包可以包含使用 `_test` 后缀的包声明的测试源文件，同时也可以包含使用包名的测试源文件。这使你能够将导出的和未导出函数的测试组织到不同的文件中。

## Tests and the Context

在为期望 context 的函数编写测试时，如第 14 章所述，不要使用 `context.Background` 来生成 context。相反，使用 `t.Context`。这会返回一个 context，当测试退出时会自动取消，如果你使用 `t.Cleanup` 注册了任何函数，则在它们运行之前立即取消 context。使用测试 context 允许被测试的任何代码正确关闭。这包括在 context 取消时退出的 goroutine。在 [“Quickly and Accurately Test Concurrent Code with `synctest`”](#ch02.html#synctest) 中，你将学习如何找到在 context 取消时未关闭的 goroutine。演示测试 context 的示例代码位于 [第 15 章仓库](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/oracle) 的 `sample_code/oracle` 目录中：

```
func TestOracle(t *testing.T) {
    ctx := t.Context()
    ch := oracle.Launch(ctx)
    outCh := make(chan string)
    ch <- oracle.Request{
        Query:    "Is the sky blue?",
        Response: outCh,
    }
    result := <-outCh
    if result != "Is the sky blue? Yes!" {
        t.Error("the oracle is wearing rose-tinted glasses")
    }
}
```

## Using go-cmp to Compare Test Results

编写复合类型两个实例的详细比较可能很冗长。虽然你可以使用 `reflect.DeepEqual` 来比较结构体、映射和切片，但有更好的方法。早在 2017 年，Google 发布了一个名为 [`go-cmp`](https://oreil.ly/9bWJf) 的第三方模块，它会为你进行比较并返回不匹配的详细描述。让我们通过定义一个简单的 `struct` 和一个填充它的工厂函数来看看它是如何工作的。你可以在 [第 15 章仓库](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/cmp) 的 `sample_code/cmp` 目录中找到此代码：

```
type Person struct {
    Name      string
    Age       int
    DateAdded time.Time
}

func CreatePerson(name string, age int) Person {
    return Person{
        Name:      name,
        Age:       age,
        DateAdded: time.Now(),
    }
}
```

在你的测试文件中，导入 `github.com/google/go-cmp/cmp`。测试函数如下所示：

```
func TestCreatePerson(t *testing.T) {
    expected := Person{
        Name: "Dennis",
        Age:  37,
    }
    result := CreatePerson("Dennis", 37)
    if diff := cmp.Diff(expected, result); diff != "" {
        t.Error(diff)
    }
}
```

`cmp.Diff` 函数接收期望的输出和被测试函数返回的输出。它返回一个描述两个输入之间任何不匹配的字符串。如果输入匹配，则返回空字符串。你将 `cmp.Diff` 函数的输出赋给名为 `diff` 的变量，然后检查 `diff` 是否为空字符串。如果不是，则发生了错误。

当你构建并运行测试时，你会看到当两个结构体实例不匹配时 `go-cmp` 生成的输出：

```
$ go test
--- FAIL: TestCreatePerson (0.00s)
    ch13_cmp_test.go:16:   ch13_cmp.Person{
              Name:      "Dennis",
              Age:       37,
        -     DateAdded: s"0001-01-01 00:00:00 +0000 UTC",
        +     DateAdded: s"2020-03-01 22:53:58.087229 -0500 EST m=+0.001242842",
          }

FAIL
FAIL    ch13_cmp    0.006s
```

带有 `-` 和 `+` 的行表示值不同的字段。测试失败是因为日期不匹配。这是个问题，因为你无法控制 `CreatePerson` 函数分配的日期。你必须忽略 `DateAdded` 字段。`go-cmp` 模块包含一个辅助包 `cmpopts`，其中包含提供对 `cmp.Diff` 执行的比较额外控制的函数。在这种情况下，使用 `cmpopts.IgnoreFields` 来忽略一个字段：

```
if diff := cmp.Diff(expected, result, cmpopts.IgnoreFields(Person{}, "DateAdded")); diff != "" {
    t.Error(diff)
}
```

这只是 `go-cmp` 中最有用功能的一个快速预览。查看其 [文档](https://oreil.ly/rmiWO) 以了解如何控制比较内容和输出格式。

# Running Table Tests

大多数情况下，验证函数是否正确工作需要不止一个测试用例。你可以编写多个测试函数来验证你的函数或在同一个函数中编写多个测试，但你会发现大量的测试逻辑是重复的。你设置支持数据和函数，指定输入，检查输出，并比较它们是否与你的期望相匹配。

而不是一遍遍地写这些，你可以利用一种称为 *table tests* 的模式。让我们看一个示例。你可以在 [第 15 章仓库](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/table) 的 `sample_code/table` 目录中找到此代码。假设你在 `table` 包中有以下函数：

```
func DoMath(num1, num2 int, op string) (int, error) {
    switch op {
    case "+":
        return num1 + num2, nil
    case "-":
        return num1 - num2, nil
    case "*":
        return num1 + num2, nil
    case "/":
        if num2 == 0 {
            return 0, ErrDivZero
        }
        return num1 / num2, nil
    default:
        return 0, &UnknownOpErr{Op: op}
    }
}
```

要测试此函数，你需要检查不同的分支，尝试返回有效结果的输入，以及触发错误的输入。你可以编写如下代码，但会非常重复：

