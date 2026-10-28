

## 3.12 使用文档模式验证传出消息

客户端应用程序对于向服务器*发送*有效消息（尤其是消息*正文*）非常重要。确保发送有效的消息正文的一种方法是使用模式文档验证这些正文。但在发送请求正文时使用模式验证总是好主意吗？

### 问题

客户端应用程序如何在调用服务请求时确保发送格式良好且有效的请求正文？

### 解决方案

使用 POST、PUT 和 PATCH 等 HTTP 方法发送格式良好且有效的请求正文很重要。为了降低请求被拒绝的可能性，你应该在将请求正文发送到服务进行处理之前验证其结构和内容。本质上，*客户端*应用程序有责任正确构造有效的请求正文，而*服务*的角色是尽力处理所有有效请求。这是 Postel 定律（又称健壮性原则）²⁷ 的体现。

满足此要求非常有效的方法是使用模式文档来确认消息正文的有效性。JSON 和 XML 都有强大的模式规范，并且有大量工具支持它们。

客户端需要处理的确认有两个层级：1) 文档是否格式良好？以及 2) 文档是否有效？格式良好的文档具有正确的*结构*。这意味着它可以成功解析。有效文档不仅格式良好，而且*结构元素*（例如 `street_name`、`purchase_price`）和这些元素的*值*都是正确的数据类型并且在可接受的值范围内（例如 `purchase_price` 是大于零且小于一千的数值）。

对于任何客户端应用程序预期发送请求消息正文的情况，这些客户端应在将请求提交到互联网之前执行某种类型的验证。如果正文格式为 JSON 或 XML 格式，使用模式文档和验证器库是可行的方法。对于其他格式（`application/x-www-form-urlencoded` 等），客户端应用程序应依赖自己的本地验证例程来确认消息正文格式良好且有效。

### 示例

以下是使用 FORMS+JSON 格式描述的简单消息正文示例：

```json
{
  id: "filter",
  name: "filter",
  href: "http://company-atk.herokuapp.com/filter/",
  rel: "collection company filter",
  title: "Search",
  method: "GET",
  properties: [
    {name: "status", value: ""},
    {name: "companyName", value: ""},
    {name: "stateProvince", value: ""},
    {name: "country", value: ""}
  ]
}
```

以下是可用于验证消息正文的基本 JSON Schema 文档：

```json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type": "object",
  "properties": {
    "status": {
      "type": "string",
      "enum": ["pending", "active", "closed"]
    },
    "companyName": {
      "type": "string"
    },
    "stateProvince": {
      "type": "string"
    },
    "country": {
      "type": "string"
    }
  }
}
```

### NOTE

本书篇幅有限，无法涵盖 JSON Schema 主题。要深入了解此主题，我建议你查看（TK ORM content?）

下面是一个客户端可能发送的可能正文：

```json
{
  "status":"pending",
  "country":"CA"
}
```

最后，这里是我经常使用的流行 JSON 验证器（ajv²⁹）的示例：

```javascript
/*
 * load the schema file from external source
 * pass in the JSON message to send
 * process and return results/errors
 */

function jsonMessageCheck(schema, message) {
  var schemaCheck = ajv.compile(schema);
  var status = schemaCheck(message);
  return {status:status,errors:schemaCheck.errors};
}
```

### 使用 XML Schema 验证 XML 消息

通过 XML Schema 文档和工具处理 XML 验证也有类似的选项。有几种方法。应用程序应查找服务响应中的模式引用。常见位置有：

- 在 HTTP Link 头中：

```
**** REQUEST
GET /api.example.org/users/search HTTP/1.1
Accept: application/forms+json;

*** RESPONSE
200 OK HTTP/1.1
Content-Type: application/forms+json;
Link: <schemas.example.org/service1/user-schema.json>;
profile=schema

{...}
```

- 作为响应正文的一部分：

```
{
  "id" : "za1xs2cd3",
  "type": "search",
  "schema" : "api.example.org/schemas/user-search.json",
  "links" : [
    {
      "id" : "q1w2e3r4",
      "name" : "user",
      "href" : "http://api.example.org/q1w2e3r4",
      "title" : "User Search",
      "method" : "GET",
      "properties": [
        {"name":"familyName", "value":""},
        {"name":"givenName", "value":""},
        {"name":"sms", "value":""},
      ]
    }
  ],
  ...
}
```

使用 JSON/XML Schema 验证消息正文可能很棘手。例如，JSON Schema 比 XML Schema 更宽容。虽然有一些工具支持在运行时将 XML Schema 转换为 JSON Schema，但很少能很好地工作。最好继续以其原生格式编写模式。

将 `application/x-www-form-urlencoded` 转换为 JSON 的技巧只要生成的 JSON 被视为简单的名称/值对就有效。大多数表单格式的消息遵循此模式，但在某些情况下，服务会使用暗示嵌套层次结构的 FORM 字段名称进行创造。例如：

```
user.familyName=mamund&user.givenName=ramund&user.sms=+1234567890
```

意味着服务器端消息看起来像这样：

```json
{
  "user": {
    "familyName": "mamund",
    "givenName": "ramund",
    "sms": "+1234567890"
  }
}
```

随着服务在命名模式上变得更聪明，建议将 FORM 字符串转换为 JSON 文档的技巧变得越来越不可靠。为避免此问题，最好编写基于代码的验证器。

在最佳情况下，服务会为客户端应用程序开发人员提供正确编写（并且是最新的）模式文档以在运行时使用。如果目标服务未执行此操作，应用程序开发人员应查看服务 API 的人类可读文档并为运行时使用制作自己的模式文档。这些文档应发布在某个在线位置（可能在受保护的 URL 后面），客户端应用程序可以访问。还必须注意，随着服务随时间变化，保持这些模式文档为最新。

> **WARNING**
>
> 遇到疑问时，直接编写自己的自定义消息检查代码以避免因编写不佳或维护不足的模式文档而出现任何问题。

### Protocol

响应是否包含预期的 HTTP 协议级详细信息（HTTP 状态码、内容类型和任何其他重要头）？

### Structure

响应正文（如果存在）是否由预期的结构元素组成？例如，是否存在预期的 `title` 属性？正文是否包含一个或多个具有预期 `rel`、`name` 或其他属性的 `link` 元素，等等？

### Value

假设预期的结构元素存在（例如链接、表单、数据属性），这些元素是否包含预期的*值*（例如 `rel="self"`）？对于数据属性，元素是否包含预期的数据类型（字符串、整数等），并且数据属性的值是否在可接受的范围内？例如，Date 类型属性是否包含有效的日历日期？数值是否在可接受的上下限范围内？

### Example

下面是验证传入消息的协议、结构和值的简化版本。

```javascript
var response = httpRequest(url, options);
var checks = {};
var message = {};

// protocol-level checks
checks.statusOK = (response.statusCode === 200 ? true : false);
checks.contentTypeCollectionJSON = (
  response.headers["content-type"].toLowerCase().indexOf("vnd.collection+json")
    ? true : false
);
checks.semanticTypeTaxes = (
  response.headers["profile-type"].toLowerCase().indexOf("taxes.v1.alps")
    ? true : false
);
```
