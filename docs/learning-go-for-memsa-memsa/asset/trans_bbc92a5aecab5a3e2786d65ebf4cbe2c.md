

如果被模糊测试的函数需要一个 `int` 和一个 `string`，对 `Add` 的调用看起来像这样：

```
f.Add(1, "some text")
```

向 `Add` 传递无效类型的值是运行时错误。

接下来，在你的 `*testing.F` 实例上调用 `Fuzz` 方法。这看起来有点像在标准单元测试中编写表测试时对 `Run` 的调用。`Fuzz` 接受一个单一参数，一个函数，其第一个参数的类型为 `*testing.T`，其余参数必须与传入 `Add` 的值的类型、顺序和数量完全匹配。这也指定了模糊测试引擎在模糊测试期间生成的数据类型。Go 编译器无法强制执行此约束，因此如果未遵循约定，则为运行时错误。

最后，让我们看看模糊测试目标的主体。记住，模糊测试用于查找未正确处理坏输入的情况。由于输入是随机生成的，你无法编写了解输出应为的内容的测试。相反，你必须使用对所有输入都为真的测试条件。在 `ParseData` 的情况下，你可以检查两件事：

* 代码是否对坏输入返回错误，还是会 panic？
* 如果你将字符串切片转换回字节切片并重新解析它，是否得到相同的结果？

让我们看看运行模糊测试时会发生什么：

```
$ go test -fuzz=FuzzParseData
fuzz: elapsed: 0s, gathering baseline coverage: 0/137 completed
failure while testing seed corpus entry: FuzzParseData/03f81b404ad91d09
fuzz: elapsed: 3s, gathering baseline coverage: 2/137 completed
--- FAIL: FuzzParseData (2.92s)
    --- FAIL: FuzzParseData (0.00s)
        testing.go:1927: panic: runtime error: makeslice: cap out of range
            goroutine 12 [running]:
            runtime/debug.Stack()
                /opt/homebrew/opt/go/libexec/src/runtime/debug/stack.go:26 +0xc4
            testing.tRunner.func1()
                /opt/homebrew/opt/go/libexec/src/testing/testing.go:1927 +0x218
            panic({0x100ca49c0?, 0x100ce5dc0?})
                /opt/homebrew/opt/go/libexec/src/runtime/panic.go:860 +0x12c
            file_parser.ParseData({0x100ce7a40?, 0x10e411dfcf30?})
                file_parser/file_parser.go:46 +0x134
```

如果你不指定 `-fuzz` 标志，你的模糊测试将被视为单元测试并针对种子语料库运行。一次只能模糊一个模糊测试。

### Note

如果你想获得完整的体验，删除 `*testdata/fuzz/FuzzParseData*` 目录的内容。这将导致模糊器生成新的种子语料库条目。由于模糊器生成随机输入，你的样本可能与所示的不同。不同的条目可能会产生类似的错误，尽管可能顺序不同。

模糊测试运行，少于一秒后，它报告了一个 panic。Panic 很糟糕，所以让我们看看生成的输入。每当测试用例失败时，模糊器都会将其写入与失败测试相同的包中的 `*testdata/fuzz/TESTNAME*` 子目录中，并向种子语料库添加一个新条目。此自动生成的单元测试在调用 `go test -fuzz=FuzzParseData` 时运行，并在你修复错误后充当回归测试。

以下是该文件的内容：

```
go test fuzz v1
[]byte("-1")
```

第一行是标头，用于指示这是模糊测试的测试数据。后续行包含导致失败的数据。

将失败消息中的文件名作为 `-run` 参数传递给 `go test` 允许你在重新运行测试时隔离此失败情况：

```
$ go test -run=FuzzParseData/03f81b404ad91d09
--- FAIL: FuzzParseData (0.00s)
    --- FAIL: FuzzParseData/03f81b404ad91d09 (0.00s)
panic: runtime error: makeslice: cap out of range [recovered, repanicked]
```

生成 panic 的行如下所示：

```
out := make([]string, 0, count)
```

你正在尝试创建一个具有负容量的切片，这会导致 panic。针对这种情况的错误类型已为你准备好。添加条件在调用 `make` 之前检查负数：

```
if count < 0 {
    return nil, ErrNegative
}
```

如果你运行 `go test -run=FuzzParseData/03f81b404ad91d09`，你会看到测试现在通过了。再次运行模糊器，看看它是否能找到更多错误：

```
$ go test -fuzz=FuzzParseData
fuzz: elapsed: 0s, gathering baseline coverage: 0/137 completed
failure while testing seed corpus entry: FuzzParseData/0e4ad8073b1a5373
fuzz: elapsed: 3s, gathering baseline coverage: 2/137 completed
--- FAIL: FuzzParseData (2.95s)
    --- FAIL: FuzzParseData (0.00s)
        file_parser_test.go:87:   []string{
              ... // 3 identical elements
              "",
              "",
            -  "\r",
            +  "",
              "0",
            }
```

这次，模糊器创建的输入文件如下所示：

```
go test fuzz v1
[]byte("7\n\n\n\n\n\n\r\r\n0")
```

模糊测试正在生成带有 `\r`（回车）字符的空白行。空白行在你的输入中是不期望的，因此向读取 `Scanner` 文本行的循环添加一些代码。你将检查一行是否仅由空白字符组成。如果是，则返回错误：

```
line = strings.TrimSpace(line)
if len(line) == 0 {
    return nil, ErrBlankLine
}
```

使用 `go test -run=FuzzParseData/0e4ad8073b1a5373` 验证你是否修复了错误，然后再次运行模糊器，看看它是否能找到更多错误：

```
$ go test -fuzz=FuzzParseData
fuzz: elapsed: 0s, gathering baseline coverage: 0/137 completed
fuzz: elapsed: 3s, gathering baseline coverage: 136/137 completed
fuzz: elapsed: 6s, gathering baseline coverage: 136/137 completed
fuzz: elapsed: 9s, gathering baseline coverage: 136/137 completed
failure while testing seed corpus entry: FuzzParseData/e8bf251ff804c663
fuzz: elapsed: 11s, gathering baseline coverage: 136/137 completed
--- FAIL: FuzzParseData (10.83s)
    fuzzing process hung or terminated unexpectedly: exit status 2
```

在这种情况下，`go` 命令报告你的程序“挂起或意外终止”。我们不希望程序挂起或终止，无论是否预期，所以让我们查看生成的测试用例以弄清出了什么问题：

```
go test fuzz v1
[]byte("3000000000000")
```

问题是你正在尝试分配一个容量足以容纳 3 万亿个字符串的切片。这需要比我的电脑（可能也比你的）更多的 RAM。你需要将预期文本元素的数量限制为合理数量。使用哨兵错误 `ErrTooManyLines` 并将最大行数设置为 1,000，通过在 `ParseData` 中解析预期行数后添加以下代码：

```
if count > 1000 {
    return nil, ErrTooManyLines
}
```

使用 `go test -run=FuzzParseData/e8bf251ff804c663` 验证错误是否已修复，并再次运行模糊器：

```
$ go test -fuzz=FuzzParseData
fuzz: elapsed: 0s, gathering baseline coverage: 0/147 completed
fuzz: elapsed: 0s, gathering baseline coverage: 147/147 completed,
    now fuzzing with 16 workers
fuzz: elapsed: 3s, execs: 1568753 (522795/sec), new interesting: 1 (total: 148)
fuzz: elapsed: 6s, execs: 3134921 (522087/sec), new interesting: 1 (total: 148)
fuzz: elapsed: 9s, execs: 4652294 (505854/sec), new interesting: 1 (total: 148)
...
fuzz: elapsed: 2m9s, execs: 59383661 (416674/sec), new interesting: 2 (total: 149)
fuzz: elapsed: 2m12s, execs: 60694659 (436957/sec), new interesting: 3 (total: 150)
fuzz: elapsed: 2m15s, execs: 61976072 (427059/sec), new interesting: 3 (total: 150)
^C
PASS
```

几分钟后，没有发现更多错误，因此按 Ctrl-C 结束模糊测试。

