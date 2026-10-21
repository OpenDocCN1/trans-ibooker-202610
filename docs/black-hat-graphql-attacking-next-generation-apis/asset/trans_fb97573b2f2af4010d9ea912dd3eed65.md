

as over-fetching (返回给客户端未使用的数据) 或 under-fetching (返回数据太少，迫使客户端进行第二次请求)。你将在下一节了解这些差异以及它们对性能的重要性。

GraphQL 具有额外的有用功能，例如 schema stitching 和 schema federation。Schema stitching 是一种从多个底层 GraphQL 服务创建单个 GraphQL schema 的方法，使 GraphQL 能够用作统一网关。基本上，它将多个 schema 打包（缝合）成一个大的 schema，为客户端创建一个单一的集成点。因为多个微服务可以定义自己的 GraphQL schema 并拥有自己的 GraphQL 端点，所以允许单个 GraphQL API 网关将许多 schema 整合为一个，可以使客户端更容易与应用程序集成。

Schema federation 与 schema stitching 类似，只是它不需要你手动缝合 schema。相反，schema federation 让你可以告诉 GraphQL API 网关在哪里查找额外的 schema。网关然后自动进行缝合。Federation 是将多个 API 整合到单个网关的维护成本更低的方法。

复杂的 API 应用程序，例如需要 schema federation 或 schema stitching 的应用程序，可能会引入安全漏洞，可能允许黑客访问他们本不应该访问的数据。一般来说，应用程序越复杂，其内部复杂性导致漏洞的可能性就越高。

## GraphQL APIs vs. REST APIs

在前面的章节中，我们讨论了 GraphQL 试图解决的传统 API 的挑战。例如，REST API 通常提供客户端不需要的更多数据（over-fetching）或数据太少（under-fetching），迫使客户端进行额外的 API 请求。在本节中，我们将通过一个示例来演示由 REST API 前置的应用程序与使用 GraphQL 的应用程序之间的比较。

考虑表 1-1，一个包含应用程序用户基础信息的数据库表。简单的 Web 应用程序可能会将此信息作为管理员面板的一部分显示，让系统管理员列出所有可用账户及其状态。我们将称之为用户管理页面。

表 1-1：用户数据库表

| User ID | Username | Email | First name | Last name | State |
|---|---|---|---|---|---|
| 1 | dsmith | david@example.com | David | Smith | Disabled |
| 2 | clarry | chris@example.com | Chris | Larry | Enabled |

在下面的章节中，我们将描述客户端如果应用程序使用 REST API 需要进行哪些 API 请求来检索用户数据，以及它在应用程序使用 GraphQL 时如何做到这一点。

## The REST Example

在应用程序使用 REST API 的情况下，我们定义特定的端点或路由，客户端可以在其中使用特定的 HTTP 方法（例如 GET 或 POST）执行读取或写入数据等操作。表 1-2 为两个目的定义了两个 REST API 端点：一个用于获取用户列表，另一个用于获取用户登录历史信息。

表 1-2：REST API 定义

| HTTP method | API endpoint | Endpoint description |
|---|---|---|
| GET | /rest/v1/users | Returns a list of all available users and their information |
| GET | /rest/v1/history/<user_id> | Returns a list of the login timestamps for a given user |

当系统管理员想要查看用户管理页面时，他们的 Web 客户端，例如 Web 浏览器，将需要通过 Web 应用程序的 API 获取有关所有可用用户的信息。要使用表 1-2 中的 API 端点检索此数据，Web 浏览器需要向 /rest/v1/users 发送 GET 请求。清单 1-5 显示了此请求及其响应。

```
# curl http://lab.blackhatgraphql.com/rest/v1/users
[
  {
    "email": "david@example.com",
    "first_name": "David",
    "id": 1,
    "last_name": "Smith",
    "state": "disabled",
    "username": "dsmith"
  },
  {
    "email": "chris@example.com",
    "first_name": "Chris",
    "id": 2,
    "last_name": "Larry",
    "state": "enabled",
    "username": "clarry"
  }
]
```

清单 1-5：GET 请求到 /rest/v1/users 列出所有系统用户

正如你所见，此请求以 JavaScript Object Notation (JSON) 格式返回所有用户的列表，以及他们的电子邮件、姓名、ID 和账户状态。

但如果系统管理员只想检索用户某些信息，例如他们的电子邮件地址，而不返回任何其他信息呢？使用表 1-2 中的 API 定义，这是不可能的。相反，清单 1-5 中的响应需要完整处理，并且需要从响应中提取 email 字段。这是 REST API 中 over-fetching 问题的一个例子：客户端接收到比所需更多的数据，然后必须对其进行过滤。

现在，假设你是系统管理员，并被要求识别网络上的任何入侵尝试。你计划编写一个脚本，每晚运行并检查可疑行为。例如，它应该标记在正常工作时间，即上午 9 点至下午 5 点之后登录的用户。为了实现这一目标，脚本需要使用 GET 方法向 /rest/v1/history/<user_id> 端点发出 API 请求。然而，如果你仔细查看端点结构，你会注意到它要求客户端提供特定的用户 ID。脚本将如何知道应用程序用户的 ID？简短的回答：它不会，除非它首先获取所有可用的用户 ID。

实际上，这意味着为了脚本成功运行、读取用户的最后登录时间戳并识别可能的入侵，它首先需要使用 API 端点 /rest/v1/users 列出系统上的所有用户账户。这应该返回每个用户的用户名、电子邮件、名字、姓氏、状态和用户 ID。

接下来，它需要向 /rest/v1/history/1 发出第二个 API 请求，其中 1 是从第一次请求获得的用户 ID，如清单 1-6 所示。

```
# curl http://lab.blackhatgraphql.com/rest/v1/history/1

--snip--
["02:03:37", "03:05:55"]
--snip--
```

清单 1-6：来自 /rest/v1/history/1 的响应

要获取所有历史用户登录的完整列表，客户端需要发出额外的请求，直到获取所有用户 ID。如果我们有 1,000 个用户，这将需要 1,000 次请求。听起来像是一个低效的过程，不是吗？这是 REST API 倾向于出现的 under-fetching 问题的一个例子。RESTful API 可以设计为返回特定信息，但允许在各种 REST 端点之间进行此类查询灵活性所需的复杂性将使其难以长期维护。

虽然一开始乍一看，为单个用户检索登录信息需要两次请求可能不算大事，但想象一下应用程序同时为数百万客户端提供服务。在这种规模下，每一次请求都很重要；任何额外的跨网络调用都会导致服务器延迟增加并影响客户端体验。这将降低应用程序的整体速度和效率。

如果你想亲眼看看这些请求，可以通过将 Web 浏览器指向位于 http://lab.blackhatgraphql.com/start 的实时实验室来尝试此示例的 API。在那里，点击两个链接以在 Web 浏览器内导航到 REST，如图 1-5 所示。

图 1-5：一个实时的 REST API 示例

我们已经演示了 REST API 的 under-fetching 和 over-fetching 问题。GraphQL 将如何解决这些问题？让我们在 GraphQL 世界中探索完全相同的场景。

## The GraphQL Example

想象一下，我们的用户管理 Web 应用程序已经放弃了其 REST API，转而使用 GraphQL，并且我们已经建立了一个定义 users 和 history 节点之间数据图边的 schema。现在，当系统管理员查看用户管理页面时，他们的 Web 浏览器将使用应用程序的 GraphQL API 端点返回所需的所有数据。

浏览器可能会使用清单 1-7 中的查询来检索诸如用户 ID、电子邮件、名字、姓氏、历史信息如最后登录时间戳以及账户状态等信息：

```
query {
  users {
    id
    email
    first_name
    last_name
    state
    history {
      last_login_timestamp
    }
  }
}
```

清单 1-7：GraphQL 查询以获取有关用户的信息

