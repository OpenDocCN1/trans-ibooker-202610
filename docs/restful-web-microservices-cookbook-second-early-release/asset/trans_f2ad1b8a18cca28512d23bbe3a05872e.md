

### 相关章节

- 第 2 章中的 TK
- 第 3 章中的 TK
- 第 4 章中的 TK

## 3.8 响应中的每个重要元素都需要一个标识符

正如 RESTful API 中的每个重要资源都有标识符（URL）一样，响应中的每个重要操作和/或数据元素也都应该有自己的标识符。客户端不仅需要能够定位到正确的资源，还必须能够在资源响应中找到并使用正确的操作或数据集合。

### 问题

如何确保客户端应用能够在响应中找到它们正在寻找的操作和/或数据？此外，如何确保随着服务的演进，查找这些重要元素的能力不会随时间而退化？

### 解决方案

确保 API 响应中拥有稳定且有意义的标识符的关键，是选择支持多种标识符类型作为结构元素的媒体类型。HTML 在这方面做得很好。我创建了机器可访问语义超媒体（MASH）格式，作为支持多种结构标识符的媒体类型的一个示例。

四种标识符是：

```
ID
```

文档级唯一的单值元素（例如 id=1q2w3e4r）

```
NAME
```

考虑这样一个场景：你需要更新十个或更多`user`账户的电子邮件地址。此外，你还必须处理多个版本的`user`服务，或者一个使用多种响应格式的服务，或者一个随着时间不断演进的服务。在这些情况下，客户端可能会使用如下脚本：

```text
# hyper-cli 示例

ECHO 正在为 mingles 更新电子邮件地址

ACTIVATE http://api.example.org/users

ACTIVATE WITH-FORM nickSearch
  WITH-QUERY {"nick":"mingles"}

ACTIVATE WITH-FORM userUpdate
  WITH-BODY
{"name":"Miguelito","nick":"mingles","email":"mingles@example.org
"}

ACTIVATE WITH-FORM emailSearch
  WITH-QUERY {"email":"mingles@example.org"}

ECHO 更新已确认

EXIT

# eof
```

注意在上述示例中，客户端应用依赖三个操作（`nickSearch`、`userUpdate` 和 `emailSearch`）以及一组属性（`name`、`nick` 和 `email`）的标识符。实际的表示格式（HTML、MASH、SIREN 等）对脚本来说并不重要——这是由客户端内部处理的。还要注意，提供的唯一 URL 是联系服务的初始 URL。其余 URL 都在响应中（作为表单的一部分）提供。

### 讨论

大多数 HTTP 设计实践都明确表示，每个*资源*都应该有自己的标识符（以 URL 的形式）。当你查看典型的 HTTP API 请求时，这一点很容易理解：

```
http://api.example.org/customers/123
http://api.example.org/users?customer=123
```

在描述单个响应中的交互时也是如此。例如，考虑来自 `/users?customer=123` 的以下响应：

```
{
  "id": "aqswdefrgt",
  "href": "http://api.example.org/customers/123",
  "name": "customer-record",
  "rel": "item http://rels.example.org/customer",
  "tags": "item customer read-only",
  "data": [
    {
      "identifier": "123",
      "companyName": "Ajax Brewing",
      "address": "123 Main St, Byt...ville, MD",
      "users": "http://api.example.org/users?customer=123"
    }
  ]
}
```

注意，该记录的元数据如下：

- `id`：文档级唯一 ID，客户端可用它在任何列表中定位此记录
- `href`：系统级 URL，客户端可用它从服务中检索此记录
- `name`：应用级语义名称，客户端可用它返回此“类型”的记录
- `rel`：系统级名称集合，客户端可用它返回此“类型”或实例的记录
- `tags`：应用级名称集合，客户端可用它以多种方式过滤记录

此处添加的 `href` 示例表明，系统级标识符（`http://api.example.org/customers/123`）不需要与内部 `id`（`aqswdefrgt`）相同。实际上，*假设*它们是解耦的，这是一个好主意，这样可以避免因服务迁移、安装新代理等导致 URL 变化时出现的问题。

### 相关章节

- 第 4 章中的 XXX
- 食谱 3.3

## 3.9 在响应中依赖超媒体控件

基于 REST 的服务的原则之一是依赖“超媒体作为应用状态的引擎”，或者如 Roy Fielding 所称的“超媒体约束”。发出超媒体格式响应的服务，使客户端应用更容易在*运行时*确定哪些操作是可能的——以及如何执行这些操作。而且响应中出现的超媒体信息可以是上下文相关的。上下文示例包括：1) 服务的当前状态（记录是否已存在？），2) 用户上下文（该用户是否有权限编辑记录？），3) 请求本身的上下文（是否已有其他人编辑过此数据？）。

### 问题

当服务提供者发出基于超媒体的响应时，API 消费者如何最好地利用所包含的信息来改善人工代理和机器代理的体验？

### 解决方案

最好将 API 消费者编写为“理解”响应格式（参见本章中的 TK）。对于超媒体类型，每种格式都有其自己的“超媒体签名”——即该媒体类型能够表达的超级媒体元素列表（称为超媒体因子或 H-Factor²¹）。

创建了解媒体类型并理解九个 H-Factor 的客户端应用，将大大提高客户端应用的灵活性和弹性。

RESTful 服务响应中可能出现九个 H-Factor。它们是：

**表 3-1. 五个链接因子**

| 标题 | 描述 | 示例 |
|---|---|---|
| L 链接<br>E 嵌入 | 将内容带入当前视图 | `HTML <img />` 标签 |
| L 链接<br>O 出站 | 将客户端导航到新视图 | `HTML <a />` 标签 |
| L 链接<br>T 模板 | 在执行前支持附加参数 | URI 模板<br>`http://example.org/users/{id}` |
| L 链接 非<br>N 幂等 | 描述非幂等操作 | `<FORM method=POST ...>` |
| L 链接<br>I 幂等 | 描述幂等操作 | `SIREN "actions":[{"name":"update-item","method":"put" ...}]` |

**表 3-2. 四个控制因子**

| 标题 | 描述 | 示例 |
|---|---|---|
| C 读取<br>R 请求控制 | 描述读取细节 | `Accept:application/vnd.hal+json` |
| C 更新<br>U 请求控制 | 描述写入细节 | `enctype="application/x-www-form-urlencoded"` |
| C HTTP<br>M 方法控制 | 描述要使用的 HTTP 方法 | `<FORM method="GET" ... >` |
| C 链接<br>L 关系控制 | 描述操作的链接关系 | `<link rel="create-form" ... />` |

### 图示

每种超媒体格式都有其自己的“H-Factor”签名。例如：

![](img/b73a1e885955ee1243f0097b04d814ff_122_0.png)

### 示例

了解你的客户端可能遇到哪些格式，可以让你对客户端进行编码，使其能够识别并在适当的时候向人工和机器驱动的代理提供选项，以便它们完成任务。

例如，**HyperCLI** 客户端（参见附录 TK）支持多种超媒体格式。当使用像 SIREN 这样的格式时，你可以编写超媒体客户端脚本，根据响应中的超媒体控件来添加、搜索、修改和/或移除内容。以下是用 HyperLANG 编写的一个简短的 SIREN 脚本。

```text
#
# SIREN 编辑会话
# 读取记录，保存，修改，写回服务器
#

# ** 发出初始请求
REQUEST WITH-URL http://rwrcbook10.herokuapp.com

# ** 获取列表中的第一条记录
REQUEST WITH-PATH $.entities[0].href

# ** 将项目属性推入堆栈
STACK PUSH WITH-PATH $.properties

# ** 修改堆栈上的 tags 属性值
STACK SET {"tags":"fishing,\\.\\skiing,\\.\\hiking"}

# ** 使用提供的编辑表单和更新后的堆栈发送更新
REQUEST WITH-FORM taskFormEdit WITH-STACK

# ** 确认更改
SIREN PATH $.entities[0]

# ** 退出会话
EXIT
```

注意，在上述示例中，只提供了起始 URL。其余操作都依赖于每个服务响应中提供的超媒体元数据来执行。

### 讨论

针对超媒体的编码已在本章的“超媒体部分”讨论过。本食谱涵盖了*使用*这些超媒体数据直接解决问题的原则。它依赖于服务提供的细节，因此，只有当服务在响应中支持超媒体格式时，本食谱才有帮助。

在脚本中依赖提供的超媒体的优势在于，你可以减少客户端应用中需要硬编码的工作量。你可以让服务更容易地修改超媒体细节（例如 URL、HTTP 方法、支持的格式等），而不会破坏客户端应用。在上面的示例中，脚本没有对 taskFormEdit 操作的 HTTP URL、方法或编码类型进行硬编码。它只是使用了服务提供的细节。

当客户端应用不仅理解响应格式（例如 SIREN），还理解服务使用的语义配置文件（主题领域）时，本食谱效果最佳。例如，在上面的示例中，客户端提前知道可能存在一个 taskFormEdit 超媒体

