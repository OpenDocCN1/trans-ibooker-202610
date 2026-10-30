
请注意，函数 `slowTask()` 和 `quickTask()` 除了使用 CPU 以便我们在分析器中看到一些内容外，并没有做任何有用的事情。你现在可以在终端会话中像这样运行 Go 程序：

```
$ go run .
Quick task result is 692530d563c476aea00dd73067d7e146edbece4c17bff4c
Slow task result is 692530d563c476aea00dd73067d7e146edbece4c17bff4c9
...
```

如果你导航到 `http://localhost:6060/debug/pprof/`，你现在将看到 `net/http/pprof` Go 包的 Web 服务器输出。在这里查看输出会显示各种提供的分析类型，包括协程、堆分配的详细信息等等。

接下来（并且你需要安装 `graphviz` `dot` [[https://graphviz.org/docs/layouts/dot/](https://graphviz.org/docs/layouts/dot/)\] 命令），你可以使用以下命令生成图像，例如渲染协程调用图：

```
$ go tool pprof -png http://localhost:6060/debug/pprof/goroutine > gr.png
 
Fetching profile over HTTP from http://localhost:6060/debug/pprof/goroutine
Saved profile in ~/tmp/pprof/pprof.goroutine.002.pb.gz
```

好吧，这个 Web 视图或图像可能有洞察力，也可能没有，但使用 `pprof` 的另一种（更流行）方式是：交互模式（想想 shell）。让我们这样做（收集 30 秒的 CPU 分析，因此你需要等待片刻）：

```
$ go tool pprof "http://localhost:6060/debug/pprof/profile?seconds=30"
 
Fetching profile over HTTP from
http://localhost:6060/debug/pprof/profile?seconds=30
Saved profile in ~/pprof/pprof.samples.cpu.002.pb.gz
Type: cpu
Time: Mar 8, 2023 at 07:01am (GMT)
Duration: 30.13s, Total samples = 29.55s (98.09%)
Entering interactive mode (type "help" for commands, "o" for options)
...
```

在这一点上，你已进入 `pprof` 交互式 shell，并且可以列出前 50 个函数（在这里称为 *nodes*）并使用累积聚焦，使用 `top 50 cum` 命令如下（已编辑以显示最重要的部分）：

```
(pprof) top 50 cum
Active filters:
   focus=cum
Showing nodes accounting for 15.54s, 52.59% of 29.55s total
Dropped 47 nodes (cum <= 0.15s)
      flat  flat%   sum%        cum   cum%
     3.72s 12.59% 12.59%      3.73s 12.62%  fmt.(*fmt).fmtSbx
     2.12s  7.17% 19.76%      4.15s 14.04%  runtime.mallocgc
     1.44s  4.87% 24.64%      1.44s  4.87%  crypto/sha256.sha256block
       ...
     0.12s  0.41% 50.19%     14.59s 49.37%  main.slowTask
       ...
         0     0% 52.59%      1.73s  5.85%  main.quickTask
```

从 `pprof` 中 `top` 命令的输出可以看出，`slowTask()` 函数（在 30 秒采样期间）占用了几乎 50% 的运行时间，而在同一时间内 `quickTask()` 函数占用了不到 6%。我在这里不会进一步详细说明，因为本次练习的目的是不是教你 Go 的 `pprof` 工具，而是让你了解所涉及的工作量。

那么让我们回顾一下我们为了看到任何东西所做的事情：

1. 我们必须通过包含 `net/http/pprof` 来为代码添加检测。
2. 我们必须包含一个 Web 服务器来公开分析文件（或者，我们可以将数据写入文件）。
3. 我们必须使用浏览器或 `curl`（以及其他依赖项，例如 `dot`）来获取“某一时刻”的结果，从而回答我们的资源使用问题。
4. 我们必须等待 30 秒让 `pprof` 收集分析文件，然后我们才能交互式查询。

注意 坦白说，我在这里使用 Go 作为示例，因为它自带强大的分析器已内置在其工具链和标准库中。对于其他编程语言来说，相同的练习可能意味着更大的工作量。

例如，这里有一些收集分析文件的特定语言方法：

* Java 有 `async-profiler` ([https://github.com/jvm-profiling-tools/async-profiler](https://github.com/jvm-profiling-tools/async-profiler))
* .NET 有 `dotTrace` ([https://www.jetbrains.com/profiler/](https://www.jetbrains.com/profiler/))
* PHP 有 `phpspy` ([https://github.com/adsr/phpspy](https://github.com/adsr/phpspy))
* Python 有 `cProfile` ([https://docs.python.org/3/library/profile.html](https://docs.python.org/3/library/profile.html))
* Ruby 有 `rbspy` ([https://rbspy.github.io/](https://rbspy.github.io/))
* Rust 有 `pprof-rs` ([https://github.com/tikv/pprof-rs](https://github.com/tikv/pprof-rs))

别误会；正如你在 Go 示例中看到的，分析是可能的，但需要专业知识，就像在 2015 年左右（Docker 出现时）之前使用容器是可行的，但需要深入了解 Linux 内核特性，包括 `cgroups` 和 `namespaces`。如今，运行容器对任何人来说只是一个 `docker run` 的距离。

但是在 2010 年，发生了某事：一群 Google 工程师发表了开创性论文“Google-Wide Profiling: A Continuous Profiling Infrastructure for Data Centers”，作者为 Gang Ren、Eric Tune、Tipp Moseley、Yixin Shi、Silvius Rus 和 Robert Hundt（2010 年，IEEE Micro，[https://research.google/pubs/pub36575/](https://research.google/pubs/pub36575/)）。在这篇论文中，作者描述了持续捕获 CPU 和内存使用情况并将此数据（分析文件）与源代码关联的想法，以便工程师能够使用其大规模的持续分析系统定位缓慢或有错误的源代码行以及共享的经验教训。这篇论文影响了社区，在随后的几年里，我们见证了该领域出现了许多商业产品。总之，使用原语进行分析是可行的，但在这个层面上创建和使用分析文件很复杂，因此它没有成为主流。

持续分析所做的权衡是以相对较低的频率进行分析并随着时间的推移收集统计显著性。这与传统分析形成对比，在传统分析中，我们以非常高的频率进行分析，因为我们希望在短时间内达到统计显著性——比如 10 秒。现在，在深入持续分析工具之前，让我们先退一步，讨论该领域广泛使用的一些常见技术和技术。

### 9.1.2 常见技术

你会在许多，如果不是全部，持续分析工具中找到三种技术。在进入工具本身之前，让我们快速讨论这些常见技术。

#### pprof

我们已经在该节的介绍中看到了 `pprof` 的实际应用，但让我们稍微放大一点。`pprof` ([https://github.com/google/pprof](https://github.com/google/pprof)) 是 Google 开发的用于分析数据的可视化和分析工具。它与 Go 集成良好，但对我们的讨论更重要的是，它定义了一种表示分析文件的数据格式。这种 `pprof` 格式基于协议缓冲区（protobuf）模式 ([https://protobuf.dev/](https://protobuf.dev/))，该模式在 `profile.proto` 中定义（请参阅以下站点以获取完整记录的 protobuf 文件，以及以下文本以获取最重要的部分描述：[https://github.com/google/pprof/blob/main/proto/profile.proto](https://github.com/google/pprof/blob/main/proto/profile.proto))：

```
message Profile {
  repeated ValueType sample_type = 1;    ❶
  repeated Sample sample = 2;            ❷
  repeated Mapping mapping = 3;          ❸
  repeated Location location = 4;        ❹
  repeated Function function = 5;        ❺
  repeated string string_table = 6;      ❻
  int64 time_nanos = 9;                  ❼
  int64 duration_nanos = 10;             ❽
  repeated int64 comment = 13;           ❾
}
```

❶ 描述值的含义和测量单位

❷ 分析文件中记录的样本集合

❸ 从地址范围到映射到该地址范围的二进制文件/库的映射

❹ 程序位置引用

❺ 位置引用的函数

❻ 消息引用的字符串的公共表

❼ 收集时间（UTC），以纳秒为单位，距 UNIX 纪元的时间

❽ 分析文件的持续时间
