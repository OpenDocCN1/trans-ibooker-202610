

以下是我们在侦察阶段应努力回答的几个关键问题：Web 服务器是否甚至拥有 GraphQL API？GraphQL 配置在哪个端点接收查询？GraphQL 实现是用什么语言编写的？目标服务器上运行的是哪种 GraphQL 实现？该实现是否已知容易受到某些攻击？针对特定的 GraphQL 实现存在哪些类型的防御？该实现的开箱即用默认配置设置有哪些？GraphQL 服务器是否部署了任何额外的安全保护层？能够回答这些问题将使我们能够针对目标服务器制定更集中的攻击计划并发现其防御中的漏洞。

> NOTE
> 在本章以及后续章节中，我们将使用 DVGA 作为我们的目标易受攻击应用。你应该已经在第 2 章构建的 GraphQL 安全实验室中将其运行起来。

## 检测 GraphQL

要在渗透测试中检测 GraphQL，首先熟悉当今野外存在的 GraphQL 服务器实现非常重要。GraphQL 存在多种用各种编程语言编写的实现，每种实现都可能具有不同的默认配置或已知弱点。表 4-1 列出了几种 GraphQL 实现及其所使用的语言。

表 4-1：GraphQL 服务器实现及其编程语言

| Server implementation | Language |
|---|---|
| Apollo | TypeScript |
| Graphene | Python |
| Yoga | TypeScript |
| Ariadne | Python |
| graphql-ruby | Ruby |
| graphql-php | PHP |
| graphql-go | Go |
| graphql-java | Java |
| Sangria | Scala |
| Juniper | Rust |
| HyperGraphQL | Java |
| Strawberry | Python |
| Tartiflette | Python |

这些是当今使用最广泛的实现，以及更小众的实现，例如 Scala 的 Sangria、Rust 的 Juniper 和 Java 的 HyperGraphQL。稍后在本章中，我们将讨论如何在渗透测试期间区分它们。

检测 GraphQL API 可以通过几种方式进行：手动进行，如果网络上有超过几个主机，通常更难扩展；或自动进行，使用各种 Web 扫描器。使用 Web 扫描工具的优势在于可扩展性。它们是多线程的，并且通常能够读取外部文件作为程序输入，例如包含要扫描的主机名列表的文本文件。这些工具已内置了检测 Web 界面的逻辑，并且使用脚本语言（如 Bash 或 Python），你可以以编程方式针对数百个 IP 地址或子域运行它们。在本章中，我们将使用流行的扫描器，如 Nmap，以及 GraphQL 专用扫描工具，如 Graphw00f，进行侦察。

## 常见端点

在第 1 章中，我们强调了 REST 和 GraphQL API 之间的一些差异。其中一个与侦察阶段相关的差异是，GraphQL API 端点通常是静态的，最常见的是 /graphql。

然而，虽然 /graphql 通常是默认的 GraphQL 端点，但 GraphQL 实现可以重新配置为使用完全不同的路径。在这些情况下，我们该如何检测它？一种方法是手动尝试几个常见的 GraphQL API 替代路径，例如版本化端点：

+   /v1/graphql
+   /v2/graphql
+   /v3/graphql

你通常会在应用程序需要支持其 API 的多个版本时看到这些版本化 API 端点，无论是为了向后兼容，还是为了以不与客户可能仍在使用的稳定 API 版本冲突的方式引入新功能。

另一种找到 GraphQL 实现的方法是通过 IDE，例如 GraphQL Playground 或 GraphiQL Explorer，我们在第 1 章中使用它们来尝试 GraphQL 查询。当启用其中任一界面时，它通常会使用一个额外的专用端点。这意味着 GraphQL 也可以存在于以下端点下：

+   /graphiql
+   /playground

如果这些端点也恰好被版本化，它们可能在其路径前加上版本号，例如 /v1/graphiql、/v2/graphiql、/v1/playground、/v2/playground，等等。

清单 4-1 显示了 Graphene，一种基于 Python 的 GraphQL 实现，如何公开两个端点，一个用于 GraphQL，另一个用于内置于 Graphene 的 GraphiQL Explorer：

```
app.add_url_rule('/graphql', view_func=GraphQLView.as_view(
    'graphql',
    schema=schema
))

app.add_url_rule('/graphiql', view_func=GraphQLView.as_view(
    'graphiql',
    schema = schema,
    graphiql = True
))
```

清单 4-1：Graphene 的端点定义

Graphene 将 /graphql 端点定义为其主要的 GraphQL 查询端点。然后它将 /graphiql 定义为第二个端点，GraphiQL Explorer 将对其进行查询。最后，它启用了 GraphiQL Explorer 界面。当客户端浏览到 /graphiql 端点时，GraphQL 服务器会向客户端呈现 IDE。

请记住，每个端点可能具有不同的安全设置。例如，一个可能比另一个更严格。当你在同一目标主机上找到两个提供 GraphQL 查询的端点时，你需要分别测试它们。

NOTE 在本书的 GitHub 仓库中，你可以找到更全面的常见 GraphQL 端点列表：https://github.com/dolevf/Black-Hat-GraphQL/blob/master/ch04/common-graphql-endpoints.txt。你可以在渗透测试或漏洞赏金狩猎期间扫描 GraphQL 服务器时将其用作 wordlist 文件。

这里最重要的收获是，虽然 GraphQL 端点通常位于可预测的路径，但开发人员仍然可以自定义它以满足其需求，也许是为了将其隐藏在好奇的目光之外，或只是为了符合内部应用程序部署标准。

## 常见响应

现在你已经了解了 GraphQL 通常从哪些端点接收查询，下一步是学习 GraphQL API 如何响应数据包。GraphQL 在网络上很容易识别。这在执行零知识渗透测试或漏洞赏金狩猎时特别有帮助。

GraphQL 规范描述了查询响应结构应如何格式化。这允许 API 消费者在解析 GraphQL 响应时预期预定的格式。以下摘自 GraphQL 规范的摘录描述了查询响应的外观：

> If the operation is a query, the result of the operation is the result of executing the operation's top-level selection set with the query root operation type.

执行查询操作时可以提供初始值：

```
ExecuteQuery(query, schema, variableValues, initialValue)
```

+   1. Let queryType be the root Query type in the schema.
+   2. Assert: queryType is an Object type.
+   3. Let selectionSet be the top-level selection set in the query.
+   4. Let data be the result of running ExecuteSelectionSet(selectionSet, queryType, initialValue, variableValues) normally (allowing parallelization).
+   5. Let errors be any field errors produced while executing the selection set.
+   6. Return an unordered map containing data and errors.

在实践中，这意味着 GraphQL API 在有结果返回给客户端查询时将返回 data JSON 字段。当客户端查询执行期间发生错误时，它还将返回 errors JSON 字段。

提前知道这两条信息很有价值。简单来说，我们现在有了响应必须满足的两个条件，然后才能说它来自 GraphQL API：

+   1. 有效的查询响应应始终填充 data 字段，其中包含查询响应信息。
+   2. 无效的查询响应应始终填充 errors 字段，其中包含出错信息。

现在我们可以将这些作为扫描和检测逻辑的一部分，以自动化在网络上发现 GraphQL 服务器。所有我们需要做的就是发送一个有效或格式错误的查询并观察收到的响应。

让我们使用 HTTP POST 方法针对 DVGA 运行一个简单的 GraphQL 查询，以查看这些响应结构在实际中的表现。打开 Altair GraphQL 客户端并确保地址栏设置为 http://localhost:5013/graphql 地址；然后通过在 Altair 的左侧窗格中输入以下查询来运行查询：

```
query {
  pastes {
    id
  }
}
```

接下来，点击播放按钮将查询发送到 GraphQL 服务器。这应该返回 pastes 对象的 id 字段。你应该能够看到类似于以下输出的响应：

```
"data": {
  "pastes": [
  {
```

