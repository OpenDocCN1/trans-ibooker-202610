

```
BenchmarkFileLen/FileLen-1-16          48  24040199 ns/op     128 B/op  3 allocs/op
BenchmarkFileLen/FileLen-10-16        490   2435033 ns/op     128 B/op  3 allocs/op
BenchmarkFileLen/FileLen-100-16      4680    255315 ns/op     240 B/op  4 allocs/op
BenchmarkFileLen/FileLen-1000-16    34837     34776 ns/op    1152 B/op  4 allocs/op
BenchmarkFileLen/FileLen-10000-16   93732     12751 ns/op   10368 B/op  4 allocs/op
BenchmarkFileLen/FileLen-100000-16  84337     14236 ns/op  106624 B/op  4 allocs/op
```

分配次数现在保持一致且很小：对于每种缓冲区大小只有三次或四次。有趣的是，你现在可以做出权衡。如果内存紧张，你可以使用更小的缓冲区大小，在牺牲性能的情况下节省内存。

如果基准测试显示你存在性能或内存问题，下一步就是弄清楚问题的具体所在。Go 包含用于从正在运行的程序中收集 CPU 和内存使用数据的分析工具，以及帮助你可视化和解释生成数据的工具。你甚至可以公开一个 Web 服务端点，从正在运行的 Go 服务远程收集分析信息。这些数据甚至可以在编译时用于自动改进性能。我在第 17 章中谈论了对代码进行仪器化以及理解分析数据>>。

## benchstat

你如何跟踪代码不同版本之间的性能变化，又如何知道差异是否显著？你可以肉眼查看输出并尝试记住，但最好使用 `benchstat`，这是一个由 Go 团队构建的工具，可自动化该过程。使用以下命令将 `benchstat` 添加到你的 Go 模块中：

```
go get -tool golang.org/x/perf/cmd/benchstat
```

在为 `benchstat` 捕获基准测试时，你需要多次运行它们并将输出保存到磁盘。`-count` 标志控制每次基准测试运行的次数；默认设置为 10：

```
go test -bench=BenchmarkFileLen$ -count=10 -benchmem > base.txt
# move the allocation from inside the for loop to before the for loop
go test -bench=BenchmarkFileLen$ -count=10 -benchmem > new.txt
```

使用 `go tool benchstat base.txt new.txt` 比较运行结果。以下是通过对 `FileLen` 函数最后两个版本运行 `benchstat` 我得到的结果：

```
$ go tool benchstat base.txt new.txt
goos: darwin
goarch: arm64
pkg: github.com/learning-go-book-3e/ch15/sample_code/bench
cpu: Apple M3 Max
                          │  base.txt   │               new.txt               │
                          │   sec/op    │   sec/op     vs base                │
FileLen/FileLen-1-16        23.83m ± 1%   23.80m ± 0%        ~ (p=0.796 n=10)
FileLen/FileLen-10-16       2.433m ± 1%   2.412m ± 0%   -0.85% (p=0.000 n=10)
FileLen/FileLen-100-16      263.3µ ± 2%   249.4µ ± 1%   -5.31% (p=0.000 n=10)
FileLen/FileLen-1000-16     38.93µ ± 1%   33.80µ ± 0%  -13.18% (p=0.000 n=10)
FileLen/FileLen-10000-16    16.13µ ± 2%   12.69µ ± 2%  -21.35% (p=0.000 n=10)
FileLen/FileLen-100000-16   19.02µ ± 2%   14.15µ ± 1%  -25.60% (p=0.000 n=10)
geomean                     238.1µ        210.5µ       -11.63%

                          │   base.txt    │                new.txt                 │
                          │     B/op      │     B/op      vs base                  │
FileLen/FileLen-1-16           128.0 ± 0%     128.0 ± 0%        ~ (p=1.000 n=10) ¹
FileLen/FileLen-10-16          128.0 ± 0%     128.0 ± 0%        ~ (p=1.000 n=10) ¹
FileLen/FileLen-100-16       73040.0 ± 0%     240.0 ± 0%  -99.67% (p=0.000 n=10)
FileLen/FileLen-1000-16     66.125Ki ± 0%   1.125Ki ± 0%  -98.30% (p=0.000 n=10)
FileLen/FileLen-10000-16     80.12Ki ± 0%   10.12Ki ± 0%  -87.36% (p=0.000 n=10)
FileLen/FileLen-100000-16    208.1Ki ± 0%   104.1Ki ± 0%  -49.97% (p=0.000 n=10)
geomean                      10.35Ki        1.277Ki       -87.66%
¹ all samples are equal

                          │   base.txt   │               new.txt                │
                          │  allocs/op   │ allocs/op   vs base                  │
FileLen/FileLen-1-16          3.000 ± 0%   3.000 ± 0%        ~ (p=1.000 n=10) ¹
FileLen/FileLen-10-16         3.000 ± 0%   3.000 ± 0%        ~ (p=1.000 n=10) ¹
FileLen/FileLen-100-16      654.000 ± 0%   4.000 ± 0%  -99.39% (p=0.000 n=10)
FileLen/FileLen-1000-16      69.000 ± 0%   4.000 ± 0%  -94.20% (p=0.000 n=10)
FileLen/FileLen-10000-16     11.000 ± 0%   4.000 ± 0%  -63.64% (p=0.000 n=10)
FileLen/FileLen-100000-16     5.000 ± 0%   4.000 ± 0%  -20.00% (p=0.000 n=10)
geomean                       16.78        3.634       -78.34%
¹ all samples are equal
```

第一个表显示 CPU 时间的差异，第二个显示堆上分配的字节数差异，第三个显示从堆上分配内存的次数差异。每个数字都给出了方差范围（例如 ± 1%）；越低越好。p 值范围从 0 到 1，告诉你比较是否有效。0 表示你可以信任结果，1 表示你不能，中间的数字表示信任程度。如果基准情况和新情况之间的差异在统计上不显著，你会看到 p 的高值；可以忽略它。在其他情况下，p 值小于 0.05 是理想的。

如果方差或 p 值很高，请确保在收集基准数据时你的计算机没有忙于运行其他任务。你也可以使用 `-count` 标志增加基准测试运行次数，或使用 `-benchtime` 标志强制每次基准测试运行设定的秒数（`-benchtime=2s` 表示运行两秒）或运行设定的次数（`-benchtime=100000x` 强制每次基准测试运行 100,000 次）。

# 使用数据竞争检测器查找并发数据访问问题

即使有 Go 内置的并发支持，错误仍然会发生。很容易在没有获取锁的情况下从两个不同的 goroutine 引用同一个变量。计算机科学术语称之为 *data race*。为了帮助找到这类错误，Go 包含一个 *race checker*。它不能保证能找到代码中的每一个数据竞争，但如果它找到了一个，你应该在它找到的地方加上适当的锁。

看看 `sample_code/race/race.go` 文件中的一个简单示例，该文件位于 [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/blob/main/sample_code/race/race.go)：

```
func getCounter() int {
    var counter int
    var wg sync.WaitGroup
    for range 5 {
        wg.Go(func() {
            for range 1000 {
                counter++
            }
        })
    }
    wg.Wait()
    return counter
}
```

此代码启动五个 goroutine，让它们各自更新共享的 `counter` 变量 1,000 次，然后返回结果。你会期望它是 5,000，所以让我们用 `sample_code/race/race_test.go` 中的单元测试来验证：

```
func TestGetCounter(t *testing.T) {
    counter := getCounter()
    if counter != 5000 {
        t.Error("unexpected counter:", counter)
    }
}
```

如果你多次运行 `go test`，有时会通过，但大多数时候会失败并出现类似这样的错误信息：

```
unexpected counter: 3673
```

问题是代码中存在数据竞争。在这么简单的程序中，原因很明显：多个 goroutine 同时尝试更新 `counter`，并且某些更新丢失了。在更复杂的程序中，这类竞争更难察觉。让我们看看数据竞争检测器会做什么。使用 `-race` 标志与 `go test` 一起启用它：

```
$ go test -race
==================
WARNING: DATA RACE
Read at 0x00c000128070 by goroutine 10:
  test_examples/race.getCounter.func1()
      test_examples/race/race.go:12 +0x45

Previous write at 0x00c000128070 by goroutine 8:
  test_examples/race.getCounter.func1()
      test_examples/race/race.go:12 +0x5b
```
