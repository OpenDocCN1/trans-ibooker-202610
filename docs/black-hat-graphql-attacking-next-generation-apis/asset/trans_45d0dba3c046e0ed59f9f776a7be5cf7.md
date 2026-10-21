

深入了解应用程序的 schema：即把潜在的类型名称塞进 __type 字段的 name 参数中。

让我们利用 DVGA 糟糕的禁用 introspection 方法，通过发送以下针对 PasteObject 的 __type introspection 查询来获取其 schema 中的字段列表：

```
{
  __type(name:"PasteObject") {
    name
    fields {
      name
    }
  }
}
```

此查询的响应应为我们提供 PasteObject 类型中所有字段名称的列表：

```
{
  "data": {
    "__type": {
      "name": "PasteObject",
      "fields": [
        {
          "name": "id"
        },
        {
          "name": "title"
        },
        {
          "name": "content"
        },
        {
          "name": "public"
        },
        {
          "name": "userAgent"
        },
        {
          "name": "ipAddr"
        },
        {
          "name": "ownerId"
        },
        {
          "name": "burn"
        },
        {
          "name": "owner"
        }
      ]
    }
  }
}
```

正如我们之前使用字段 stuffing 来识别字段名称一样，我们可以尝试不同的类型名称，直到找到一个存在的名称为止。就命名约定而言，GraphQL 中的类型名称通常以 UpperCamelCase 形式书写（例如，PrivatePasteProperties）。

我们现在已经具备了手动测试和分析 GraphQL 应用程序是否存在若干信息泄露弱点的理论知识。接下来，我们将研究如何将我们对 GraphQL 的新理解应用于利用自动化工具，使我们的攻击更高效。

## 使用 Clairvoyance 自动化字段建议和 Stuffing

Clairvoyance 可以利用字段建议和 stuffing 功能，从目标中发现有效的字段信息。在本节中，我们将使用 Clairvoyance 执行暴力请求。我们的目标是拼接多个建议，并在不依赖 introspection 的情况下尽可能多地发现 schema 信息。

Clairvoyance 以 wordlist 作为输入，并将其内容塞入多个 GraphQL 查询中，以识别任何有效的操作、字段、参数、输入类型和其他关键 schema 元素。在幕后，它使用正则表达式匹配错误消息中的有效字段，依赖于字段建议。一旦完成对整个 wordlist 的解析，它就会输出一个 schema。

我们可以使用此输出 schema 来探测敏感信息泄露的机会。

使用 Clairvoyance 等工具进行字段 stuffing 时，当使用的 wordlist 与我们针对的 GraphQL schema 的元素相匹配时效率最高。网上有许多 wordlist，但大多数是为猜测密码、目录或用户名而设计的。因为我们试图猜测字段、操作和参数的名称，所以我们可能使用通用英语词典单词列表会取得最大成功。

> NOTE
> 如果你针对的 GraphQL 应用程序不是用英语编写的，最好使用应用程序客户端或用户原生使用的语言的 wordlist。

一个合适的 wordlist 是 Derek Chuank 创建的高频词汇 wordlist。这个包含 30,000 个常见英语单词的列表是一个很好的起点。要获取此 wordlist，请运行以下命令：

```
# cd ~
# git clone https://github.com/nicholasaleks/high-frequency-vocabulary
```

现在我们有了可以使用的 wordlist，让我们将 Clairvoyance 投入行动并攻击 DVGA 实例。请记住，它应处于 Expert（加固）模式以禁用 introspection。

进入你安装 Clairvoyance 的目录，然后使用 -w（words）参数用 wordlist 对其执行 DVGA。-o 参数告诉 Clairvoyance 在运行时生成的 schema 应输出到何处：

```
# cd ~/clairvoyance
# python3 -m clairvoyance http://localhost:5013/graphql -w ~/high-frequency-vocabulary/30k.txt -o clairvoyance-dvga-schema.json
```

根据 wordlist 的大小，Clairvoyance 可能需要几分钟才能完成执行。完成后，你应该在 clairvoyance 目录中看到一个名为 clairvoyance-dvga-schema.json 的新文件。

让我们通过将 Clairvoyance 给我们的 schema 与从 introspection 查询生成的 schema 进行比较来测试我们 wordlist 的效率。为了最好地表示这些差异，我们可以利用位于 http://lab.blackhatgraphql.com:9000 或 https://ivangoncharov.github.io/graphql-voyager 的 GraphQL Voyager，并上传两个 schema。图 6-1 显示了 DVGA 的 schema，图 6-2 显示了 Clairvoyance 重建的 schema。

正如你所见，Clairvoyance 能够恢复 DVGA schema 的几乎每个字段和操作！对于一个未启用 introspection 的应用程序来说，这已经相当不错了。

另一个不错的选择是生成我们自己的 wordlist。如前所述，像 Clairvoyance 这样的工具的强大程度仅取决于我们提供的 wordlist。我们可以通过做出有根据的猜测，或通过从信息收集阶段收集的 HTTP 流量、静态文件和其他资源中提取关键字来扩展我们的列表。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_178_0.png)

图 6-1：原始 DVGA schema

![](img/2f244b00cd9f8f37cfc22f928aaa471a_179_0.png)

图 6-2：由 Clairvoyance 重建的 DVGA schema

像 Custom Word List Generator（CeWL）这样的工具，可以从应用程序的前端 HTML 中提取关键字，它已预装在 Kali 中。尝试使用以下一行命令来分析和提取 DVGA 界面的信息：

```
# cewl http://localhost:5013/
```

此命令将返回一个单词列表，你可以在手动字段 stuffing 攻击中使用它。或者，将其与你的 30,000 个单词列表合并并与 Clairvoyance 一起使用。你可以通过简单的 Bash 命令合并两个文本文件：

```
# paste -d "\n" wordlist1.txt wordlist2.txt > merged_wordlist.txt
```

## 利用错误消息

通过错误消息进行信息泄露是一种安全弱点，即应用程序或系统在错误消息中向最终用户泄露敏感信息。这些消息如果应用程序未正确处理，可能泄露诸如密钥、用户凭据、用户信息、数据库详细信息、应用程序环境变量以及文件或操作系统详细信息等数据。

正如我们通过对字段建议的探索所发现的，GraphQL 错误消息可能非常冗长。默认情况下，GraphQL 倾向于向客户端过度分享，以改善整体开发体验。通过了解 GraphQL 错误消息，我们可以利用它们所揭示的信息来开展攻击。

我们已经提到，GraphQL 错误消息与使用标准 HTTP 状态码的 REST 错误消息不同。根据规范，GraphQL 错误响应不需要 HTTP 状态码，通常只包含三个唯一字段：Message、Location 和 Path。要亲眼看到这一点，请尝试发送以下 mutation 在 DVGA 中创建新 paste。此请求缺少必需的 title 参数：

```
mutation {
  createPaste(content:"Hi", public: false) {
    paste {
      id
    }
  }
}
```

如果我们向 DVGA 发送此不正确的 mutation 请求，它将返回一个标准的错误 JSON 对象供我们分析。此错误响应应包含查询中识别出的所有错误的数组：

```
{
  "errors": [
    {
      "message": "mutate() missing 1 required positional argument: 'title'",
      "locations": [
        {
          "line": 2,
          "column": 3
        }
      ],
      "path": [
        "createPaste"
      ]
    }
  ],
  "data": {
    "createPaste": null
  }
}
```

错误响应格式可能包含 message、location 和 path 等特殊键。这些键向客户端提供错误的描述，以及查询中错误发生的位置：

message  message 字段是每个 GraphQL 错误中必需的，包含错误的 high-level 描述。在本例中，message 字段让我们知道我们的 mutation 操作缺少一个必需的位置参数 title。大多数信息泄露弱点都发生在 message 字段中，因此请务必留意它。

location  对于长而复杂的 GraphQL 文档（例如大型 fuzzing 文档），返回的错误响应可能

