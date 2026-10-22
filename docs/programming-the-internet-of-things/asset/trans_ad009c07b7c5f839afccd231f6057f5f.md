

Operating System doesn’t support this, each test will result in negative value, as follows:

```
Test 1: CPU Util not supported on this OS: -1.0
Test 2: CPU Util not supported on this OS: -1.0
Test 3: CPU Util not supported on this OS: -1.0
Test 4: CPU Util not supported on this OS: -1.0
Test 5: CPU Util not supported on this OS: -1.0
```

## NOTE

你可能还记得 psutil 是你在设置 virtualenv 环境时导入的库之一，并且使用 pip 安装了 basic_imports.txt。这个库使你能够监控系统指标，例如 CPU 利用率和内存利用率。你可以在网上阅读其所有功能（https://psutil.readthedocs.io/en/latest/#）。

仔细查看指定以下代码行的操作：

```
puUtilPct = self.perfMgr.cpu_percent()
```

该库会将所有核心聚合为单一的 CPU 百分比，这对你作为开发者来说非常方便。这个类本质上就是返回这一行代码的值的简化。

好吧，在我们继续之前，为什么要在代码周围加上花哨的包装？这不是过度设计吗？简单来说，是的。但你会看到一种模式在形成，你将用类似的代码来表示其他更复杂的传感器。

每个练习的一个目标是建立 *控制分离* 的模式，或将关键功能分离，以便它们可以与应用程序其余逻辑分开管理和更新。

> **NOTE**
> 
> 就像任何软件设计一样，复杂性、巧妙编码和直接完成工作之间存在平衡。我将在整本书中尝试把握这种平衡。你可能有不同的想法——太好了！我鼓励你考虑如何以其他方式实现每个练习以满足你的特定需求。这正是编程富有创造性和乐趣所在。

## CREATE THE SYSTEMMEMUTILTASK MODULE

我相信你已经注意到了这里的模式。我们正在 CDA 中复制的一些功能也在 GDA 中复制。这是由分布式物联网设备以及各种操作系统或运行时环境的性质决定的，正如我在前言中提到的。

下一个任务是创建内存利用率收集逻辑。让我们将其命名为 *PIOT-GDA-02-005 - Create module SystemMemUtilTask*，并使用以下内容作为描述：

## Description

- 创建 SystemMemUtilTask 模块并实现检索 JVM 内存利用率的功能。

## Actions

- 在 programmingtheiot.gda.system 包中，创建一个名为 SystemMemUtilTask 的新 Java 类。
- 添加以下 import 语句：
- import java.lang.management.MemoryUsage;
- 添加 getTelemetry() 方法。它将检索 JVM 内存利用率并以 float 形式返回该值。使用以下代码作为值：ManagementFactory.getMemoryMXBean().getHeapMemoryUsage().getUsed()

## Estimate (Small = < 2 hrs ; Medium = 4 hrs ; Large = 8 hrs )

- Small

## Tests

- 运行 SystemMemUtilTaskTest 单元测试。它应该通过并记录 0.0% 到 100.0% 之间的值。

关键功能封装在以下代码行中：

```
memUtilPct = self.perfMgr.virtual_memory().percent
```

你可以从对 virtual_memory() 的调用中提取其他属性，并且欢迎你进行实验。现在，只需返回百分比利用率即可。

确保你的 SystemMemUtilTaskTest JUnit 测试全部通过后再继续。现在，让我们将 SystemCpuUtilTask 和 SystemMemUtilTask 都连接到 SystemPerformanceManager。

## RUNNING REPEATABLE TASKS IN JAVA

虽然你的 GDA 并没有收集传感器数据，但它显然需要收集和评估其自身的系统性能（例如 CPU 和内存利用率，当然）。这种类型的功能通常在后台以固定间隔收集，就像 Python 一样，Java 提供了创建轮询系统的选项。

幸运的是，没有必要导入任何单独的库，因为这是内置于核心 Java SDK 中的。

## NOTE

Java 的并发库非常强大，允许你使用基本的 Timer 功能以及 ScheduledExecutorService（如果你真的想，当然也可以创建自己的线程轮询系统）。我们将使用 ScheduledExecutorService，因为它提供了一种半保证的方式以固定间隔轮询，为我们处理了大部分复杂性。现代 Java 虚拟机会在 CPU 架构上处理负载分配，意味着如果可能的话它将利用多个核心。

## INTEGRATING SYSTEMCPUUTIL TASK AND SYSTEMMEMUTIL TASK WITH SYSTEMPERFORMANCEMANAGER

在本节中，你将创建两个非常相似的任务：*PIOT-GDA-02-006 - Connect SystemCpuUtilTask and SystemMemUtilTask to SystemPerformanceManager。*

这个练习涉及的内容稍微多一些，即使完成它不需要太多代码行。这是因为你将同时使用并发和 *Runnable* 实现，

后者只是一个接口定义，用于可以由 Java 线程一次或多次调用的方法。

让我们使用以下描述来记录这项工作：

## Description

- 将 SystemCpuUtilTask 和 SystemMemUtilTask 连接到 SystemPerformanceManager，并在管理器启动时启动的线程中调用每个实例的 handleTelemetry() 方法，并在管理器停止时停止。此工作应在 SystemPerformanceManager 类中实现。

## Actions

- 添加以下 import 语句：

```
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.ScheduledFuture;
import java.util.concurrent.TimeUnit;
```

- 向类添加以下成员：

```
private ScheduledExecutorService schedExecSvc = null;
private SystemCpuUtilTask sysCpuUtilTask = null;
private SystemMemUtilTask sysMemUtilTask = null;
private Runnable taskRunner = null;
private boolean isStarted = false;
```

- 创建一个名为 handleTelemetry() 的公共方法，包含以下内容：
  - cpuUtilPct = this.cpuUtilTask.getTelemetry()
  - memUtilPct = this.memUtilTask.getTelemetry()
  - 记录一条包含 cpuUtilPct 和 memUtilPct 值的 info 消息

- 在构造函数中，添加以下内容：

```
this.schedExecSvc =
Executors.newScheduledThreadPool(1);
this.sysCpuUtilTask = new SystemCpuUtilTask();
this.sysMemUtilTask = new SystemMemUtilTask();
this.taskRunner = () -> {
    this.handleTelemetry();
};
```

- 在 startManager() 方法中，添加以下内容：

```
if (! this.isStarted) {
    ScheduledFuture<?> futureTask =
    this.schedExecSvc.scheduleAtFixedRate(this.taskRunner,
    0L, this.pollSecs, TimeUnit.SECONDS);
    this.isStarted = true;
}
```

- 在 stopManager() 方法中，添加以下内容：
  - this.schedExecSvc.shutdown();

## Estimate (Small = < 2 hrs ; Medium = 4 hrs ; Large = 8 hrs )

- Medium

## Tests

- 在 GatewayDeviceAppTest 测试用例中，注释掉 testRunGatewayApp() 单元测试前的 @Test 注解，并取消注释 testRunTimedGatewayApp 单元测试前的 @Test 注解。运行后者测试——它应产生与以下类似的输出：

```
Jul 19, 2020 1:53:19 PM
programmingtheiot.gda.app.GatewayDeviceApp <init>
INFO: Initializing GDA...
Jul 19, 2020 1:53:19 PM
programmingtheiot.gda.app.GatewayDeviceApp
parseArgs
INFO: No command line args to parse.
Jul 19, 2020 1:53:19 PM
programmingtheiot.gda.app.GatewayDeviceApp
initConfig
INFO: Attempting to load configuration: Default.
Jul 19, 2020 1:53:19 PM
programmingtheiot.gda.app.GatewayDeviceApp
startApp
INFO: Starting GDA...
Jul 19, 2020 1:53:19 PM
programmingtheiot.gda.system.SystemPerformanceManager
  startManager
INFO: SystemPerformanceManager is starting...
Jul 19, 2020 1:53:19 PM
programmingtheiot.gda.app.GatewayDeviceApp
startApp
INFO: GDA started successfully.
Jul 19, 2020 1:53:20 PM
programmingtheiot.gda.system.SystemPerformanceManager
  handleTelemetry
INFO: Handle telemetry results: cpuUtil=-1.0,
memUtil=6291456.0
Jul 19, 2020 1:53:50 PM
programmingtheiot.gda.system.SystemPerformanceManager
  handleTelemetry
INFO: Handle telemetry results: cpuUtil=-1.0,
memUtil=6291456.0
Jul 19, 2020 1:54:20 PM
programmingtheiot.gda.system.SystemPerformanceManager
  handleTelemetry
INFO: Handle telemetry results: cpuUtil=-1.0,
memUtil=6291456.0
```

