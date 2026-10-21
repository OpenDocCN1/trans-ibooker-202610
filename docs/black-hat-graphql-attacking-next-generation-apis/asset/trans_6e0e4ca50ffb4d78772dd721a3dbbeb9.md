

这个漏洞之所以特别有趣，是因为它利用起来非常简单直接。能够以未认证的方式进行攻击，也大大增加了其严重性。

## 通过 WebSocket 访问内省查询（Nuri）

这是一份有趣且相当独特的报告。2020 年 4 月，一位化名为 zerodivisi0n 的道德黑客披露了 Nuri API 中的一个漏洞，导致通过内省查询泄露了模式信息（https://hackerone.com/reports/862835）。该 GraphQL API 使用 WebSocket 作为传输协议，而不是 HTTP。

在前面的章节中，你已经在订阅操作的上下文中了解了 GraphQL 和 WebSocket；客户端可以通过 WebSocket 协议订阅某些感兴趣的事件以获取实时信息。某些 GraphQL 库，例如 graphql-ws（https://github.com/enisdenjo/graphql-ws），不仅允许通过 WebSocket 发送订阅，还允许发送查询和变更。

所报告的漏洞使黑客能够直接通过 WebSocket 连接执行内省查询。虽然报告中并没有详细介绍 GraphQL 实现的设计方式，但内省在非基于 WebSocket 的接口上是被禁用的，例如通过 HTTP 发送的查询操作。

通过 WebSocket 客户端到服务器消息的内省查询可能如下所示：

```
{"type":"start","payload":{"query":"query Introspection { __schema {...} }"}}
```

通过 WebSocket 发送的查询和变更操作目前并不常见。你更有可能看到 GraphQL 订阅操作通过 WebSocket 传输，但随着 GraphQL 趋势的发展，这可能会随时间而改变。

## 注入

以下公开披露的 GraphQL 漏洞导致了应用注入缺陷。第 8 章介绍了注入以及如果被利用会产生多大的影响。

## GET 查询参数中的 SQL 注入（HackerOne）

2018 年 11 月，Jobert 在 HackerOne 的 GraphQL 生产端点中发现了一个 SQL 注入（https://hackerone.com/reports/435066）。Jobert 发现了一个传递给 HackerOne GraphQL /graphql 端点的非标准参数 embedded_submission_form_uuid，看起来如下：

/graphql?embedded_submission_form_uuid=value

该 URL 参数在 GraphQL API 中并不标准，你更有可能看到以下参数：

- query
- variables
- operationName

你应该已经熟悉这些：query 采用其值作为完整的 GraphQL 查询，variables 采用传递给查询的额外数据（例如参数值），operationName 是操作的名称。Jobert 能够确定传递给自定义参数的值在后端未被检查，从而有效地允许他们注入 SQL 命令。

HackerOne 的分诊团队分享了负责处理 GraphQL 参数的 Ruby 代码，我们在此对其进行了修改以使问题更明显：

```
unless database_parameters_up_to_date
  safe_query = ''

  ① new_parameters = {"embedded_submission_form_uuid":"PAYLOAD"}

  new_parameters.each ② do |key, value|
    safe_query += "SET SESSION #{key} TO #{value};"
  end

  begin
    # safe_query ="SET SESSION embedded_submission_form_uuid TO PAYLOAD"
    connection.query(safe_query)
  rescue ActiveRecord::StatementInvalid => e
    raise e unless e.cause.is_a? PG::InFailedSqlTransaction
  end

end
```

new_parameters 变量 ① 是一个哈希映射，包含自定义的 embedded_submission_form_uuid URL 参数及其值（由客户端控制）。在 ② 处，一个循环对分配给该变量的键和值执行字符串插值，有效地将参数及其值组合成一个字符串。它将此字符串与 SET SESSION SQL 命令组合。

新的 SQL 命令最终被分配给 safe_query 变量，此时攻击者可以无需任何检查对其进行控制。我们使用注释突出显示了分配给该变量的值：GET 参数 embedded_submission_form_uuid 键及其值。该变量最终被转换为 SQL 查询并执行。GraphQL 参数也不会自动进行清理，这也促成了 SQL 注入条件。

Jobert 制作了一个特殊的 cURL 请求来验证注入：

```
time curl -X POST https://hackerone.com/graphql?embedded_submission_form_uuid=\n1%27%3BSELECT%201%3BSELECT%20pg_sleep\(10\)%3B--%27

0.02s user 0.01s system 0% cpu 10.557 total
```

此 cURL 请求的 URL 解码版本如下所示：

/graphql?embedded_submission_form_uuid=1';SELECT 1;SELECT pg_sleep\(10\);--'

该请求使用基于时间的 SQL 注入技术（第 8 章中介绍）通过使用 PostgreSQL 命令 pg_sleep 在服务器处理中引入了 10 秒的时间延迟。然后攻击者使用 Linux time 命令跟踪服务器响应请求所需的时间。完成耗时 10.557 秒。

该技术不仅证实了漏洞的存在，而且避免了意外泄露敏感信息或向数据库发送可能导致数据丢失的危险命令。

## 对象参数中的 SQL 注入（Apache SkyWalking）

Apache SkyWalking 是由 Apache 软件基金会创建的面向微服务和云原生架构的性能监控平台。2020 年 6 月，它因通过传递给 GraphQL 字段参数的值而引入的 SQL 注入漏洞而受损。该漏洞被分配了标识符 CVE-2020-9483。

SkyWalking 可以使用各种存储后端，例如 H2、OpenSearch、PostgreSQL 和 TiDB。一位化名为 Jumbo-WJB 的黑客发现，当 SkyWalking 与 H2 或 MySQL 存储后端一起使用时，它容易通过 getLinearIntValues 字段 metric 参数进行 SQLi。

Jumbo-WJB 发布了该漏洞的利用代码，在 GraphQL 查询中构造了一个特殊有效负载，利用该漏洞实现 SQLi。在以下示例查询中，你可以看到作为输入传递给 metric 参数的 id 的值包含 SQL 查询语法：

```
query SQLi($d: Duration!) {
  getLinearIntValues(metric:
  {name: "all_p99", id: "') UNION SELECT 1,CONCAT('~', '9999999999', '~')--"},
  duration: $d) {
    values {
      value
    }
  }
}
```

对于一个值，metric 参数期望一个包含 id 和 name 等键的对象。该漏洞似乎出现在 id 键中，该键在插入 H2 或 MySQL 数据库之前未经过清理。

NOTE 该漏洞的完整利用代码可以在 Nuclei 漏洞扫描器项目的公共存储库中找到，地址为 https://github.com/projectdiscovery/nuclei-templates/blob/master/cves/2020/CVE-2020-9483.yaml。

通过检查包含修复的 SkyWalking GitHub 仓库的拉取请求，我们可以了解易受攻击的代码区域可能是什么样子（图 10-5）。

```
110 public IntValues getLinearIntValues(String tableName, DownSampling downsampling, List<String> ids,
111 String valueCName) throws IOException {
112 - StringBuilder idValues = new StringBuilder();
113 - for (int valueIdx = 0; valueIdx < ids.size(); valueIdx++) {
114 - if (valueIdx != 0) {
115 - idValues.append(",");
116 }
117 - idValues.append("'").append(ids.get(valueIdx)).append("'");
118 }
119 
120 IntValues intValues = new IntValues();
121 
122 try (Connection connection = h2Client.getConnection()) {
123 
124 try (ResultSet resultSet = h2Client.executeQuery(
125 - connection, "select id, " + valueCName + " from " + tableName + " where id in (" + idValues.toString() + ")") {
```

图 10-5：Apache SkyWalking 的易受攻击代码

getLinearIntValues 方法接受一些参数，例如 tableName、valueCName 和 ids（第 110 行），并使用 Java 的 StringBuilder 进行一些字符串构建（第 112 行）。然后使用循环遍历传递给 ids 参数的值并通过连接它们并使用单引号装饰它们来构造一个字符串（第 113 至 118 行）。新构建的字符串最终被用作 SQL 查询的一部分而未进行清理（第 123 至 125 行）。

metric 对象的 id GraphQL 参数很可能直接插入到 ids 列表中，因此允许注入 SQL 命令。

## 跨站脚本（GraphQL Playground）

CVE-2021-41249 是影响 GraphQL Playground IDE 的反射型 XSS 漏洞，该 IDE 提供了发送查询到 API 的界面，以及原始模式信息、有关 API 功能的文档以及从内联 SDL 代码注释中获取的信息。此信息部分由在 GraphQL Playground 加载时自动发送的内省查询填充。其他信息可能来自 GraphQL 服务器。

与本章迄今为止介绍的漏洞相比，此漏洞相当不同。首先，它直接影响 API 消费者，因为成功的

