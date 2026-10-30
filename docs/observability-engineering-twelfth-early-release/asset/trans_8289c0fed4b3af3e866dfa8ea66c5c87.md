

你可以给这些头部起任何你想要的名字，只要接收端的程序理解相同的名称即可。通常，HTTP 头部遵循特定的标准，例如万维网联盟（[W3C](W3C)）或 [B3](B3)。对于我们的示例，我们将使用 B3 标准。我们需要发送以下头部（见 [图 6-3](Figure 6-3)）以确保子 span 能够正确构建并发送它们的 span：

```
X-B3-TraceId
```

包含整个 trace 的 trace ID（来自前面的示例）

```
X-B3-ParentSpanId
```

包含当前 span ID，该 ID 将被设为子 span 生成的 span 的父 ID

现在让我们确保这些头部在我们的出站 HTTP 请求中被发送：

```
func callAuthService(originalRequest *http.Request, traceData map[string]interface{}) {
    req, _ = http.NewRequest("GET", "http://authz/check_user", nil)
    req.Header.Set("X-B3-TraceId", traceData["trace.trace_id"])
    req.Header.Set("X-B3-ParentSpanId", traceData["trace.span_id"])
    // ... make the request ...
}
```

我们也会对 `callNameService` 函数做类似的修改。这样，每当调用一个服务时，它就可以从这些头部中提取信息并将其添加到其自身生成的 `traceData` 中的 `trace_id` 和 `parent_id`。这些服务中的每一个也会将它们生成的 span 发送到 tracing 后端。在后端，这些 trace 会被拼接起来以创建我们想要看到的瀑布式可视化。

![](img/aaaa2d27e08e754554d501192f304d8f_107_0.png)

*图 6-3. 我们的示例应用现在会将 `traceID` 和 `parentID` 传播到每个子 span*

现在你已经看到了为仪器化并创建有用的 trace 视图所涉及的内容，让我们看看我们还可能想为 span 添加什么内容以使其对调试更有用。

## 向 Trace Span 中添加自定义字段

理解父子关系和执行持续时间是一个好的开始。但你可能希望在必要的 trace 数据之外添加其他字段，以便更好地了解在分布式系统中深埋的每个 span 的操作中发生了什么。

例如，除了存储服务名称以便于识别之外，了解执行工作的确切主机以及是否与特定用户相关可能很有用。让我们修改我们的示例，将这些细节作为 trace span 的一部分捕获，将它们作为键值对添加：

```
hostname, _ := os.Hostname()
traceData["tags"] = make(map[string]interface{})
traceData["tags"]["hostname"] = hostname
traceData["tags"]["user_name"] = name
```

你可以进一步扩展此示例以捕获任何你认为对调试相关的其他系统信息，例如应用程序的 build_id、instance_type、有关运行时环境的信息，或第 5 章中示例中列出的任何大量细节。目前，我们保持简单。

把所有内容放在一起，我们从头开始创建 trace 的完整示例应用看起来会像这样（代码为了清晰而重复且冗长）：

```
func rootHandler(r *http.Request, w http.ResponseWriter) {

    traceData := make(map[string]interface{})
    traceData["tags"] = make(map[string]interface{})

    hostname, _ := os.Hostname()
    traceData["tags"]["hostname"] = hostname
    traceData["tags"]["user_name"] = name

    startTime := time.Now()
    traceData["timestamp"] = startTime.Unix()
    traceData["trace_id"] = uuid.String()
    traceData["span_id"] = uuid.String()
    traceData["name"] = "/oauth2/login"
    traceData["service_name"] = "authentication_svc"

    func callAuthService(originalRequest *http.Request, traceData map[string]interface{}) {
        req, _ = http.NewRequest("GET", "http://authz/check_user", nil)
        req.Header.Set("X-B3-TraceId", traceData["trace.trace_id"])
        req.Header.Set("X-B3-ParentSpanId", traceData["trace.span_id"])
```

```
// ... make the auth request ...
}

func callNameService(originalRequest *http.Request, traceData map[string]interface{}) {
    req, _ = http.NewRequest("GET", "http://authz/check_user", nil)
    req.Header.Set("X-B3-TraceId", traceData["trace.trace_id"])
    req.Header.Set("X-B3-ParentSpanId", traceData["trace.span_id"])
    // ... make the name request ...
}

authorized := callAuthService(r)
name := callNameService(r)

if authorized {
    w.Write([]byte(fmt.Sprintf(`{"message": "Waddup %s"}`, name)))
} else {
    w.Write([]byte(`{"message": "Not cool dawg"}`))
}

traceData["duration_ms"] = time.Now().Sub(startTime)
sendSpan(traceData)
}
```

本节中使用的代码示例有点牵强，目的是说明这些概念在实践中如何结合起来。好消息是，你通常不必自己生成所有这些代码。分布式 tracing 系统通常有自己的支持库来完成大部分样板设置工作。

这些共享库通常是针对你希望使用的 tracing 解决方案的特定需求而定制的。不幸的是，特定于厂商的解决方案与其他 tracing 解决方案配合得不好，这意味着如果你想尝试不同的解决方案，就必须重新对代码进行仪器化。在下一章中，我们将研究开源的 OpenTelemetry 项目以及它如何使你只需仪器化一次即可使用各种解决方案。

现在你已经对仪器化和创建有用的 trace 视图所需的内容有了完整了解，让我们将其应用到你在上一章中学到的内容上，以了解为什么 tracing 是可观测系统中的关键要素。

## 将事件拼接成 Trace

在 [第 5 章](Chapter 5) 中，我们研究了结构化事件作为可观测系统的构建块的使用。我们将 *事件* 定义为记录了单个请求与你的服务交互时发生的一切的记录。在该请求生命周期内，为返回结果而发生的所有有趣细节都应附加到事件中。

在代码示例中，我们的函数非常简单。在真实应用中，单个请求执行过程中所做的每个服务调用都会有其自身的有趣细节：变量值、传递的参数、返回的结果、相关上下文等。每个事件都会捕获这些细节以及稍后允许你查看和调试分布式系统中各种服务和组件之间关系的 `traceData`。

在我们的示例中——以及一般分布式 tracing 系统中——我们使用的仪器化是在远程服务调用级别添加的。然而，在可观测系统中，你可以使用相同的方法将来自不同来源的任意数量的相关事件联系起来。例如，你可以通过应用这些相同的 trace 概念，将当前的单行日志解决方案迁移到更连贯的视图，从而迈出实现可观测性的第一步。

为此，你可以从生成非结构化多行日志迁移到生成结构化日志（见 [第 5 章](Chapter 5)）。然后你可以将 `traceData` 的相同必需字段添加到你的结构化日志中。完成此操作后，你可以从日志数据中生成相同的瀑布式视图。我们

在本章中，我们通过手工编码每个必要步骤的方式对 trace 进行了仪器化。开始使用 tracing 的更实际方法是使用仪器化框架。在下一章中，我们将研究开源且厂商中立的 OpenTelemetry 项目，以及你如何以及为何使用它来对生产应用进行仪器化。

## 第 7 章. 使用 OpenTelemetry 进行仪器化

## 早期发布读者的说明

使用 Early Release 电子书，你可以在这些标题正式发布之前很久就利用这些技术——作者在写作时提供的原始和未编辑内容，以便你能够提前利用这些技术。

这将是最终书籍的第 7 章。

如果你对我们如何改进本书的内容和/或示例有任何评论，或者如果你注意到本章中缺少材料，请联系编辑 vwilson@oreilly.com。

在前两章中，我们描述了结构化事件和 tracing 的原则。事件和 trace 是你可以用来自理解软件应用程序行为的可观测性构建块。你可以通过在应用程序中添加仪器化代码来在每次调用时发出遥测数据，从而生成这些基本构建块。然后你可以将发出的遥测数据路由到后端数据存储，以便稍后分析以了解应用程序健康状况并帮助调试问题。

在本章中，我们将向你展示如何对代码进行仪器化以发出遥测数据。你选择的方案可能取决于你的可观测性后端支持的仪器化方法。供应商通常会创建专有的 APM、指标或 tracing 库来为其特定解决方案生成遥测数据。然而，为了本厂商中立书籍的目的，我们将描述如何实现

