
* 组件仅提供（网络）API。您选择的云提供商的任何托管服务都属于此类。

无论您拥有代码并可以决定发出何种信号类型，还是在使用您不拥有的组件，在某个时间点，您都希望收集信号并将其发送到中央位置进行处理和存储。这就是遥测部分。

在可观测性方面，您希望采取以投资回报率（ROI）驱动的方法。换句话说，您不仅需要了解可用的不同信号，还需要了解每种信号类型的成本以及它们使您能够做什么。

在本章中，我们将回顾最常见的信号类型，检查如何对每种信号进行仪器化并收集它们，并讨论这样做的成本和收益。

## 2.1 Reference example

虽然有多个信号需要考虑，但实际上最常用的三个是日志、指标和追踪。在本章中，我们将使用一个运行中的参考示例，即一个名为 `echo` 的简单 Go 程序，公开 HTTP 接口，以查看三种常见信号类型的实际应用。

进入 `ch02/base/` 目录，您可以在其中找到第 2.1 节所示的 `main.go` 程序。随着我们的进展，我们将使用不同的信号对参考示例进行仪器化，目标是使用开源工具（包括 Grafana、Prometheus 和 Jaeger）使用和交互信号。

Listing 2.1 示例 Go 程序以供仪器化

```
package main

import (
    "fmt"
    "log"
    "math/rand"
    "net/http"
    "time"
)

func handleEcho(w http.ResponseWriter, r *http.Request) {
    message := r.URL.Query().Get("message")                   ❶
    if rand.Intn(100) > 90 {                                  ❷
        w.WriteHeader(http.StatusInternalServerError)
        return
    }
    fmt.Fprintf(w, message)                                   ❸
}

func main() {
    rand.Seed(time.Now().UnixNano())
    http.HandleFunc("/echo", handleEcho)                      ❹
    log.Fatal(http.ListenAndServe(":4242", nil))              ❺
}
```

❶ 从查询参数 `message` 中检索要回显的负载，其结果是可以向 `/echo?message` 发出 HTTP GET 请求来与服务交互

❷ 模拟 10% 的错误情况

❸ 在 90% 的情况下返回我们检索到的确切负载

❹ 为 `/echo` 路径注册处理器

❺ 启动 HTTP 服务器，该服务器将在所有可用网络接口上的 4242 端口提供服务

首先，让我们构建 `echo` 服务，如以下列表所示。

Listing 2.2 构建我们的示例 Go 程序

```
$ go build -o echosvc .     ❶
```

❶ 构建可执行文件，然后它将作为 `./echosvc` 可用在当前目录中

现在，您可以在同一目录中使用 `./echosvc` 运行服务。要通过其 HTTP API 调用它，您可以使用以下代码（在单独的终端会话中）：

```
$ curl localhost:4242/echo?message=test

test
```

默认情况下，我将使用 Grafana 作为目标，因为它允许我们以统一的方式可视化和交互各种数据源。这将允许您免费使用开源重现所有内容。现在我们已经介绍了参考示例，让我们在进入仪器化本身之前谈谈评估仪器化成本的方法。

### Tip

要在 Grafana 中看到后续的信号，您需要对 `echo` 服务施加一些负载。一个选项是直接从 shell 执行类似以下操作（每秒调用一次端点）：

```
while true ; do curl -s -o
/dev/null localhost:4242/echo ; sleep 1 ; done
```

另外，您可以使用更高级的东西，例如像 Salvo 这样的完整测试工具（[https://github.com/tarekziade/salvo](https://github.com/tarekziade/salvo)）甚至 K6（[https://k6.io/docs/testing-guides/api-load-testing/](https://k6.io/docs/testing-guides/api-load-testing/)）。

## 2.2 Assessing instrumentation costs

为了评估每种信号类型的仪器化成本，我们将使用一个简单的度量：*业务逻辑与仪器化*（B2I）比率。

### The B2I ratio

要计算 B2I 比率，请确定添加仪器化（为信号类型添加发出信号的代码）之前的代码行数（LOC），然后确定仪器化后的 LOC。B2I 比率如下

```
B2I = LOC_AFTER_INSTRUMENTATION/LOC_BEFORE_INSTRUMENTATION
```

在理想世界中，B2I 比率将为 1，表示代码中仪器化成本为零。然而，实际上，您用于仪器化的 LOC 越多，B2I 比率就越高。例如，如果您的代码有 3800 行 LOC，并且您为仪器化添加了 400 行 LOC（例如，用于发出日志和指标），那么您的 B2I 比率将为 1.105，计算方式为 (3800 + 400) / 3800。

关于 B2I 有两点值得提及：

* 您是否要在 B2I 比率计算中考虑遥测库，例如 OpenTelemetry SDK（[https://opentelemetry.io/docs/instrumentation/](https://opentelemetry.io/docs/instrumentation/)），由您决定。从务实的观点来看，专注于您自己的代码（排除这些依赖项）可能最有意义。在本章的剩余部分，我将这样做。
* B2I 比率是一个简单的度量，绝不应作为代码仪器化程度好坏的指标，也不应告诉您代码是否 *过度仪器化*。古德哈特法则（参见 Strathern and Marilyn, 1997, “Improving Ratings: Audit in the British University System,” *European Review*, John Wiley & Sons）指出，当一个度量成为目标时，它就不再是一个好的度量——这里适用。用 B2I 比率作为回归验证中的粗略指南，而不是绝对的北极星。

在理论和概述之后，让我们现在回顾三种基本的信号类型：日志、指标和追踪。在本章的剩余部分，我们将对每种信号类型执行以下操作：

1. 定义信号特征和语义。
2. 讨论如何对我们的示例 `echo` 服务进行仪器化。
3. 设置遥测以收集和路由信号。
4. 分析信号的成本（以资源使用和工作量衡量）和收益。
5. 解释相应的信号如何有助于可观测性解决方案。

## 2.3 Logs

让我们首先关注日志。日志是具有文本负载的信号，用于捕获事件。*事件* 是代码中发生的任何感兴趣事情的统称，无论是在正常路径（完成的上传）还是意外或错误行为（“failed to store items in shopping basket”）。

日志带有时间戳，并且应该是结构化的，最好包括一些上下文（例如标签），可用于与其他信号进行关联。通常，人类会消费日志，要么直接通过搜索事件，要么间接通过关联。

### Note

有许多方式来指代单个日志事件，包括日志行、日志项、日志条目，以及我可能尚未遇到过的其他名称。为了保持简单，我们将在此一致使用 *事件* 一词，但这些术语在很大程度上是可以互换的。

在日志的上下文中，我们通常区分 *级别*，它可以表示日志事件的性质。不同的编程语言、库和框架随着时间的推移建立了约定；然而，以下是一些常见语义：

* 与开发或修复错误相关的细粒度日志事件（`DEBUG`）
* 生产类型日志事件，包括以下内容：
    * 预期行为（通常为 `INFO`）
    * 意外行为，例如 `WARNING` 或 `ERROR`
