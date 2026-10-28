

```text
**** REQUEST ****
OPTIONS / HTTP/1.1
Host: api.example.org
...

**** RESPONSE ****
HTTP/1.1 200 OK
Content-Type: application/vnd.siren+json
Content-Length: XX
Cache-Control: max-age=604800
Allow: GET PUT DELETE HEAD OPTIONS
Accept: application/zip application/gzip
Accept-Charset: utf-8
Accept-Encoding: compress
Accept-Language: en
Link: <https://api.example.org/service-desc>; rel="service-desc"
```

有关使用 OPTIONS 方法的更多信息，请参见方案 4.5。

### 讨论

如果可能，最好将 SSD 文档存放在服务接口的根目录下。将文档命名为 service-desc 也会很有帮助。如果你始终如一地做到这两点，API 消费者就可以“假设”SSD 的位置，这对 API 开发者来说会更方便。

上述列表并非详尽无遗。读者可能正在使用其他一些 SSD 格式，一些大型组织甚至拥有并使用自己内部的 SSD。我收录了据我经验在“现实世界”中出现最频繁的 SSD。

在某些情况下，服务支持多种接口风格（例如 HTTP CRUD 和 GraphQL）。此时，你可以在响应的标头或正文中返回多个 SSD 文档：

```text
HTTP/1.1 200 OK
Content-Type: application/vnd.hal+json
Link: <http://api.example.org/openapi/service-desc>;
rel="service-desc openapi"
Link: <http://api.example.org/graphql/service-desc>; rel="service-desc sdl"
...
```

目前还没有用于指定特定 SSD 格式（OpenAPI、AsyncAPI 等）的预定义链接关系值。如果你为此使用了一些术语（如上例所示），请务必在 API 的共享词汇中包含这些术语（参见方案 4.7、方案 4.8）。

目前，超媒体风格的 API 还没有公认的标准 SSD 格式。我建议暂时改为在 API 的配置文件中包含一个 ALPS 文档（参见方案 4.7）。

### 相关

- 第 2 章
- 第 3 章
- 第 4 章
- [链接待补充]

## 4.10 发布 API 元数据

随着开放网络上应用接口数量的增长，需要一种通用格式和通用词汇来描述 API 的元数据。当服务提供商希望在无需线下会议、交互以及定制化描述和文档工作的情况下沟通关于 API 的重要细节时，这一点尤其重要。幸运的是，有一个强有力的候选方案可以承载这些信息：APIs.json 规范。65

### 问题

我们如何以标准化方式表示服务接口的重要元数据（支持的格式、词汇、文档、安全性等），让 API 消费者能够轻松找到并理解接口的元级细节？

### 解决方案

一个非常可靠的编目和发布 API 元数据的方法是使用 **APIs.json** 开放规范。⁶⁶ 它具有一种词汇表和结构布局，涵盖 API 的大多数重要元数据元素（基本定义、API 属性、重要 URL、关键联系人和维护者等）。它还是可扩展的，因此 API 提供方可以在需要时以向后兼容的方式扩展 **APIs.json** 文档以满足自身需求。

> ### APIS.JSON、APIS.YAML 与 APIS.TXT
>
> **APIs.json** 文档可以格式化为 JSON、YAML 或 TXT 文件。你可以访问规范网站⁶⁷了解更多关于 **APIs.json** 的信息。你还可以在 *http://apis.io/* 找到公开托管的 **APIs.json** 文档搜索引擎版本。

**APIs.json** 规范定义了文档的几个关键部分：

- **Root：** 关于 API 的名称、描述、URL 和其他基本信息
- **APIs：** 文档中标识的 API 列表
- **Tools：** 运营中可用的开源工具列表
- **Specifications：** 运营中采用的开源规范列表
- **Resources：** 可用于 API 运营的资源列表
- **Common：** 供所有 API 和工具使用的公共属性列表
- **Include：** 要包含在此文档中的其他 **APIs.json** 文档
- **Maintainers：** 维护此 API 的人员和/或组织集合

服务提供方可以在服务的根目录中提供一个名为 `apis.json` 的单一文档（例如 *http://api.example.org/apis.json*），用其记录 API 的元数据。API 还可以提供一个链接关系值，该值可以包含在 HTTP 标头和/或 HTTP 响应正文中。`service-meta` 关系值（定义

```http
HTTP/1.1 200 OK
Content-Type: application/apis+json
...

{
  "name": "Example API",
  "type": "Index",
  "description": "This is an example APIs.json file.",
  "image": "https://api.example.org/logo.jpg",
  "tags": ["Application Programming Interface", "API"],
  "created": "2014-04-07",
  "modified": "2020-09-03",
  "url": "http://example.com/apis.json",
  "specificationVersion": "0.14",
  "apis": [
    {
      "name": "Example API",
      "description": "This provides details about a specific API.",
      "humanURL": "http://example.com",
      "baseURL": "http://api.example.com",
      "tags": ["API", "Application Programming Interface"],
      "properties": [
        {"type": "Documentation", "url": "https://example.com/documentation"},
        {"type": "OpenAPI", "url": "http://example.com/openapi.json"}
      ],
      "contact": [{"FN": "APIs.json", "email": "info@apisjson.org"}]
    }
  ],
  "specifications": [
    {"name": "OpenAPI", "url": "https://openapis.org"},
    {"name": "JSON Schema", "url": "https://json-schema.org/"}
  ],
  "common": [
    {"type": "Signup", "url": "https://example.com/signup"},
    {"type": "Authentication", "url": "http://example.com/authentication"},
    {"type": "Login", "url": "https://example.com/login"}
  ],
  "maintainers": [{"FN": "Mark Morkelson", "email": "mork@example.org"}]
}
```

服务可以在 HTTP 响应中通告其提供了 **APIs.json** 文档：

```text
HTTP/1.1 200 OK
Content-Type: HTML
Link: <http://api.example.org/apis.json>; rel=service-meta
...
```

```html
<html>
    <head>
        <link rel="service-meta"
href="http://api.example.org/apis.json" />
    </head>
    <body>
        ...
    </body>
</html>
```

API 客户端应用还可以按照规范中的说明，通过请求文件的默认位置来自动“查找” **APIs.json** 文档：

```text
GET /apis.json HTTP/1.1
Accept: application/apis+json, application/yaml, text/plain
Host: http://api.example.org
```

### 讨论

需要特别指出的是，**APIs.json** 规范旨在承载与服务接口相关的信息，但并不是要详细*描述*或*定义*该接口。OpenAPI、AsyncAPI、Protobuf 等其他格式正是为此目的而设计的。有关服务定义格式以及如何与 API 消费者共享这些格式的详细信息，请参见方案 4.9。

> 警告
>
> 在撰写本文时，**APIs.json** 规范仍处于草稿模式——尚未定稿。不过，我发现该格式（以及相关的搜索引擎）稳定且有用，并建议读者持续关注这一规范。

采用 **APIs.json** 格式作为服务元数据的原因之一是，有一个相关的开源项目提供了一个搜索引擎，该搜索引擎

还有另一个可用于承载 API 元数据的规范：JSON Home 规范。⁷¹ 不过，这个规范提案已有数年没有更新，而且我并没有像经常看到这里讨论的 **APIs.json** 规范那样，经常看到 JSON Home 在“现实世界”中的示例。

### 相关

- 第 2 章
- 第 3 章
- 第 4 章
- 方案 4.9
- [链接待补充]
- [链接待补充]

## 4.11 支持服务健康监控

在 Web 上使用微服务的一个优点是，你不必自行编写所有功能；你可以依赖他人的创造力。然而，在自己的服务中增加依赖服务会增加分布式网络上发生故障的可能性。监控依赖服务状态的一个好方法是定期对其他服务进行“健康检查”，以确保它们正常运行。

### 问题

我们如何才能一致且定期地监控 Web 上服务的状态（或健康）？我们应该监控哪些常见值来评估服务的健康状况？传递这些状态值的标准格式是什么？我们如何通告对健康监控的支持，以及如何共享这些状态数据？

### 解决方案

