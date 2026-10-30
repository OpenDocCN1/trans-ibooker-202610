
## ❾ 与配置文件关联的自由文本（描述）

`Profile` 消息使用了多个其他消息（再次，仅编辑了最重要的部分；详情请参阅 `profile.proto`），包括以下内容：

```
message ValueType {     ❶
}
 
message Sample {        ❷
}
 
message Location {      ❸
}
 
message Line {          ❹
}
 
message Function {      ❺
}
```

❶ 对每个 `Sample.value` 关联的样本的描述
❷ 每个 `Sample` 记录程序上下文中遇到的值。
❸ 描述函数和行表调试信息
❹ 源代码中的行号
❺ 捕获源文件名以及函数和起始行号

## pprof 格式和工具

目前尚无行业标准规定如何在传输中或静态存储（例如作为文件系统中的文件）表示配置文件，`pprof` 格式是我们迄今为止最接近此类标准的格式，许多可用工具都可以读取或生成它。

`pprof` 作为一种格式高度高效，因为它基于 `Protocol Buffers`，但这也意味着与从 `HTTP API` 获取 `JSON`（并且可以使用 `curl` 和 `jq` 过滤任何值）的设置相比，它可能更难使用。有一些工具可用于处理 `protobuf`，例如

* `Protoc` ([https://grpc.io/docs/protoc-installation/](https://grpc.io/docs/protoc-installation/)) — 缩写为 protocol buffer 编译器，您可以用它来编译 `.proto` 文件，也可以对其进行解码或反序列化（用于调试很有用）
* `Buf` ([https://github.com/bufbuild/buf](https://github.com/bufbuild/buf)) — 用于处理 protobuf 的功能强大的 CLI 工具
* `Protoman` ([https://github.com/spluxx/Protoman](https://github.com/spluxx/Protoman)) — 一个类似 Postman 的 API 客户端，用于基于 protobuf 的消息
* `Postman` ([https://github.com/postmanlabs/postman-app-support/issues/2801#issuecomment-1002247567](https://github.com/postmanlabs/postman-app-support/issues/2801#issuecomment-1002247567)) — 一款流行的工具，已通过新的 `gRPC` API 类型添加了（间接）支持

## 调试 protobuf 文件是什么样的？

我们用 `protoc` 来做这件事。首先，我们需要获取一个 `pprof` protobuf 格式的文件。我们确实在之前的示例中已经生成了一些这样的文件，当时我们使用 `go tool pprof` 进行分析。请注意，您的设置可能配置了不同的路径；请查看 `go tool pprof` 命令的输出，其中显示 Saved Profile In ...（我的在 `~/pprof` 中）。在 `ch09/cp/` 目录中执行以下操作：

```
$ cp ~/pprof/pprof.samples.cpu.001.pb.gz \
     ./pprof.gz && gzip -d pprof.gz
```

现在我们有了 `pprof` protobuf 文件，可以使用 `protoc` 对其进行反序列化：

```
$ protoc --decode=perftools.profiles.Function pprof.proto \    ❶
        < pprof | \                                            ❷
        grep --color=auto -e ".*Task"                          ❸
 
6: "main.slowTask"                                             ❹
```

❶ 我们告诉 `protoc` 使用 `pprof.proto` 作为模式，并要求它解码特定消息：`perftools.profiles.Function`。
❷ 这是我们上一步创建的 `pprof` protobuf 编码文件，也是 `protoc` 应该用作输入的文件。
❸ `protoc` 完成后，我们搜索包含 "Task" 的所有函数符号。
❹ 正如预期的那样，我们在 `pprof` 文件中找到了至少一个函数，在我的例子中它就是 `slowTask()`。

就目前而言，我们暂时完成了 `pprof` 的介绍，但我们肯定会在本章后面再次看到它的实际应用。

## 火焰图和冰柱图

Netflix 著名的性能大师 Brendan Gregg 在处理 MySQL 性能问题时需要了解 CPU 使用情况，于是发明了火焰图（[https://www.brendangregg.com/flamegraphs.html](https://www.brendangregg.com/flamegraphs.html)）。Brendan 于 2011 年底发布了它们，此可视化的核心思想（图 9.2）如下：

* 堆栈概要位于 x 轴上。请注意，x 轴 *不* 表示时间，而是堆栈跟踪的集合。每个矩形代表一个堆栈跟踪（可与编程语言中的调用堆栈相比），或者更简单地说，是对函数或操作系统系统调用的调用。矩形的宽度表示函数被调用的次数或运行花费的时间。例如，我们在图 9.2 中看到 `slowTask()` 在 CPU 上花费的时间远多于 `quickTask()`。
* y 轴显示堆栈深度，从零开始计数（函数调用入口点）。顶部的矩形代表在堆栈跟踪收集时处于 CPU 上（即实际运行）的函数调用。在我们的示例中，我们可以看到 `slowTask()` 是由 `main()` 调用的。
* 最初，Brendan 使用随机颜色来帮助区分堆栈帧；然而，它们可用于传达额外信息。换句话说，颜色本身并没有语义，仅用于帮助您导航火焰图。

![](OEBPS/Images/09-02.png)

图 9.2 使用我们的 `pprof` Go 示例的火焰图

虽然资源火焰图最初侧重于 CPU 使用率，但它们已扩展到涵盖更广泛的资源，包括主内存（RAM）或网络 I/O 以及运行时和操作系统级别（例如 Java 虚拟机或 Go 中的垃圾回收）。

尽管如此，不要认为火焰图仅限于系统级任务。事实上，能够同时看到操作系统（或 JVM）在做什么以及应用程序在执行时间、CPU 和内存使用方面的表现才是真正的最佳点。您也可能会遇到术语 `icicle charts`，它只是指上下颠倒的火焰图（就像冰柱从顶部向下生长一样）。如果您想深入了解该主题，我建议阅读 Polar Signal 的文章，“Ice and Fire: How to Read Icicle and Flame Graphs” ([http://mng.bz/2DP0](http://mng.bz/2DP0))。

## eBPF

许多（开源）连续分析工具使用的另一项技术是 Linux 内核功能 eBPF。如今，它本身就被用作一个术语，但最初，该术语源自 `Berkeley Packet Filter`（`BPF`）。

Linux kernel v3.18（2014 年 12 月 7 日发布）引入了对 eBPF 的支持。这一内核版本要求以及需要良好的调试符号支持才能有意义地使用它，意味着 eBPF 至少在 2023 年初撰写时，仍限于某些环境，包括 dev/test（您可以在这里完全控制使用的 Linux 版本，而与生产环境形成对比，在生产环境中使用调试符号会减慢执行速度）。

注意 您可能会想，为什么我在查看引入它的内核版本时说 eBPF 仅限于某些环境。截至撰写时，这已经过去近 10 年了；然而，由于发行版或其他原因，云提供商和大型企业将默认 Linux 版本迁移到“更新”的内核需要更长的时间。询问您的平台团队是否可以在生产中使用 eBPF，但如果答案是“我们正在进行中”，不要感到惊讶。

现在，为什么我们需要 eBPF，它是如何工作的？它使您能够通过添加您认为合适的新的功能来安全高效地扩展 Linux 内核。它通过 `bpf(2)` 系统调用暴露给 Linux 用户空间（有关更多详细信息，请参阅 `man` 手册页 [[https://man7.org/linux/man-pages/man2/bpf.2.html](https://man7.org/linux/man-pages/man2/bpf.2.html)]），并在内核中作为虚拟机（`VM`）实现，该虚拟机定义了自定义的 64 位 RISC 指令集。图 9.3 显示了 eBPF 工作原理的高级概述，摘自 Brendan Gregg 的著作 *BPF Performance Tools*（2019 年，Addison-Wesley，[http://mng.bz/1q1j](http://mng.bz/1q1j)）。如果您想了解更多关于 eBPF 的信息，我强烈推荐阅读 Liz Rice 的 *What Is eBPF?*（2022 年，O’Reilly，[http://mng.bz/Pzln](http://mng.bz/Pzln)）。

![](OEBPS/Images/09-03.png)
