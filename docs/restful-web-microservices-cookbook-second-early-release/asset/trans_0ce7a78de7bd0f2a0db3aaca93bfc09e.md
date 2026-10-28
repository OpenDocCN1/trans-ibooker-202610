
同样，细节有所缺失，但这里的重点是客户端应用的功能植根于以下工作：发出请求、处理响应消息、显示结果、输入控件，然后捕获任何点击事件。仅此而已。实际出现的操作基于消息的内容，而不是代码。这样一来，当服务添加新功能（如 `setDueDate` 操作）时，该客户端将自动支持该功能。

绑定到消息意味着新功能和小的修改都是“免费”的——无需额外编码。

### 讨论

将客户端代码从特定领域转向以消息为中心，可能是为 Web 创建弹性应用程序的更有效方式。这正是基于 HTML 的 Web 浏览器所采用的方法，它已存在近 30 年，界面没有任何重大变化。尽管事实上 HTML 浏览器的内部编码已被完全替换不止一次，多个供应商提供了多个相互竞争的浏览器版本，而且 HTML 的特性也随时间发生了变化。

就像让你的 API 客户端“感知 HTTP”的秘诀（参见秘诀 3.2）一样，让它们以消息为中心会为你的解决方案增加额外一层弹性。正如我们将在本章后面看到的（参见秘诀 3.5），这个“客户端弹性蛋糕”还有更多层次需要处理。每增加一层，都会为构建你的特定解决方案奠定更健壮、更稳定的基础。

这种方法的一个缺点是，它依赖于服务恰当地支持资源响应的强类型消息模型（见 TK）。如果你需要连接的服务仅以 XML 和 JSON 等非结构化媒体类型响应，那么你的客户端应用将很难利用以消息为中心的编码模型。如果你对服务团队有任何影响力（例如，你在企业 IT 部门工作），那么说服他们使用高度结构化的媒体类型（如 HTML、HAL、SIREN、CollectionJSON 等）确实会带来回报。参见**第 2 章**中关于媒体类型作为 Web 服务指导原则价值的几个秘诀。

### 相关

- 秘诀 2.1
- 秘诀 2.2
- 秘诀 3.6
- *第 4 章*

## 3.4 编写有效的客户端以理解已注册的词汇表配置文件

正如让客户端能够“说”HTTP 并绑定消息格式很重要一样，让你的客户端应用能够用服务和/或领域的词汇表“交谈”也很重要。为此，你应该确保客户端能够操作几种词汇表格式之一。

### 问题

即使服务可能随时间变化，你如何确保客户端应用能够“理解”它将与之交互的服务的词汇表？

### 解决方案

为了确保客户端和服务器在运行时“说同一种语言”，你应该确保客户端和服务器都经过编码，以理解相同的词汇表术语（例如数据名称和操作名称）。你可以通过依赖以下一种众所周知的词汇表格式来实现这一点：

- RDFS : 资源描述框架模式⁴
- OWL : Web 本体语言⁵
- DCAP : 都柏林核心应用程序配置文件⁶
- ALPS : 应用程序级配置文件语义⁷

由于我是 ALPS 规范的合著者之一，你会看到很多使用 ALPS 作为词汇表标准文档的示例。你也可以使用这里未列出的格式——这完全没问题。重要的是，你与你的客户端应用所交互的服务就一种描述问题领域的词汇表格式达成一致。

### 示例

将服务的领域表达为一组词汇表术语，允许客户端和服务以通用、稳定的方式*共享理解*。这在秘诀 2.4 中已介绍。下面是一个简单领域的一组词汇表术语：

```
# Simple ToDo

## Purpose
We need to track 'ToDo' records in order to improve both
timeliness
and accuracy of customer follow-up activity.

## Data
In this first pass at the application, we need to keep track of
the
following data properties:

* **id** : a globally unique value for each ToDo record
* **body** : the text content of the ToDo record

## Actions
This edition of the application needs to support the following
operations:

* **Home** : starting location of the service
* **List** : return a list of all active ToDo records in the system
* **Add** : add a new ToDo record to the system
* **Remove** : remove a completed ToDo record from the system
```

接下来，将该故事文档转换为 ALPS 后，如下所示：

```json
{
  "$schema": "https://alps-io.github.io/schemas/alps.json",
  "alps": {
    "version": "1.0",
    "title": "ToDo List",
    "doc": {"value": "ALPS ToDo Profile (see [ToDo Story](to-do-story.md))"},
    "descriptor": [
      {"id":"id", "type":"semantic", "tag":"ontology"},
      {"id":"title", "type":"semantic", "tag":"ontology"},
      {"id":"home", "type":"semantic", "tag":"taxonomy",
       "descriptor": [{"href":"#goList"}]
      },
      {"id":"list", "type":"semantic", "tag":"taxonomy",
       "descriptor": [
         {"href":"#id"},
         {"href":"#title"},
         {"href":"#goHome"},
         {"href":"#goList"},
         {"href":"#doAdd"},
         {"href":"#doRemove"}
       ]
      },
      {"id":"goHome", "type":"safe", "rt":"#home", "tag":"choreography"},
      {"id":"goList", "type":"safe", "rt":"#list", "tag":"choreography"},
      {"id":"doAdd", "type":"unsafe", "rt":"#list", "tag":"choreography",
       "descriptor": [{"href":"#id"}, {"href":"#title"}]
      },
      {"id":"doRemove", "type":"idempotent", "rt":"#list", "tag":"choreography",
       "descriptor": [{"href":"#id"}]
      }
    ]
  }
}
```

该 ALPS 文档包含所有可能的数据元素和操作元素，以及关于如何构造对服务的请求以及期望何种响应的附加元数据。该 ALPS 文档可以作为创建客户端应用的源材料，这些应用将能够成功与任何支持此配置文件的服务交互。对于支持超媒体响应的服务尤其如此。

例如，假设一个符合 todo 规范的服务运行在 localhost:8484，一个符合 todo 规范的客户端可能会执行以下操作：

```text
# data to work with
STACK PUSH {"id":"zaxscdvf","body":"testing"}

# vocabulary and format supported
CONFIG SET {"profile":"http://api.examples.org/profiles/todo-alps.json"}
CONFIG SET {"format":"application/vnd.mash+json"}

# write to service
REQUEST WITH-URL http://api.example.org/todo/list WITH-PROFILE
WITH-FORMAT
REQUEST WITH-FORM doAdd WITH-STACK
REQUEST WITH-LINK goList
REQUEST WITH-FORM doRemove WITH-STACK
EXIT
```

### 讨论

词汇表文档是将 API 生产者和 API 消费者绑定在一起的粘合剂，而无需诉诸紧耦合。正如我们在上一个示例中看到的，依赖清晰的词汇表和众所周知的媒体类型格式，可以在不丢失任何细节的情况下大大减少客户端和服务之间的耦合。

服务通常发布 API 定义文档（例如 OpenAPI ⁸、AsyncAPI ⁹、RAML ¹⁰ 等）。这对于想要实现 API 服务单个实例的人来说很方便，但对 API 消费者的价值有限。当 API 客户端应用使用服务实现规范来创建 API 消费者时，该应用将紧密绑定到这一特定服务实现。而且，对服务实现的任何更改都可能破坏该客户端应用。更好的方法是将 API 消费者绑定到已发布的配置文件文档。

如果你使用的服务没有发布稳定的词汇表文档，最好创建自己的文档。以 API 定义文档（OpenAPI 等）为指导，编写你自己的 ALPS 文档（如果你愿意，也可以是其他格式），然后发布该文档，供你的团队（和其他人）在为该服务创建 API 客户端时使用。将 ALPS 文档包含在客户端应用的源代码库中以便将来参考也是一个好主意。

如上面最后一个示例所示，当你使用的服务返回超媒体响应时，词汇表配置文件能带来最大的好处。如果不是这种情况（并且你无法影响服务团队），你仍然可以像响应中包含超媒体一样编写客户端（详见秘诀 3.10）。

### 相关

- 秘诀 2.4
- 秘诀 3.10
- TK 第 4 章
- TK 其他客户端秘诀

## 3.5 在运行时协商配置文件支持

