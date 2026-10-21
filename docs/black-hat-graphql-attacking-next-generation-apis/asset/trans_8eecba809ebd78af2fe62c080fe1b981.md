

# 理解编辑距离算法

为了确定拼写错误是否与模式中的有效对象、字段或参数相似，GraphQL 实现依赖于简单的编辑距离算法。理解编辑距离可以帮助我们优化用于从字段建议中发现名称的暴力破解脚本。

该匹配算法比较任意两个字符串，并根据使它们匹配所需的字符操作数量返回它们的相似度。从一个字符串中添加、替换或删除一个字符都算作一次操作。例如，要将错误的字段名 titlr 与正确的名称 title 匹配，我们需要将字符 r 替换为 e，由此得到的编辑距离为 1。表 6-1 显示了额外的字符串比较及其对应的编辑距离。

## 表 6-1：两个字符串之间的编辑距离

| String | Typo | Operations | Edit distance |
|---|---|---|---|
| title | titl | Add e | 1 |
| content | rntent | Replace r with c, add o | 2 |

GraphQL 实现使用可变的编辑距离阈值，根据清单 6-4 中所示的公式计算，以决定是否显示字段建议。此示例直接取自 GraphQL 参考实现 GraphQL.js 的源代码。

```
const threshold = Math.floor(input.length * 0.4) + 1;
```

清单 6-4：来自 GraphQL.js 的编辑距离阈值代码片段

此代码取字符串的长度，将其乘以 0.4，使用 Math.floor 函数向下取整，然后加 1。例如，像 content 这样七个字符的字符串必须具有 3 或更小的编辑距离阈值才能触发相关的字段建议。

## 优化字段建议使用

你会发现很有用的是，单个拼写错误可以返回多个字段名称。GraphQL 将返回所有可能与提供的拼写错误匹配的字段。例如，以下查询请求了来自 pastes 顶级字段的拼写错误的 owne 字段（owner）：

```
query {
  pastes {
    owne
  }
}
```

这个单个 owne 拼写错误在 owner 和 ownerId 字段的编辑距离阈值范围内。当这种情况发生时，GraphQL 实现不知道客户端想要请求哪个字段，因此它返回两者：

```
{
  "errors": [
    {
      "message": "Cannot query field \"owne\" on type \"PasteObject\". Did you mean \"owner\" or \"ownerId\"?",
      "locations": [
        {
          "line": 24,
          "column": 3
        }
      ]
    }
  ]
}
```

另一个有用的事实是，客户端在单个请求中发送的拼写错误数量没有限制。对于每个拼写错误，GraphQL 服务器都会尝试建议自动更正。例如，在以下请求中，我们发送了一个包含多个字段的查询，所有字段都有拼写错误：

```
query {
  pastes {
    tte
    tent
    bli
    urn
  }
}
```

GraphQL 服务器分析每个拼写错误并返回编辑距离阈值内的所有可能的字段建议列表。这种 GraphQL 响应行为允许批量信息收集：

```
{
  "errors": [
    {
      "message": "Cannot query field \"tte\" on type \"PasteObject\". Did you mean \"title\"?",
      --snip--
    },
    {
      "message": "Cannot query field \"tent\" on type \"PasteObject\". Did you mean \"content\"?",
      "locations": [
      --snip--
      ]
    },
    {
      "message": "Cannot query field \"bli\" on type \"PasteObject\". Did you mean \"public\"?",
      --snip--
    ]
  ]
}
```

```
},
{
  "message": "Cannot query field \"urn\" on type \"PasteObject\".\n    Did you mean \"burn\"?",
--snip--
  ]
}
```

第 5 章讨论的查询批处理可以让你通过在单个 HTTP 请求中批处理许多请求来进一步优化此类攻击。

## 考虑安全发展

截至本文撰写时，正在进行的安全发展可能会影响未来字段建议的使用。2019 年 11 月 5 日，针对 GraphQL 参考实现 GraphQL.js 中字段建议的使用提出了一个 GitHub 问题。

该问题指出，攻击者可以通过发送无效的 GraphQL 文档来探测服务器的模式详细信息。它引用了一个文件 didYouMean.ts，该文件被多个验证规则使用。此文件可以在开发 API 时为开发人员提供有用的建议，但也可以用于泄露信息。

作为回应，GraphQL 联合创始人 Lee Byron 评论如下：

> I would expect that a schema with introspection disabled would also disable didYouMean. I can’t think of a reason why you would want to disable introspection but enable didYouMean or vice versa.

在支持 Byron 对此事看法的评论线程之后，于 2022 年 1 月 28 日提出了一个拉取请求，以在禁用内省时禁用字段建议。如果合并，此拉取请求将使在禁用内省时滥用字段建议变得困难。

虽然这一变化是 GraphQL 安全性的积极发展，但我们黑客应该考虑几个要点。首先，在问题首次提出后，社区花了两年多时间才开发出潜在的解决方案。在像 GraphQL 这样以开源和社区驱动的技术中，重大安全问题不一定会迅速得到修补。

其次，虽然在 GraphQL 参考实现中得到解决，但此补丁在所有服务器实现和 GraphQL 使用的生产部署中获得广泛采用可能需要时间。

现在，如果同时禁用内省和字段建议，我们该如何继续探索目标的模式？在下一节中，我们将深入研究另一种技术，我们可以用它来潜在地发现看似无害的 GraphQL 查询背后的敏感信息。

## 使用字段填充

字段填充是一种 GraphQL 信息泄露技术，其中字段列表被插入到 GraphQL 查询中。我们可以使用字段填充来潜在地发现诸如密码、密钥和 PII 等敏感信息，通过猜测并将这些潜在的字段名称传递到我们知道有效的查询请求中。

例如，假设我们通过使用 Burp Suite 在观察目标上正常用户操作如何工作时拦截流量，从而捕获了以下查询。这是查找信息泄露漏洞的良好初始步骤。（第 2 章解释了如何使用 Burp Suite 拦截流量。）

```
query {
  user {
    name
  }
}
```

像这样的查询可能返回一些无害的内容，比如当前登录用户帐户的名称。而且由于内省被禁用，我们不能确定在这个 user 对象中还有哪些诱人的字段可供我们使用。

字段填充可能允许我们绕过这一点。从本质上讲，此技术利用了 GraphQL 模式中对象的字段可能与数据库列密切映射的可能性。表 6-2 显示了一个示例 MySQL 数据库模式，可能代表我们的用户表。

## 表 6-2：示例用户表 MySQL 数据库模式

| MySQL schema | GraphQL type and field |
|---|---|
| id BIGINT(20) | User.id (Int) |
| name VARCHAR(50) | User.name (String) |
| mobile VARCHAR(50) | User.mobile (String) |
| email VARCHAR(50) | User.email (String) |
| password_hash VARCHAR(32) | User.password_hash (String) |
| registered_at DATETIME | User.registered_at (custom DATETIME scalar type or String) |
| last_login DATETIME | User.last_login (custom DATETIME scalar type or String) |
| intro TEXT | User.intro (String) |
| profile TEXT | User.profile (String) |
| api_key VARCHAR(50) | User.api_key (String) |

为了表示整数和字符串，MySQL 使用 BIGINT 和 VARCHAR 等类型，而 GraphQL 使用 Int 和 String 等标量类型。MySQL 还有用于日期和时间等事物的内置类型，使用 DATETIME 数据类型。在 GraphQL 中，我们可能需要使用自定义标量类型，例如 DATETIME，或 String 标量类型。序列化为实际日期时间表示将由应用程序的逻辑完成。

作为攻击者，我们显然事先不知道数据库模式是什么，但我们可以对这些额外的数据库列可能是什么做出有根据的猜测，并开始将它们的可能字段名称填充到查询中。以下是添加到我们用户查询中的潜在字段名称列表：

```
query {
  user {
    name
    username
    address
    birthday
    age
    password
    sin
    ssn
    apiKey
    token
    emailAddress
    status
  }
}
```

请注意你尝试的字段名称的格式。SDL 文件中的字段和参数通常以 snake_case 样式设置，在这种样式中，每个空格被下划线 (_) 符号替换，并且每个单词的首字母为小写。例如，API 密钥字段很可能定义为 api_key。然而，当作为客户端查询 GraphQL API 时，这些字段和参数可能以 camelCase 显示，在这种样式中，由多个单词组成的名字连接成一个没有标点符号的单词，并且该单词的首字母为小写（也称为 lowerCamelCase）。这是因为某些 GraphQL 实现会自动转换字段和参数的样式。然而，命名约定可以更改，因为它们完全取决于应用程序维护者。有关命名约定的更多信息可以在 https://graphql-rules.com/rules/naming 上找到。

将数百个潜在字段名称填充到单个查询中，就像蒙着眼睛玩飞镖游戏并希望击中靶心一样。如果我们幸运的话，我们的一个或多个查询字段将解析并返回数据（或可能甚至建议一些在编辑距离阈值内的替代字段名称）。

## 类型填充在 __type 元字段中

在本章前面，我们提到某些应用程序在尝试禁用内省时可能无法拒绝使用 __type 元字段的查询。如果是这样，我们可以使用与字段填充类似的技术来获得

