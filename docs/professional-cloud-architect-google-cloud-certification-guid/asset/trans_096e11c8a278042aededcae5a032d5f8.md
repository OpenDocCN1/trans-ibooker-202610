

我们还应理解，随着托管实例组规模的增长，添加相同规模的 VM 所带来的影响会减小。让我们澄清一下这里的意思。假设我们将 CPU 利用率设置为 75%，实例组中的最小实例数为三台。自动扩缩器会检查 VM 实例的总体平均值是否高于 75%。如果自动扩缩器添加了第四台 VM，它就为该组增加了 25% 的容量。

如果我们实例组的最大值为 10 台 VM，那么第十台 VM 仅贡献了 10% 的额外容量——因此第十个节点的<|message|>影响与第四个节点不同。在设置利用率和最大容量时，我们应牢记这一点。还需要注意的是，自动扩缩始终会保守地向上取整统计数据。它宁愿启动一个实际上不需要的额外 VM 实例，也不愿可能耗尽资源。

基于 HTTP 负载均衡的扩缩将根据 VM 实例的负载进行扩缩。负载均衡器会在创建 HTTP 负载均衡器时配置的后端服务之间分配负载。当实例组用于后端时，外部负载均衡器提供两种平衡模式——**utilization**，允许我们为实例组中所有实例的平均后端利用率指定一个最大目标，以及 **rate**，我们必须指定每秒请求数的目标值。这是按实例或按组进行的。

自动扩缩策略中的 **Target HTTP load balancing utilization** 值应为负载均衡器设置中配置值的分数。例如，如果将策略设置为每秒 100 个请求，那么明智的做法是略低于此值进行扩缩，并将策略中的使用量设置为大约每秒 80 个请求（80%）：

## New metric

Metric type

HTTP load balancing utilization

Target HTTP load balancing utilization

80 %

Done

Cancel

+ Add new metric

+ Add new scaling schedule

## Predictive autoscaling

Use predictive autoscaling to predict future capacity needs based on historical CPU load.

Learn more

Off
No predictive method is used. The autoscaler scales the group to meet current demand based on real-time metrics.

Optimize for availability
Predictive autoscaling improves availability by monitoring daily and weekly CPU load patterns and scaling out ahead of anticipated demand.

Predictive autoscaling can only be enabled when a CPU utilization metric is applied.

## Cool down period

Specify how long it takes for your app to initialize from boot time until it is ready to serve.

Cool down period

60 seconds

Minimum number of instances

Maximum number of instances

1

10

Figure 4.40 – Autoscaling – HTTP utilization

## Tip

We will discuss load balancers in more detail in Chapter 10, Networking Options in GCP.

我们也可以基于 Google Cloud operations suite 指标进行扩缩，其中所选指标将为托管实例组中的每个实例或整个组提供数据。这些指标可以是开箱即用的，也可以是你创建的自定义指标。请注意，在撰写本文时，在某些文档中，该选项在设置中仍被称为 **Stackdriver**；不过，你应注意该名称已重新品牌为 Google Cloud's operations suite。

> **Important Note**
> 如果你有时需要将扩缩缩小到零个实例，那么你应该使用按组指标。

让我们再看一个例子，使用 Google 提供的演示指标 `custom.googleapis.com/appdemo_queue_depth_01` 作为自定义指标。

在本例中，**Utilization target type** 设置为 **Gauge**，因为这将指示自动扩缩器计算过去几分钟内收集的数据的平均值并将其与目标值进行比较：

![](img/de2c97ab017f36b3750a5bade111edd7_116_0.png)

Figure 4.41 – Autoscaling – Stackdriver (Google Cloud's operations suite) metrics

假设我们现在在实例模板上有一个启动脚本，该脚本将调用另一个负责生成这些自定义指标的脚本。我们可以从实例组内的图表监控标签页中看到，我们的实例组的负载在下午 5:55 左右开始增加，但在下午 6 点左右又下降了：

![](img/de2c97ab017f36b3750a5bade111edd7_117_0.png)

Figure 4.42 – Monitoring load

鉴于队列深度出现峰值，我们可以预期 **autoscaling policy** 会生成一个新实例。如果我们检查自动扩缩器日志，可以确认这一点。它随后添加了第三个实例，然后在需求再次下降时减少回两个实例：

![](img/de2c97ab017f36b3750a5bade111edd7_117_1.png)

Figure 4.43 – Adding and removing instances

这就是真正的弹性计算！这些是自动扩缩工作原理的小例子，我们建议你确保能够创建自己的策略并复制任何导致机器实例增加和减少的负载。

**Important Note**
每个托管实例组只能创建一个自动扩缩器。

## Autohealing

Autohealing 也是托管实例组设置的一部分，但值得单独一个章节来进一步讨论。GCP 的托管实例组负责验证我们组中每个 VM 实例是否正在运行并准备好接受客户端请求。为了执行此验证，它需要一个健康检查，这基本上是一个探针，用于联系实例组中的每个成员以检查其当前健康状况。策略可以基于某些协议，即 HTTP(S)、TCP 或 SSL。

我们还需要配置标准来告知健康检查多久检查一次实例、可以接受无响应的时间以及对其探针的连续失败次数。这些设置定义了 VM 何时会被归类为不健康。如果特定实例不再健康，自动扩缩器将添加一个新实例：

![](img/de2c97ab017f36b3750a5bade111edd7_118_0.png)

Figure 4.44 – Autohealing configuration

在健康检查中，我们还必须指定健康阈值。此数值用于确定在将不健康实例标记为健康并将流量导向它之前，必须返回多少次连续成功的检查。

然后我们可以将此健康检查作为自动修复策略应用于我们的实例组。以下屏幕截图显示了本例中选择的健康检查：

![](img/de2c97ab017f36b3750a5bade111edd7_119_0.png)

Figure 4.45 – Autohealing information

自动修复策略还可以提高应用程序的可用性。在下一节中，我们将查看 Compute Engine 上的容器。

## Containers on Compute Engine

作为运行 GKE 的替代方案，GCP 提供了在 Compute Engine 上运行容器的能力。容器将在第 6 章《Managing Kubernetes Clusters with Google Kubernetes Engine》中更详细地介绍；不过，重要的是要注意，容器可以在 Linux 和 Windows 服务器 VM 实例或 Compute Engine 内部提供的容器优化操作系统上运行。Docker 和 Podman 是两种常见的容器技术，可让我们运行容器化应用程序。

我们应注意，Container-Optimized OS 确实存在一些限制：

- 它不包含包管理器，因此我们不能直接在实例上安装软件包。
- 它不支持执行非容器化应用程序。
- 内核被锁定。
- 它在 GCP 环境之外不受支持。

## Running a simple Windows container on Compute Engine

假设我们有一台正在运行的 Windows VM，并使用 *Windows Server 2019 DC core for containers* 镜像。我们可以像在 *Deploying our first GCE instance* 部分中所做的那样开始 RDP 到它。由于容器运行的是 Windows 的核心版本，因此它运行的是具有最小 UI 的操作系统。我们能看到的只有命令提示符窗口。

如果我们运行 `docker images` 命令，我们可以看到默认安装了哪些镜像：

![](img/de2c97ab017f36b3750a5bade111edd7_120_0.png)

让我们创建一个新镜像并运行它：

- 1. 运行 `mkdir my-new-container` 命令。
- 2. 运行 `cd my-new-container` 命令。
- 3. 运行 `mkdir content` 命令。
- 4. 运行 `notepad content\index.html` 命令。
- 5. 输入以下文本以创建一个简单的 **Internet Information Services (IIS)** 服务器：

```
<html>
    <head>
        <title>Windows containers</title>
    </head>
    <body>
        <p>This is my test windows container!</p>
    </body>
</html>
```

- 6. 使用记事本创建一个名为 Dockerfile 的文件。

