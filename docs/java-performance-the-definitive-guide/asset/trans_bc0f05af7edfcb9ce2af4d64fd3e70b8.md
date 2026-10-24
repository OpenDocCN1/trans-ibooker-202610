
在第 5 章开头。如果 JVM 在该循环中途执行 GC，其中一些短命的 BigDecimal 对象会很不幸：它们刚刚被创建并正在使用，因此不能被释放——但它们活不到足以证明将其移动到老年代的合理性。

这就是年轻代被划分为两个 survivor 空间和 eden 的原因。这种设置允许对象在仍处于年轻代时有额外的机会被回收，而不是被晋升到（并填满）老年代。

当年轻代被回收且 JVM 发现某个对象仍然存活时，该对象会被移动到一个 survivor 空间，而不是移动到老年代。在第一次年轻代回收期间，对象会从 eden 移动到 survivor space 0。在下一次回收期间，存活的对象会从 survivor space 0 和 eden 都移动到 survivor space 1。此时，eden 和 survivor space 0 完全为空。下一次回收会将 survivor space 1 和 eden 中的存活对象移动到 survivor space 0，以此类推。（survivor 空间也称为 *to* 和 *from* 空间；在每次回收期间，对象会从 from 空间移动到 to 空间。*from* 和 *to* 只是指针，在每次回收时在两个 survivor 空间之间切换。）

显然这不能无限持续，否则永远不会有对象被移动到老年代。对象会在两种情况下被移动到老年代。首先，survivor 空间相当小。当目标 survivor 空间在一次年轻回收期间填满时，eden 中剩余的存活对象会直接移动到老年代。其次，对象可以在 survivor 空间中停留的 GC 周期数是有限的。该限制称为 *tenuring threshold*。

调优会影响这两种情况。survivor 空间占用年轻代分配的一部分，与堆的其他区域一样，JVM 会动态调整其大小。survivor 空间的初始大小由 `-XX:InitialSurvivorRatio=N` 标志决定，该标志用于以下公式：

```
survivor_space_size = new_size / (initial_survivor_ratio + 2)
```

对于默认的初始 survivor ratio 为 8，每个 survivor 空间将占用年轻代的 10%。

JVM 可能会将 survivor 空间大小增加到由 `-XX:MinSurvivorRatio=N` 标志设定的最大值。该标志用于以下公式：

```
maximum_survivor_space_size = new_size / (min_survivor_ratio + 2)
```

默认情况下，该值为 3，这意味着 survivor 空间的最大大小将为年轻代的 20%。再次注意，该值是一个比例，因此比例的最小值给出 survivor 空间的最大大小。名称因此有点违反直觉。

要将 survivor 空间保持在固定大小，请将 SurvivorRatio 设置为所需值并禁用 UseAdaptiveSizePolicy 标志（不过请记住，禁用自适应大小将同时适用于老年代和新年代）。

JVM 根据 GC 后 survivor 空间有多满来决定是否增加或减少 survivor 空间的大小（受定义比例的限制）。survivor 空间将被调整大小，使其在 GC 后默认填满 50%。该值可以用 -XX:TargetSurvivorRatio=N 标志更改。

最后，问题是对象在 survivor 空间之间乒乓多少次 GC 周期后会被移动到老年代。答案由 tenuring threshold 决定。JVM 会持续计算它认为最佳的 tenuring threshold。阈值从 `-XX:InitialTenuringThreshold=N` 标志指定的值开始（对于 throughput 和 G1 GC 收集器，默认值为 7，对于 CMS 为 6）。JVM 最终会确定一个介于 1 和 `-XX:MaxTenuringThreshold=N` 标志指定的值之间的阈值；对于 throughput 和 G1 GC 收集器，默认最大阈值为 15，对于 CMS 为 6。

## Always and Never Tenure

tenuring threshold 总是会在 1 和 MaxTenuringThreshold 之间取值。即使 JVM 启动时初始 tenuring threshold 等于最大 tenuring threshold，JVM 也可能降低该值。

两个标志可以在极端情况下规避这种行为。如果你知道在年轻回收后存活的对象将始终存在很长时间，可以指定 -XX:+AlwaysTenure（默认 false），这本质上等同于将 MaxTenuringThreshold 设置为 0。这是一种罕见的情况；这意味着对象将始终被晋升到老年代而不是存储在 survivor 空间中。

第二个标志是 -XX:+NeverTenure（默认也为 false）。该标志影响两件事：它表现得好像初始和最大 tenuring thresholds 为无穷大，并且阻止 JVM 将该阈值调低。换句话说，只要 survivor 空间有空间，就永远不会有对象被晋升到老年代。

鉴于所有这些，在什么情况下可能调整哪些值？查看 tenuring 统计信息很有帮助；这些信息不会使用我们迄今为止使用的 GC 日志命令打印出来。

在 JDK 8 中，可以通过包含标志 -XX:+PrintTenuringDistribution（默认 false）将 tenuring 分布添加到 GC 日志中。在 JDK 11 中，它是通过在 Xlog 参数中包含 age*=debug 或 age*=trace 添加的。

最重要的是要查看 survivor 空间是否太小，以至于在 minor GC 期间，对象会直接从 eden 晋升到老年代。避免这种情况的原因是短命对象最终会填满老年代，导致 full GC 过于频繁地发生。

在 throughput 收集器生成的 GC 日志中，该条件的唯一提示是以下行：

```
Desired survivor size 39059456 bytes, new threshold 1 (max 15)
                [PSYoungGen: 657856K->35712K(660864K)]
                165987K->1073807K(2059008K), 0.0095400 secs]
                [Times: user=0.32 sys=0.00, real=0.09 secs]
```

使用 age*=debug 的 JDK 11 日志类似；它会在回收期间打印所需的 survivor 大小。

这里单个 survivor 空间的期望大小为 39 MB，而年轻代为 660 MB：JVM 计算出两个 survivor 空间应占年轻代的约 11%。但悬而未决的问题是这是否足够大以防止溢出。该日志没有明确答案，但 JVM 已将 tenuring threshold 调整为 1 这一事实表明，它已确定无论如何都会直接将大多数对象晋升到老年代，因此它已将 tenuring threshold 最小化。该应用程序可能在未充分使用 survivor 空间的情况下直接晋升到老年代。

当使用 G1 GC 时，在 JDK 8 日志中可以获得更有信息量的输出：

```
Desired survivor size 35782656 bytes, new threshold 2 (max 6)
  - age  1:   33291392 bytes,   33291392 total
  - age  2:    4098176 bytes,   37389568 total
```

在 JDK 11 中，通过在日志配置中包含 age*=trace 来获取该信息。

期望的 survivor 空间与前一个示例相似——35 MB——但输出还显示了 survivor 空间中所有对象的大小。需要晋升 37 MB 的数据，survivor 空间确实溢出了。

是否可以改善这种情况取决于应用程序。如果对象将活得比再经历几次 GC 周期更久，它们最终无论如何都会进入老年代，因此调整 survivor 空间和 tenuring threshold 并不会真正有帮助。但如果对象在再经历几次 GC 周期后就会消失，可以通过使 survivor 空间更高效来获得一些性能提升。

如果增加 survivor 空间的大小（通过降低 survivor ratio），内存将从年轻代的 eden 部分中夺走。那里是对象实际分配的地方，这意味着在发生 minor GC 之前可分配的对象更少。因此该选项通常不推荐。
