# 第4章 超媒体服务模式

### 致早期版本读者

通过早期版本电子书，你可以在书籍的最早形态下获得它们——即作者写作时的原始且未经编辑的内容——从而在这些技术正式发布之前就能利用它们。

这将是最终书的第4章。请注意，GitHub 仓库将在稍后激活。

如果你对如何改进本书内容和/或示例有任何建议，或者你发现本章有遗漏的内容，请通过 mca@amundsen.com 联系作者。

> 最好的软件架构“知道”什么经常变化，并让这些变化变得容易。¹
>
> ——保罗·克莱门茨

设计和实现服务 API 的一个主要挑战是平衡可用性与可演化性。服务 API 必须清晰易懂，这一点很重要。同时，这些接口的定义方式必须允许未来进行修改，这一点也至关重要。最后，服务 API 的价值往往与其长期可靠性相关。能够定义一个解决当下问题的 API 固然很好。但随着问题随时间变化、运行参数改变、需求和目标逐渐漂移，这个 API——理想情况下——应该继续有用。这对服务接口设计来说要求很高！

正如本章开头引语所暗示的，知道软件设计中什么经常变化并使之易于应对是一个有价值的目标。对服务 API 而言尤其如此。API 是契约——需要遵守的承诺。

HTTP 协议的设计方式就是一个很好的例子。HTTP 几乎每个关键方面都是可修改的。HTTP 方法、状态码、URL、可能的请求头列表以及可能的请求体格式集合，都是可以随时间修订的抽象集合。HTTP 消息的*结构*是一致的——这是承诺。然而，HTTP 消息的*内容*是可变的。这是创建稳定而有价值的服务 API 的关键，也是本章将要讨论的内容。

> **注意**
>
> 由于本书聚焦于*接口*，本章不会花太多时间讨论微服务的内部细节。然而，深入理解 Web 上可扩展、健壮且可靠服务的内部运作方式是很重要的。在设计和实现成功的微服务时，我手边常备的书是：1）Sam Newman 的《微服务设计》，2）Chris Richardson 的《微服务模式》，以及 3）Eberhard Wolff 的《微服务》。我还强烈推荐 Nadareishvili 和 Mitra 的《微服务快速上手》。

本书这一部分中的模式都专注于让设计和实现 API 变得容易，这些 API 要在具体性和可演化性之间取得适当的平衡。这种 API 的一个关键特性是能够在运行时包含操作元数据，而不是仅依赖设计和构建时的描述。就本书而言，这种在运行时修改消息的能力将通过使用超媒体格式来实现，这些格式遵循 HTTP 协议本身同样的总体设计模式：清晰的消息结构，以支持可变的内容。

在深入这些模式之前，让我们更深入地探讨创建服务 API 时面临的挑战：如何在支持可修改性的同时不违背我们对 API 使用者的承诺。为此，我们将探讨可修改性“问题”的本质，超媒体消息如何帮助克服这一挑战，以及我们如何在创建成功的服务 API 时运用“自服务”概念作为指导。

### 使用超媒体服务支持稳定性与可修改性

设计成功服务 API 的关键挑战在于平衡稳定性与可演化性——即既要对 API 使用者信守承诺，又要支持接口背后的服务能力不断演进。这里选择的所有模式都是为了帮助应对这一挑战。

问题的根源在于随时间变化的现实。服务在生产环境中运行的时间越长，其发生变化的可能性就越大。在另一个极端，短命的、一次性的服务很少能“活”得足够久，以至于需要成长或变化。它们凭空出现，完成任务后便消失。正是*时间*这个架构要素让我们不得不面对变化的现实。

好消息是，采用超媒体方法设计服务接口为我们提供了一些便捷工具，既支持随时间变化的演进，又仍能向客户端提供稳定、可预测的接口。通过将超媒体作为消息设计的一部分，你的接口可以建立可修改性的“安全区”。超媒体控件（链接和表单）为支持变更提供了载体。表单所做的正是保罗·克莱门茨建议优秀软件架构应该做的——它*知道*什么经常变化，并使之容易改变。

但是，仅仅在消息中使用超媒体还不够。服务之间常常需要与其他*服务*通信。毕竟，我们身处网络之中。当一个服务依赖另一个服务时，我们就有创建致命依赖链的风险——链条中一个服务的变化会向上传递给所有依赖它的其他服务。当这种情况发生时，可能会出现意想不到的问题。有时，某个*被调用*服务的变化会导致其接口发生变化。有时这些变化可能与*发起调用*服务的任务不兼容——就会暴露破坏性变更。我们也需要一种方式来解释并解决依赖问题。




## Kimi文档助手

这是一份用于展示文档解析能力的示例文本。

本示例包含多种文档元素，例如标题、段落、列表和引用。

- 支持标题识别
- 支持段落提取
- 支持列表和引用解析

> 解析准确，格式规范，输出结构清晰。

以上内容展示了文档的结构化信息。

## 不可删减

一旦你编写文档并发布了端点URL、对协议或媒体类型的支持、响应消息、输入参数或输出值，就不能在后续的接口更新中将其移除。你也许可以在未来版本中将值设为`null`或忽略该值，但你不能将其删减掉。

## 不要重新定义

你可以更改接口现有元素的含义或用途。例如，如果响应消息已发布为用户对象，你就不能将其更改为用户集合。如果输出属性`count`在文档中被定义为包含集合中的记录数，你就不能将其含义更改为单页响应中的记录数。

## 新增内容必须可选

在API首次发布之后，任何额外的输入或输出都必须被记录为*可选*——你不能为现有接口添加新的**必需**属性。这意味着，例如，你不能为创建新`user`记录的接口操作添加新的输入属性（`backup_email_address`）。

## 不要更改，而是添加

遵循这三条规则，你可以避免最常见的API破坏性变更。不过，还有一些其他细节。首先，你始终可以创建*新的*端点（URL），在那里你可以重新规定预期的输入和输出。“添加更多选项”的能力是一种宝贵的支持服务可演化性的方式，而无需更改现有元素。

例如，你可能有机会创建一种新的、改进的方式来返回过滤后的内容。初始API调用不支持修改响应中返回记录数的能力；它始终固定为最多100条。但现在服务支持更改`page_size`

对于服务API设计者来说，明智的做法是将API视为“不可破坏的”，并假设它们会“永远”持续下去。亚马逊CTO Werner Vogels这样说道：“我们知道设计API是一项非常重要的任务，因为我们只有一次机会把它做对。”⁸ 虽然这看起来是一项艰巨的任务，但如果你在API设计中内置了随时间安全更改元素的能力，这项工作就会容易得多。

当然，事情会随着时间而改变。服务会演化。它们添加功能、修改输入并更新输出。这意味着我们与API消费者达成的契约需要反映出未来变化的可能性。本质上，变化需要被“写入协议中”。

虽然要求接口设计者能够准确预测实际的接口变化是不合理的（“两年后，我们将添加一个名为`better-widget`的第三个输出属性”），但要求设计者创建一个考虑到未来变化可能性的接口是合理的（“API消费者应忽略任何他们不理解的额外属性。”）。换句话说，API设计不仅需要准确描述当前的服务接口，还需要以帮助API消费者“在接口的任何意外演化中幸存”的方式来考虑未来可能的变化。

幸运的是，我们有一种成熟的方法可以用来处理接口随时间的变化。这种方法被称为*超媒体*。

## 超媒体如何提供帮助

在本章开头，我提到HTTP消息模型简单且稳定，并且其大多数关键元素被定义为“可以随时间修订的抽象集合”。基本上，HTTP被设计为同时支持稳定性和可修改性。而且这种方法不仅限于协议层面。我们还可以设计通过HTTP来回传递的消息，使其具有相同的特征：支持未来变化的稳定性。

此时值得花点时间问一句：“为什么设计支持稳定性和可演化性的接口如此重要？”这肯定比仅仅发出直接绑定到内部数据和/或对象模型的静态接口要难。而且，大多数开发者也无需担心“随时间的变化”。Roy Fielding曾说过：“软件开发者一直难以处理时间和请求消息，以便有效操作。”实现这一目标的一种方法是承诺使用众多已注册的结构化消息格式之一。Web浏览器客户端也是这样做的。它依赖一种稳定的格式（HTML）来将大多数基于文本的消息从服务传递到客户端。在Web上，无论服务（网站）返回的是会计信息、社交网络数据、游戏渲染等，HTML都被用作默认响应格式。承诺使用一种众所周知、设计良好的格式对于创建稳定接口至关重要。

> 注意
> 关于使用结构化媒体类型格式进行编程的更多内容，请参阅我的书《RESTful Web Clients》¹¹

在实现服务API时，有几种值得选择的格式。IANA媒体类型注册表¹²是我用来查找具有持久性和支持性、使其成为API良好候选者的消息格式的来源。当前最常用的格式包括HAL¹³、SIREN¹⁴和Collection+JSON¹⁵，还有其他几种。

本章中有几个方案涉及选择和支持结构化媒体类型的过程。

## 使用超媒体控制支持可演化性

如果第一步是通过结构化媒体类型为API消费者提供稳定性，那么第二步（支持可演化性）可以通过选择富含超媒体控制的结构化媒体类型来实现。正是超媒体控制使API设计者能够遵循Paul Clements的建议（“知道什么经常变化，并让该变化变得容易”）。

你在消息中包含的超媒体特性越多，你就能在时间推移中支持越多的可演化性。我在设计中使用的格式


包括 Collection+JSON、SIREN 和 UBER。HAL 加上 HAL-FORMS 也是一个不错的选择。这些格式并不是唯一可能的选择，但根据超媒体因素¹⁶的衡量标准，它们是包含最多超媒体特性的格式。

> **警告**
>
> 请注意，仅使用 HAL 只能描述安全、幂等的操作（链接），并不支持包含表单的 HTTP 方法和属性细节。因此，在基于 HAL 表示格式创建 API 时，我建议除了原始的 HAL 格式之外，还使用 HAL-FORMS 扩展。

当你使用超媒体格式时，你能够反映新的操作（新增的表单）以及对这些操作的安全更新（新增的属性、更新的 URL 和更改的 HTTP 方法）。这些是服务接口随着时间推移最常见的变化。

这种方法仍然存在局限性。使用超媒体格式可以让你轻松、一致且安全地更改地址（URL）、对象（数据属性）和操作（表单）。然而，你可能仍然需要随着时间推移修改领域属性（事物的名称）。例如，你可能需要在可能的输入列表中添加一个新属性（middleName），或添加一个新的输出值（alternateEmail），或添加一个新的请求参数（/filter/?newRegionFilter=south）。在这种情况下，你需要创建（并使用 ALPS 或其他格式记录）一份新的词汇表文档，并允许客户端在运行时发现和选择这些附加词汇表（详见第 2 章）。本章中的一些方案也涉及选择词汇表的能力。

## 从自助服务到查找与绑定

Web 服务的另一个重要元素是 API 消费者能够“自助”完成接入体验。能够简单地选择所需服务并立即使用，这是一个值得追求的目标。这基本上就是人们今天与 Web *网站*交互的方式。有人找到感兴趣的内容，然后“获取该内容的链接”，要么与他人分享该链接，要么将该链接复制到自己的网页上，其他人就可以看到并点击该链接访问相同的内容。这是 Web 上如此常见、基础的一种体验，以至于我们并不真正将其视为“集成”或接入体验。我们只是认为“Web 就是这样运作的。”

服务也应当以这种方式运作。但大多数时候，它们并没有。相反，开发者只能通过手动过程来定位、选择并将现有服务集成到自己的解决方案中。这可能是一个令人沮丧的过程，充满波折。可能很难找到你想要的东西。一旦找到一个可能可用的 API，又可能很难理解 API 文档。此外，将外部 API 成功集成到自己的服务中也颇具挑战。通常，开发者需要多次经历这一过程（服务所依赖的每个 API 都要经历一次）才能完成工作。而且，即使在服务完成、测试并发布之后，只要任何一个依赖的 API 更改了*它们的接口*，整个过程就可能重新开始。

> **注意**
>
> 本节中的多个方案都引用了这种“查找与绑定”模型，我们将在[待插入链接]中直接讨论运行时自助服务的问题。目前，重要的是认识到需要一种简单、直接、可靠的方式，让服务在运行时相互发现（“查找”）并建立兼容的连接（“绑定”）。

我们需要的是将上述大部分活动自动化，变为一个在运行时发生的过程——我称之为“查找与绑定”。通过标准化服务的命名和描述方式（元数据），我们可以自动化服务的发现方式（查找）及其集成方式（绑定）。这种查找并与你从未“见过”的远程服务交互的能力，是当前域名系统（DNS）¹⁷在互联网上连接彼此机器的基础。

既然我们已经讨论了支持稳定且可演进 API 的挑战，接下来让我们进入实际方案，这些方案可用于设计和实现能够安全支持随时间变化的服务接口。

## 4.1 发布至少一个稳定 URL

当服务在网络上发布接口时，这些服务需要能被 API 消费者轻松（且一致地）访问。这意味着将你的 API 发布在一个稳定的位置（使用 URL）。但这是否意味着你永远不能更改已发布的位置？如果可以更改，你又如何告知 API 消费者这一更改？

### 问题

如何为你的服务 API 建立一个稳定的网络位置（URL），并且需要采取哪些步骤来确保即使底层服务已迁移到新位置，你的 API 仍然可以被找到？

### 解决方案

服务接口应承诺至少一个稳定 URL，API 消费者可以使用它来定位并与该服务交互。确切的 URL 并不重要。这应该是 API 的“起点”。它可以是一个返回 API 详细信息（包括入口点）的资源。它可以是服务的初始列表或状态（例如默认用户列表等）。或者也可以是 API 消费者可以依赖以启动与服务访问的其他响应（例如登录、建立有状态会话的方式等）。

URL 的值可以简单如 *http://api.example.com/home*，也可以复杂如 *http://v1.home.api.example.org/q1w2e3r4t5*。重要的是至少有一个消费者可以记住（例如写入源代码或配置文件）的位置，以确保 API 消费者能够连接到服务 API。最后，并不要求服务 API 只能有一个已发布的稳定 URL。然而，你的 API 应该至少有一个 API 消费者可以长期依赖的 URL。

稳定 URL 应发布在 API 的人类可读文档中。它们也可以作为 HTTP 中的 Link 头¹⁸出现，或作为支持内联链接元素的 API 响应体中的链接元素出现。

> **注意**
>
> 在某些情况下，为你的服务 API 注册一个“well-known” URL 可能是明智的。然而，这并不是一项简单的工程。详见 RFC8675¹⁹ 和 RFC7595²⁰。

发布稳定 URL 并不意味着该 URL 背后的服务不能迁移。如果服务迁移了，对稳定 URL 的请求可以使用 HTTP 状态码 301 Moved Permanently 重定向到新位置。

### 示例

在人类可读文档中指明 API 的稳定 URL 时，务必将其明确为 API 的承诺：该 URL 将来仍会生效，并且如果需要，会将 API 客户端应用重定向到新的服务位置。

你可以在 HTTP 响应中使用 Link 头来为你的 API 提供稳定 URL：

```http
**** REQUEST
GET / HTTP/1.1
Host: api.example.org

**** RESPONSE
HTTP/1.1 200 OK
Content-Type: application/vnd.collection+json
ETag: "p0o9i8u7y6t5r4e3w2q1"
Link: <http://api.example.org/home>; rel="home"
...
```

稳定 URL 也可以出现在任何（或所有）支持内联链接的响应体中。下面是一个以 Collection+JSON 响应展示的示例：

```json
{ "collection" :
  {
    "version" : "1.0",
    "href" : "http://api.example.org/friends/",

    "links" : [
      { "rel" : "home", "href" : "http://api.example.org/home" }
    ],

    "items" : [...],
    "queries" : [...],
    "template" : [...]
  }
}
```

当服务位置发生迁移时，API 应继续遵循最初发布的稳定 URL，并自动将 API 消费者重定向到新的位置 URL。

```http
**** REQUEST
GET /home HTTP/1.1
Host: api.example.org

**** RESPONSE
HTTP/1.1 301 Moved Permanently
Location: http://new.example.org/home

**** REQUEST
GET /home HTTP/1.1
Host: new.example.org

**** RESPONSE
HTTP/1.1 200 OK
Content-Type: application/vnd.collection+json
ETag: "p0o9i8u7y6t5r4e3w2q1"
Link: <http://api.example.org/home>; rel="home"
...
```

### 讨论

我通常将我的 API 属性建模为单一的扁平集合（如上面的最后一个示例）。这简化了客户端的处理过程，并使支持服务可以自由使用它们希望的任何本地数据存储模型。服务甚至可以更改存储模型，而不会对 API 产生不利影响。

> **提示**
>
> 多年前，在讨论这个话题时，我提出了一个准则：“*请记住，在设计 Web API 时，你的数据模型不是你的对象模型，不是你的资源模型，也不是你的消息模型。*”²²。还有一个名为“Web API 设计成熟度模型”²³ 的演讲对此进行了扩展。

API 很少需要操心数据规范化规则。API 只是接口。如果接口期望创建新记录的人发送一个“非规范化”的属性集合，这些属性将被重新分组并写入多个存储位置，这完全可以。只要服务能够安全地管理数据（例如确保没有丢失更新或数据冲突），任何方式都可以。有关处理通过 API 传递的数据的更多建议，请参阅[链接待添加]。

> **注意**
>
> 我们将在[链接待添加]中深入探讨处理数据存储的细节。

API 设计者可以自由创建任意数量的 HTTP 资源，以支持预期的交互。而且这些交互不必遵循 CRUD（创建、读取、更新、删除）这个在 HTTP API 中非常常见的模式。例如，下面是一个资源设计，用于支持本方法中提到的待办事项服务：

| 操作 | URL | 方法 | 请求体 | 响应体 |
|---|---|---|---|---|
| 读取列表 | `/todo/` | GET | 无 | `[{id,text,status,nick,name}]` |
| 过滤列表 | `/todo/?text={text}` | GET | 无 | `[{id,text,status,nick,name}]` |
| 创建项目 | `/todo/` | PATCH | `{id,text,status,nick,name}` | `[{id,text,status,nick,name}]` |
| 更新项目文本 | `/todo/` | PATCH | `{id,text}` | `[{id,text,status,nick,name}]` |
| 更新项目昵称 | `/todo/` | PATCH | `{id,nick,name}` | `[{id,text,status,nick,name}]` |
| 更新项目状态 | `/todo/` | PATCH | `{id,status}` | `[{id,text,status,nick,name}]` |

请注意，上表表明待办事项列表是一个单一资源，可以通过该单个资源进行修改以添加和编辑项目。只要客户端应用能够完成所需的工作，服务在内部如何组织这些信息以及/或者*存储*这些信息，对接口消费者来说并不真正重要。

## 相关

- 第 2 章
- 第 3 章
- 第 4 章
- [链接待添加]

## 4.3 将内部模型转换为外部消息

为了支持不断演化的服务之间的松耦合，您需要为这些服务之间的通信建立稳定的基础。一个已被证明成功的策略是依赖众所周知的、高度结构化的已注册媒体类型作为 API 响应的表示格式。

这意味着您的服务接口需要以不同的消息格式响应相同的*语义*信息。

### 问题

我们如何组织服务接口以支持 API 消费者的内容协商，同时仍然保持低成本/低维护的服务 API？我们应该在接口代码中支持哪些模式，以便以标准化且众所周知的媒体类型一致地表示后端服务数据？最后，为不同的响应消息格式表示相同的内部数据有哪些优势和挑战？

### 解决方案

确保您的服务*接口*设计不会与服务的内部数据（或对象）模型过度耦合的一个重要方法是，将表示步骤视为一个独立的设计工作。换句话说，主动参与设计您的表示的过程——不要仅仅满足于将内部模型直接序列化为外部消息。

有一系列格式是为了表示 HTTP 信息而创建的。这些格式包含足够的结构（例如 `<html>`、`<head>`、`<body>` 等），以支持能够绑定到消息格式而非消息*内容*（例如 `givenName`、`id`、`telephone` 等）的客户端应用程序。

> **提示**
>
> 有关推荐的响应媒体类型列表，请参阅[链接待添加]。

将服务内部数据/对象模型的各个部分映射到接口的外部对象模型的过程，通常需要算法应用和创造性设计的结合，并且需要实现两个关键的运行时支持元素。首先，服务 API 需要能够发现客户端偏好哪些格式；其次，服务 API 需要使用该信息以首选格式渲染内部数据。

HTTP 定义了 `Accept` 头，允许客户端在发送请求时包含它们的格式偏好。当服务 API 看到 `Accept` 头时，它需要解析该头的值并做出适当的选择。您可以查阅关于 `Accept` 头的 HTTP 规范，了解如何解释其内容的详细信息。

一个简单的交换可能如下所示：

```
*** REQUEST
GET /todo/list HTTP/1.1
Host: api.example.org
Accept: application/vnd.collection+json, application/vnd.uber+xml

*** RESPONSE
HTTP/1.1 200 OK
Content-Type: application/vnd.collection+json
```

请注意，服务接口在此选择了 `application/vnd.collection+json` 格式。当客户端提供了多个偏好时，服务在返回哪种格式上有一定的自由——甚至可以返回客户端未列出的默认格式！有关更多信息，请参阅方法 4.5。

一旦服务 API 选择了一种用于渲染响应的格式，就需要根据所选响应格式的规范，将内部数据映射到输出表示。通常这很容易契合，但有时内部数据有点难以渲染，甚至可能被排除在响应之外。

以下是一些需要表示在响应中的内部服务数据示例：

```
{
  "user" : {
    "id":"q1w2e3r4t5",
    "givenName":"Mark",
    "familyName":"Morkelsen",
    "nickName":"mork",
    "telephones": [
      {"type":"home", "value":"1-234-567-8901"},
      {"type":"work", "value":"1-987-654-3210"}
    ]
  }
}
```

现在，假设客户端发送了以下请求：

```
GET /users/q1w2e3r4t5 HTTP/1.1
Host: api.example.org
Accept: application/json
```

表示内部数据的一种简单方式是直接将其序列化：

```
*** RESPONSE
HTTP/1.1 200 OK
Content-Type: application/json
...
{
  "user" : {
    "id" : "q1w2e3r4t5",
    "givenName" : "Mark",
    "familyName" : "Morkelsen",
    "nickName" : "mork",
    "telephones" : [
      {"type" : "home", "value" : "1-234-567-8901"},
      {"type" : "work", "value" : "1-987-654-3210"}
    ]
  }
}
```

接下来，让我们看看一个使用 text/csv 格式表示请求的示例：

```
GET /users/q1w2e3r4t5 HTTP/1.1
Host: api.example.org
Accept: text/csv

*** RESPONSE
HTTP/1.1 200 OK
Content-Type: text/csv
...
"id","givenName","familyName","nickName","telephone_home","telephone_work"
"q1w2e3r4t5","Mark","Morkelsen","mork","1-234-567-8901","1-987-654-3210"
```

请注意，在 CSV 响应中，内部的 telephone 对象被渲染为一组扁平属性。客户端甚至可能不知道数据的内部形态。

最后，这是一个客户端在请求中包含 application/vnd.collection+json 媒体类型时的表示示例：

```json
{
  "collection": {
    "version": "1.0",
    "href": "http://example.org/users/q1w2e3r4t5",
    "links": [
      {"rel": "users", "href": "http://example.org/users"},
      {"rel": "products", "href": "http://example.org/products"},
      {"rel": "services", "href": "http://example.org/services"}
    ],
    "items": [
      {
        "href": "http://example.org/users/q1w2e3r4t5",
        "data": [
          {"name": "id", "value": "q1w2e3r4t5", "prompt": "Identifier"},
          {"name": "givenName", "value": "Mark", "prompt": "First Name"},
          {"name": "familyName", "value": "Morkelsen", "prompt": "Last Name"},
          {"name": "nickName", "value": "Mork", "prompt": "Nick"}
        ],
        "links": [
          {
            "rel": "telephones",
            "prompt": "Telephones",
            "href": "http://example.org/users/q1w2e3r4t5/telephones"
          }
        ]
      }
    ]
  }
}
```

在最后一种情况下，API 服务中的表示器提供了相当多的“额外”信息，包括将 `prompt` 值映射到每个数据点，包含指向相关数据的 `link` 元素，甚至将 `telephone` 数据数组移动到另一个相关资源中。

这里的关键信息是，为服务数据设计表示器不应局限于盲目序列化内部模型。

### 讨论




在服务 API 的实现阶段，最好记录你用来将内部模型转换为外部消息的规则。这将有助于与其他开发人员共享算法，并更轻松地调试你在此过程中发现的任何错误。然而，将这种表示描述发布给 API 消费者并不是一个好主意。他们不需要知道你接口背后的内部数据或对象模型。

随着时间推移，你可能会遇到服务内部模型发生变化的情况。在许多情况下，这些变化**不应该**导致外部消息发生变化。例如，当内部模型属性 `user.id` 更改为 `user.identifier` 时，没有必要更改外部属性。

然而，一些内部模型更改会包含新属性（`user.hatsize`），如果这些更改不会导致破坏现有 API 消费者，那么这些新属性可能会反映到外部消息中。在 `text/csv` 行的末尾添加一个字段可能没问题，但重新排列行中的字段顺序（例如，将新字段插入到行的第一个字段）很可能是个坏主意。

> **提示**
> 
> 如果你预计内部模型会频繁更改，请务必选择一种旨在减少此类更改影响的表示格式。例如，Collection+JSON 适用于多变的模型（所有属性都表示为 `data.name` 和 `data.value`），但 SIREN 和 HAL（因为它们直接绑定到对象属性）则不适用。

虽然你可以“硬编码”服务接口以**始终输出**相同的格式（HAL、SIREN 等），但更好的做法是遵循 HTTP 内容协商过程，允许 API 消费者在运行时使用 `Accept` 请求头表明其格式偏好。有关如何处理客户端媒体类型偏好的详细信息，请参见技巧 4.5。

在服务 API 中支持多种表示格式是一个好主意。我通常至少支持两种，并根据情况尽量支持三种或更多。首先，最好选择一种 Hypermedia Factor²⁵ 得分高的媒体类型。有关 H-Factors 的更多信息，请参见**技巧 3.9**。例如，Collection+JSON 或 SIREN。其次，最好支持一种简单的序列化格式，如 JSON、XML 和/或 CSV，因为许多 API 消费者能够快速使用这些简单格式。如果你想支持其他格式，可以添加任何其他你或你的 API 消费者喜欢使用的格式。

如果合适，为你的服务 API 支持 HTML 表示也会很有帮助。它可以在服务开发人员测试接口设计时提供帮助。它还可以帮助那些想要“不投入大量时间编写 API 客户端就能试用 API”的 API 消费者。最后，HTML 格式可以作为服务集合中高 H-Factor 媒体类型之一的一个可靠选择。

## 相关

- 第 2 章
- 第 3 章
- 技巧 3.6
- 技巧 3.9
- 第 4 章
- 技巧 4.5
- 技巧 4.17
- [链接待添加]

## 4.4 将内部函数表达为外部动作

一旦你采用了使用注册媒体类型标准化响应的做法，你还需要应对如何正确表达接口背后服务内部功能的挑战。要做到这一点，你需要依赖一种不仅表达服务数据模型，还表达该服务动作模型——即需要通过外部 API 访问的服务功能——的做法。

### 问题

如何决定服务中的哪些内部函数需要通过 API 暴露？以及如何以一致的方式表达这些方法，以便 API 消费者能够按预期找到并操作它们？有哪些格式和/或模式可用于将内部服务函数转换为外部接口动作？

### 解决方案

将服务内部函数作为外部动作暴露的最直接方式是使用超媒体控件来表示这些函数（参见技巧 3.9）。例如，在 HTML 中，函数名称（`<form name="approveUser" ...>`）、输入参数（`<input>` 控件）和输出返回值可以用来表示内部服务函数。然而，将外部动作直接绑定到内部服务操作并不总是可行的（甚至不是一个好主意）。

在表达内部服务函数时，通常最好依赖既定的外部词汇表。例如，内部数据参数可能命名为 `firstName`。但同样的参数在外部接口中会被表达为 `givenName`（参见“依赖语义词汇表”）。API 代码可以处理内部和外部名称之间的转换。这样做的好处是，外部接口可以免受服务内部变更的影响。再进一步举例，服务更新可能导致 `firstName` 变为 `fNameValue`。在这种情况下，只需更新中间件代码，而无需更新实际的外部接口。

最后，一些内部动作——尤其是涉及复杂计算或过程的动作——最好以更简化的方式表达。一个服务（`onboardingSvc`）可能需要联系多个依赖服务（`userSvc`、`customerRelationsSvc` 和 `accountSalesSvc`），并在它们之间传递数据以完成任务。通常最好在外部将此表达为单个动作（`sendOnboardingData`），而不是多个依赖动作（详见下面的示例）。

本质上，将 API 流程交互的设计视为一项独立活动。你可以尽情发挥创造力和想象力。设计一个好的接口；不要只照搬服务内部实现。要假设在 API 交互和内部服务创新之间可能存在一个“转换”步骤。最重要的是，努力保持底层服务与你发布的服务接口之间的松散耦合。

### 示例

在从内部函数到外部动作的直接转换中，你可以从一个服务函数的示例开始：

```javascript
function approveUser(userId, nick, approver, level) {
  var approval = {};

  approval.userId = userId;
  approval.nick = nick;
  approval.approver = approver;
  approval.level = level;

  return data.create(approval);
}
```

然后将这些内部元素转换为响应消息中的 HTML FORM 对象：

```html
<form name="approveUser" enctype="application/x-www-form-urlencoded"
  method="post" action="http://api.example.org/users/approvals"
  target="approvalDisplay">
  <input name="userNick" type="string" value="mork" />
  <input name="approverName" type="string" value="Mr. Roboto" />
  <input name="approveLevel" type="string" value="nominal" />
  <input type="submit" value="Approve User" />
</form>
<iframe id="approvalDisplay" />
```

然而，正如我们上面已经讨论过的，直接转换通常不是一个好主意。这里有一个示例，它依赖一个众所周知的外部词汇表（本例中来自 Schema.org 库的值）来使用标准化的词汇表表达外部动作，即使底层服务并不支持该词汇表。

考虑这个用于更新用户对象的简单内部方法：

```javascript
function updateUser(id, fname, lname, email) {
  ...
  return userObject
}
```

现在，让我们看看接口动作（表示为 Collection+JSON 模板）：

```json
{
  "collection": {
    "template": {
      "data": [
        {"name": "identifier", "value": "q1w2e3r4", "prompt": "User ID"},
        {"name": "givenName", "value": "Mark", "prompt": "First Name"},
        {"name": "familyName", "value": "Morkelsen", "prompt": "Last Name"},
        {"name": "email", "value": "mork@example.org", "prompt": "Email"}
      ]
    }
  }
}
```

最后，接受更新动作的代码可以处理内部和外部名称之间的转换：

```javascript
function updateAction(identifier, givenName, familyName, email) {
  var user = data.read(identifier);
```





```text
if(user) {
    user.id = identifier;
    user.fname = givenName;
    user.lname = familyName;
    user.email = email;
    user = data.write(user);
}
return user;
}
```

在某些场景下，接口可能包含无法直接映射到内部函数的外部操作。考虑以下外部操作：

```html
<form name="declineContract" method="post" action="...">
    <input name="customerId" value="q1w2e3r4t5" />
    <input name="contractId" value="o9i8u7y6.t5r4" />
    <input name="salesRepName" value="Mandy Miningham" />
    <input name="reviewerName" value="Mark Morkelsen" />
    <input name="reasonCode" value="0201.B" />
    <textarea name="comments">Unable to locate
collateral</textarea>
    <input type="submit" value="Decline Contract" />
</form>
```

此接口背后的底层服务代码中没有 `declineContract` 方法。但是，当合同被拒绝时，还必须完成若干操作：

1. 创建一条 `declined` 日志记录
2. 更新 `customer` 记录
3. 更新 `contract` 记录
4. 更新 `salesRep` 记录
5. 更新 `reviewer` 记录
6. 更新 `nationalCredit` 机构

在这种情况下，服务 API 很可能需要针对多个依赖服务执行一组函数。其中一些函数可能是本地的（例如服务代码中的内部对象），而另一些可能是远程的（例如像 `nationalCredit` 机构这样的其他外部服务）。所有这些细节对 API 调用者而言都是“隐藏”的。

### 讨论

采用“设计外部操作”的做法是改善 API 松耦合性的一个好方法。当你创建没有直接服务等价物的外部操作时，这一点尤其重要，因为底层服务的变更无需直接反映在这些独立操作中。基本上，你在设计接口操作上投入的工作越多，未来服务变更（即使是看似破坏性的变更）对 API 产生不利影响的可能性就越小。

有时，服务会有一系列内部操作（`initializeCustomer`、`collectUserData`、`collectBankData`、`finalizeCustomerData`）。只要有可能，最好将这些内部函数表示为一个单一的外部操作，即收集所有重要数据，然后让 API 代码来处理操作顺序。这可以通过一个包含所有输入字段的“表单”来完成，或者在需要时使用“进行中”工作流（参见[链接待补充]）。这样可以避免将来处理操作顺序的变化或步骤的增删。

有时，内部服务会依赖布尔标志来控制操作。例如：

```text
function approveUser(bool) {
    if(bool===true) {
        ...
    }
    else {
        ...
    }
    return results;
}
```

- [链接待补充]

## 4.5 通告对客户端响应偏好的支持

HTTP 协议提供了一些与内容相关的参数，客户端可以用它们来表示偏好。HTTP 支持一组标头参数、OPTIONS 方法和链接关系值。然而，不是所有可能性都在现有 HTTP 规范中得到了很好的体现。这可能会让我们难以向客户端充分传达所有可能的选项。解决方案是结合使用 HTTP 协议选项和一些额外的语义引用。

### 问题

服务如何以一致且可扩展的方式向 API 调用者通告其所有可能的偏好选项？有哪些 HTTP 标头可用？在什么情况下使用 HTTP OPTIONS 方法才有意义？何时最好依赖链接关系和其他值来表明服务 API 的能力？

### 解决方案

使用 HTTP 协议访问网络服务的优点之一是，HTTP 具有广泛的“可调”值，客户端和服务器可以用它们来协商请求和响应参数的详细信息。本质上，服务可以“通告”消息交换中一个或多个可选择的方面，而客户端可以利用这些信息在运行时告知服务其偏好。

HTTP `language` 标签<sup>38,39</sup>可用于返回响应正文支持的自然语言列表。有效值列表应取自 IANA 语言子标签注册表。<sup>40</sup>

### profile

`profile` 链接关系值<sup>41</sup>可用于返回响应支持的语义配置文件列表。此元素的内容可以是一个有效 URI 列表，这些 URI 表示语义词汇表（详见方案 4.7）。

> **注意**
>
> HTTP 还支持另一个客户端偏好值：`accept-ranges`。这允许客户端向服务表明，它们可以通过 `range` 标头支持返回响应的范围值。你可以查看 HTTP 规范<sup>42</sup>了解详情。

上述值应通过两种途径提供给客户端应用程序：1) **HTTP OPTIONS 方法**，2) 在遵循 **“meta” 链接关系值**<sup>43</sup>（`rel="meta"`）时返回的资源表示。详见下面的示例。

客户端可以请求服务支持的客户端偏好，然后在自己的请求中使用此响应中的值，以向服务表明他们希望服务在响应中采用哪些格式、语言、编码等。

即使服务只支持一种可能的响应（例如 `"language":"en"`），服务仍应返回完整的 `meta` 资源，以帮助客户端发现这些重要参数的值。

### 示意图

最好在服务接口的 “Home” 资源上支持一个指向客户端偏好 `meta` 资源的链接。这个 `meta`

```json
{
  "class": [ "meta preferences" ],
  "links": [
    { "rel": ["self"], "href": "http://api.example.org/file-system/download" },
    { "rel": ["home"], "href": "http://api.example.org/file-system/" },
    { "rel": ["profile"], "href": "https://rwmbook.github.io/alps-documents/meta-preferences/meta-preferences.json" }
  ],
  "properties": {
    "allow": "GET PUT DELETE HEAD OPTIONS",
    "accept": "application/zip application/gzip",
    "enctype": "application/json",
    "charset": "utf-8",
    "encoding": "compress",
    "language": "en",
    "profile": "https://alps.example.org/downloads/"
  }
}
```

在上面的 OPTIONS 响应中，你可以看到标头空间和响应正文都包含客户端偏好参数列表。最好同时包含这两组信息，因为某些客户端应用程序可能只解析标头集合而跳过响应正文中的内容。

### 讨论

单个服务接口很少支持这里列出的所有客户端偏好。客户端最常关注的是：`allow`、`accept`、`enctype`，有时还有 `language`。`charset` 和 `encoding` 偏好很少被指明。

即使服务接口只支持偏好项的一个值，该值也应列在 `meta-preferences` 响应中。这样，`meta-preferences` 响应就成为服务选项的运行时文档，这意味着 API 调用者可以依赖此响应获得这些参数的最新列表。

> **提示**
>
> 请务必在 API 文档中加一项，告诉开发者你的接口支持 `OPTIONS` 和/或 `meta` 链接关系值。

仅依赖 HTTP `OPTIONS` 来传达支持的客户端偏好有一个缺点。HTTP 规范明确规定 `OPTIONS` 不可缓存。如果有大量 API 调用者发送 `OPTIONS` 请求，你的 API 可能会遇到扩展问题。然而，提供一个独立的 `meta-preferences` 资源（例如 `<link rel="meta" href="http://api.example.org/meta-preferences/" />`）意味着你可以将该资源标记为具有较长缓存生存期，从而减少构造和返回客户端偏好响应的开销。

### 相关

- 第 2 章
- 第 3 章
- 方案 4.7
- 第 4 章
- [链接待补充]

## 4.6 支持 HTTP 内容协商

HTTP 协议的独特之处之一在于能够选择偏好的响应正文格式。HTTP 的 `content-type` 标头实现借鉴自电子邮件规范，已促成近 1500 种不同的媒体类型在官方 IANA 媒体类型注册表<sup>45</sup>中注册，而且每年还在增加。难点在于了解使用哪种媒体类型、何时使用，以及如何使其


使客户端和服务双方能够确定内容协商的细节。

### 问题

HTTP 允许客户端和服务进行“内容协商”——即在交换消息时选择使用哪种媒体类型的过程。这是如何运作的？主动式内容协商和反应式内容协商有什么区别？服务接口在适当的情况下需要做什么来支持内容协商？

### 解决方案

HTTP 的一个重要特性是支持多种响应格式。这使得同一资源可以为某些消费者返回 HTML，为其他消费者返回 CSV，等等。更强大的是 HTTP 支持在运行时协商响应格式。换句话说，API 消费者和提供者可以在请求发出时确定响应中使用的消息格式。

HTTP 规范中描述了两种主要的“类型”的内容协商：主动式（Proactive）和反应式（Reactive）。⁴⁶ 每种都有其优点和挑战。

> **注意**
>
> HTTP 规范还提到了其他类型的内容协商，包括“条件式”（conditional）、“主动式”（active）和“透明式”（transparent）内容。这里我不打算讨论这些。如果你想了解更多相关信息，请查阅相关的 RFC 文档。⁴⁷ ⁴⁸

### 主动式内容协商（PCN）

主动式内容协商（PCN）是 HTTP 服务最容易实现的方式。使用 PCN 时，API 客户端发送一个或多个首选响应媒体类型的列表，服务器根据所提供的列表主动决定使用哪种媒体类型。这种方式有时也被称为“服务器驱动的内容协商”。

客户端通过请求中的 `Accept` 头表明其响应偏好，服务器则通过响应中的 `Content-Type` 头表明其最终选择（参见下面的示例）。

在 PCN 中，客户端*告知*服务器其偏好，而服务器最终决定响应的格式。需要注意的是，服务可能会以客户端 `Accept` 头中*未包含*的格式进行响应。以上面的示例来说，如果服务不支持任何 JSON 格式，该服务可能会以 `text/plain` 或 `application/HTML` 格式响应。

### 反应式内容协商（RCN）

除了由服务进行的主动式内容协商之外，还有另一种选择，称为反应式内容协商（RCN）。在 RCN 中，服务向客户端发送一个可能的表示格式列表，客户端需要从列表中选择首选格式，并再次发出请求，指明已选择的响应格式。这将选择权完全交到客户端应用程序手中，但也意味着每次都要多一次“往返”通信。

当存在大量变量（语言、消息格式、编码方案等）时，RCN 效果很好，但服务器和客户端如何交互以选择首选响应格式的细节在 HTTP 规范中并未明确规定。一种常见的方法是服务器以 HTTP 状态码 `300 Multiple Choices` 响应，并在响应体中列出供客户端查看的选项。`300 Multiple Choices` 响应⁴⁹ 通常携带一系列 `Link` 头，也可能带有 `Location` 头，以便客户端可以直接自动重定向。

### 示例

HTTP 同时支持主动式和反应式内容协商。以下是一些示例。

#### 主动式内容协商（PCN）

在 PCN 中，客户端在 `Accept` 头中发送首选响应格式列表，服务器决定最终选择。

```
**** REQUEST ****
GET /list HTTP/1.1
Accept: application/vnd.siren+json, application/vnd.hal+json,
application/json
...

**** RESPONSE ****
200 OK HTTP/1.1
Content-Type: application/json
....
```

在上面的例子中，服务器选择了 `application/json` 作为媒体类型。客户端可以通过“质量”或 q 值的形式提供额外的偏好信息。参见下面的示例：

```
**** REQUEST ****
GET /list HTTP/1.1
Accept: application/vnd.hal+json;q=0.8, application/json;q=0.4
...

**** RESPONSE ****
200 OK HTTP/1.1
Content-Type: application/json
....
```

在上面的例子中，客户端表示更偏好 `application/vnd.hal+json` 媒体类型。然而，服务器仍然选择了 `application/json` 作为响应格式。

> **提示**
>
> 关于质量（q）值的更多信息，请查阅 HTTP 规范<sup>50</sup>

#### 反应式内容协商（RCN）

在 RCN 中，服务器返回支持的表示格式列表，客户端做出最终选择并重新发送请求。

```
**** REQUEST ****
GET /search HTTP/1.1

**** RESPONSE ****
HTTP/1.1 300 Multiple Choices
Link: <http://api.example.org/html/search>;rel="alternate html"
Link: <http://api.example.org/api/search>;rel="alternate api"
Location: http://api.example.org/html/search
```

另一种 RCN 响应会在响应体中以 HTML 锚点标签的形式返回链接值：

```
**** REQUEST ****
GET /search HTTP/1.1

**** RESPONSE ****
HTTP/1.1 300 Multiple Choices
Content-Type: text/html

<html>
  <title>Multiple Choices</title>
  <body>
    <h1>Multiple Choices</h1>
    <ul>
      <li><a href="http://api.example.org/html/search">HTML</a>
    </li>
      <li><a href="http://api.example.org/api/search">API</a></li>
    </ul>
  </body>
</html>
```

### 讨论

当服务支持多种表示格式时，最常见的方法是使用主动式（服务器驱动）内容协商。这是最简单的交互方式，尽管客户端可能无法获得其首选格式。

RCN 有一些主要缺点。服务器本质上是在“猜测”要返回什么格式。当客户端和服务双方都支持……

## 4.7 为机器客户端发布完整词汇表

在创建将被其他服务访问的服务（例如机器对机器接口）时，确保双方对彼此有扎实的“理解”非常重要。这意味着每条消息都是完整的，并且双方都能识别消息中的内容。这就是 RESTful 实现的本质。Roy Fielding 这样解释：“一个 RESTful API（如果做得正确）就是一个面向词汇量有限的客户端的网站。” ⁵¹

### 问题

我们如何确保 API 消费者能够充分“理解”服务的响应？发送“自描述”消息意味着什么？在描述消息时需要包含哪些内容？

### 解决方案

确保 API 消费者“理解”API 提供者消息的最佳方式是编写一份*完整词汇表*文档，列出消息中可能出现的所有重要数据和操作属性。Leonard Richardson 将这些值称为“魔法字符串” ⁵²。这些字符串被服务称为“有特定含义”的值，API 提供者和消费者可以利用它们来“相互告知”重要信息。在本书中，我将这些魔法字符串的集合称为*词汇表*（*vocabularies*），有时也称为*配置文件*（*profiles*）。

> **换个名字的配置文件**
>
> 我见过几个用来标识这份权威数据和操作属性列表的术语，比如“字典”（dictionary）和“术语表”（glossary）。但我不会使用“数据字典”或“数据模型”这两个术语。因为服务词汇表定义的不仅仅是服务的数据属性，它们还定义了所有操作细节，包括链接、表单和查询字符串。

服务配置文件包含其在消息中使用的*所有*标识符。例如，考虑以下 HTML 表单：

```
<form name="create-template"
  action="http://api.example.org/users"
  method="post" enctype="application/x-www-form-urlencoded">
  <input name="id" value="q1w2e3r4" />
  <input name="familyName" value="Mark" />
  <input name="givenName" value="Morkelson" />
  <input name="telephone" value="+1-555-123-4567" />
  <input name="email" value="mork@example.org" />
  <input name="status" value="active" />
  <input type="submit" />
</form>
```

这个表单中的词汇信息包括：`create-template`、`id`、`familyName`、`givenName`、`telephone`、`email` 和 `status`。API 服务和 API 消费者都需要识别并“理解”这些值所代表的含义。注意，我并没有包含这些元素的*值*（例如 `id=q1w2e3r4`），只包含了名称。此外，我也没有在列表中包含媒体类型的结构元素（例如 `name`、`action`、`method`、`enctype` 等）。

仅仅识别词汇表元素还不够。你还需要对它们代表什么以及它们如何出现在消息中进行一些解释。例如，`create-template` 这个语义值可能作为 HTTP 消息中 `form` 元素的 `name` 值出现。这标识了客户端可以用来向系统添加新记录的表单。

你还可以在语义配置文件文档中包含其他信息，例如字符串值的来源。例如，`familyName` 定义在在线字典 Schema.org ⁵³ 中。

### 示例

以下是 `person` API 的一些示例词汇表文档（也称为语义配置文件），见 [链接待补充]。以下是该服务的一些示例输出：

```
{
  "collection": {
    "version": "1.0",

```




```json
"href": "http://localhost:8181",
"title": "BigCo Activity Records",
"links": [
  {
    "name": "home", "href": "http://localhost:8181/",
    "rel": "home", "prompt": "Home"
  },
  {
    "name": "list", "href": "http://localhost:8181/list/",
    "rel": "list", "prompt": "List"
  }
],
"items": [
  {
    "id": "22s3k36pkn4",
    "rel": "person",
    "href": "http://localhost:8181/22s3k36pkn4",
    "data": [
      {"name": "id", "value": "22s3k36pkn4", "prompt": "id"},
      {"name": "givenName", "value": "Mork", "prompt": "givenName"},
      {"name": "familyName", "value": "Mockery", "prompt": "familyName"}
    ],
    "links": [
      {
        "name": "read", "href": "http://localhost:8181/22s3k36pkn4",
        "rel": "read", "prompt": "Read"
      }
    ]
  }
],
"queries": [
  {
    "name": "filter",
    "href": "http://localhost:8181/filter/",
    "rel": "filter",
    "prompt": "Search",
    "data": [
      {"name": "givenName", "value": ""},
      {"name": "familyName", "value": ""}
    ]
  }
],
"template": [
  {"name": "id", "value": "", "prompt": "id"},
  {"name": "givenName", "value": "", "prompt": "givenName"},
  {"name": "familyName", "value": "", "prompt": "familyName"}
]
```

至少，你应该列出所有语义标识符并包含描述：

| Identifier | Description |
|---|---|
| id | Record identifier for a person record |
| givenName | givenName for a person record |
| familyName | familyName for a person record |
| person | indicates a person record |
| home | Hypermedia control to navigate to Home view |
| list | Hypermedia control to navigate to List view |
| read | Hypermedia control to navigate to single person record |
| filter | Hypermedia control to filter the List view |

作为附加信息，你还可以列出每个元素可能出现在消息中的位置：

| Identifier | Description | Element |
|---|---|---|
| id | Record identifier for a person record | name |
| givenName | givenName for a person record | name |
| familyName | familyName for a person record | name |
| person | indicates a person record | rel |
| collection | Identifies a collection of records | rel |
| item | Identifies a single record | rel |
| home | Hypermedia control to navigate to Home view | rel, name |
| list | Hypermedia control to navigate to List view | rel, name |

请注意，对于 FormsJSON 格式，一些语义值作为 JSON 中的结构元素出现（`id`、`givenName`、`familyName`、`person`），而其他则作为值元素出现（`collection`、`item`、`list`、`home`、`read`、`filter`）。这种结构与值的混合使得每种媒体类型独一无二。如果你的服务支持多种媒体类型，你需要提供语义配置文件信息，以便正确地将语义元素映射到消息元素。

```json
{
  "person": {
    "links": [
      {
        "id": "home",
        "name": "home",
        "href": "http://localhost:8181/",
        "rel": "home",
        "title": "Home",
        "method": "GET",
        "properties": []
      },
      {
        "id": "list",
        "name": "list",
        "href": "http://localhost:8181/list/",
        "rel": "list collection",
        "title": "List",
        "method": "GET",
        "properties": []
      },
      {
        "id": "filter",
        "name": "filter",
        "href": "http://localhost:8181/filter/",
        "rel": "filter collection",
        "title": "Search",
        "method": "GET",
        "properties": [
          {"name": "givenName", "value": ""},
          {"name": "familyName", "value": ""}
        ]
      }
    ],
    "items": [
      {
        "links": [
          {
            "id": "read_22s3k36pkn4",
            "name": "read",
            "href": "http://localhost:8181/22s3k36pkn4",
            "rel": "read item",
            "title": "Read",
            "method": "GET",
            "properties": []
          }
        ],
        "id": "22s3k36pkn4",
        "givenName": "Mork",
        "familyName": "Mockery"
      }
    ]
  }
}
```

## Discussion

本质上，语义配置文件承载了服务的域元素（person、list、filter 等）。你可以将语义配置文件视为记录服务域的方法。

值得指出的是，这些语义配置文件并不能解释 *如何* 过滤超媒体控件工作——只是它可能出现在响应中。语义配置文件假定 API 消费者应用程序独立于配置文件理解该媒体类型。理解消息格式与理解消息 *内部* 的语义之间的这种分离，是设计良好的超媒体服务的重要特性。

当你的服务不使用已注册的媒体类型而是使用纯 JSON 或 XML 等序列化格式时，服务通常会将域的语义嵌入到消息的结构中。例如，这里是 person 服务在 XML 中的一个可能响应：

```xml
<lists>
  <home href="..." rel="collection"/>
  <list href="..." rel="collection"/>
  <filter href="..." method="get" rel="collection">
    <givenName></givenName>
    <familyName></familyName>
  </filter>
  <person id="..." rel="item read" href="...">
    <familyName>...</familyName>
    <givenName>...</givenName>
  </person>
</lists>
```

而语义配置文件映射看起来像这样：

| Identifier | Description | Element (Cj) | Element (Fj) | Element (XML) |
|---|---|---|---|---|
| id | Record identifier for a person record | name | KEY | ATTRIBUTE |
| givenName | givenName for a person record | name | KEY | ELEMENT |
| familyName | familyName for a person record | name | KEY | ELEMENT |
| person | indicates a person record | rel | KEY | ELEMENT |
| collection | Identifies a collection of records | rel | rel | rel |
| item | Identifies a single record | rel | rel | rel |
| home | Hypermedia control to navigate to Home view | rel, name | rel, name, id | ELEMENT |
| list | Hypermedia control to navigate to List view | rel, name | rel, name, id | ELEMENT |
| read | Hypermedia control to navigate to single person record | rel, name | rel, name, id | rel |
| filter | Hypermedia control to filter the List view | rel, name | rel, name, id | ELEMENT |

在这种情况下，域语义与消息格式如此交织，以至于内部域元素的更改会导致外部接口的破坏性结构更改（例如重命名元素、添加/删除属性等）。只要可能，就应保持消息细节（外部接口）独立于域细节（内部功能）。当我看到域语义作为响应中的结构元素出现时，这会提醒我重新思考消息设计，以确保我没有将内部细节泄露到外部接口。参见 Recipe 4.4 了解更多关于此主题的内容。

## Related

- Chapter 2
- Chapter 3
- Chapter 4
- Recipe 4.4
- [即将发布的链接]

## 4.8 Supporting Shared Vocabularies in Standard Formats

当服务接口采用使用标准、已注册媒体类型来交换消息的做法时，它们还需要标准化在运行时表达服务域特定属性和操作（或 *语义*）的方式。这与消息格式挑战类似。解决方案是依赖标准化格式来表达含义。真正的难点在于将内部数据/对象模型表达为一致的外部语义模型。

## Problem

我们如何标准化机器之间传输内容含义的方式？有哪些格式可用于此工作，以及表达内部数据和流程模型为标准化语义模型的模式是什么？

## Solution

正如 API 消费者依赖标准化协议（例如 HTTP）和标准化消息格式（例如 SIREN、Collection+JSON 等），它们也可以从标准化词汇表中受益。这意味着以客户端理解的标准格式列出所有可能出现在 API 中的数据属性和操作名称。

这意味着需要有一个单一来源，包含可能出现在 API 中所有事物的可能名称。例如，`id`、`name`、`class`、`rel` 或类似结构元素中可能出现的任何值。这些值的集合可以涵盖一个主题或域（如“用户管理”或“会计”）。

有几种选项可以以标准化形式携带词汇数据。最知名的是使用资源描述框架（RDF）⁵⁴ 格式，如 RDF/XML⁵⁵、Turtle⁵⁶ 和 JSON-LD⁵⁷。JSON-LD 格式是我在 API 想要使用基于 RDF 的格式时更常看到的。

我用来记录词汇的格式是 Application-Level Profile Semantics（ALPS）⁵⁸ 格式。ALPS 有 XML、JSON 甚至 YAML 的序列化。

### Example

下面是用 FOAF（Friend of a Friend）⁵⁹ ⁶⁰ 词汇表达的 `person` 的 RDF 版本。

```turtle
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .

<#JW>
        a foaf:Person ;
        foaf:name "James Wales" ;
        foaf:mbox <mailto:jwales@bomis.com> ;
        foaf:homepage <http://www.jameswales.com> ;
        foaf:nick "Jimbo" ;
        foaf:depiction <http://www.jameswales.com/aus_img_small.jpg>
;
        foaf:interest <http://www.wikimedia.org> ;
        foaf:knows [
```



a foaf:Person ;
    foaf:name "Angela Beesley"
].

<http://www.wikimedia.org>
    rdfs:label "Wikimedia"
```

下面是同一份RDF文档的图示，使用了“:isSemantic”可视化服务61。

![](img/b73a1e885955ee1243f0097b04d814ff_213_0.png)

另一种方法是，下面是ALPS文档的一部分，它详细说明了我们在本食谱中一直引用的人员服务API。

有关ALPS格式的详细信息，请参阅食谱2.4。

```
{
  "$schema": "https://alps-io.github.io/schemas/alps.json",
  "alps":
  {
    "version": "1.0",
    "title": "Person Service API",
    "descriptor" : [
      ...
      {"id": "home", "type": "semantic",
        "title":"Home (starting point) of the person service",
        "tag":"taxonomy",
        "descriptor": [
          {"href": "#goHome"},
          {"href": "#goList"}
        ],
        "doc" : {"value" : "Person API starting point"}
      },
      {"id": "collection", "type": "semantic",
        "title":"List of person resources",
        "tag":"taxonomy",
        "descriptor": [
          {"href": "#person"},
          {"href": "#goHome"},
          {"href": "#goList"},
          {"href": "#goFilter"},
          {"href": "#goItem"},
          {"href": "#doCreate"}
        ],
        "doc" : {"value" : "List of person resources"}
      },
      {"id": "item", "type": "semantic",
        "title":"Single person resource",
        "tag":"taxonomy",
        "descriptor": [
          {"href": "#person"},
          {"href": "#goHome"},
          {"href": "#goList"},
          {"href": "#goFilter"},
          {"href": "#goItem"},
          {"href": "#doUpdate"},
          {"href": "#doStatus"},
          {"href": "#doRemove"}
        ],
        "doc" : {"value" : "A single person resource"}
      },
      ...
```

这是使用“app-state-diagram (ASD)”工具[62]绘制的人员完整配置文件的图形表示。

![](img/b73a1e885955ee1243f0097b04d814ff_215_0.png)

你会看到RDF和ALPS之间的一个关键区别：RDF关注*数据*项之间的关系，而ALPS关注*动作*项之间的关系。有一种词汇格式能够很好地弥合这一差距，那就是Hydra本体[63]。

TK：在此展示Hydra/JSON-LD版本（参见：https://www.hydraecosystem.org/Example

## 讨论

服务API可以将语义配置文件URL用作*标识符*，而无需完全理解语义配置文档中包含的内容。例如，通过在响应的头部集合中包含配置文件URL，服务API正在告知API消费者该服务API“使用”的词汇表。

```
**** REQUEST ****
GET /shopping/ HTTP/1.1
Host: api.example.org
Accept: application/vnd.collection+json

**** RESPONSE ****
HTTP/1.1 200 OK
Content-Type: application/vnd.collection+json
Link: <http://docs.alps.io/shopping-v2.json>; rel="profile"
```

在上面的示例中，API服务正在“宣传”其支持购物语义配置文件的版本2。有关API提供者和消费者如何协商语义配置文件支持的更多信息，请参阅食谱3.5。

语义配置文件与模式文档的作用不同。模式用于描述对象或文档消息。语义配置文件用于描述这些对象（或文档）彼此交互的方式。发布配置文件文档有助于开发人员（和机器）理解问题空间，即API内部的可能性。根据你使用的消息格式，你可能还需要发布模式文档，以帮助人们和机器理解将在问题空间内传递的对象和文档类型。有关如何利用API的模式文档的更多信息，请参阅[待定链接]。

还值得指出的是，像ALPS这样的语义配置文件文档与OpenAPI、AsyncAPI、Protobuf、SOAP等API定义文档不是一回事。语义配置文件为一组API（支持该配置文件的API）提供了完整的属性和动作词汇表。API定义文档提供了关于具体如何

## 问题

常见的服务定义文档（SDD）格式有哪些？如何轻松地与API消费者共享SDD？共享SDD时应使用哪些URL和/或链接关系值？除了API中的直接链接外，SDD的链接是否还应列在其他位置？是否应出现在其他文档中？

## 解决方案

对于大多数API，存在一组在运行时共享的固定URL和消息主体。这些固定的“端点”和“对象”可以根据服务接口实现的风格，采用多种不同格式进行文档化。

最常见的API风格（及其相关的SDD）包括：

- **HTTP 增删改查**：OpenAPI、Web 应用程序描述语言（WADL）
- **事件驱动**：AsyncAPI、CloudEvents
- **远程过程调用**：Protocol Buffers、XML-RPC、JSON-RPC
- **远程数据查询**：Schema Definition Language、OData
- **远程消息传递**：Web 服务描述语言（WSDL）
- **超媒体**：应用程序级配置文件语义（ALPS）

通常，SDD旨在简化生成人类可读文档和/或生成API消费者代码的工作。有时，SDD实际上是*从*代码生成的。例如，OpenAPI和WSDL都可以从现有源代码生成。在撰写本文时，许多格式都是作为服务接口设计过程的一部分“手工”编写的。

无论文档是如何创建的，都最好将其发布在一个容易找到的位置。这将帮助API客户端应用程序开发人员快速上手并成功使用服务API。最好的方法是在HTTP响应中返回指向SDD的链接，可以作为`link`头、响应主体中的链接，或两者兼有。RFC8631定义了链接关系值`service-desc`⁶⁴，用于标识指向SDD文档的链接。在响应HTTP `OPTIONS`请求时，最好也在`link`头中返回指向SDD的指针。详见下面的示例。

> **提示**
> 
> 最好在API元数据文档中包含指向SDD文档的链接（参见食谱4.10）。

## 示例

有三个好地方可以用来宣传你的API的SDD位置：1）HTTP `link`响应头，2）作为接口的服务元数据的一部分，以及3）作为HTTP `OPTIONS`响应的一部分。

### HTTP 响应中的 SDD

你可以在任何API响应中将指向API SDD的指针作为`link`头返回：

```
HTTP/1.1 200 OK
Content-Type: application/vnd.hal+json
Link: <http://api.example.org/service-desc>; rel=service-desc
...
```

你也可以在响应主体中返回此信息：

```
HTTP/1.1 200 OK
Content-Type: application/vnd.collection+json
...

{
  "collection": {
    "links": [
      {"rel": "service-desc", "href": "http://api.example.org/service-desc"}
    ]
    ...
  }
}
```

当格式支持时，你可以同时在响应的头部和主体中返回SDD位置。

### 服务元数据响应中的 SDD

你还可以在API的元数据文档中返回SDD的位置（参见食谱4.10）。我推荐的服务元数据格式是APIs.json，该规范为最常见的SDD格式保留了关键字。

以下是一个APIs.json文档片段，其中声明了对OpenAPI SDD的支持：

```
{
  "name": "Example API",
  "type": "Index",
  "created": "2014-04-07",
  "modified": "2020-09-03",
  "url": "http://example.com/apis.json",
  "specificationVersion": "0.14",
  "apis": [
    {
      "name": "Example API",
      "humanURL": "http://example.com",
      "baseURL": "http://api.example.com",
      "properties": [
        {"type": "OpenAPI", "url": "http://example.com/openapi.json"}
      ]
    }
    ...
  ]
}
```

> **注意**
> 
> 如果你使用的SDD格式目前未被APIs.json规范覆盖，你也可以将自己的格式作为扩展添加。

### HTTP OPTIONS 响应中的 SDD

你也可以在OPTIONS响应的链接头中，和/或主体中，包含SDD。





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




对 Web 上运行的服务进行健康与状态监控是一个常见的挑战。好消息是，在过去几年中，在标准化这一过程方面已经涌现出许多优秀成果。在我撰写本文时，我的电子邮件收件箱提示，IETF 正在审查一份名为“HTTP API 健康检查响应格式”（Health Check Response Format for HTTP APIs）的草案文件。⁷² 该规范首次发布于 2018 年，我在相当长一段时间内一直将其作为实现自身健康与状态检查的指南。

健康检查规范定义了几个属性。以下是基本属性及一些说明：

- **status**：表示服务状态（`pass`、`fail`、`warn`）。
- **version**：服务的公共版本。
- **releaseId**：服务发布/版本标识。
- **notes**：与当前健康状态相关的注释数组。
- **output**：在`fail`或`warn`状态下的原始错误输出。
- **checks**：一个对象，提供其他下游系统的详细健康状态。
- **links**：一个对象，包含链接关系和 URI，用于指向可能包含更多服务健康信息的外部链接。
- **serviceId**：服务的唯一标识符。
- **description**：服务的人类可读描述。

> **注意**
>
> `checks`对象表示一组相关的“下游”服务。这有点像对其他依赖服务健康状态的“前瞻”。该对象还定义了更多属性，你可以参考规范了解详情。

在实现健康检查规范时，还需要记住以下额外事项。

## 健康资源

## HTTP API 健康检查响应格式

![](img/b73a1e885955ee1243f0097b04d814ff_231_0.png)

> **注意**
>
> 关于健康检查资源的详细信息，请参阅与本书相关的 ALPS 文档在线合集。⁷³

### 示例

一个典型的 `application/health+json` 响应如下所示：

```json
HTTP/1.1 200 OK
Content-Type: application/health+json
Cache-Control: max-age=3600
ETag: W/"i8u7y6t5r4e3w2"
...
{
  "status": "pass",
  "version": "1",
  "releaseId": "1.2.2",
  "notes": [""],
  "output": "",
  "serviceId": "f03e522f-1f44-4062-9b55-9587f91c9c41",
  "description": "health of authz service",
  "checks": {
    "cassandra:responseTime": [
      {
        "componentId": "dfd6cf2b-1b6e-4412-a0b8-f6f7797a60d2",
        "componentType": "datastore",
        "observedValue": 250,
        "observedUnit": "ms",
        "status": "pass",
        "affectedEndpoints": [
          "/users/{userId}",
          "/customers/{customerId}/status",
          "/shopping/{anything}"
        ],
        "time": "2018-01-17T03:36:48Z",
        "output": ""
      },
      ...
    ]
  }
}
```

请注意使用 `cache-control` 和 `etag` 响应头来指示客户端应用保留文档的本地副本，以减少服务的流量负载。

### OPTIONS 响应中的健康检查信息

你可以通过在响应中包含一个 `link` 关联值来“宣传”对健康检查模式的支持。我对所有 OPTIONS 响应都采用这一做法。

```http
**** REQUEST ****
OPTIONS / HTTP/1.1
Accept: application/vnd.collection+json, application/html, application/json
...

**** RESPONSE ****
HTTP/1.1 200 OK
Content-Type: application/vnd.collection+json
Link: <http://api.example.org/health>; rel="https://api.example.org/rels/health"
...
```

### Service-Meta 响应中的健康检查信息

我还在服务元数据文档中包含对健康检查的支持（参见[方法 4.10](#)）。

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
        {"type": "OpenAPI", "url": "http://example.com/openapi.json"},
        {"type": "Health", "url": "http://example.com/health.json"}
      ],
      "contact": [{"FN": "APIs.json", "email": "info@apisjson.org"}]
    }
  ],
  "..."
}
```

请注意，**APIs.json** 中的“health”类型是一个扩展值（未包含在 **APIs.json** 规范中）。详见[方法 4.10](#)。

### 讨论

在大多数情况下，健康检查只要遵循基本要点就会很有用。报告 `pass`、`fail` 和 `warn` 通常对服务消费者来说已经足够了。试图将健康检查用作调试或诊断工具并不是一个好主意。这些用途应留待其他资源请求。

> **警告**
>
> 本方法基于“HTTP API 健康检查响应格式”文档的 draft-06 版本。当你读到本书时，可能已经出现新草案和/或已发布最终批准的 RFC。请务必关注该规范文档，以便及时了解变更。

有些读者可能想设置一个“回调”健康端点，允许其他人订阅来自你服务的定期响应。但这并不被推荐。随着服务用户数量的增长，你可能会收到数百乃至数千个需要处理的回调请求。这很容易压垮你的服务。因此，请坚持提供一个带有 `cache-control` 值的 HTTP GET 端点，以降低大量健康检查请求的影响。如果最终竟是健康检查导致服务无法满足服务级别协议，那就真是莫大的讽刺了！

规范指出，健康检查的响应是“动态的”；响应内容可根据调用上下文进行定制。例如，匿名请求可能只返回`status`值（`pass`、`fail`、`warn`），而授权请求（如管理员用户）则可能看到服务状态的详细信息。

该规范没有提供太多关于扩展 `application/health+json` 响应的细节。如果你想包含未在已定义属性中覆盖的额外信息，请务必以向后兼容的方式进行，并提供指向人类可读文档的链接以解释你的扩展。

### 相关

- 第 2 章
- 第 3 章
- 第 4 章
- [链接待补充]

成功的关键在于将错误视为“备用响应”，而不是失败的请求。换句话说，始终将错误报告作为服务接口的一个特性，而不是失败条件。

### 示例

问题详情规范（Problem Details，RFC7807）定义了一小组元素：

- **type**：标识问题类型的 URI 引用 [RFC3986]。
- **title**：问题类型的简短、人类可读摘要。
- **status**：服务生成的 HTTP 状态码（这是数字，不是字符串）。
- **detail**：特定于此次问题发生情况的人类可读说明。
- **instance**：标识问题具体发生情况的 URI 引用。

规范为**type**定义了一个默认值："about:blank"。它还假定在**type**中找到的任何 URI 都是指向描述问题的人类可读文档的指针。

下面是一条典型的问题详情消息：

```
HTTP/1.1 403 Forbidden
Content-Type: application/problem+json
Content-Language: en

{
  "type": "https://example.com/probs/out-of-credit",
  "title": "You do not have enough credit.",
  "detail": "Your current balance is 30, but that costs 50.",
  "instance": "/account/12345/msgs/abc",
  "status": 403
}
```

还可以使用你自己的属性来扩展问题详情记录：

```http
HTTP/1.1 403 Forbidden
Content-Type: application/problem+json
Content-Language: en

{
  "type": "https://example.com/probs/out-of-credit",
  "title": "You do not have enough credit.",
  "detail": "Your current balance is 30, but that costs 50.",
  "instance": "/account/12345/msgs/abc",
  "status": 403,
  "balance": 30,
  "accounts": ["/account/12345", "/account/67890"]
}
```

注意，`type` 元素中的 URL 应指向一个文档，该文档定义了“额度不足”问题的用途和含义。该文档还应包含 `balance` 和 `accounts` 元素的定义。这样，`type` 属性就指向了该问题表示的语义配置文件。有关语义配置文件的详细信息，请参阅方案 4.7。

问题详情消息中的 `type`、`title` 和 `status` 是“固定的”——无论哪个 API 或服务返回该消息，它们始终保持不变。但 `detail` 和 `instance` 属性则特定于当前案例。请参见下面的示例：

```http
HTTP/1.1 403 Forbidden
Content-Type: application/problem+json
Content-Language: en

{
  "type": "https://example.com/probs/out-of-credit",
  "title": "You do not have enough credit.",
  "detail": "Your current balance is 20, but that costs 50.",
  "instance": "/account/12345/msgs/q1w2e3",
  "status": 403,
  "balance": 20,
  "accounts": ["/account/r4er3w2", "/account/y6t5r4"]
}
```

现在将上面看到的问题详情响应与之前的进行比较。在这两种情况下，`type`、`title` 和 `status` 的值相同。但 `detail`、`instance` 以及 `balance` 和 `accounts` 的值不同。

RFC7807 规范注册了 `application/problem+json` 和 `application/problem+xml` 两种媒体类型标识符。然而，它没有提供问题详情消息的 XML 版本示例。下面是我一直用于问题详情响应 XML 版本的表示形式。

```http
HTTP/1.1 403 Forbidden
Content-Type: application/problem+xml
Content-Language: en
```

```xml
<problem-details>
  <type>https://example.com/probs/out-of-credit</type>
  <title>You do not have enough credit.</title>
  <detail>Your current balance is 30, but that costs 50.</detail>
  <instance>/account/12345/msgs/abc</instance>
  <status>403</status>
</problem-details>
```

> **警告**
>
> 截至撰写本文时，RFC7807 正在 IETF 标准委员会进行更新。读者应密切关注该规范，并为未来可能发生的任何变化做好准备。

## 讨论

该规范特别指出，当简单的 HTTP 4xx 或 5xx 状态报告就足够时，不应使用问题详情消息。例如，如果用户尝试更新现有记录但无权这样做，对 PUT 请求返回 403 响应可能就足够了。但是，如果用户为 PUT 消息发送了无效的请求体，返回一个问题详情响应可能会有所帮助，该响应描述问题并提供了在重新提交前修复错误的方法。

服务不应使用这种格式向客户端应用程序返回“调试”信息。该格式旨在共享有关*接口*的详细信息，而不是 API 背后底层服务的信息。

在创建新的问题详情 `type` 时，你应该记录三件事：

- 一个 `type` URI（例如 *http://api.example.org/problems/insufficient-funds*）
- 一个 `title`（例如“您的账户余额不足以完成此交易。”）
- 一个适用于此问题的 `status` 代码（例如 403）

只要有可能，最好限制你定义的新问题详情 `type` 的数量，并创建足够通用、可重复使用的类型。你还可以使用消息的 `detail`（文本）和 `instance`（URL）元素来自定义问题表示。

还可以在问题详情响应中指示使用 `Retry-After` 头。这样在需要时可以向 API 使用者指示重试详情。

## 相关

- 第 2 章
- 第 3 章
- 第 4 章
- [待添加链接]

## 4.13 通过客户端提供的标识符提升吞吐量

通常，期望服务器为客户端应用程序创建的任何资源生成并返回标识符是一种常见做法。这简化了确保资源 URL 唯一的过程，但也有缺点。当涉及多步骤流程时尤其如此（创建 A，从 A 获取唯一 ID，用它来创建 B，等等）。

返回 `409 Conflict` 状态码，并附带关于客户端如何解决问题的附加信息。

## 示例

生成唯一标识符的一个非常简单的方法是使用你所用编程语言的随机数生成器。下面是 JavaScript 中的一个示例实现：

```javascript
// generating unique identifiers
function makeId() {
  var rtn;

  rtn = String(Math.random());
  rtn = rtn.substring(2);
  rtn = parseInt(rtn).toString(36);

  return rtn;
}
```

调用上面的 `makeId()` 函数将生成如下紧凑的标识符字符串："1oyte4x0zep"。

有些随机数生成器并不像我们大多数人所希望的那样“随机”。因此，你可以将日期时间值与随机数混合，生成符合 RFC4122 的 UUID 值。下面是一个可以实现此功能的示例函数：

```javascript
// generating UUIDs (Public Domain/MIT)
function generateUUID() {
  var d = new Date().getTime();
  var d2 = (typeof performance !== 'undefined' && performance.now && (performance.now() * 1000)) || 0;
  return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g,
  function(c) {
    var r = Math.random() * 16;
    if (d > 0) {
      r = (d + r) % 16 | 0;
      d = Math.floor(d / 16);
    } else {
      r = (d2 + r) % 16 | 0;
      d2 = Math.floor(d2 / 16);
    }
    return (c == '=' ? 'x' : (r & 0x3 | 0x8)).toString(16);
  });
}
```

UUID 函数的输出产生冲突的可能性要小得多，但生成的输出（例如 "e6db8698-7128-478d-8658-12c2cb9dd126"）比前面展示的简单示例要长。

这些唯一值可以从客户端应用程序作为 HTTP POST 操作消息体的一部分发送：

```html
<p>Create a New Person</p>
<form name="create" action="/persons/" method="post">
  <input type="hidden" name="unique-id" value="1oyte4x0zep" />
  <input type="text" name="name" value="Mark Morkleson" />
  <input type="submit" />
</form>
```

或者作为 HTTP PUT 操作的 URL 的一部分：

```html
<p>Create a New Person</p>
<form name="create" action="/persons/1oyte4x0zep" method="put">
  <input type="text" name="name" value="Mork Markleson" />
  <input type="submit" />
</form>
```

> **提示**
>
> 有关使用 HTTP PUT 创建新资源的更多信息，请参阅[方案 4.14](#)。

服务端组件负责确认客户端提供的唯一标识符是否可接受。例如，该标识符（及相关资源）是否已存在？该标识符是否格式正确（例如，具有适当长度的字符串、合适的字符等）？如果出于任何原因，客户端提供的标识符不可接受，服务接口应拒绝写操作，并返回 HTTP 状态码 `409 Conflict`。

## 讨论

本质上，每一步都必须等待前一步完成后才能继续。然而，如果依赖客户端提供的标识符，你或许可以做到以下几点：

```javascript
var cId = makeId();
var aId = makeId();
var sId = makeId();
Promise.all([
  writeCustomer(cId),
  writeAccount(cId,aId),
  writeSalesRecord(cId,aId,sId)
])
.then(() => console.log('All done!'));
```

客户端提供的标识符方案的一个副作用是，资源标识符不是顺序的，而且人们很难阅读或记住它们。这种非顺序性可能使恶意编码者更难猜到你的资源标识符。但这也可能使合法开发人员更难浏览记录集合。你可以通过允许客户端提供“友好 ID”来缓解第二个问题，这些友好 ID 可以显示在列表和其他输出中，而生成的标识符则隐藏在用户界面中。

```html
<ul>
  <li id="q1w2e3r4">Sally-R</li>
  <li id="p0o9i8u7">Jane-Q</li>
  <li id="6y5t4r3e">Barb-Z</li>
</ul>
```

## 相关

- 第 2 章
- 第 3 章
- 第 4 章
- [待添加链接]





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


另一种方式。通常这意味着提供一个操作，从其他地方“导入”（`importState`）状态，以及一个操作，将你的服务状态“导出”（`exportState`）到其他地方。

与其在接口中添加显式的导入/导出操作，你也可以将该功能构建到 API 的选定现有操作中。例如，在创建新用户帐户时，你可以安排通过 FORM 元素接受输入。

以上用例是按“值”转移状态的示例——使用运行时 FORMS 描述将确切的状态值从一个服务发送到下一个服务。这是与你无法控制的其他服务进行互操作的最简单方式。

你还可以安排按“引用”转移状态——通过共享一个指向你想要传输的数据集合的 URL。这需要服务之间进行更多的协调，因为双方都需要提前了解通过 URL 共享数据的想法——以及共享数据的存储格式。

## 示例

有三种方式可以在服务之间实现状态转移：

- 使用内联现有 FORMS 按值传递
- 使用专用操作（例如 `importState` 和 `exportState`）按值传递
- 使用专用操作或 FORM 按引用传递（通过共享 URL）

### 使用现有 FORMS 按值传递

通过超媒体表单在服务之间传输状态数据是解决问题最简单、最直接的方式：

```html
<form action="http://api.example.org/shopping/cart"
      method="post" name="cartCreate">
  <input name="cartId" value="q1w2e3r4t5y6u7i8" />
```

当要传输的状态数据量相对较大和/或复杂（例如包含 15 个项目加元数据的集合）时，这种方法效果很好。优点是你可以在一步中传输大量数据。缺点是你需要提前协调这种状态转移，因为你需要确保双方同意要传输的数据属性名称、内容以及集合的大致形态。

### 按引用传递

你还可以通过共享一个“指针”来在服务之间传递状态数据，该指针指向存储在双方都可访问的其他地方的某个状态集合。最简单的做法是共享一个 URL，该 URL 指向的资源将以调用方期望（或明确请求）的媒体类型和词汇表格式进行响应。

```html
<form action="http://api.example.org/users/a1w2e3r4"
      method="post" enctype="multipart/form-data">
  <input type="file" name="userData"
         accept="application/vnd.collection+json" />
```

上面的示例描述了目标服务可以实现的一种“上传”操作。同样，成功的关键在于双方提前就预期的数据格式和词汇表内容达成一致。你可以在发布 API 时记录这些传输操作，以便让他人了解你的服务接口期望如何向其他服务发送信息或从其他服务接收信息。

## 讨论

如果可能的话，在你的 API 响应中提供超媒体表单，以便在一步内处理状态转移。这将允许其他服务作为 API 客户端直接执行转移。这不需要客户端和服务器之间进行额外的协调。

另外，保持服务之间的编排最少。例如，最好一步完成状态转移，而不是要求 API 消费者在执行其他操作之前先“启动会话”或“登录”。如果需要身份验证，请将该交互作为目标状态转移重定向的结果。

```
**** REQUEST ****
POST /shopping/cartCreate HTTP/1.1
Host: api.example.org
Content-Type: application/x-www-form-urlencoded

cartId=q1w2e3r4&cartName=Mike's%20Cart

**** RESPONSE ****
HTTP/1.1 Unauthorized
WWW-Authenticate: Basic

**** REQUEST ****
POST /shopping/cartCreate HTTP/1.1
Host: api.example.org
Content-Type: application/x-www-form-urlencoded
Authorization: Basic q1i8w2o9e3p0r4u7t5...

cartId=q1w2e3r4&cartName=Mike's%20Cart

**** RESPONSE ****
HTTP/1.1 200 OK
Content-Type: application/vnd.collection+json
Authorization: Basic q1i8w2o9e3p0r4u7t5...
Link: <http://docs.alps.io/shopping.json>; rel="profile"

{
  "alps" : {
    ...
  }
}
```

如果你决定支持“按引用”状态转移，最好使用结构化媒体类型（如 HAL、SIREN、Collection+JSON 等）作为消息格式，并在传递数据时提供词汇表引用（例如 ALPS URI）。这将提高上传数据被接受的可能性，仅在接收服务确认格式和词汇表已被接收方“理解”时才会接受。

## 处理跨源资源共享（CORS）限制

请注意，HTML 浏览器不支持从“外部”域跨源 POST 消息或上传文档。这与 HTML 浏览器实现的跨源资源共享（CORS）<sup>78</sup> 限制有关。如果你正在实现一个支持“按引用”状态转移（例如文件上传）的 API，你可能还需要发出标头（例如 `Access-Control-Allow-Origin` 等），以允许 API 消费者成功上传其文档。

## 相关

- 第 2 章
- 第 3 章
- 第 4 章
- [链接待添加]

## 4.16 为依赖服务提供运行时回退

当一个服务调用另一个服务时，第一个服务*依赖*于第二个服务。由于这些服务调用依赖网络，因此可能会出现一长串问题。虽然单个依赖服务调用失败的可能性可能很小，但如果你的服务依赖多个外部服务，你的失败概率会呈指数级增长。在实现服务 API 时，你需要考虑这种失败可能性并缓解任何问题。

### 问题

在某个服务依赖另一个外部服务的情况下，如何降低失败风险？可以使用哪些模式来限制这类“致命依赖”的可能性，并在运行时发生时绕过它们？我们如何确保维护

shopping.example.com/home"). 然后，当初始位置无法响应时，你可以更新运行时代码以指向备用服务。这对状态管理有一些影响——详见方法 4.15。

### 动态回退选项

你还可以编写你的 API 聚合器，使其能够利用服务注册中心为所需功能查找另一个可用的替代方案。这本质上是一种运行时服务查找，类似于定位机器的运行时域名系统（DNS）。更多细节请参阅[链接待添加]。此外，我们将在[链接待添加]中更详细地介绍这个注册中心概念。

### 将请求排队以便以后重放

如果你无法连接到初始服务或预期的替代服务（见上文），你可以简单地将请求“保存在”队列中，并在当前失败的服务再次可用时稍后重放。这需要设置和管理一个本地队列，以及一些处理该队列内容的代码。通常，在这种情况下，初始响应是 HTTP `202 Accepted`，响应体描述了 API 消费者如何监控延迟请求的处理细节（参见[链接待添加]）。当然，你需要在文档化的接口设计中包含这种可能性，以便 API 消费者为这类响应做好准备。

### 放弃

最后一个选项是直接停止处理请求，告诉调用你 API 的消费者“当前无法处理该请求”，并返回 HTTP 500 错误。这是最安全但功能最少的选项。在这种情况下，如果可能，你应该返回一个响应体，表明 API 消费者可以再次尝试此请求之前的大致等待时间。请记住，*你的* `500 Internal Server Error` 可能会触发 API 消费者

从而启动其自身的缓解代码，而这可能还会影响该 API 消费者应用程序的消费者，依此类推。

> **注意**
>
> 如果所依赖的服务主要用作简单数据源（例如邮政编码、国家、州/地区、产品编号等列表），那么你可以使用其中一些数据缓存方案，参见[链接待补充]。

基本上，你需要这样实现你的 API：假定所依赖的系统其他组成部分会发生故障，并针对每个可能的故障点至少采用上述一种缓解方案。归根结底，你无法*阻止*故障的发生，但或许可以缓解其影响。引用约翰·高尔（John Gall）的话：“任何大型系统大部分时间都将处于故障模式中运行。”

> **断路器模式呢？**
>
> 你可能期望在这个方案中看到其他一些解决方案，比如断路器（Circuit-Breaker）。由于本书主要关注机器与机器之间的交互层面，我略过了许多成熟的以代码为中心的解决方案，如断路器和其他模式。有关微服务编码的指南，请参阅前言中我强烈推荐的一些书籍，其中包括断路器和其他模式。

## 示例

本方案概述了几种可能的解决方案。下面是每种方案的示例。

### 自动重试

你可以让 API 调用代码自动重试请求。需要管理的关键要素包括：

1. 请求失败的类型（例如返回 HTTP 502）
2. 所发起请求的类型（例如 GET、PUT、DELETE）
3. 重试前等待的时间（例如重试前等待 250ms）
4. 放弃前重试的次数（例如重试三次）

例如，内部 API 代码可能如下所示：

```
var reqParams = {} // request params
reqParams.host = "https://api.example.com"
reqParams.url = "/users/qlw2e3";
reqParams.body = "mork-mamund&name=Mike Morkelsen";
reqParams.method = "PUT";
reqParams.waitMS = 300;
reqParams.retryAttempts = 3;
reqParams.successFunction = requestSucceeded;
reqParams.failFunction = requestFailed;

httpLib.request(reqParams);
```

上述重试失败后，你可以尝试本方案中的其他缓解措施（静态回退、动态回退、请求排队、放弃）。

### 静态回退

你可以更新请求功能，使其尝试调用备用主机，方法是在请求集合中包含备用位置，并在请求方法中添加切换主机和进行额外请求尝试的代码：

```
var reqParams = {} // request params
reqParams.host = "https://api.example.com"
reqParams.url = "/users/qlw2e3";
reqParams.body = "mork-mamund&name=Mike Morkelsen";
reqParams.method = "PUT";
reqParams.waitMS = 300;
reqParams.retryAttempts = 3;
reqParams.successFunction = requestSucceeded;
reqParams.failFunction = requestFailed;
reqParams.alternateHost = "https://alternate-api.example.com";

httpLib.request(reqParams);
```

上述操作失败后，你可以尝试此处列出的其他选项。

### 动态回退

好消息是，前面列举的这几个选项（**自动重试**、**静态回退**和**动态回退**）都可以在与 API 客户端不进行协调的情况下实现。它们属于“隐藏”的实现细节。

相反，**请求排队**选项需要与 API 客户端进行大量协调，因为它不仅要求客户端对 `202 Accepted` 响应做好准备，还可能需要将这一排队问题告知其他服务。考虑这样的情况：我们的服务负责处理支付，而某个所依赖的服务不可用。向 API 调用方返回 `202 Accepted` 可能会在其端造成混乱。也许他们已经承诺减少库存并安排所购商品的配送。他们现在需要决定是否应该“回滚”这些其他活动，或者保持这些活动不变且不告知调用 API，又或者为调用 API 提供等待或取消这些活动的选项。

> **警告**
>
> 不要对 POST 或 PATCH 这类非幂等 HTTP 方法尝试重试。这些缓解措施应仅针对 GET、HEAD、PUT 和 DELETE 方法提供支持。在极少数情况下，HTTP POST 操作即使已成功完成，也可能被解释为失败。例如，当目标服务的 `201 Created` 响应始终未能返回给发起调用的 API 消费者时，就会出现这种情况。有关此场景的更多信息，请参见[链接待补充]。对于 PUT 而言，这种“假失败”的后果导致目标服务数据损坏或重复的可能性要小得多。

除了**放弃**选项之外，这里提到的所有缓解措施都涉及到 API 实现方的额外处理。这些附加功能（重试、回退、排队）都需要在本地实现才能发挥作用。

不要犯这样的错误：将这些缓解措施实现为由你的 API 调用的共享外部服务。这也会使它们变成（可能致命的）依赖！如果你的 API 代码无法访问其他底层服务来完成工作，那么它可能也无法访问你的外部缓解服务。

虽然最常见的情况是对任何 HTTP 5xx 级别的响应使用其中一种缓解措施，但你可能需要限制使用这些缓解措施的情形。例如，HTTP `408 Request Timeout` 就是适合这些解决方案的候选场景。

未来可能还会新增 5xx 级别的 HTTP 响应，而且对于任何 *5xx* 响应都启用这些缓解措施可能并非好主意。我推荐作为重试、回退和排队候选的场景包括：HTTP 500、502、503 和 504。对于其他 5xx 值，可能更好的做法是直接返回**放弃**选项。

请记住，这些选项并不需要底层（发生故障的）服务的同意。例如，你的服务接口可以实现重试、回退或排队缓解措施，而无需让未能及时响应的服务参与其中。

在实现重试选项时，你应当查看目标服务（你计划向其发送重试请求的服务）的文档，确保接口代码不会触发拒绝服务响应。务必不要通过过于频繁或过于快速地发送重试请求来滥用底层服务。

> **提示**
>
> 最好保留你的服务所处理的所有请求的运行历史。在服务需要额外工作来存储、转发和处理 API 消费者请求的情况下（如本方案所涵盖的场景），这一点尤为重要。

在实现回退选项时，如果你计划跟踪这些活动（例如为 API 消费者保留过去操作的历史记录），则可能需要提供额外的请求/响应管理。通过设置使用其他服务作为回退，你使处理目标成为一个*变量*——并非所有请求都可能由同一个服务处理。如果这一细节很重要，你将需要安排服务接口来跟踪这些请求，并可能为其提供额外的请求跟踪和管理选项。

## 相关章节

- 第 2 章
- 第 3 章
- 第 4 章
- [链接待补充]

## 4.17 使用语义代理访问非合规服务

本书中各方案的核心思想是：通过使用基于超媒体的服务接口来支持具有弹性的 API 客户端。但有时，我们需要暴露到网络上的服务并不实现超媒体交互（例如 FTP 上传服务）。而且，在某些情况下，我们无法更改这些服务，因为它们由我们自身 IT 生态系统之外的第三方运营。

### 问题

我们如何在 RESTful Web 微服务生态系统中安全地暴露非合规服务？什么时候适合创建本地代理服务来包装其他（可能是远程的）第三方服务？为我们未设计且不维护的服务创建稳定、可靠的代理需要做哪些工作？

### 解决方案

有些时候，我们需要现有服务的功能，但该服务的设计并不打算在 RESTful 生态系统中支持可适应、可演化的接口。在这些情况下，我们需要创建本地的合规服务代理，作为这些非合规服务与我们服务生态系统中其他部分之间的“翻译”设备。

通常，我们需要设计*所需*的服务接口，然后在 API“包装器”中编写我们自己的本地代码，用于将 RESTful


requests into ones that are supported by the non-compliant services. These proxies also need to translate any responses from the non-compliant services into RESTful resource representations.

Sometimes the only challenge for an existing service is that it does not operate using the preferred semantic vocabulary or it doesn’t support the desired media types for message exchange. In these cases, it is not too much work to introduce a kind of “semantic translation” proxy that hides the non-compliant service from API consumers.

> **LEARNING FROM THE WIRELESS APPLICATION PROTOCOL (WAP) GATEWAY**
>
> Created in 1998 by Dutch mobile phone company Telfort BV, the Wireless Application Protocol (WAP)⁸¹ was specifically designed to offer a unique markup environment for small handheld devices not yet capable of supporting HTTP and the HTML stack (markup, CSS, & JavaScript). Most of the implementations turned out to be done through HTTP↔WAP proxies. Companies who were just getting started using HTTP/HTML also invested in a WAP/WML gateway to make sure mobile users had access to their content, too. But, in reality, most users were served a “subset of HTTP and HTML” instead of what looked and felt like a native WAP/WML experience. WAP/WML eventually faded away as mobile devices gained native support for HTTP and HTML.

The proxy solution makes sense when you need to expose functionality that is “trapped” within non-compliant (see [Link to Come]) services and it is not possible (or viable) to modify those services to bring them into line with RESTful Web Microservices principles.

There are three types of service proxies we’ll discuss in this recipe: the Semantic Profile Proxy (SPP), the Enterprise-Level Proxy (ELP), and the Custom One-Off Proxy (COP).

## Enterprise-Level Proxy (ELP)

In some cases, you might be able to create an enterprise-level proxy (ELP) that algorithmically translates between the non-compliant and compliant services. A good example of an ELP is IBM’s CICS External Call Interface (EXCI)⁸². This solution works well when your organization has invested a great deal of effort in developing a domain-specific service platform (e.g.

Each proxy model (ELP, COP, and SPP) requires a slightly different implementation approach.

## Custom One-Off Proxy (COP) Example

An example of a custom one-off proxy (COP) is implementing a RESTful proxy for an FTP file upload service. Below is the underlying service interface we need to “cover” along with a RESTful interface implementation.

```text
// HTTP upload external action
function httpUpload(file) {
  var uploader = new httpService();
  var file = uploader.read();
  return file;
}

// FTP client service
function ftpUpload(file) {
  var client = new ftpService();
  var results = client.put(file);
  return results;
}

// proxy function for file uploads
function proxyUpload(file) {
  var results = null;
  var file = httpUpload(file);
  if(file) {
    results = ftpUpload(file)
  }
  return results;
}
```

And here’s a RESTful interface for uploading files (in HTML)

```http
**** REQUEST ****
GET /upload-file/ HTTP/1.1
Accept: text/html
...

**** RESPONSE ****
HTTP/1.1 200 OK
Content-Type: text/html
....
```

```html
<form method="post" action="https://api.example.org/uploads/"
      enctype="multipart/form-data" >
  <input type="file" name="file" value="daily-batch.txt" />
  <input type="submit" value="Upload" />
</form>
```

```http
**** REQUEST ****
POST /uploads/ HTTP/1.1
Accept: text/html
...

Content-Disposition: form-data; name="file";
...

**** RESPONSE ****
HTTP/1.1 200 OK
Content-Type: text/html
....

<p>File has been uploaded</p>
```

## Enterprise-Level Proxies (ELP) Example

In the case of an enterprise-level proxy (ELP), you can approach this as a translation gateway similar to the one used for Wireless Application Protocol (WAP) example cited earlier in this recipe. Other examples of algorithmic proxies are ones that convert XML-based messages to JSON-based formats, natural language translators (English to Spanish, etc.) and even protocol translators like the FTP⟷HTTP proxy shown above or the WAP⟷HTTP example referred to earlier.

## Semantic Profile Proxy (SPP) Example

Simple SPPs (e.g. format translators) are usually easy to implement as long as the responses from the underlying service are consistent and the level of customization for output formats is not too high. For example, you might have an XSLT implementation that consistently converts XML to Collection+JSON:

```text
results = convert(xmlDocument, xsltCollectionJSON);
```

However, you might need to inject hypermedia controls in the responses, too. This works well if the underlying service provides a status interface

semantics of the information that passes through the gateway. In this way, implementing an ELP is very close to designing, defining, and implementing a new message protocol or an end-to-end programming framework.

In general, protocol translation proxies (FTP⟷HTTP, etc.) are relatively easy to implement because application-level protocols are mature and stable specifications. There are lots of “rabbit holes” when it comes to metadata (HTTP headers, dealing with FTP status codes, etc.) but most of the time you can smooth these over with generic responses.

Conversely, SPPs are often the most challenging proxies to successfully implement. Often the realities of poorly-specified vocabularies, ambiguous meaning of terms, and long-standing tribal knowledge buried in local service implementations all conspire to make building stable SPPs a daunting task. The smaller the vocabulary (e.g. the target domain), the more likely you are to succeed.

In my experience, there are two types of successful service proxies. The first is one that works “just well enough” just long enough until a better, more stable solution comes along (e.g. a new product) to replace it; and the sooner the better. The other successful proxy implementation is one that has been around for years, humming along, and no one has any idea how it works. None of the original designers/programmers are around anymore and only a fool would ever attempt to modify any part of that mysterious (but stable) proxy.

### Related

- Chapter 2
- Chapter 3
- Chapter 4
- Recipe 4.9
- Recipe 4.7
- Recipe 4.4
- [Link to Come]

1. https://www.pearson.com/us/higher-education/program/Bass-Software-Architecture-in-Practice-3rd-Edition/PGM317124.html
2. https://www.oreilly.com/library/view/building-microservices-2nd/9781492034018/
3. https://microservices.io/book
4. https://microservices-book.com/
5. https://www.oreilly.com/library/view/microservices-up-and/9781492075448/
6. https://en.wikipedia.org/wiki/Hippocratic_Oath
7. https://en.wikipedia.org/wiki/Hippocratic_Oath#%22First_do_no_harm%22
8. https://www.allthingsdistributed.com/2016/03/10-lessons-from-10-years-of-aws.html
9. https://www.infoq.com/articles/roy-fielding-on-versioning/
10. https://www.theguardian.com/technology/2019/mar/12/tim-berners-lee-on-30-years-of-the-web-if-we-dream-a-little-we-can-get-the-web-we-want
11. https://learning.oreilly.com/library/view/restful-web-clients/9781491921890/
12. https://www.iana.org/assignments/media-types/media-types.xhtml
13. https://stateless.group/hal_specification.html
14. https://github.com/kevinswiber/siren#siren-a-hypermedia-specification-for-representing-entities
15. http://amundsen.com/media-types/collection/
16. http://amundsen.com/hypermedia/hfactor/
17. https://en.wikipedia.org/wiki/Domain_Name_System
18. https://datatracker.ietf.org/doc/html/rfc8288#section-3
19. https://datatracker.ietf.org/doc/html/rfc8615
20. https://datatracker.ietf.org/doc/html/rfc7595
21. https://microformats.org/wiki/rel-home
22. https://twitter.com/mamund/status/767212233759657984
23. http://amundsen.com/talks/2016-11-apistrat-wadm/2016-11-apistrat-wadm.pdf
24. https://datatracker.ietf.org/doc/html/rfc7231#section-5.3.2
25. http://amundsen.com/hypermedia/hfactor/
26. https://schema.org
27. https://datatracker.ietf.org/doc/html/rfc7231#section-5.3.2
28. https://www.iana.org/assignments/media-types/media-types.xhtml
29. https://datatracker.ietf.org/doc/html/rfc7231#section-7.4.1
30. https://www.iana.org/assignments/http-methods/http-methods.xhtml
31. https://www.w3.org/TR/html401/interact/forms.html#def-enctype
32. https://www.iana.org/assignments/media-types/media-types.xhtml
33. https://datatracker.ietf.org/doc/html/rfc7231#section-5.3.3




- 34. https://www.iana.org/assignments/character-sets/character-sets.xhtml
- 35. https://datatracker.ietf.org/doc/html/rfc7694#section-3
- 36. https://datatracker.ietf.org/doc/html/rfc7694#section-3
- 37. https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-parameters-1
- 38. https://datatracker.ietf.org/doc/html/rfc7231#section-3.1.3.1
- 39. https://datatracker.ietf.org/doc/html/rfc7231#section-5.3.5
- 40. https://www.iana.org/assignments/language-subtag-registry/language-subtag-registry
- 41. https://www.rfc-editor.org/rfc/rfc6906.html
- 42. https://datatracker.ietf.org/doc/html/rfc7233#section-2.3
- 43. https://microformats.org/wiki/rel-meta
- 44. https://rwmbook.github.io/alps-documents/
- 45. https://www.iana.org/assignments/media-types/media-types.xhtml
- 46. https://datatracker.ietf.org/doc/html/rfc7231#section-3.4
- 47. https://datatracker.ietf.org/doc/html/rfc7231#section-3.4
- 48. https://datatracker.ietf.org/doc/html/rfc2295
- 49. https://datatracker.ietf.org/doc/html/rfc7231#section-6.4.1
- 50. https://datatracker.ietf.org/doc/html/rfc7231#section-5.3.1
- 51. https://www.infoq.com/articles/roy-fielding-on-versioning/
- 52. https://learning.oreilly.com/library/view/restful-web-apis/9781449359713/ch09.html
- 53. https://schema.org/familyName
- 54. https://www.w3.org/RDF/
- 55. https://www.w3.org/TR/rdf-syntax-grammar/
- 56. https://www.w3.org/TR/turtle/
- 57. https://www.w3.org/2018/json-ld-reports/json-ld/
- 58. https://datatracker.ietf.org/doc/html/draft-amundsen-richardson-foster-alps-07
- 59. https://en.wikipedia.org/wiki/FOAF_(ontology)
- 60. http://web.archive.org/web/20210216012603/http://xmlns.com/foaf/spec/
- 61. https://issemanic.net/rdf-visualizer
- 62. https://github.com/korim/app-state-diagram#alps-asd
- 63. https://www.markus-lanthaler.com/hydra/
- 64. https://datatracker.ietf.org/doc/html/rfc8631#section-4.2
- 65. http://apisjson.org/format/apisjson_0.16.txt
- 66. http://apisjson.org/format/apisjson_0.16.txt
- 67. http://apisjson.org
- 68. https://datatracker.ietf.org/doc/html/rfc8631
- 69. https://rwmbook.github.io/alps-documents/
- 70. https://github.com/apisio/apis.io
- 71. https://datatracker.ietf.org/doc/draft-nottingham-json-home/
- 72. https://datatracker.ietf.org/doc/html/draft-inadarei-api-health-check-06
- 73. https://rwmbook.github.io/alps-documents/
- 74. https://datatracker.ietf.org/doc/html/rfc7807
- 75. https://www.rfc-editor.org/rfc/rfc4122.html
- 76. https://datatracker.ietf.org/doc/html/rfc7231#section-4.3.3
- 77. https://datatracker.ietf.org/doc/html/rfc7231#section-4.2.2
- 78. https://en.wikipedia.org/wiki/Cross-origin_resource_sharing
- 79. https://en.wikipedia.org/wiki/Systemantics
- 80. https://microservices.io/patterns/reliability/circuit-breaker.html
- 81. https://en.wikipedia.org/wiki/Wireless_Application_Protocol
- 82. https://www.microfocus.com/documentation/enterprise-developer/ed30/ESdotNET/GUID-CE7E0E23-DAEF-4238-AFC0-83C892B2DAB.html
