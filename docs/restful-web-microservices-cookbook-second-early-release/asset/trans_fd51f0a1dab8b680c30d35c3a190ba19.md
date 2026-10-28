
依赖使用预定义语义配置文件来提供响应的服务的客户端应用程序，需要一种方法确保这些服务会以所需的词汇“说话”。

## 问题

你已编写了客户端，使其能够与任何符合（例如）To-Do 语义配置文件的服务协作。你如何在运行时确认即将使用的服务支持 To-Do 词汇？

## 解决方案

客户端应用检查服务是否支持语义配置文件的一种可靠方法是使用“配置文件协商”模式。客户端可以使用 `accept-profile` 请求头来表示期望的语义配置文件，服务可以使用 `content-profile` 响应头来表示支持的语义配置文件。

与内容协商（参见 TK）类似，配置文件协商允许客户端和服务器共享有关资源表示的元数据细节，并决定服务器的响应是否可为客户端接受。当服务返回的资源配置文件与客户端请求不匹配时，客户端可以用错误拒绝响应，向服务器请求更多信息，或者继续处理。

## 示例

客户端可以通过多种方式确定服务资源支持的语义配置文件。配置文件协商是客户端指示并验证服务配置文件支持的最直接方式。

下面显示了一个配置文件协商的简单示例：

```http
*** REQUEST
GET /todo/list HTTP/1.1
Host: api.example.org
Accept-Profile: <http://profiles.example.org/to-do>

*** RESPONSE
HTTP/2.0 200 OK
Content-Profile: http://profiles.example.org/to-do

...
```

在上面的示例中，客户端使用 `accept-profile` 头来表示所需的词汇，服务使用 `content-profile` 头来告知客户端，用于组合所返回资源表示的配置文件是什么。

当客户端请求的服务不支持的语义配置文件时，服务器也可能会返回 406 HTTP 状态码（Not Acceptable）。此时，服务应返回元数据，以指示该服务准备支持哪些配置文件。

```http
*** REQUEST
GET /todo/list HTTP/1.1
Host: api.example.org
Accept-Profile: <http://profiles.example.org/to-do/v3>

*** RESPONSE
HTTP/2.0 406 Not Acceptable
Content-Type: application/vnd.collection+json

{ "collection":
  {
    "links" : [
      {"rel":"profile",
       "href":"http://profiles.example.org/todo/v1"},
      {"rel":"profile",
       "href":"http://profiles.example.org/todo/v2"}
    ]
  },
  "error" : {
    "title" : "Unsupported Profile",
    "message" : "See links for supported profiles for this resource."
  }
}
```

服务也有可能为客户端应用提供一种方式，以预先请求受支持语义配置文件的详细信息。为此，服务可以提供具有“profile”关系值的一个或多个链接，每个链接指向一个受支持的语义配置文件文档。示例如下：

```json
{
  "collection": {
    "title": "Supported Semantic Profiles",
    "links": [
      {"rel":"profile", "href":"http://profiles.example.org/todo/v1"},
      {"rel":"profile", "href":"http://profiles.example.org/todo/v2"},
      {"rel":"profile", "href":"http://profiles.example.org/todo/v3"}
    ]
  }
}
```

这些链接可以出现在任何支持该配置文件的资源表示中。即使只支持一个语义配置文件，在响应中发出“配置文件链接”也是一个好主意，以帮助客户端（及其开发人员）识别受支持的词汇。在这种情况下，客户端可以查看配置文件链接集合，以确定其所需的语义配置文件标识符是否位于响应所列出的内容中。

受支持的配置文件列表也可以通过 HTTP 头来报告。

```http
*** REQUEST
GET /todo/list HTTP/1.1
Host: api.example.org
Accept-Profile: <http://profiles.example.org/to-do/v3>

*** RESPONSE
HTTP/2.0 406 Not Acceptable
Content-Type: application/vnd.collection+json
Links <http://profiles.example.org/todo/v3>; rel="profile",
<http://profiles.example.org/todo/v2>; rel="profile",
<http://profiles.example.org/todo/v1>; rel="profile"
```

## 讨论

对于“按配置文件编写”的客户端应用来说，验证所有传入响应是个好主意，以确保客户端能够正确处理资源。因为服务器实际报告的格式可能不止 HAL、SIREN、Collection+JSON 等。这意味着需要在运行时识别和处理多种消息格式。

## 问题

需要能够处理多种消息类型的客户端应用，如何请求并验证从服务接收的消息类型，并一致地处理来自不同格式的信息？

## 解决方案

运行的应用程序应能使用 HTTP `Accept` 和 `Content-Type` 头来管理资源表示格式（HTML、HAL、SIREN、Collection+JSON 等）。这种对以消息为中心的编码（参见第 3.3 节）的关注将为你的客户端代码增加稳定性，而不会将其与所使用服务的特定领域紧密绑定。

> **媒体类型优先开发**
> 
> 在 Web 上管理消息格式的第一步，是利用 HTTP 的 Accept 和 Content-Type 头向服务发出格式偏好信号，并验证服务返回的表示格式。

每当你的客户端发出 API 请求时，应包含 HTTP `Accept` 头，以指示你的应用程序“理解”的结构化格式。然后，当服务器回复时，应检查 `Content-Type` 头，以确认返回的是你支持的哪种格式。如果返回的格式不是你的客户端能够处理的，你应该停止处理并报告问题。在由人驱动的应用中，通常通过弹出消息或内联内容来完成。对于机器对机器的应用，可以通过向调用机器返回错误消息和/或将日志消息发送给某个监管机构来完成。

```http
*** REQUEST
GET /todo/list HTTP/1.1
Host: api.example.org
Accept: application/vnd.collection+json

*** RESPONSE
HTTP/1.1 200 OK
Content-Type: application/vnd.collection+json
...
```

客户端应用还需要以某种方式编码，以在服务之间交换的消息格式与信息的内部表示之间创建关注点分离（SoC）。当服务器返回响应时，客户端应用可以将消息体路由到适当的处理程序进行处理。这是将消息转换为内部模型的地方，客户端应用可以使用该模型来检查和操作响应。这意味着需要在内部实现消息转换器模式，以便客户端能够在内部对象模型和外部消息之间进行一致的转换。

## 示例

下面是示例代码，用于验证服务响应的媒体类型并相应地处理它。

```javascript
function handleResponse(ajax,url) {
  var ctype
  if(ajax.readyState===4) {
    try {
      ctype = ajax.getResponseHeader("content-type").toLowerCase();
      switch(ctype) {
        case "application/vnd.collection+json":
          cj.parse(JSON.parse(ajax.responseText));
          break;
        case "application/vnd.siren+json":
          siren.parse(JSON.parse(ajax.responseText));
          break;
        case "application/vnd.hal+json":
          hal.parse(JSON.parse(ajax.responseText));
          break;
        default:
          dump(ajax.responseText);
          break;
      }
    } catch(ex) {
      alert(ex);
    }
  }
}
```

请注意，在上面的示例中，客户端可以使用三种消息格式“交流”：Collection+JSON、SIREN 和 HAL。如果服务返回其他任何内容，应用程序会将其发送到 `dump()` 例程进行额外处理。

## 讨论

当服务支持高度结构化的格式（如 HTML、HAL、SIREN、Collection+JSON 等）时，组织客户端代码以由消息格式驱动效果最佳。当服务使用非结构化的响应格式（如 XML 和 JSON）时，你需要额外的元数据来支持消息驱动的客户端代码。在这种情况下，像 XML Schema 13 和 JSON Schema: 14 这样的模式文档会非常有用。有关在客户端使用模式文档的更多信息，请参见第 3.7 节。

识别传入的消息格式仅仅是第一步。你还需要能够解析并经常转换该消息以供内部使用。对于 Web 浏览器，你可以构建一个单一的 Javascript 库，将传入的 JSON 格式（HAL、SIREN、Collection+JSON）转换为 HTML 进行渲染。下面是用于将基于 JSON 的 API 消息转换为 HTML 的高级方法：

```javascript
// collection+JSON-->HTML
function parse(collection) {
    var elm;

    g.cj = collection;

    title();
    content();
    links();
    items();
    queries();
    template();
```
