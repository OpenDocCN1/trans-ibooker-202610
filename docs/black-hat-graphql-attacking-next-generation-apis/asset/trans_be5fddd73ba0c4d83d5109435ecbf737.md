

documentation in Altair。图 8-4 显示了 Query 部分。文档中还包含 mutation 和 subscription 部分，请也查看一下。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_223_0.png)

图 8-4：DVGA 中的查询

如你所见，我们有几个可供选择的查询。现在我们必须确定优先关注的区域。请注意，某些字段，例如 systemUpdate、systemHealth、audits 和 deleteAllPastes，不接受任何参数，因此我们最好关注那些接受参数的字段。让我们聚焦于 pastes 字段，它接受三个可选参数：

- public，类型为 Boolean
- limit，类型为 Integer
- filter，类型为 String

filter 参数可能是 SQLi 测试的有价值候选，因为它接受字符串值，并且其名称暗示它会过滤结果。这种过滤可能涉及后端查询解析器逻辑，使用 SQL 操作，例如 SQL WHERE 运算符，以完成查询。

现在我们有了目标，让我们开始与 DVGA 交互并代理流量。通过 Kali 的 Applications 菜单打开 Burp Suite，然后点击 Open Browser 打开位于 Proxy 选项卡下的内置浏览器，并导航到 http://localhost:5013。应用程序加载后，确保 Burp Suite 处于 Intercept 模式。导航到 DVGA 左侧边栏中的 Private Pastes 页面。你应该会看到与图 8-5 类似的 GraphQL 请求。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_224_0.png)

## 图 8-5：在 Burp Suite 中拦截 GraphQL 查询

如你所见，DVGA 使用 GraphQL pastes 查询操作发送 HTTP POST 请求，以从 GraphQL API 服务器获取私有粘贴列表。

NOTE 如果你没有立即看到发往 /graphql 的初始 POST 请求，请点击 Forward 直到它出现。

如果切换到 WebSockets History 选项卡，你会注意到 DVGA 也使用了 subscription 操作（图 8-6）。在此上下文中，subscription 操作允许客户端通过订阅 pastes 事件，在粘贴创建后立即从 API 读取新的粘贴。

为了更轻松地操作请求，通过在请求窗口的任意位置右键单击并点击 Send to Repeater 将请求发送到 Burp Suite 的 Repeater。然后点击 Repeater 选项卡查看捕获的请求。这允许你按需重放请求。

让我们更改查询以使用 filter 参数。首先，将查询修改为如下所示：

```
query {
  pastes(filter:"My First Paste") {
    id
    content
    title
  }
}
```

图 8-6：Burp Suite 中的历史 WebSocket 流量视图

请注意，当查询包含双引号时，我们必须在 Burp 中使用反斜杠 (\ ) 字符转义引号，如图 8-7 所示。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_225_0.png)

图 8-7：使用 Burp Repeater 发送修改后的 GraphQL 查询

> NOTE
> 如果你使用的是 Burp Suite 的授权专业版，可以从 Burp 的 BApp Store 安装 GraphQL Raider 插件。Raider 允许你直接在 Burp Suite 中发送原生 GraphQL 查询，而无需担心字符转义。

点击 Send 将查询发送到 GraphQL 服务器。作为对此查询的响应，我们应该收到一个与我们的过滤搜索模式匹配的粘贴。更具体地说，它匹配 content 字段：

```
"pastes": [
  {
    --snip--
    "title":"Testing Testing",
    "content":"My First Paste"
    --snip--
  }
]
```

此过滤搜索模式表明幕后可能正在发生某种 SQL 查询，并且该查询的行为类似于以下内容：

```
SELECT id, content, title FROM pastes WHERE content LIKE 'My First Paste'
```

此 SQL 查询将从 pastes SQL 表中返回 id、content 和 title 列。使用 WHERE 运算符，结果将被过滤，仅返回 content 中包含字符串 My First Paste 的粘贴，如 LIKE 运算符所定义。

我们希望向应用程序投放一些字符，这些字符可能会破坏此查询并导致错误，这可能表明应用程序将我们的输入直接发送到查询中。例如，如果我们在搜索字符串后添加一个单引号 (')，SQL 查询就会被破坏，因为这会导致一个孤立的开头单引号而没有对应的闭合单引号。

让我们向 DVGA 发送以下查询，看看收到的响应（请注意单引号的添加）：

```
query {
  pastes(filter:"My First Paste'") {
    id
    content
    title
  }
}
```

在 Burp 中，修改请求使其看起来像图 8-8 所示。GraphQL 应返回包含应用程序错误的响应，通过 errors JSON 键揭示一些有趣的信息。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_227_0.png)

图 8-8：使用 Burp Suite 通过单引号破坏 SQL 查询

看起来我们的字符串导致 SQL 查询变得无效，因为它被直接注入到 SQL LIKE 搜索模式中。应用程序没有转义我们引入的单引号，这使我们能够完全破坏 SQL 查询。因此，SQLite（运行 DVGA 的 SQL 引擎）会抛出错误，正如错误输出中字符串 sqlite3.OperationalError 所显示的那样。

> NOTE 除了我们刚刚识别出的潜在 SQLi 漏洞外，这也是一个信息泄露漏洞，因为应用程序没有优雅地处理错误。泄露数据库信息的应用程序错误很有价值，因为它们为我们提供了数据库模式结构的洞察，例如后端数据库中存在哪些列或表。此信息可以帮助我们构建更精确的 SQLi 命令。

所以，我们认为我们找到了一个 SQLi 漏洞。那又怎样？好吧，我们可以通过更改 SQL 查询来检查是否能够从数据库中获取额外信息，例如返回所有粘贴：

```
query {
  pastes(filter:"My First Paste' or 1=1--") {
    title
    content
  }
}
```

现在 GraphQL 在解析传入的 GraphQL 查询后查询数据库时使用的 SQL 语句可能如下所示：

```
SELECT id, content, title FROM pastes WHERE content LIKE 'My First Paste' or 1=1--'
```

通过添加单引号，我们在 My First Paste 过滤模式之后立即结束 SQL LIKE 运算符。然后我们可以通过添加 1=1 比较引入一个使 SQL 查询始终为真的 or 条件。我们通过使用 SQL 中的注释双破折号 (--) 语法结束 SQL 查询，这会注释掉查询末尾的单引号，从而有效地确保我们的语法在修改后仍然有效。

图 8-9 显示了 Burp Suite 中此 SQLi 查询的外观及其结果。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_228_0.png)

图 8-9：使用 Burp Suite 成功进行 SQL 注入

服务器响应包含 DVGA 数据库中的所有粘贴！这是一个基于布尔的 SQLi 示例。

## 自动化 SQL 注入

其他工具尝试自动化 SQLi 漏洞的检测。特别是，SQLmap 可以帮助使用针对各种数据库引擎（如 MySQL、PostgreSQL、SQLite 等）定制的有效载荷模糊测试 GraphQL API。

在执行 SQLi 测试时，你可以采用任何潜在的 GraphQL 查询并使用星号 (*) 标记 SQLmap 应注入有效载荷的特定位置。例如，考虑以下片段：

```
query {
  pastes(filter:"test*") {
    id
  }
}
```

在此示例中，SQLmap 将用其 SQLi 有效载荷数据库中的条目替换星号。

SQLmap 可以从文件中读取完整的 HTTP 请求。我们可以获取任何 HTTP 请求并将其输入 SQLmap，SQLmap 将读取查询并使用它执行 SQL。图 8-10 显示了如何在 Burp Suite 中将请求保存到文件。右键单击请求窗口中的任意位置并选择 Copy to File。将文件命名为 request.txt 并保存。

