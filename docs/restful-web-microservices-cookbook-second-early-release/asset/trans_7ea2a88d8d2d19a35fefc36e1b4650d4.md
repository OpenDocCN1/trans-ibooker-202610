

## 4.14 使用幂等创建提高可靠性

虽然在 Web API 中常见的做法是使用 HTTP POST 来创建新资源，但这并非最可靠的方式。事实上，当涉及机器对机器交互（例如，没有人类参与）时，使用 HTTP POST 可能会因“丢失响应”问题的可能性而带来麻烦。我们需要的是一种即使在网络连接本身可能出错的情况下，也能可重复且可靠地创建资源的解决方案。

### 问题

使用 HTTP POST 创建新资源时，可能会遇到“丢失响应”问题。例如，API 客户端发送一个 POST 请求，其请求体包含将 500 美元从账户 A 转账到账户 B 的操作，但该 API 客户端从未收到 HTTP 响应。没有 `200 OK`，没有 `400 Bad Request`，没有 `500 Server Error`——什么都没有。

那么客户端该怎么办？请求到底有没有到达服务器？服务器端是否发生了拒绝该请求的错误？如果请求到达了服务器并且已完成，但 `200 OK` 响应却在网络上丢失了呢？在后一种情况下，重复发送请求可能会导致转账被执行两次。

在向服务写入数据时，我们如何避免丢失响应问题带来的不良影响？

### 解决方案

HTTP POST 操作的响应丢失之所以麻烦，是因为服务器资源的状态不明确。服务器到底更新了没有？更糟糕的是，针对 POST 的 HTTP 规范⁷⁶并未将该方法定义为*幂等*。这意味着，与 HTTP PUT 不同，HTTP POST 请求无法在“客户端能够读取服务器响应之前发生通信故障时自动重复发送。”⁷⁷

> **提示** 本方案的一个重要元素是使用客户端提供的唯一 URL。更多细节请参见方案 4.13。

这个问题的解决方案是：只要 API 客户端想要修改服务上的资源，就使用 PUT。无论意图是创建新记录还是更新现有记录。由于 HTTP PUT 是幂等方法，客户端应用程序可以放心地重复发送那些旨在修改服务数据的请求，而不必担心会意外“重复发布”资源。

客户端应用程序还可以使用 `If-None-Match` 头来表示他们想要创建新资源，或者当客户端打算更新现有资源时使用 `If-Match` 头。

通过这种方式，客户端应用程序避免了丢失响应问题带来的不良影响。

### 示例

解决丢失响应问题的关键在于始终支持使用幂等的 HTTP 方法（PUT）向服务器写入数据。当您希望客户端应用程序创建新资源时，指示他们使用 PUT 方法，提供完整的 URL，并包含值为 `"*"` 的 `If-None-Match` 头。

下面是一个使用 PUT 创建 HTTP 资源的示例：

```
PUT /persons/q1w2e3r4
Host: api.example.org
Content-Type: application/json
Content-Length: XXX
If-None-Match: "*"

{"name":"Mark Morkelson"}
```

收到此请求后，API 服务可以检查所提供的 URL 处是否已有资源，如果与通配符实体标签（"*"）“没有匹配项”，服务器即可创建该资源并返回如下所示的响应：

```
201 Created
Location: http://api.example.org/persons/q1w2e3r4
```

但是，如果该 URL 处已存在资源，服务器可以返回以下响应：

```
409 Conflict
Content-Type: text/plain

Unable to create. Resource already exists.
```

当客户端应用程序想要更新现有资源时，他们需要在发送更新 PUT 请求时提供该资源的唯一实体标签（由服务器提供）。下面是一个示例交互：

```
**** REQUEST ****
GET /persons/q1w2e3r4
Accept: text/plain

**** RESPONSE ****
200 OK
Content-Type: application/vnd.collection+json
ETag: "w/p009i8u7y6yt5r4"

{"collection": {
  "items": [
    {"href" : "/persons/q1w2e3r4", "data" : [ {"name" : "Mark Morkleson"} ]}
  ],
  "template" : { "data" : [ {"name" : "Mork Markleson"} ] }
}}

**** REQUEST ****
PUT /persons/q1w2e3r4
If-Match: "w/p009i8u7y6yt5r4"
Content-Type: application/x-www-form-urlencoded
Accept: application/vnd.collection+json

name=Mork%20Markleson
```

```
**** RESPONSE ****
200 OK
Content-Type: application/vnd.collection+json
ETag: "w/i8u7y6t5r4e3"

{"collection": {
  "items": [
    {"href": "/persons/q1w2e3r4", "data": [ {"name": "Mork Markleson"} ]}
  ]
}}
```

请注意，在更新场景中，`If-Match` 头会与现有资源的 `ETag` 头的值一起发送。服务器在收到 PUT 请求后，可以使用 `If-Match` 头的值来确认资源存在——并且没有被其他更新过程修改过（如果被修改过，实体标签的值会发生变化）。如果更新失败（例如，不存在现有资源或实体标签不匹配），服务器可以返回 `409 Conflict` 响应（见上文）。

### 讨论

在创建记录时提供完整的 URL 可能在某些 API 设计者看来有些奇怪。但这通常是大多数 HTTP 客户端应用程序实现文件上传（图像、PDF 文档等）的方式。

本方案依赖于客户端应用程序与 HTTP 头（`ETag`、`If-None-Match` 和 `If-Match`）的交互。这对某些 API 消费者（例如，仅限于 HTML 表单的浏览器）来说可能是一个挑战。您仍然可以通过在服务器提供的表单中以隐藏字段的形式提供实体标签相关信息来使此方案生效。从技术上讲，您甚至也可以使用 HTTP POST 来实现本方案。HTML 浏览器中的实现如下所示：

```
<p>Create a New Person</p>
<form name="create" action="/persons/q1w2e3r4" method="post">
  <input type="hidden" name="if-none-match" value="*" />
  <input type="text" name="name" value="Mark Morkleson" />
  <input type="submit" />
</form>
```

```html
<p>Update an Existing Person</p>
<form name="update" action="/persons/q1w2e3r4" method="post">
  <input type="hidden" name="if-match" value="w/p009i8u7y6yt5r4" />
  <input type="text" name="name" value="Mork Markleson" />
  <input type="submit" />
</form>
```

虽然这在 HTTP POST 中可行，但我仍然鼓励您在实现服务器时改用 HTTP PUT，因为 PUT 被定义为幂等的（可可靠重复），而这种 POST 的使用方式只是按约定将交互“视为”可重复。

通过坚持对所有写操作使用幂等方法，服务还可以为更新提供自动重试支持。大多数 HTTP 库都支持 HTTP GET 的自动重试，但我还没有发现有哪个库也对 PUT 或 DELETE 这样做。有关此选项的更多信息，请参阅[方案 4.16](#)。

### 相关

- [第 2 章](#)
- [第 3 章](#)
- [第 4 章](#)
- [方案 4.16](#)
- [方案 4.13](#)
- [待添加链接](#)

## 4.15 支持服务间状态传输的互操作性

在 Web 上创建一组可互操作服务的一个重要部分是确保每个服务既能独立运行，又能被纳入更大的目标或解决方案中。我们需要确保我们的服务 API 不会假设唯一有效的用例是客户端被该服务“锁定”。服务需要被视为整体的“一部分”，而不仅仅是“整体”。

### 问题

我们如何确保服务接口在服务被纳入更大解决方案的一部分时仍然能够正常工作？这种解决方案由其他人（API 消费者应用程序）设计，并与其他 API 集成。如何在维护系统安全和数据完整性的同时，确保我们的服务能够轻松与之前未见过且不受我控制的其他服务集成？

### 解决方案

确立您的服务应能够与其他未知服务互操作的原则可能是一个挑战。最简单且最安全的做法是将 API 实现为一组独立的、无状态的操作。这些操作可以接受输入、执行动作、返回结果，然后完全忘记整个过程。

计算值是服务之间简单、无状态传输的一个好例子。考虑一个邮政编码验证服务，它可以支持传入包含完整地址（可能还有一些身份元数据）的请求体，并返回相关的邮政编码。所有需要的状态数据都在请求/响应对中传输。

然而，有时您需要将一大块数据传输到服务中，让该服务对数据进行一段时间的操作，然后导出结果。在这些情况下，更显式的导入/导出操作可能是一个不错的解决方案。

在服务之间传输状态的最简单方法是使用*接收*状态信息的服务所提供的表单来传递数据属性。从根本上说，这正是 HTML 和其他超媒体格式的工作方式。然而，一些服务可能需要传输额外的元数据才能处理传入的数据，或者可能不支持原生的超媒体控件。在这些情况下，您需要在接口中设计额外的支持来显式地传输状态数据。

创建他人易于使用的服务的次简单方式是支持这样一个步骤：允许将状态从一个服务传输到

