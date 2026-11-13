# 编写用户服务

好的——让我们开始为我们的商店应用程序构建第一个服务！此时一切应该已准备就绪，您可以开始编写服务。本章所学将使您能够在更大的背景下构建微服务。您将逐步了解微服务代码的样子以及如何与 Vapor 交互。

到目前为止，我们一直在为这一刻做准备。您已经学习了构建微服务的理论，并准备好了开始所需的一切。完成本章后，您将能够运行您自己的、功能齐全的用户管理服务！这是我们示例项目中的第一个微服务，让您很好地了解如何编写自己的用户服务。用户服务在应用程序和网站中非常常见，希望本章的内容能帮助您应对未来遇到的此类服务。

在本章中，我们将涵盖以下主题：

*   设置并迈出第一步
*   探索路由
*   探索模型
*   登录和注册
*   管理用户
*   理解地址管理
*   启动服务

让我们开始吧！

## 技术要求

本章需要以下软件组件已启动并运行：

*   Vapor Toolbox
*   Swift 5
*   Xcode 11+
*   macOS 或 Linux

我们将假设您使用的是 macOS，但所有这些在 Linux 下也应该以相同方式工作。

您可以在以下 GitHub 仓库中找到本章的所有代码：[`github.com/PacktPublishing/Hands-On-Swift-5-Microservices-Development/tree/master/Chapter 7/UserService`](https://github.com/PacktPublishing/Hands-On-Swift-5-Microservices-Development/tree/master/Chapter%207/UserService)

## 设置并迈出第一步

在本节中，我们将设置我们的环境。我们在代码和软件方面所需的一切将在这里准备就绪。

要开始编写此服务，您需要准备好以下要素：

*   此服务的一个空文件夹
*   一个可用的新 Git 仓库
*   我们在前几章中使用的微服务模板

如果您尚未完成之前的步骤以准备好这三个要素，请返回先完成它们。微服务可能很复杂，如果组织不好，最终会变得一团糟。

本节将涵盖以下内容：

*   设置模板
*   安装 SendGrid
*   设置 **JSON Web Tokens (JWT)** 和工具函数
*   设置数据库

### 设置模板

让我们首先设置上一章创建的模板。我们不需要重写之前创建的内容，只需将其作为该服务的基础。我们现在将获取创建的模板并将其复制到当前项目文件夹中。然后，我们将对其进行修改以满足我们的需求，即安装 SendGrid。请执行以下步骤：

1.  进入用户管理器的文件夹，如下所示：

```
cd ~/Shop\ Backend/UserService
```

2.  现在，将模板从原始文件夹复制到我们的新文件夹中，如下所示：

```
cp -r ../template/. . && rm -rf .build
```

3.  现在，使用以下命令打开 `Package.swift` 文件：

```
open Package.swift
```



4. 将 `Package.swift` 文件中的 `name` 属性更改为 `UserService`。

现在我们已经设置好模板，可以通过添加一些包和依赖项来对其进行自定义。

# 安装 SendGrid

用户在注册时验证邮箱是常见做法。我们希望我们的服务包含此功能。为此配置我们自己的 **简单邮件传输协议** (**SMTP**) 邮件服务器是一项复杂的任务，且要求我们遵守许多发送邮件的通用规则。使用已提供此功能的服务会更容易，这样我们就不必担心这些问题。此类服务包括 **Amazon Web Services** (**AWS**) 的 **简单电子邮件服务** (**SES**)、Gmail、Yahoo 和 SendGrid。由于我们想要自定义发件人，我们将使用 SendGrid，它为小型项目提供免费套餐。还有一个不错的 Vapor 包可供我们使用：[`github.com/vapor-community/sendgrid-provider`](https://github.com/vapor-community/sendgrid-provider)。

要设置 SendGrid，请执行以下操作：

1. 访问 [`sendgrid.com/`](https://sendgrid.com/) 并创建一个账户。他们的免费套餐对我们来说足够了。

2. 通过在 `Package.swift` 文件中添加以下行来安装 Vapor 包：

```
.package(url: "https://github.com/skelpo/sendgrid-provider.git", from: "4.0.0")
```

3. 将 `SendGrid` 添加到您的 `.target` 函数中。

4. 在 `Configuration.swift` 文件的 `configure` 函数中添加以下行：

```
guard let sendgridApiKey = Environment.get("SENDGRID_API_KEY")
  else {
        fatalError("No value was found at the given public key
         environment 'SENDGRID_API_KEY'")
    }
let sendgridClient = SendGridClient(client: app.client, apiKey: sendgridApiKey)
```

5. 将 `try routes(app)` 行更改为以下内容：

```
try routes(app, sendgridClient)
```

6. 打开 `Routes.swift` 并更改文件行，如下所示：

```
import Vapor
import SendGrid

func routes(_ app: Application, _ sendgridClient: SendGridClient) throws {
```

安装完 SendGrid 后，我们接下来需要设置 JWT 和工具函数。

# 设置 JWT 和工具函数

我们的模板已经包含正确设置 JWT 所需的一切。对于我们的后端，我们现在需要创建用于签名和验证的实际密钥。为此，请遵循以下步骤：

1. 访问 [`mkjwk.org/`](https://mkjwk.org/) 并为我们的后端创建一个新的密钥对。选择 2048 作为密钥大小，Signing 作为密钥用途，RS256 作为算法，backend 作为密钥 ID。

2. 点击 Generate。

3. 结果应如下所示：

![](img/90d2d79d-bbdd-45a4-bc28-f64937c86875.png)

4. 将 Keypair set 下的内容存储在 `Shop Backend` 文件夹中的一个文件内，并将其命名为 `keypair.jwks`。切勿发布此文件。

生成密钥的方式可以不同。这只是一个简单的入门方法。一旦进入生产环境，您应该使用命令行工具重新生成密钥，正如 [mkjwk.org](http://mkjwk.org) 上提到的那样。

5. 在 `Shop Backend/UserService` 文件夹中创建一个 `.env` 文件并在其中写入以下内容（您也可以将 `.env` 文件添加到 `.gitignore` 文件中）：

```
JWKS=<COPY_THE_KEYPAIR_AS_ONE_LINE_HERE>
```

这就是我们要做的一切。`SimpleJWTMiddleware` 已经为我们处理了其余部分。

让我们看看实际的中间件。从 [`github.com/proggeramlug/SimpleJWTMiddleware/blob/master/Sources/SimpleJWTMiddleware/SimpleJWTMiddleware.swift`](https://github.com/proggeramlug/SimpleJWTMiddleware/blob/master/Sources/SimpleJWTMiddleware/SimpleJWTMiddleware.swift) 打开 `SimpleJWTMiddleware.swift` 文件，您应该会看到以下内容：

```
import Vapor
import JWT

public final class SimpleJWTMiddleware: Middleware {
    public init() { }

    public func respond(to request: Request, chainingTo next: Responder) ->
     EventLoopFuture<Response> {
        
        guard let token = request.headers.bearerAuthorization?.token.utf8
         else {
            return request.eventLoop.makeFailedFuture(Abort(.unauthorized,
             reason: "Missing authorization bearer header"))
        }

        do {
            request.payload = try request.jwt.verify(Array(token),
             as: Payload.self)
        } catch let JWTError.claimVerificationFailure(name: name,
           reason: reason) {
            request.logger.error("JWT Verification Failure: \(name),
             \(reason)")
            return request.eventLoop.makeFailedFuture(
             JWTError.claimVerificationFailure(
             name: name, reason: reason))
        } catch let error {
            return request.eventLoop.makeFailedFuture(error)
        }

        return next.respond(to: request)
    }

}
extension AnyHashable {
    static let payload: String = "jwt_payload"
}

extension Request {
    public var loggedIn: Bool {
        if (self.userInfo[.payload] as? Payload) != nil {
            return true
        }
        return false
    }
    public var payload: Payload {
        get { self.userInfo[.payload] as! Payload }
        set { self.userInfo[.payload] = newValue }
    }
}
```

让我们将这段代码分解成更小的部分。

`SimpleJWTMiddleware` 类是实际的中间件。我们有一个在请求传递给控制器之前进行响应的函数，如下所示：

```
public func respond(to request: Request, chainingTo next: Responder) -> EventLoopFuture<Response> 
```

在该函数中，我们首先检查是否有一个 JWT 有效的 `Authorization` 令牌，如下所示：

```
guard let token = request.headers.bearerAuthorization?.token.utf8 else {
            return request.eventLoop.makeFailedFuture(Abort(.unauthorized,
             reason: "Missing authorization bearer header"))
        }
```

如果我们没有有效的 JWT 头，我们将返回一个错误，且调用永远不会到达控制器。之后，我们使用 Vapor 提供的 JWTKit 函数检查给定的令牌是否有效，如下所示：

```
request.payload = try request.jwt.verify(Array(token), as: Payload.self)
```

Payload 定义如下：

```
import Foundation
import Vapor
import JWT

public struct Payload: JWTPayload {
    public let firstname: String?
    public let lastname: String?
    public let email: String
    public let id: Int
    public let status: Int = 0
    public let exp: String
    public let iat: String
    
    init(id: Int, email: String) {
        self.id = id
        self.email = email
        self.role = role
        self.firstname = nil
        self.lastname = nil
        self.exp = String(Date().addingTimeInterval(
         60*60*24).timeIntervalSince1970)
        self.iat = String(Date().timeIntervalSince1970)
    }
    
   public func verify(using signer: JWTSigner) throws {
        let expiration = Date(timeIntervalSince1970: Double(self.exp)!)
        try ExpirationClaim(value: expiration).verifyNotExpired()
    }
}
```

由于此模型在我们所有的服务中都是相同的，我们将其保留在包中原样。

在我们的 `Configure.swift` 文件中，您可以看到我们正在全局配置 JWT，如下所示：

```
try app.jwt.signers.use(jwksJSON: jwksString)
```

**JSON Web Key Set** (**JWKS**) 文件是定义 JWT 密钥和凭据设置的一种非常强大的方式。因此，在这里，我们可以简单地将其作为字符串传递，JWTKit 将为我们处理其余部分。

就是这些！目前您已完成应用的配置。让我们继续设置数据库。

# 设置数据库

要设置数据库，我们只需执行以下操作：

1. 将以下行添加到 `.env` 文件中：

```
MYSQL_CRED='mysql://service_users:password@localhost:3306/
service_users?ssl=false'
```

请确保您输入了正确的密码。



在 Xcode 中使用 `.env` 文件，你需要调整 schema 以在项目中运行：

![](img/47987567-6d42-455a-82ee-aa1c3b94a696.png)

现在，当你在 Xcode 中运行项目时，它将自动读取 `.env` 文件。

你已成功为服务设置数据库。现在，让我们着手处理路由。

# 探索路由

我们的服务需要通过 URL 访问。Vapor 的解决方式是使用路由器。路由器将传入的请求 URL 与我们告诉它要做的事情进行比较，并执行所需的函数。对于作为 **Representational State Transfer** (**REST**) API 的每个服务，我们都需要这样做。

让我们首先定义所需的所有路由。该服务将服务于三个主要目的：

*   用户账户管理
*   登录/验证
*   用户账户的地址管理

下表列出了我们所需的所有路由。`\*` 代表任意字符，以提供运行同一 API 多个版本的能力。`:variablename` 代表将为数字 ID 的占位符。所有输入和输出均为 JSON 格式：

| **Path** | **Method** | **Parameters** | **Output/Description** |
| --- | --- | --- | --- |
| `/\*/users/register` | `POST` | Email<br>Password<br>First Name (optional)<br>Last Name (optional) | 错误描述或带用户配置文件的成功消息 |
| `/\*/users/login` | `POST` | Email<br>Password | 错误描述或访问令牌和刷新令牌 |
| `/\*/users/accessToken` | `POST` | Refresh Token | 错误描述或访问令牌 |
| `/\*/users` (Auth Required) | `GET` | (none) | 当前登录用户 |
| `/\*/users` (Auth Required) | `PATCH` | First Name (optional)<br>Last Name (optional) | 更新后的用户 |
| `/\*/users` (Auth Required) | `DELETE` | (none) | 如果正确删除则无内容 |
| `/\*/users/addresses` (Auth Required) | `GET` | (none) | 所有已保存地址的列表 |
| `/\*/users/addresses` (Auth Required) | `POST` | Street<br>City<br>ZIP | 向用户添加新地址 |
| `/\*/users/addresses/:id` (Auth Required) | `PATCH` | Street<br>City<br>ZIP | 更新现有地址并返回用户配置文件 |
| `/\*/users/addresses/:id` (Auth Required) | `DELETE` | (none) | 删除地址并返回 HTTP 状态 `200` |
| `/\*/users/health` | `GET` | (none) | 只要服务正在运行，返回 HTTP 状态 `200` |

正如你所见，我们要求用户对某些路由进行授权。此外，我们可以看到我们想要创建三个控制器，对应前面的三个逻辑功能。

让我们看看流程，如下所示：

1.  在 `Controllers` 中创建名为 `AuthController.swift` 的文件。
2.  在 `Controllers` 中创建名为 `UsersController.swift` 的文件。
3.  在 `Controllers` 中创建名为 `AddressesController.swift` 的文件。
4.  打开 `Routes.swift` 文件并用以下内容替换：

```swift
import Fluent
import Vapor
import SimpleJWTMiddleware
import SendGrid

func routes(_ app: Application, _ sendgridClient: SendGridClient) throws {
    let root = app.grouped(.anything, "users")
    let auth = root.grouped(SimpleJWTMiddleware())

    root.get("health") { request in
        return "All good!"
    }
    
    //try root.register(collection: AddressController())
    //try root.register(collection: AuthController(sendgridClient))
    //try auth.register(collection: UserController())
}
```

你可以看到，我们正在注册遵循 `/\*/users/` 模式的路由。由于这种初始模式，负载均衡器能够正确地将诸如 `/v1/users/addresses` 之类的请求引导至我们的服务。

这三个控制器被注释掉了，因为我们尚未创建它们。随着我们创建它们，请取消注释。

此外，我们只注册了一条直接路由（`health`），因为它是我们唯一不来自控制器的路由。由于该路由的简单性，我们只将其作为回调函数保留。否则，我们使用 `Application` 的功能来注册 `RouteCollection`，这允许我们的控制器自行注册所需的路由。

请注意，我们在此处传递了 `SendGridClient` 引用。只注册一次并简单地将引用传递给控制器的性能更高。

每当调用 `/\*/users/health` 时，我们都会返回 `"All good!"` 消息以及 HTTP 状态 `200`。这是必要的，以便负载均衡器知道该服务仍处于活动和运行状态。如果服务崩溃，负载均衡器将知道不再将任何流量引导至该服务。

我们现在已配置路由并创建了控制器文件。让我们继续，创建存储用户及其地址所需的模型。

# 探索模型

正如我们在 Chapter 2 *Understanding Server-Side Swift* 中讨论的，我们处理两种类型的模型：数据库模型和 **input/output** (**I/O**) 模型。我们现在将创建这两种模型，并学习如何将它们连接到 Fluent。本节将涵盖：

*   数据库模型
*   将模型连接到 Fluent
*   I/O 模型

让我们从数据库模型开始。

# 数据库模型

让我们设计并创建数据库模型。我们需要它们让 Vapor 连接到数据库，然后返回这些模型中的信息。

由于我们处理用户和地址，我们需要以下两个数据库模型，将在接下来的两节中介绍：

*   User
*   Address

让我们从用户开始！

# User

我们将创建一个用户，请遵循以下步骤：

1.  在 `Sources/App/Models/Database/` 中创建 `User.swift` 文件并输入以下内容：

```swift
import Vapor
import Fluent

final class User {
    
    init() {
    }
    
    @ID(key: "id")
    var id: Int?
    
    @Field(key: "firstname")
    var firstname: String?
    
    @Field(key: "lastname")
    var lastname: String?
    
    @Field(key: "email")
    var email: String
    
    @Field(key: "password")
    var password: String
    
}
```

数据库模型必须始终是 `final`。我们现在编写的类——到目前为止——只是一个包含几个属性的常规 Swift 类。目前，我们输入了以下内容：

*   `id`：唯一标识用户的数字。
*   `firstname`：用户的名字。
*   `lastname`：用户的姓氏或家族名。
*   `email`：用户的电子邮件地址。
*   `password`：用户的密码，尽管我们只会在此处保存哈希码。

构造函数仅接受电子邮件并分配空密码。虽然构造函数用于创建新实例，但对于 Fluent，我们将做的不仅仅是调用构造函数。因此，我们在此保持简单精简。

2.  泛型类不能是 Fluent 模型，所以请继续将第一行更改为以下内容：

```swift
final class User: Model
```

这就是使该类成为有效 Fluent 模型所需的一切。但我们还没完全准备好，因为我们想将此类用于比简单数据存储更多的功能。

3.  向类添加以下属性：

```swift
static let schema = "users"

@Timestamp(key: "createdAt", on: .create)
var createdAt: Date?
    
@Timestamp(key: "updatedAt", on: .update)
var updatedAt: Date?
    
@Timestamp(key: "deletedAt", on: .delete)
var deletedAt: Date?
```

第一个属性 `schema` 定义数据库表的名称。Fluent 需要此变量以正确命名数据库表。

其他三个属性存储关于模型何时创建、修改和删除的信息。我们使用属性包装器来定义键和设置时间戳的事件。我们将在迁移中拾取这些键。

4.  要初始化用户并正确设置密码，请添加以下构造函数：



```
init(_ email: String, _ firstName: String? = nil, _ lastName: String? = nil, _ password: String) throws {
    self.email = email
    
    self.firstname = firstName
    self.lastname = lastName
    self.password = try BCryptDigest().hash(password)
}
```

我们现在设置名、姓以及密码。

此处密码使用了简单的哈希函数。对于生产环境设置，你应该使用更高级的密码哈希存储方法。

最后，我们希望能将用户信息存储在 JWT 的载荷中。

这就完成了！我们的 `User` 模型已完成！让我们继续 `Address` 模型。

现在，让我们在下一节中看看如何添加地址。

# Address

请按照以下步骤添加地址：

1. 在 `Sources/App/Models/Database/` 中创建一个 `Address.swift` 文件并输入以下内容：

```
import Vapor
import Fluent

final class Address: Model {
    
    static let schema = "addresses"
    
    @ID(key: "id")
    var id: Int?
    
    @Field(key: "street")
    var street: String
    
    @Field(key: "city")
    var city: String
    
    @Field(key: "zip")
    var zip: String
    
    @Timestamp(key: "createdAt", on: .create)
    var createdAt: Date?
    @Timestamp(key: "updatedAt", on: .update)
    var updatedAt: Date?
    @Timestamp(key: "deletedAt", on: .delete)
    var deletedAt: Date?
    
    init() {
    }
}
```

你可以看到，我们也在类中存储了基本属性（`street`、`city`、`zip`）。就像 `User` 一样，我们给 `Address` 提供了正确的依赖项，并通过 `schema` 自定义了表名。

为保持简单，我们假设地址属性只需要街道、邮政编码和城市。在实际应用中，你肯定想使用更多属性（如国家等）来指定地址。查看这篇文章了解更多：[`www.mjt.me.uk/posts/falsehoods-programmers-believe-about-addresses/`](https://www.mjt.me.uk/posts/falsehoods-programmers-believe-about-addresses/)。

我们确实希望地址与 `User` 关联，因此我们将创建一个保存用户 ID 的 `userId` 属性。

2. 在 `zip` 属性后添加以下行：

```
@Field(key: "userId")
var userId: Int
```

3. 调整构造器如下：

```
init(street: String, city: String, zip: String, userId: Int) {
    self.street = street
    self.city = city
    self.zip = zip
    self.userId = userId
}
```

由于 `userId` 非可选，创建新实例时必须提供它。这是设计使然，以确保不存在无用户关联的地址。

此函数现在获取 `userId` 字段并将其连接到 `User.id` 属性。这样，两个模型在逻辑上和数据库中都建立了连接。

# 将模型连接到 Fluent

我们现在已创建所需的所有模型。你还记得吗？正如在第 3 章 *Getting Started with the Vapor Framework* 中所见，Vapor 使用 Fluent 作为其数据库系统？现在让我们将模型与 Fluent 连接起来。

某些框架会自动检测模型。由于 Vapor 和 Swift 旨在实现最佳性能，所有模型都必须在 `Configuration.swift` 文件中声明。

1. 返回 `Configure.swift` 文件并输入以下代码行以向 Fluent 注册我们的模型：

```
app.migrations.add(CreateUser())
app.migrations.add(CreateAddress())
```

2. 现在，在 `Sources/App/Migrations/` 中创建一个新的 `CreateUser.swift` 文件并按如下方式填充：

```
import Fluent

struct CreateUser: Migration {

    func prepare(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("users")
            .field("id", .int, .identifier(auto: true))
            .field("firstname", .string)
            .field("lastname", .string)
            .field("email", .string, .required)
            .field("password", .string, .required)
            .field("createdAt", .datetime)
            .field("updatedAt", .datetime)
            .field("deletedAt", .datetime)
            .create()
    }

    func revert(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("users").delete()
    }
}
```

3. 接下来，我们需要对地址做同样的操作。在同一文件夹中创建一个 `CreateAddress.swift` 文件并按如下方式填充：

```
import Fluent

struct CreateAddress: Migration {
    func prepare(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("addresses")
            .field("id", .int, .identifier(auto: true))
            .field("userId", .int, .references("users", "id"))
            .field("street", .string, .required)
            .field("zip", .string, .required)
            .field("city", .string, .required)
            .field("createdAt", .datetime)
            .field("updatedAt", .datetime)
            .field("deletedAt", .datetime)
            .create()
    }

    func revert(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("addresses").delete()
    }
}
```

从这两个例子中可以非常清楚地看到如何通过 Fluent 创建数据库表。遵循 `Migration` 协议的类将有两个函数：`prepare(on database: Database) -> EventLoopFuture<Void>` 和 `revert(on database: Database) -> EventLoopFuture<Void>`。`prepare` 用于创建数据库表，`revert` 用于删除它。对于这两个模型，我们通过定义字段来创建数据库表。注意，我们使用了前面模型中属性包装器定义的键。

4. 现在，运行以下代码：

```
$ swift build && vapor run migrate
```

我们构建应用，数据库表现在将被创建。

创建完数据库模型后，我们接下来需要关注 I/O 模型。

# I/O 模型

I/O 模型是我们用于接收输入或向响应者返回输出的模型。它们是数据库模型与用户请求之间的桥梁。正如我们在第 6 章 *Application Structure and Database Design* 中讨论的，它们与数据库模型紧密相关，但应该是独立的类或结构体。

我们需要以下组件的模型：

*   访问令牌
*   刷新令牌
*   响应模型
*   输入模型

## 访问令牌

访问令牌也称为载荷。访问令牌是我们想要连同头部一起传递的已签名载荷。在我们的例子中，我们想要保存 ID、名、姓和电子邮件。

`SimpleJWTMiddleware` 包已经为我们提供了 `Payload` 类；我们无需自行定义。不过，让我们来看一下它，如下所示：

1. 从 [`github.com/proggeramlug/SimpleJWTMiddleware/blob/master/Sources/SimpleJWTMiddleware/Payload.swift`](https://github.com/proggeramlug/SimpleJWTMiddleware/blob/master/Sources/SimpleJWTMiddleware/Payload.swift) 打开 `Payload.swift` 文件并查看以下内容：

```
import Foundation
import Vapor
import JWT

public struct Payload: JWTPayload {
    public let firstname: String?
    public let lastname: String?
    public let email: String
    public let id: Int
    public let status: Int = 0
    public let exp: String
    public let iat: String
    
    init(id: Int, email: String) {
        self.id = id
        self.email = email
        self.firstname = nil
        self.lastname = nil
        self.exp = String(Date().addingTimeInterval(
         60*60*24).timeIntervalSince1970)
        self.iat = String(Date().timeIntervalSince1970)
    }
    
   public func verify(using signer: JWTSigner) throws {
        let expiration = Date(timeIntervalSince1970: Double(self.exp)!)
        try ExpirationClaim(value: expiration).verifyNotExpired()
    }
}
```



# 访问令牌

请注意，`firstname` 和 `lastname` 是可选的。它们并不总是存在于 `User` 模型中，这里也不必强制包含它们。

为了存储验证 `JWT` 所需的信息，我们还需要添加令牌的过期时间以及创建时间。

### 请注意这三个变量

```swift
let status: Int = 0
let exp: TimeInterval
let iat: TimeInterval
```

这三个属性包含验证 `JWT` 所需的信息。

### 为了验证，payload 具有以下函数

```swift
func verify(using signer: JWTSigner) throws {
    let expiration = Date(timeIntervalSince1970: self.exp)
    try ExpirationClaim(value: expiration).verifyNotExpired()
}
```

在这种情况下验证非常简单：如果过期日期（`exp`）不在过去，则令牌有效。你也可以添加进一步的验证声明，并根据其他标准检查令牌。

为了签发令牌，我们需要一个构造器，它使用来自 `User` 类的正确信息并设置正确的内部值。

### 查看类的以下构造器

```swift
init(id: Int, email: String) {
    self.id = id
    self.email = email
    self.firstname = nil
    self.lastname = nil
    self.exp = String(Date().addingTimeInterval(60*60*24).timeIntervalSince1970)
    self.iat = String(Date().timeIntervalSince1970)
}
```

我们使用当前时间（`Date()`）并由此构建 `exp` 和 `iat` 变量。其余的值只是从用户那里复制过来的。请注意，我们省略了用户配置文件中的信息（最重要的是 `password`）。从技术上讲，你可以将整个 `User` 模型用作 payload，但由于敏感信息（如 `password`）的存在，你绝不应该这样做。

`iat` 变量在我们的服务中完全未被使用。不过，客户端可能需要它来验证令牌何时即将过期。

好的。你现在已经看到了访问令牌所需的一切。现在让我们看看刷新令牌。

# 刷新令牌

刷新令牌与访问令牌非常相似。我们不需要任何特定于用户的信息，除了 `ID`。原因是刷新令牌仅用于获取新的访问令牌。刷新令牌永远不会发送给除用户服务之外的任何服务。刷新令牌的过期时间比访问令牌长，因此默认过期时间会更长。

在 `Sources/App/Models` 中创建一个 `RefreshToken.swift` 文件并插入以下内容：

```swift
import Vapor
import JWT

struct RefreshToken: JWTPayload {
    let id: Int
    let iat: TimeInterval
    let exp: TimeInterval
    
    init(user: User, expiration: TimeInterval = 24 * 60 * 60 * 30) {
        let now = Date().timeIntervalSince1970
        
        self.id = user.id ?? 0
        self.iat = now
        self.exp = now + expiration
    }
    
    func verify(using signer: JWTSigner) throws {
        let expiration = Date(timeIntervalSince1970: self.exp)
        try ExpirationClaim(value: expiration).verifyNotExpired()
    }
}
```

此代码包含用于签发 `RefreshToken` 的 `struct`。`RefreshTokens` 允许用户在无需重新提交密码或电子邮件的情况下刷新其 `AccessToken`。

请注意，`RefreshToken` struct 符合 `JWTPayload`——就像之前展示的 payload 一样。

`verify` 函数与访问令牌的相同，`iat` 和 `exp` 的功能也保持不变。

# 响应模型

正如前面章节所讨论的，你几乎总是会有 I/O 模型。提醒一下，这些模型代表数据库模型的数据，但仅包含特定用例所需的数据。

由于这些模型本身较小，我们可以将它们全部放在同一个文件中。如果你想创建单独的文件，也可以。所有 I/O 模型都符合 `Content` 协议，以便它们可以作为 JSON 返回。为了将它们标记为响应模型，我们将它们命名为 `NameOfClassResponse`。

请执行以下步骤：

1. 在 `Sources/App/Models/Responses/` 中创建一个 `UserResponse.swift` 文件并插入第一个模型，如下所示：

```swift
import Vapor

struct UserResponse: Content {
    let id: Int?
    let firstname, lastname: String?
    let email: String
    let addresses: [AddressResponse]?
}
```

`UserResponse` 模型用于每当我们想要返回一个 `User` 时。因此，它包含我们希望在返回用户时看到的信息。由于此服务仅返回已通过身份验证的用户，我们可以包含电子邮件地址等信息。如果我们想返回公共用户列表，我们将创建另一个仅包含姓氏首字母或类似信息的模型。

请注意，我们在这里引用的是 `AddressResponse` 而不是 `Address`。原因是我们想使用 `AddressResponse` 而不是 `Address`，以正确地仅使用响应模型。

`Content` struct 或 class 的所有属性也必须符合 `Content` 协议。

返回用户时，我们可能希望包含一个状态，指示请求成功。

2. 继续创建一个新的 `UserSuccessResponse.swift` 文件，内容如下：

```swift
import Vapor

struct UserSuccessResponse: Content {
    let status: String = "success"
    let user: UserResponse
}
```

请注意，此 struct 引用了我们的 `UserResponse` struct。

我们还需要在用户登录时返回访问令牌和刷新令牌。

3. 在另一个名为 `LoginResponse.swift` 的文件中输入以下 struct：

```swift
import Vapor

struct LoginResponse: Content {
    let status = "success"
    let accessToken: String
    let refreshToken: String
    let user: UserResponse
}
```

最后，每当返回新的访问令牌时，我们都需要返回它。

4. 然后我们需要地址响应。创建一个 `AddressResponse` 文件并输入以下内容：

```swift
import Vapor

final class AddressResponse: Content {
    var id: Int?
    var street: String
    var city: String
    var zip: String
    var userId: Int
    var createdAt: Date?
    var updatedAt: Date?
    var deletedAt: Date?
}
```

我们在这里使用了 `Address` 的所有属性，因为我们可能需要它们。

5. 添加最后一个文件 `RefreshTokenResponse.swift`，内容如下：

```swift
import Vapor

struct RefreshTokenResponse: Content {
    let status = "success"
    let accessToken: String
}
```

所有 `status` 属性都已定义为 `"success"`。原因是如果发生错误，我们不会返回这些 struct，所以它们——根据定义——只会在成功时返回。

定义完响应模型后，让我们开始定义它们的对应物：输入模型！

# 输入模型

输入模型将接收处理请求所需的所有信息。我们只需要四个模型：

- `AddressInput`：传递地址信息时。
- `NewUserInput`：注册新用户时。
- `EditUserInput`：编辑现有用户时。
- `LoginInput`：登录时。

请执行以下步骤以创建模型：

1. 在 `Sources/App/Models/Inputs/` 中创建一个 `AddressInput.swift` 文件并插入第一个模型，如下所示：

```swift
import Vapor

struct AddressInput: Content {
    let street: String
    let city: String
    let zip: String
}
```

该模型仅反映我们从模型中需要的属性。它符合 `Content` 协议。

2. 创建一个新的 `NewUserInput.swift` 文件，如下所示：

```swift
import Vapor

struct NewUserInput: Content {
    let firstname: String?
    let lastname: String?
    let email: String
    let password: String
}
```

同样，我们只获取 `firstname`、`lastname`、`email` 和 `password` 属性。注册新用户只需要这些。

3. 创建一个 `RefreshTokenInput.swift` 文件，内容如下：




# 刷新访问令牌

```swift
import Vapor

struct RefreshTokenInput: Content {
    let refreshToken: String
}
```

刷新访问令牌仅需要刷新令牌（因为所需的所有信息都保存在该令牌中），因此它是该结构体的唯一属性。

## 4. 创建 `EditUserInput.swift` 文件，内容如下：

```swift
import Vapor

struct EditUserInput: Content {
    let firstname: String?
    let lastname: String?
}
```

我们只允许客户端修改名和姓，因此只需要这两个字段。

当用户想要登录时，我们需要获取他们的信息。

## 5. 最后，创建 `LoginInput.swift` 文件，内容如下：

```swift
import Vapor

struct LoginInput: Content {
    let email: String
    let password: String
}
```

至此完成。现在让我们将响应模型连接到数据库模型！

# 将响应模型连接到数据库模型

现在我们已经准备好响应模型，可以将它们连接到背后的模型。

按以下步骤创建模型：

## 1. 打开 `UserResponse.swift` 并向类中添加以下函数：

```swift
init(user: User, addresses: [AddressResponse]? = nil) {
    self.id = user.id
    self.firstname = user.firstname
    self.lastname = user.lastname
    self.email = user.email
    self.addresses = addresses
}
```

该函数接收一个用户和可选的地址列表，将它们赋值给对应属性。

## 2. 现在，在 `AddressResponse.swift` 中添加以下函数：

```swift
init(_ address: Address) {
    self.id = address.id
    self.street = address.street
    self.city = address.city
    self.zip = address.zip
    self.userId = address.userId
    self.createdAt = address.createdAt
    self.updatedAt = address.updatedAt
    self.deletedAt = address.deletedAt
}
```

该函数接收一个地址并将其值赋给响应对象的属性。

我们现在已经创建并连接了所有模型，接下来开始编写控制器以使服务具备功能！

# 登录与注册

用户服务的主要功能是用户注册和登录。在本节中，我们将介绍 `AuthController` 的所有功能，它处理访问管理所需的所有路由。

以下章节将涵盖：

- 准备工作
- 注册
- 登录
- 刷新访问令牌

让我们从准备服务开始。

# 准备工作

登录和注册是我们服务的两个核心功能。我们需要做一些准备才能让它们正常工作。在本节中，我们将确保拥有创建这些功能所需的一切。

你应该已经创建了 `AuthController.swift` 文件。让我们通过以下步骤实现逻辑：

## 1. 在编辑器（Xcode）中打开此文件。

## 2. 编写以下类：

```swift
import Vapor
import Fluent
import SimpleJWTMiddleware
import SendGrid

final class AuthController: RouteCollection {

    private let sendGridClient: SendGridClient
    
    init(sendGridClient: SendGridClient) {
        self.sendGridClient = sendGridClient
    }
    
    func boot(routes: RoutesBuilder) throws {
       
    }
}
```

请记住：每个控制器本质上只是函数的集合，因此可以是结构体或类。在本例中，我们希望它是 `RouteCollection` 以便注册自定义路由。为此，我们需要添加 `func boot(routes: RoutesBuilder)` 函数。现在你可以取消 `Configure.swift` 中相应行的注释。

## 3. 编写我们需要覆盖的路由的空函数，如下所示：

```swift
func register(_ request: Request) throws -> EventLoopFuture<UserSuccessResponse> { }

func login(_ request: Request) throws -> EventLoopFuture<LoginResponse> { }

func refreshAccessToken(_ request: Request) throws -> EventLoopFuture<RefreshTokenResponse> {  }
```

你可以看到我们接收一个参数：`Request`。通过 `Request`，我们可以解析请求体。

所有三个函数都返回包装在 `EventLoopFuture` 中的先前定义的响应模型。

## 4. 在 `boot` 函数中编写以下代码：

```swift
routes.post("register", use: register)
routes.post("login", use: login)
routes.post("accessToken", use: refreshAccessToken) 
```

第一行注册了一个 `register` 路由，它位于之前附加到 `routes` 的所有内容之后。在这种情况下，完整路径如下：`/*/users/register`。该路由接收 JSON 请求体，可解析为 `NewUserInput`。如果 JSON 请求体不符合该结构体，Vapor 会自动抛出错误并返回给请求者。该路由连接到 `register` 函数。

第二行注册 `login` 并将其连接到 `login` 函数。

第三行与第一行类似，仅定义路由 `accessToken` 并将其与我们的输入模型和 `refreshAccessToken` 函数连接。

好的。让我们开始填充这些函数，从注册开始。

# 注册

当用户注册时，我们主要需要确保输入的数据正确（有效）且没有冲突（已注册）。调用函数时我们会接收 `Request` 和一个 `NewUserInput` 实例。

按照以下步骤添加注册逻辑。

## 1. 首先，我们需要解析输入并创建 `User` 实例。可以通过简单调用以下代码实现：

```swift
let user_ = try request.content.decode(NewUserInput.self)
let user = try User(user_.email, user_.firstname, user_.lastname, user_.password)
```

注意 `user_` 的类型是 `NewUserInput`。

接下来，我们需要确保用户尚未注册。这很简单：请求 Fluent 查找所有具有该邮箱的现有用户，并检查数量是否大于零。请记住 Fluent 始终返回 `EventLoopFuture`，这意味着我们需要处理一个不是该函数返回类型的 `EventLoopFuture`。

## 2. 在函数最后一行后插入以下代码：

```swift
return User.query(on: request.db).filter(\.$email == user.email).count().flatMap { all in
    if all > 0 {
        return request.eventLoop.makeFailedFuture(Abort(.badRequest, reason: "This email is already registered."))
    }
    
    return user.save(on: request.db).transform(to: user)
}
```

第一行创建一个 `EventLoopFuture<Int>`，返回 `email == user.email` 的所有用户数量。如果数量大于 0，我们中止并抛出错误，说明该邮箱已在系统中存在。否则，返回我们之前创建的 `User` 实例。请注意，由于 `map` 函数，计数 Future（`EventLoopFuture<Int>`）转变为 `EventLoopFuture<User>`。如果你对这种转换不清楚，请回顾 第 3 章 *Getting Started with the Vapor Framework* 了解 `EventLoopFuture` 的工作原理。

路由处理函数要求返回 `UserSuccessResponse`，到目前为止我们只检查了用户是否已存在。因此，我们仍需保存用户并构建返回结构体。

## 3. 在上一个代码片段最后一个 `}` 后直接添加以下代码：

```swift
.map { user in
    return UserSuccessResponse(user: UserResponse(user: user))
}
```

我们将之前创建的 `EventLoopFuture<User>` 转换为另一个 Future，它将在保存完成后立即执行。`EventLoopFuture` 由 `user.save(on: request.db)` 构建，这是将保存 `user` 的 Future。




# 注册完成

最后，我们想要输出信息并发送一封漂亮的邮件给用户。

4. 将以下代码添加到文件中——同样，直接在最后一个 `}` 之后：

```
.flatMap { (userResponse) in
    let subject: String = "Your Registration"
    let body: String = "Welcome!"
    let name = [user.firstname, user.lastname].compactMap({ $0 }).joined(separator: " ")
    let from = EmailAddress(email: "info@domain.com", name: nil)
    let address = EmailAddress(email: user.email, name: name)
    let header = Personalization(to: [address], subject: subject)
    let email = SendGridEmail(personalizations: [header], from: from, subject: subject, content: [["type": "text", "value": body]])
    return self.sendGridClient.send([email], on: request.eventLoop).transform(to: userResponse)
}
```

在前两行中，我们定义了邮件的内容。你可以根据需要自定义和扩展它们。从*第 3 行*到*第 7 行*，我们正在构建 `SendGridEmail`。SendGrid 提供了许多功能，我建议你去了解一下。在这里，我们只是创建一封普通的邮件，不包含 HTML 格式。

在最后一行，我们使用 `sendGridClient` 实例发送邮件，它返回一个不同类型的 `EventLoopFuture`。最后的 `.transform(to: user)` 忽略了 future 类型，并返回一个包含原始实例的 `EventLoopFuture<User>`，这正是我们想要的。

你做到了！你已经使用模型、futures、扩展和 SendGrid 构建了你的第一个复杂的控制器函数！给自己鼓鼓掌，继续加油！

既然我们已经完成了注册，接下来让我们处理登录，以便用户能够获取他们的访问令牌。

# 登录

为了让用户登录，我们的控制器需要验证凭据，然后签发两个 JWT：访问令牌和刷新令牌。幸运的是，几乎所有这些都通过现有的提供者和中间件完成。

让我们添加登录函数的逻辑：

1. 将以下代码添加到 `login` 函数中：

```
let data = try request.content.decode(LoginInput.self)
return User.query(on: request.db).filter(\.$email == data.email).all().flatMap { users in
    if users.count == 0 {
        return request.eventLoop.makeFailedFuture(Abort(.unauthorized))
    }
    let user = users.first!
    var check = false
    do {
        check = try Bcrypt.verify(data.password, created: user.password)
    } catch {}
    if check {
    }
    return request.eventLoop.makeFailedFuture(Abort(.internalServerError))
}
```

我们首先将传入的数据解析为 `LoginInput` 结构体。然后，我们根据邮箱查找该用户。如果找不到，我们返回一个错误。如果找到了，我们继续验证密码。

注意密码检查在一个 `do {}` 块中。我们不能在 `flatMap` 回调中抛出错误，必须在这里隔离此调用。

2. 在 `if check {` 之后添加以下行：

```
let userPayload = Payload(id: user.id!, email: user.email)
```

这一行根据用户创建我们的 payload。如果你想自定义这部分，请查看 `Payload` 模型。接下来，我们需要创建两个令牌，`JWT` 包提供了一个简单的 `sign` 函数。

3. 向函数添加以下行：

```
do {
    let accessToken = try request.application.jwt.signers.sign(userPayload)
    let refreshPayload = RefreshToken(user: user)
    let refreshToken = try request.application.jwt.signers.sign(refreshPayload)
} catch {
    return request.eventLoop.makeFailedFuture(Abort(.internalServerError))
}
```

首先，我们使用之前创建的 payload 创建访问令牌。之后，我们根据用户创建刷新令牌。

最后，我们需要创建 `LoginResponse` 并填充详细信息。`LoginResponse` 的一部分是 `UserResponse`，所以让我们先创建它。

4. 在 `let refreshToken ...` 之后创建 `UserResponse`，如下所示：

```
let userResponse = UserResponse(user: user)
```

如果你需要在登录后立即获取用户的地址，可以在这里填充。对于我们的目的，我们暂时将其留空。

5. 现在，像这样创建 `LoginResponse`：

```
return user.save(on: request.db).transform(to: LoginResponse(accessToken: accessToken, refreshToken: refreshToken, user: userResponse))
```

我们只需要将 `LoginResponse` 作为 future 返回，所以我们使用 Vapor 来创建一个。这就完成了！`login` 函数已准备就绪。

6. 最后，添加以下 `else` 以便在密码不正确时返回适当的错误：

```
else {
    return request.eventLoop.makeFailedFuture(Abort(.unauthorized))
}
```

现在我们需要设置一个函数，允许用户使用刷新令牌来刷新他们的访问令牌。

# 刷新访问令牌

要刷新访问令牌，用户只需将刷新令牌传给我们，我们验证它，如果一切正常，返回一个新的访问令牌：

1. 用以下代码填充函数：

```
let data = try request.content.decode(RefreshTokenInput.self)
let refreshPayload: RefreshToken = try request.application.jwt.signers.verify(data.refreshToken, as: RefreshToken.self)

return User.query(on: request.db).filter(\.$id == refreshPayload.id).all().flatMap { users in
    if users.count == 0 {
        return request.eventLoop.makeFailedFuture(Abort(.badRequest, reason: "No user found."))
    }
    let user: User = users.first!
}
```

我们获取输入并将其解析为 `RefreshToken`，同时进行验证。然后，我们从令牌中加载用户，如果存在则可以继续。

2. 在最后一个 `}` 之前编写以下代码：

```
let payload = Payload(id: user.id!, email: user.email)

var payloadString = ""
do {
    payloadString = try request.application.jwt.signers.sign(payload)
} error {}

return user.save(on: request.db).map { _ in
    return RefreshTokenResponse(accessToken: payloadString)
}
```

这现在创建一个新的 `Payload`、一个访问令牌，并将其返回给用户。

如果你想实现某种阻塞机制，在这里可以验证用户*是否仍然*被允许访问。

完成登录和注册后，现在让我们处理用户数据，以便用户可以更新他们的个人资料。

# 管理用户

现在让我们介绍服务中需要的一些用户功能，为客户端提供足够的能力来管理用户的个人资料。在本节中，我们要涵盖三个路由：

* 个人资料 (`GET "/*/users"`)：此路由仅返回当前登录的用户。
* 更新 (`PATCH "/*/users"`)：此路由保存关于该用户的新信息。
* 删除 (`DELETE "/*/users"`)：此路由删除当前用户。

此外，我们首先需要创建一个控制器。

以下四个部分将实现 `UserController` 类：

* 准备工作
* 个人资料
* 更新
* 删除

让我们在以下部分讨论这些内容。

## 准备工作

这里的准备工作很简单。我们只需要一个类来注册正确的路由并将它们连接到我们的内部函数。

执行以下步骤：

1. 在 `App/Sources/Controllers/UsersController.swift` 文件中写入以下代码：




```swift
import Fluent
import Vapor

final class UsersController: RouteCollection {
    
    func boot(routes: RoutesBuilder) {
    }
    
    func profile(_ request: Request) throws -> EventLoopFuture<UserSuccessResponse> {
    }
    
    func save(_ request: Request) throws -> EventLoopFuture<UserSuccessResponse> {
    }
    
    func delete(_ request: Request) throws -> EventLoopFuture<HTTPStatus> {
    }
}
```

该类也实现了 `RouteCollection`，以便我们可以相应地注册路由。这三个空函数代表我们即将注册的三个路由。请注意，每当我们处理用户数据时，都会返回 `UserSuccessResponse`。我们必须让 `delete` 函数返回其他内容，因为用户将不再存在；HTTP 状态码是确认删除的好方法。

2. 在 `boot` 函数中注册这三个路由，如下所示：

```swift
routes.get("profile", use: profile)
routes.post("profile", use: save)
routes.delete("user", use: delete)
```

现在，我们已经准备好，可以开始编写控制器函数了。让我们从 profile（个人资料）开始。

# 个人资料

此路由是最容易实现的。我们知道用户已经登录，并且我们还有一个函数可以为经过身份验证的请求返回用户。

因此，我们只需在函数中写入以下代码行：

```swift
return User.query(on: request.db).filter(\.$id == request.payload.id).all().map { users in
    return UserSuccessResponse(user: UserResponse(user: users.first!))
}
```

就这些。我们使用的是 `SimpleJWTMiddleware` 传递给我们的 `payload` 中的 `id` 用户。

你可能会想，如果我们返回的所有内容已经在 JWT 中，为什么还要写这样的调用？有时，你会想验证 JWT 是否仍然准确。想象一下，用户更改了他们的名字，但 JWT 并未更新。在我们示例中的在线商店设置中，你会希望在提交订单之前验证这一点。

这既快速又简单。现在，让我们创建 `update` 函数。

# 更新

`update` 函数也很简单，并不复杂。就像在 `profile` 函数中一样，我们可以通过 `request.payload.id` 获取 `User` 实例。然后我们更改想要更改的内容，并保存它。

请执行以下步骤：

1. 在 `update` 函数中编写以下代码：

```swift
let content = try request.content.decode(EditUserInput.self)
return User.query(on: request.db).filter(\.$id == request.payload.id).first().flatMap { user in
}
```

这将解析传入的内容并获取 `User` 实例。

2. 更改属性，然后在最后一条语句后编写以下代码：

```swift
let user = user!
if let name = content.firstname {
    user.firstname = name
}
if let name = content.lastname {
    user.lastname = name
}
```

在我们的例子中，我们只关心调整名和姓。当然，你可以添加任意数量的属性。请注意，在我们的 `EditUserInput` 结构体中，`firstname` 和 `lastname` 是可选属性。因此，我们需要检查它们是否已设置，然后才能将它们赋值给用户。

3. 编写以下代码行以保存用户：

```swift
return user.update(on: request.db)
```

“这最后一行不是返回 `EventLoopFuture<User>` 吗？”，你可能会想。没错；我们还没完全完成。我们需要将其映射为正确的返回类型。

4. 在最后一个 `)` 后面写入以下代码：

```swift
.map { _ in
    return UserSuccessResponse(user: UserResponse(user: user))
}
```

因此，整行代码看起来像这样：

```swift
return user.update(on: request.db).map { _ in
    return UserSuccessResponse(user: UserResponse(user: user))
}
```

就这样：`update` 函数完成了。现在，让我们处理 `delete` 函数。

# 删除

这种情况会发生：用户可能想要删除他们的账户，虽然这很遗憾。如果他们想删除，我们要确保他们能正确地执行此操作。在我们的例子中，我们需要删除两种类型的模型：附加在用户上的地址，以及用户本身。

1. 像之前一样获取用户，然后在 `delete` 函数中写入以下代码行：

```swift
return User.query(on: request.db).filter(\.$id == request.payload.id).first().flatMap { user in
}
```

现在，我们需要收集所有地址并删除它们。还记得我们编写了一个便利函数，允许我们直接从 `user` 模型获取地址吗。

2. 在 `{ user` 后写入以下代码行：

```swift
if let user = user {
    return Address.query(on: request.db).filter(\.$userId == user.id!).delete().flatMap {
        return user.delete(on: request.db).transform(to: .ok)
    }
} else {
    return request.eventLoop.makeFailedFuture(Abort(.badRequest, reason: "No user found!"))
}
```

这将删除我们为用户找到的所有地址。要获取实例，我们可以在该行添加 `.transform(to: user)`，这样我们将再次获得一个 `EventLoopFuture<User>` 实例。Fluent 允许我们从这里开始，只需向该 future 添加 `.delete(on: request.db)` 即可删除用户。最后，我们需要返回一个 `EventLoopFuture<HTTPStatus>`，这可以通过再次使用 `transform` 来实现。

3. 修改该行，使其如下所示：

```swift
return user.delete(on: request.db).transform(to: .ok)
```

你可以看到我们是如何将各种 Fluent 函数链接在一起的，它们都在做我们想要它们做的事。就这样：用户现在被删除了。

我们使用的是 Fluent *软删除*，这意味着模型仍在数据库中，但被标记为已删除。如果你想更改此行为以便真正删除它们，只需将 `delete()` 修改为 `delete(force: true)`。

好的——我们现在已经完成了 `UsersController` 的所有函数。接下来让我们处理地址管理。

# 理解地址管理

我们服务的地址管理是一个传统的 REST API。地址可以被读取、添加、编辑和删除。所有这些操作都只能针对已登录且因此在 JWT 中的用户进行。

将涵盖以下路由：

* **获取地址** (`GET /\*/users/addresses`)：此路由仅返回属于该用户的所有地址。
* **创建地址** (`POST /\*/users/addresses`)：此路由为用户创建一个新地址。
* **更新地址** (`PATCH /\*/users/addresses/:id`)：此路由更新具有特定 ID 的地址。
* **删除地址** (`DELETE /\*/users/addresses/:id`)：此路由删除具有特定 ID 的地址。

我们将涵盖以下章节：

* 准备控制器和路由
* 获取地址
* 创建地址
* 更新地址
* 删除地址

让我们逐一介绍。

# 准备控制器和路由

与 `UsersController.swift` 文件类似，我们可以从填写类的大致结构开始。

请执行以下步骤：

1. 将以下代码行写入 `Sources/App/Controllers/AddressesController.swift` 文件中：

```swift
import Fluent
import Vapor

final class AddressesController: RouteCollection {

    func boot(routes: RoutesBuilder) {
    }
    
    func addresses(_ request: Request) throws -> EventLoopFuture<[AddressResponse]> {}
    
    func create(_ request: Request) throws -> EventLoopFuture<AddressResponse> {}
    
    func update(_ request: Request) throws -> EventLoopFuture<AddressResponse> {}
    
    func delete(_ request: Request) throws -> EventLoopFuture<HTTPStatus> {}
}
```

到目前为止，这与 `UsersController` 类几乎相同。

2. 在 `boot` 函数中输入以下代码行以注册我们的路由：




```
routes.get("", use: addresses)
routes.post("", use: self.create)
routes.patch(":id", use: self.update)
routes.delete(":id", use: delete)
```

正如之前一样，基础路由（`/*/users/addresses`）已经传递给了这个函数，所以我们不必把所有这些都写下来。你应该对前两行非常熟悉，并理解它们的作用。

第三行和第四行添加了一个新元素：`:id`。

当你为路由写下路径时，到目前为止你只是将其作为字符串写入，例如 `users`。不过，从技术上讲，你可以用几种不同的方式来指定路径。一种方式是提供 `RouteComponent` 的 `PathComponent`。它可能看起来像这样：

```
router.get("a/b/:id", use: func) // "/a/b/:id"
```

任何符合 `Parameter` 协议的类都可以用作 `RouteComponent`。在我们的例子中，我们只是想传递地址的 ID，所以参数类型是 `Int`。

你可能会想知道两个路由怎么可能指向同一个 URL。请记住，它总是绑定到 `HTTP` 方法。我们正在为 `PATCH` 和 `DELETE` 注册路由。如果不指定 `HTTP` 方法，URL 本身将无法被调用。然而，如果没有特别指定，我们通常倾向于假设它是 `GET` 调用。

好的——现在，让我们填充这些函数。

## 获取地址

返回用户的所有地址很简单。你可能已经猜到了，但我们有一些很好的辅助函数让这变得容易。这个函数的整个主体只有一行。

写下以下行：

```
return Address.query(on: request.db).filter(\.$userId == request.payload.id).all().map { addresses in
    return addresses.map { AddressResponse($0) }
}
```

由于我们要返回 `EventLoopFuture<[AddressResponse]>`，我们确实必须修改 Fluent 的结果。注意，此函数仅在用户经过验证后调用，因此我们可以假设 payload ID 是真实的。

接下来，让我们处理 `create` 函数。

## 创建地址

要创建地址，我们必须先获取授权用户，然后构建新地址，最后保存它。

执行以下步骤：

1. 将以下行写入 `create` 函数：

```
let content = try request.content.decode(AddressInput.self)
```

到现在为止，这对你来说应该非常熟悉；我们只是在解析传入的内容。

2. 接下来，构建地址，如下所示：

```
let address = Address(street: content.street, city: content.city,
    zip: content.zip, userId: request.payload.id)
```

请记住，Vapor 已经确保传递的用户内容符合 `AddressInput`。

虽然 Vapor 确保我们输入的内容与结构体匹配，但 Vapor 并不知道具体的规则。如果你想确保邮政编码至少有五位数，你需要在控制器中添加该逻辑。

3. 最后，我们需要保存地址并返回 `UserSuccessResponse`，如下所示：

```
return address.save(on: request.db).map { _ in
    return AddressResponse(address)
}
```

就是这么简单！简单吧？现在，让我们继续下一个函数：`Update Address`（更新地址）。

## 更新地址

更新地址与我们更新用户的方式相当相似。主要区别在于我们保存的是一个*关联*到用户的模型。

执行以下步骤：

1. 首先写下我们的标准行，如下所示：

```
let content = try request.content.decode(AddressInput.self)
```

2. 现在，从路由器获取 ID，如下所示：

```
let id = try request.query.get(Int.self)
```

此 ID 为 `Int` 类型（非可选），且必须存在。如果请求者尝试发送 `Int` 以外的内容，`Router` 将拒绝该调用。

接下来，我们需要在数据库中找到正确的地址。我们知道地址的 ID，也知道拥有该地址的用户的用户 ID。因此，我们将简单地为 Fluent 生成的数据库查询设置这两个条件。

3. 写下以下 `return` 函数：

```
return Address.query(on: request.db).filter(\.$id == id).filter(\.$userId == request.payload.id).all().flatMap { addresses in
    
}
```

第一部分看起来应该很熟悉：我们正在用 Fluent 构建一个地址 `QueryBuilder`。接下来，我们筛选出匹配我们 ID 的，然后，我们也筛选出匹配用户 ID 的。

你可能会想知道为什么我们要筛选匹配我们用户 ID 的，而不是稍后将找到的地址的用户 ID 进行比较。你完全可以两者都做，效果都一样。然而，通过将用户 ID 作为过滤器添加到查询中，我们利用的是数据库服务器的 CPU 和内存来进行筛选，而不是我们自己的。这也意味着我们不必输出另一个错误消息并确保其工作。我们可以简单地假设我们只得到想要的结果。

4. 在 `addresses in` 行后的 `callback` 函数中添加以下代码：

```
if addresses.count == 0 {
    return request.eventLoop.makeFailedFuture(
        Abort(.badRequest, reason: "No address found!"))
}
let address = addresses.first!
```

虽然我们可以卸载用户 ID 验证，但 Fluent 可能找不到任何行并返回空数组。在这种情况下，我们确实想要中止并返回错误。请注意，如果用户 ID 不匹配，也会发生这种情况；我们不区分原因。

5. 在 `let address ...` 语句后添加以下行：

```
address.street = content.street
address.city = content.city
address.zip = content.zip
```

这只是更新生成的地址。根据我们的数据库布局，我们知道如果能走到这一步，数组中将只有一个结果。所以，我们抓取它并存储在 `address` 中。然后，我们从 `AddressInput` 结构体分配新值。

6. 最后，我们需要保存地址并返回 `AddressResponse`，如下所示：

```
return address.save(on: request.db).map { _ in
    return AddressResponse(address)
}
```

我们完成了 `update` 函数。现在，让我们用 `delete` 函数完成服务。

## 删除地址

`delete` 函数与 `update` 函数相匹配，直到我们保存或删除地址。

执行以下步骤：

1. 为 `delete` 函数填写以下内容：

```
let id = try request.query.get(Int?.self)

return Address.query(on: request.db).filter(
    \.$id == id!).filter(\.$userId == request.
    payload.id).all().flatMap { addresses in
    if addresses.count == 0 {
        return request.eventLoop.makeFailedFuture(
            Abort(.badRequest, reason: "No address found!"))
    }
    
}
```

所有这些都与 `update` 函数相同。

2. 现在，在最后一个 `}` 之前添加一个返回调用，如下所示：

```
return addresses.first!.delete(on: request.db).transform(to: .ok)
```

我们抓取数组中的第一个地址——也就是里面唯一的地址。然后，我们告诉 Fluent 删除它并返回一个简单的 `OK` `HTTPStatus`。

这就完成了！我们完成了服务的编写——太棒了！所以，让我们享受辛勤工作的成果，启动服务。

## 启动服务

你做到了！我们现在需要编译服务并启动它。为此，请运行以下命令：

1. 运行 `swift build`。
2. 运行 `./.build/debug/Run`。

如果你在运行 `swift build` 后遇到任何问题，你应该检查以下几点：

* 你有没有犯任何小拼写错误？正如你所知，Swift 不会原谅这些错误。
* Swift 能否拉取所有正确的依赖项？

否则，如果你运行第二个命令后服务很快就退出了，请务必检查以下几点：

* 凭据是否正确地在环境中。请参阅前面的部分了解如何操作。





*   数据库服务器可访问且已配置。

如果您对学习本章内容有任何疑虑，建议查看 GitHub 上的代码并进行对比。

太棒了！我们已经为示例案例编写、编译并启动了第一个服务！现在，让我们来总结一下。

## 总结

在本章中，您学习了如何基于前几章创建的模板编写微服务。您规划了路由并为该服务编写了整个代码库。我们首先配置并定义了该服务的路由。然后开始编写辅助函数和扩展。我们还编写了 I/O 和数据库模型，最后完成了控制器函数的编写。

现在，您已经学会了如何编写一个扩展的微服务，这是我们商店后端用例的第一个构建块。这也很重要，因为您可以在其他应用程序中使用该服务。

您应该为自己感到自豪；这是一个巨大的进步。此外，该服务可以成为许多其他 Web 项目的起点。在下一章中，我们将介绍微服务之间如何相互通信，您将学习如何测试微服务。

## 问题

通过完成以下任务来测试您的知识和理解：

1.  扩展用户服务，使新用户需要先确认其电子邮件。

2.  将最后登录时间保存到用户模型中。

3.  通过添加 `Country`、`LAT`、`LNG` 和 `State` 等属性来完善地址模型。

4.  进阶：编写管理员功能，允许管理员用户（您需要定义什么是管理员）编辑其他用户的地址和个人资料。
