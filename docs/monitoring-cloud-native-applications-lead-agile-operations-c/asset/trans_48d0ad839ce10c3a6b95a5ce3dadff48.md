

当 `#` 后的第一个词是 `HELP` 或 `TYPE` 时。无论指标的类型如何，`HELP` 都是对指标的描述，并且保持不变。`TYPE` 表示指标的类型，可以是以下之一：`counter`、`gauge`、`histogram`、`summary` 和 `untyped`。当指标类型未知时，使用 `untyped`。

如果 `#` 后的词是 `HELP`，则其后跟指标名称。对于给定的指标，只能有一行 `HELP`。如果 `#` 后的词是 `TYPE`，则其后依次是指标名称和预期的指标类型。与 `HELP` 行类似，对于给定的指标，只能有一行 `TYPE`。如果 `TYPE` 行中没有定义指标类型，则 `TYPE` 会自动设置为 `untyped`。文本格式语法如下：

```
metric_name [
  "{" label_name "=" "\" label_value "\"" { "," label_name "=" "\" 
  label_value "\"" } [ "," ] "}"
] value [ timestamp ]
```

**指标名称：** 这是指标的名称，通常是一个描述，提供更多关于指标本身的信息。指标名称通常保持唯一，以避免各库之间重复。它指的是特定库中某个特定文件中的特定代码行，指标就是从该处收集的。

**标签：** 可以包含多个标签，用逗号分隔；但重要的是，每次抓取之间要保持一致的顺序。例如：

```
# HELP counter_metric Just an example of counter
# TYPE counter_metric counter
counter_metric_total{foo="bar",baz="qux"} 2520
counter_metric_total{foo="bar",baz="qux"} 219
```

**时间戳：** 时间戳是以毫秒为单位的整数值，由 Prometheus 自动附加在值之后。

图 4-20 展示了 `HELP` 行、`TYPE` 行和仪表指标：

```
# HELP node_scrape_collector_success node_exporter: Whether a collector succeeded.
# TYPE node_scrape_collector_success gauge
node_scrape_collector_success{collector="arp"} 1
node_scrape_collector_success{collector="bcache"} 1
node_scrape_collector_success{collector="bonding"} 1
node_scrape_collector_success{collector="conntrack"} 1
node_scrape_collector_success{collector="cpu"} 1
node_scrape_collector_success{collector="cpufreq"} 1
node_scrape_collector_success{collector="diskstats"} 1
node_scrape_collector_success{collector="edac"} 1
node_scrape_collector_success{collector="entropy"} 1
node_scrape_collector_success{collector="filefd"} 1
node_scrape_collector_success{collector="filesystem"} 1
node_scrape_collector_success{collector="hwmon"} 1
node_scrape_collector_success{collector="infiniband"} 1
node_scrape_collector_success{collector="ipvs"} 1
node_scrape_collector_success{collector="loadavg"} 1
node_scrape_collector_success{collector="mdadm"} 1
node_scrape_collector_success{collector="meminfo"} 1
node_scrape_collector_success{collector="netclass"} 1
node_scrape_collector_success{collector="netdev"} 1
node_scrape_collector_success{collector="netstat"} 1
node_scrape_collector_success{collector="nfs"} 1
node_scrape_collector_success{collector="nfsd"} 1
node_scrape_collector_success{collector="pressure"} 1
node_scrape_collector_success{collector="sockstat"} 1
node_scrape_collector_success{collector="stat"} 1
node_scrape_collector_success{collector="textfile"} 1
node_scrape_collector_success{collector="time"} 1
node_scrape_collector_success{collector="timex"} 1
node_scrape_collector_success{collector="uname"} 1
node_scrape_collector_success{collector="vmstat"} 1
node_scrape_collector_success{collector="xfs"} 1
node_scrape_collector_success{collector="zfs"} 1
```

## 图 4-20. 仪表指标的暴露格式

图 4-21 展示了 `HELP` 行、`TYPE` 行和摘要指标：

```
# HELP go_gc_duration_seconds A summary of the GC invocation durations.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 1.0783e-05
go_gc_duration_seconds{quantile="0.25"} 1.6019e-05
go_gc_duration_seconds{quantile="0.5"} 3.7685e-05
go_gc_duration_seconds{quantile="0.75"} 8.7524e-05
go_gc_duration_seconds{quantile="1"} 0.014988136
go_gc_duration_seconds_sum 1624.240639472
go_gc_duration_seconds_count 4.577738e+06
```

## 图 4-21. 摘要指标的暴露格式

图 4-22 展示了 `HELP` 行、`TYPE` 行和无类型指标：

```
# HELP node_vmstat_pgpigin /proc/vmstat information field pgpgin.
# TYPE node_vmstat_pgpigin untyped
node_vmstat_pgpigin 4.64097761e+08
# HELP node_vmstat_pgp gout /proc/vmstat information field pgpgout.
# TYPE node_vmstat_pgp gout untyped
node_vmstat_pgp gout 2.135340909e+09
```

图 4-22. 无类型指标的暴露格式

## 服务发现

服务发现，顾名思义，关注的是 Prometheus 发现抓取端点的过程。

为了抓取目标，您必须先在 `prometheus.yml` 文件的 `scrape_configs` 部分配置一个已定义的 `job_name`、目标主机和端口号。例如，只需在 `scrape_configs` 部分添加相应的 `job_name`，Prometheus 就可以抓取 Prometheus 服务器本身，如图 4-23 所示：

![](img/6646c41cea18bb4f7d3c2c6513a4d992_123_0.png)

图 4-23. 静态配置

对于您想要监控的所有目标，您可以定义相应的作业名称，然后在 YAML 文件中逐个列出它们。然而，在监控目标是临时性的动态环境中，手动添加并不实际。为了解决这一特定挑战，Prometheus 使用服务发现，并支持通过任何受支持的服务发现机制动态发现目标，例如 Consul、DNS、Openstack、Kubernetes、EC2、Azure 和 GCE。如果您偏好的选项不受支持，您还可以使用基于文件的服务发现。

服务发现可以分为两类。一类是实例通过诸如 Consul 之类的服务发现机制自行注册。这被称为

## 图 4-30. 经典 UI 中的表达式浏览器

表达式浏览器有一个中心字段，您可以在其中输入任意表达式以进行快速调试。您还可以在下拉菜单中查看已发现的指标列表，该下拉菜单为“在光标处插入指标”，紧挨着“执行”按钮。在底部，有两个选项可以检查您之前执行过的表达式的输出。一个选项是“控制台”选项卡，它为您提供结果，以及实例和作业的标签，以及执行查询时对应的值。第二个选项是图形，它显示该值随时间变化的情况。

它还会让您了解其他三项相关信息：

- 1. 加载时间
- 2. 分辨率
- 3. 总时间序列

如果您对该表达式的图形表示感兴趣，可以轻松切换到 Graph 选项卡，如图 4-32 所示，并看到两条彩色折线图，每条代表一个特定目标。您可以将鼠标悬停在特定的折线图上，以查看该折线图对应的是哪个实例。在图 4-33 的示例中，我们可以看到红线对应于 localhost:9090（Prometheus 服务器），绿线对应于 localhost:9100（Node Exporter）。您可以看到，当节点启动并开始报告时，对应 Node Exporter 的绿线直接从 0 上升到 1。总之，Graph 帮助您分析在一段时间内从不同目标收集的时间序列数据值的变动情况。

如果有多个目标报告多条折线图，而您想分析其中某一条折线图的变化，您可以点击特定的表达式，Graph 将只显示一条折线图，供您专门分析其变化。这有助于消除杂乱，让您将精力集中在单个目标上。

![](img/6646c41cea18bb4f7d3c2c6513a4d992_134_0.png)

图 4-33. 聚焦于单个表达式

