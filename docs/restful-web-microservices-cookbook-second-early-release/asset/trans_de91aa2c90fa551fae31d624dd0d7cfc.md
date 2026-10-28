

```javascript
error();

elm = d.find("cj");
if(elm) {
    elm.style.display="block";
}
}

// HAL --> HTML
function parse(hal) {
    g.hal = hal;

    title();
    setContext();
    if(g.context!=="") {
        selectLinks("app", "toplinks");
        selectLinks("list", "h-links");
        content();
        embedded();
        properties();
    }
    else {
        alert("Unknown Context, can't continue");
    }

    elm = d.find("hal");
    if(elm) {
        elm.style.display="block";
    }
}

// SIREN --> HTML
function parse(msg) {
    var elm;

    g.siren = msg;

    title();
    getContent();
    links();
    entities();
    properties();
    actions();

    elm = d.find("siren");
    if(elm) {
        elm.style.display="block";
    }
}
```

每个消息解析器都是专门为将选定的消息格式转换为可在浏览器中渲染的 HTML 而设计的。这是消息转换器（Message Translator）模式[^15]的一种实现。对于高度结构化的格式，编写这些单向转换器并不具有太大挑战性。由于目标格式（HTML）并非领域专有，因此单个转换器即可有效地服务于任何应用主题或领域。我始终保留一个稳定的、基于浏览器的媒体类型解析器，以便为使用这些格式的服务快速构建渲染应用。

当需要将传入消息转换为特定领域的内部对象模型或图时，挑战会更大。在这种情况下，最好借助其他元数据，例如语义配置文件（参见后面的章节：*编写有效客户端以理解已注册词汇配置文件*）或模式文档（参见**方法 3.7**），来指导传入消息的转换。

当你的客户端只能处理非结构化文档（如 XML 或 JSON），*而且*没有提供可靠的模式或配置文件时，工作难度会成倍增加。在这种情况下，你需要花费时间基于人类可读的文档来创建自定义转换器。而且，文档中的小改动也很可能给自定义转换器带来大问题。

### 相关

- 方法 2.4
- 方法 2.2
- *TK：第 3 章，节：配置文件*
- 方法 3.7
- *第 4 章*

## 3.7 使用模式文档作为消息元数据的来源

当服务不支持语义配置文件或结构化媒体类型时，如果服务返回描述消息内容的模式文档引用，客户端应用程序仍然可以依据语义规范进行编码。

- 在配置文件（profile）和媒体类型（media-type）不足时使用模式
- 不要使用模式文档验证传入消息
- 要使用模式验证传出消息

### 问题

我们如何基于非结构化媒体类型（XML 或 JSON）的消息模式，而不是语义配置文件，来构建健壮的客户端应用程序？

### 解决方案

如果服务始终在运行时响应中返回带有非结构化消息格式（XML 或 JSON）的模式文档，那么即使没有语义配置文件的支持，你也有可能编写出健壮的客户端应用程序。为此，你需要组织完全由模式信息驱动的客户端代码。

这些模式格式最常见的例子是 XML Schema[^16] 和 JSON Schema[^17]。

### 示例

当服务提供这些模式文档时，你也可以将其作为传入消息的额外元数据信息。当响应本身包含指向模式的指针时，效果最佳。在运行时，不建议根据模式文档验证传入消息，除非你打算完全拒绝无效输入。通常，运行时响应会存在一些微小差异（例如，包含模式中没有的字段、响应中元素顺序调整等），这些微小变化并不是拒绝输入的充分理由。不过，将提供的模式 URI/URL 用作*标识符*很方便，可以确认服务返回的响应具有预期的语义元数据。

### 警告

使用模式文档验证*传入*消息与 Postel 定律[^18]相悖，该定律指出：“自己做事时要保守，接受他人时要宽容”。对传入消息进行强类型验证（而非“宽容地接受”）很可能会拒绝那些原本对客户端和服务器都能很好响应的响应。由 Robert Braden 编写的规范文档 RFC1122[^19] 详细阐述了如何设计和实现能够安全容忍消息与协议细节微小差异的功能。关于这一挑战的另一面——在将模式应用于*传出*消息时“自己做事时要保守”——请参见 TK。

### 协商模式

客户端应用程序有可能与服务进行模式协商。当客户端和服务器双方事先都知道同一通用模式存在多个版本（user-v1、user-v2 等），并且你想确认双方都使用相同版本时，这种方法非常有用。W3C 的一份已过期提案[^20] 建议使用 `accept-schema` 和 `schema` HTTP 头：

```
*** REQUEST
GET /todo/list HTTP/1.1
Host: api.example.org
Accept-Schema: <urn:example:schema:e-commerce-payment>

*** RESPONSE
HTTP/1.1 200 OK
Schema: <urn:example:schema:e-commerce-payment>
...
```

我在“实际环境”中没有遇到过这种协商方式。如果某项服务提供了这种方式，作为客户端开发者是值得研究的——任何语义元数据都比没有要好。

### Content-Type 头中的模式标识符

模式标识符也可以通过 `Content-Type` 头从服务器传递给客户端：

```http
*** REQUEST
GET /todo/list HTTP/1.1
Host: api.example.org
Accept: application/vnd.hal+json

*** RESPONSE
HTTP/1.1 200 OK
Content-Type: application/vnd.hal+json;schema=urn:example:schema:e-commerce-payment
...
```

需要指出的是，向媒体类型字符串添加附加参数（即“;”后面的部分）可能会使消息格式验证变得复杂。此外，某些媒体类型定义明确禁止在媒体类型标识符字符串中包含额外元数据（待补 JSON 示例？）。

### 文档中的模式标识符

最常见的做法是在消息体中提供模式标识符：

```json
{
  "$schema": "https://alps-io.github.io/schemas/alps.json",
  "alps": {
    "version": "1.0",
    "title": "ToDo List",
    "doc": {"value":"A suggested ALPS profile for a ToDo service"},
    "descriptor": [
      {"id":"id", "type":"semantic", "def":"http://schema.org/identifier"},
      {"id":"title", "type":"semantic", "def":"http://schema.org/title"},
      {"id":"completed", "type":"semantic", "def":"http://mamunda.site14.com/alps/def/completed.txt"}
    ]
  }
}
```

模式标识符不必是一个可解引用的 URL（*https://alps-io.github.io/schemas/alps.json*）。它可以是 URI 或 URN 标识符字符串（urn:example:schema:e-commerce-payment）。使用可解引用 URL 的价值在于，人类可以跟随链接获取更多信息。

### 编写识别模式的客户端

一旦你知道如何识别模式信息，就可以用它来驱动客户端应用程序代码。

```javascript
function handleSchema(ajax,schemaIdentifier) {
    var schemaType
    try {
        schemaType = schemaIdentifier.toLowerCase()
        switch(schemaType) {
            case "https://api.example.com/schemas/task":
                task.parse(JSON.parse(ajax.responseText));
                break;
            case "https://api.example.com/schemas/task-v2":
                task.parse(JSON.parse(ajax.responseText));
                break;
            case "https://api.example.com/schemas/user":
            case "https://api.example.com/schemas/user-v2":
                user.parse(JSON.parse(ajax.responseText));
                break;
            case "https://api.example.com/schemas/note":
                note.parse(JSON.parse(ajax.responseText));
                break;
            default:
                dump(ajax.responseText);
                break;
        }
    }
    catch(ex) {
        alert(ex);
    }
}
```

请注意，围绕模式定义组织代码通常比依赖像 ALPS 这样的语义配置文件格式要费力得多。模式通常是领域专有的，而且比语义配置文件更容易随时间变化。在上面的示例中，你可以看到 `task` 对象有两个（显然）不兼容的模式引用。此外，该示例

