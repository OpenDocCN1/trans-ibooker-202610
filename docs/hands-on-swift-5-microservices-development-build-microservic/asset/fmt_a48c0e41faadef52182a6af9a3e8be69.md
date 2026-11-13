
在 Xcode 中使用 `.env` 文件，你需要调整 schema 以在项目中运行：

![](assets/47987567-6d42-455a-82ee-aa1c3b94a696.png)

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

正如我们在 [Chapter 2](#0437cb27-3342-4d58-8253-7093af9626ac.xhtml) *Understanding Server-Side Swift* 中讨论的，我们处理两种类型的模型：数据库模型和 **input/output** (**I/O**) 模型。我们现在将创建这两种模型，并学习如何将它们连接到 Fluent。本节将涵盖：

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
