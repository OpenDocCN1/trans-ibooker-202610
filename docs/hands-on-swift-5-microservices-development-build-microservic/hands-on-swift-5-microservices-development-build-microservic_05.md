# 创建你的第一个微服务

你做到了！理论部分基本结束，现在我们开始写代码！在本章中，你将编写第一个微服务。但在编写之前，你将了解 `Vapor` 应用程序的实用部分。使用 `Vapor` 框架将帮助你比从头开始更快地编写应用程序。本章结束时，你将编写完第一个微服务并整合必要的组件。

在本章中，你将学习如何开发一个通用微服务，作为我们在线商店后端应用程序的基础。这也将是我们在整本书中编写的所有其他服务的基础。我们将开发一个模板，用于创建其他服务，你也可以将其用于自己的项目。本章结束时，你将完成第一个微服务的编写！

首先，你将配置包含所有必要依赖项的包。之后，你将设置并学习如何整合模型和控制器。

本章中的大量信息基于十年的实践经验。你可能会发现其他更适合你的方法、途径和手段，这没关系——将其作为一个良好的起点。

本章将涵盖以下主题：

*   启动新服务
*   使用模型
*   使用控制器
*   使用工作线程

让我们开始吧！

## 技术要求

本节介绍你的机器上需要运行什么才能编写此微服务。需要安装以下组件：

*   `Vapor Toolbox`
*   `Swift 5.2+`
*   `Xcode 11+`
*   `Docker`
*   `macOS/Linux`

我们还假设你已经学习了前几章。本章所有代码在 `macOS` 和 `Linux` 下运行方式相同。

本章的 GitHub URL 是 [`github.com/PacktPublishing/Hands-On-Swift-5-Microservices-Development/tree/master/Chapter 5`](https://github.com/PacktPublishing/Hands-On-Swift-5-Microservices-Development/tree/master/Chapter%205)。

## 启动新服务

在本节中，我们将为第一个微服务设置所有内容。微服务本质上就像其他后端应用程序一样；也就是说，它只是被配置和设置为在服务网络中运行。有一些含义和最佳实践使网络管理变得更加容易。因为服务不面向人类，所以我们在设计时可以更加机械化。

我们将探讨以下主题：

*   版本控制：完美记录我们何时更改了什么
*   使用 API 模板：一个适用于大多数服务的简单模板
*   依赖管理：包含所需的包
*   配置：正确设置
*   路由：如何向外部暴露服务

让我们探索一下！

### 版本控制



# Git 和版本控制

这甚至不应该成为一个讨论点，但为了完整起见，*始终* 将代码保存在版本控制系统中，例如 `git`。虽然存在替代方案，但我强烈建议如果你还不熟悉 `git`，请熟悉它。要托管 `git` 仓库，你应该使用一个你选择的在线云服务提供商。`GitHub` 是最流行的，拥有有吸引力的定价模型（包括免费模型！），但其他提供商如 `GitLab` 或 `Bitbucket` 也同样好用。你不应该只将代码保存在本地机器上：硬盘可能会损坏，从云端恢复会让你更安心。

`GitHub` 是最流行的 `git` 提供商。大多数服务端 Swift 项目目前托管在 `GitHub` 上。除非所有者（即 `Microsoft`）以戏剧性的方式改变政策，否则我不指望这会改变。`GitHub` 为开源项目提供免费计划。

## 关于微服务和仓库

大多数云提供商提供某种方式自动部署后端应用程序。我建议以下存储代码的准则：

*   **一个仓库仅用于一个微服务。**
*   **至少拥有 `master` 和 `development` 分支。**
*   **使用 Pull Request 更新 `master` 分支。**
*   **`master` 是部署的分支。**
*   **从仓库（`master`）自动部署到服务器。**
*   **每个项目都应有自己的仓库。**

使用一个仓库管理多个项目可能很诱人。在某些非常通用的用例中，这可能行得通。然而，根据我的经验，你经常需要稍微定制一下。此外，除非仓库是开放的，否则你可能需要处理各种权限设置，如果每个项目都有自己的仓库，这会容易得多。

所以，继续设置你的账户并准备仓库。完成后，我们可以继续了解如何使用 `Vapor` 的标准 API 模板。

# 使用 API 模板

如果一切设置正确，你可以通过在命令行中输入以下命令来创建模板：

## 1. 创建一个新的 Vapor 应用程序

```
$ vapor-beta new template
```

撰写本文时，`Vapor` 的 CLI 仅在 Beta 版中支持 `Vapor 4`。一旦 `Vapor 4` 完全发布，你也可以执行以下操作：

```
$ vapor new template
```

`Vapor` 的 CLI 现在会自动复制标准 API 模板。你可以在 [`github.com/vapor/api-template`](https://github.com/vapor/api-template) 找到该模板；它是 API 的良好起点。

## 2. 进入新创建的文件夹

```
$ cd template
```

模板原样即可编译和构建，但我们想稍微修改一下，使其成为我们自己用途的良好起点。因此，我们将执行以下操作：

*   移除 `SQLite`。
*   添加 `MySQL`。
*   添加 `JWT` 包。
*   添加 `CORS` 中间件。

通过添加这些包，我们为微服务配备了在微服务架构中运行所需的一切。你将在后续章节学习如何添加和移除包。

## 3. 编译项目。这也将加载所有当前依赖项

```
$ swift build
```

## 4. 在 Xcode 中打开项目，输入以下命令

```
$ xed .
```

或者，你也可以通过打开 `Package.swift` 来打开 Xcode。在终端中，可以这样做：

```
$ open Package.swift
```

现在你已成功编译模板，让我们看看 `Vapor` 应用程序的组件。

# 通用文件文件夹

我们可以保持模板的文件结构不变；它已经提供了一个组织良好的层次结构。你唯一想添加的文件夹是 `Workers`，与 `Controllers` 同级，因此你应该将其添加到 `Sources/App/` 中。在此文件夹中，我们可以放置充当 *Workers* 的类。在后面的 *Workers* 部分，你将详细了解为什么需要这样一个文件夹。

此外，在 `Sources/App/` 中创建另一个 `Helpers` 文件夹。此文件夹用于不绑定到我们自己类的扩展和函数，但它们要么是通用的（全局的），要么是来自其他框架的扩展类。

你可以保留其余的文件夹和文件不变。不过，我们将坚持一些准则，以保持一致性：

*   文件名应大写。
*   每个文件应包含一个类或结构体。
*   类、函数或结构体名称应与文件名相同。
*   扩展文件使用 `+` 符号标记原始类。例如，`Application+ImageRequest.swift` 文件将使用与图像请求相关的函数扩展 `Application` 类。

继续将任何非大写的文件名改为大写（`main.swift` 除外；编译器要求它必须小写）。Swift 对文件夹或文件名没有严格要求，但这将使以后接手该项目或让其他人使用它变得容易得多。

在活跃开发期间，打破其中一些规则完全是人之常情。只需确保在提交到仓库之前清理它们。

现在你已设置好文件夹，让我们设置依赖项。

# 依赖管理

对于服务端 Swift 项目，经常需要添加或移除依赖包。接下来让我们探讨一个如何操作的示例。

## 添加和移除包

我们需要移除 `SQLite`，因为我们不会使用它：

### 1. 继续从 `Package.swift` 中移除以下行

```
.package(url: "https://github.com/vapor/fluent-sqlite-driver.git", from: "4.0.0"
```

### 2. 然后，在同一部分添加一行

```
.package(url: "https://github.com/vapor/fluent-mysql-driver.git", from: "4.0.0"),
```

这将 `MySQL Fluent` 包添加到项目中。注意，这也添加了通用的 `Fluent` 包，因此无需单独添加。

### 3. 现在你需要更改以下行

```
.target(name: "App", dependencies: ["Fluent", "FluentSQLiteDriver", "Vapor"]),
```

你可以将前面的行更改为以下内容：

```
.target(name: "App", dependencies: ["Fluent", "FluentMySQLDriver", "Vapor"]),
```

这使 `Fluent` 和 `FluentMySQL` 可用于项目。

### 4. 你现在可以以相同方式添加以下包

*   [`github.com/proggeramlug/SimpleJWTMiddleware`](https://github.com/proggeramlug/SimpleJWTMiddleware) 的 `master` 分支

你应该最终得到一个如下所示的 `.target` 行：

```
.target(name: "App", dependencies: ["FluentMySQLDriver", "Vapor", "SimpleJWTMiddleware"]),
```

### 5. 完成此设置后，我们可以运行以下命令

```
$ swift package update
```

请注意，如果你在 Xcode 中完成了所有这些操作，Xcode 将自动更新你的依赖项。

这将导致 Swift 更新你的依赖项。请注意，加载所有包可能需要一两分钟。如果遇到任何错误，请确保你正确拼写了包名称。

在本章的 GitHub 文件夹中，你将找到此服务的完全工作示例。使用它与你自己的版本进行比较。

### 6. 如果你尚未打开 Xcode，现在可以打开

```
$ open Package.swift
```

设置好模板后，我们现在准备调整配置。

# 配置

API 标准模板附带一个 `configuration.swift` 文件，其中包含一个函数，如下所示：

```
public func configure(_ app: Application) throws {
```

此函数在应用程序启动时调用。我们用它来添加应用程序所需的所有配置，包括中间件、服务、配置、服务器等。

我们想要添加一些我们肯定需要的中间件、服务和配置。接下来让我们做这件事。

## 中间件

将以下中间件添加到你的配置中：

```
app.middleware.use(CORSMiddleware())
```

我们需要的中间件如下：

*   **CORS**：响应浏览器 CORS 请求。




## JWT 中间件与 JWKS 配置

`SimpleJWTMiddleware`；不过，我们无需在此添加它，因为稍后我们会在路由设置期间完成。

我们确实需要向应用程序传递一个 `JWKS`，其中将包含适当的密钥；我们可以这样做：

```swift
guard let jwksString = Environment.process.JWKS else {
    fatalError("在给定的公钥环境变量 'JWKS' 中未找到值")
}
```

Vapor 提供的 `JWTKit` 包允许我们以多种方式设置 JWT 凭据；这是最容易设置的一种。

因此，在 `app.database.use()` 语句下方添加以下行：

```swift
try app.jwt.signers.use(jwksJSON: jwksString)
```

如果 `JWKS` 有问题，这将阻止应用程序启动，从而不会暴露任何安全问题。

## CORS

此中间件仅通过正面返回 `OPTIONS` 请求来响应，而不执行任何其他操作。浏览器会向与源文件不同的主机发送 `OPTIONS` 请求。因此，例如，如果你在 `https://www.domain.com` 下运行前端，而在 `https://api.domain.com` 下运行 API，那么浏览器将发出 `OPTIONS` 请求，因为 `www.domain.com` 不等于 `api.domain.com`。这没有任何问题，且这个方便的中间件会为我们处理它。

你也可以将进入 `www.domain.com/api` 的请求转发到 `api.domain.com`，这样做可以防止请求离开原始域，因此浏览器也不会发送此请求。不过这取决于前端设计，无论如何安装此中间件都无妨。

## Error

此中间件将发生的所有错误和问题转换为格式良好的 `JSON` 消息。虽然你希望尽可能防止这种情况发生，但此中间件将其转换为我们到处都在使用的输出格式。

## 服务

现在，让我们配置一些我们将需要的服务：

*   `Router`（路由器）
*   `DatabaseConfig`（数据库配置）
*   `Middleware`（中间件）
*   `Fluent`

除了我们在前一段中介绍的中间件之外，我们可以按如下方式安装这些服务中的每一个。

### Router

路由器是 `Application` 的一部分；通过使用以下行——它已包含在模板中：

```swift
try routes(app)
```

该行调用 `routes(_ application: Application)` 函数来注册我们想要的路由。

### DatabaseConfig

为了让我们的数据库知道连接到哪里，我们需要配置凭据：

```swift
guard let mysqlUrl = Environment.process.MYSQL_CRED else {
    fatalError("在给定的公钥环境变量 'MYSQL_CRED' 中未找到值")
}
guard let url = URL(string: mysqlUrl) else {
    fatalError("无法正确解析: \(mysqlUrlString)。")
}
app.databases.use(try DatabaseDriverFactory.mysql(url: url), as: DatabaseID.mysql)
```

你可以看到，首先，我们检查是否具有最低限度的凭据变量。我们需要它来连接数据库；因此，如果我们没有它，将会报错（这将阻止应用程序启动）。验证通过后，我们就拥有了使用该配置注册 `MySQLDatabase`（即一个 `Service`）所需的一切。

如果我们希望在标准日志记录器中记录所有针对数据库的查询，只需向 `--log debug` 参数传递一个 application 即可。我发现此参数对于调试和理解底层发生的事情非常有用。

### Fluent

现在我们已经安装了数据库，可以将迁移安装到 Fluent，它将为我们创建数据库表：

```swift
app.migrations.add(CreateTodo())
```

在这里，我们可以为想要放入数据库的每一个 `Model` 类重复该行。注册完迁移后，我们基本就完成了，因为标准设置将涵盖其余所有内容。

除了路由之外，我们的设置现在已完全就绪。接下来让我们来看看路由吧！

## 路由

路由是我们 API 的端点；它们定义了调用哪个 URL 来触发服务器响应。由于它们本质上高度依赖于服务，因此我们在这里无法做太多准备工作。我们只想定义一条每个服务都将共享的路由：

```
/serviceName/health
```

我们的托管基础设施将使用此 URL 来确保服务正常工作。在这种情况下，我们还将在 `routes` 函数中直接提供控制器函数。

将以下代码写入你的 `Routes.swift`：

```swift
import Fluent
import Vapor

func routes(_ app: Application) throws {
    app.get([.anything, "name", "health"]) { req in
        return "Healthy!"
    }

    // additional routes
}
```

在未来的服务中，我们需要替换此处第一个路由中的 `"name"` 变量。由于创建微服务是一个手动过程，我们不应养成过度简化设置过程的习惯，因此这不应是一个常量变量。相反，每次设置新服务时都应手动设置它。

好的，接下来，让我们看看模型，以便我们可以与数据库交互。

## 使用模型

每当你需要存储数据并移动它时，就会用到模型。我们将模型区分为两类，如下所示：

*   **输入和输出** (**I/O**) **模型**
*   **数据库** (**DB**) **模型**

这两种模型的区别在于，I/O 模型主要仅包含用于传递的信息。例如，请求有一个载荷（例如 `POST` 正文），其中包含应存储的信息。数据被解析为 I/O 模型。要保存此信息，你需要将 I/O 模型转换为 DB 模型。你可能会想知道为什么我们不将两者合并为同一个类或结构体。

好吧，除非是一个简单直接的模型——而我没见过多少这样的模型——你几乎总是需要自定义输入和输出，使其与数据库中的内容有所不同。例如，考虑时间戳：你将它们保存在数据库中，它们通常以一种代表服务器时区内时间的字符串形式保存。现在，你想将其作为输出，但你需要考虑用户的时区，这可能与数据库所基于的时区不同。

你可以修改模型，使其在请求时返回不同的值——或者我首选的方案——你可以拥有一个专门处理这些情况的单独模型。DB 模型可能非常复杂且宽泛；只有极少数情况下你会想原样输出整个模型。

让我们看一些例子：

*   I/O 模型：包含来自用户和发往用户的信息的模型
*   DB 模型：我们用于在数据库中存储数据的模型
*   返回 DB 模型：为 DB 模型创建一个 I/O 模型
*   预定义模型

### I/O 模型

几乎每个应用程序都会遇到的一个典型模型是登录模型。它保存用户登录时输入的所有数据，例如电子邮件和密码。在其最原始的形式中，它如下所示：

```swift
struct LoginInput: Content {
    let email: String
    let password: String
}
```

当用户成功登录时，你将想要返回一些用户信息以及访问和刷新令牌。注意，这里你已经想要一个不同于用户 DB 模型本身的模型，因为你不想返回密码哈希。

因此，返回的 I/O 模型将如下所示：

```swift
struct LoginOutput: Content {
    let userId: Int
    let email: String
    let firstname: String?
    let lastname: String?
    let accessToken: String
    let refreshToken: String
}
```

一般来说，你几乎总是想要定义单独的输入和输出模型。

### DB 模型





我们在前一节中定义了 I/O 模型，现在让我们为它设置相应的用户模型——即 DB 模型。请看下面的代码：

```
final class User: Model {
    var firstname: String?
    var lastname: String?
    var email: String
    var passwordHash: String
    var lastLoginTime: Date?
    var registrationTime: Date
}
```

正如你所见，此模型包含的信息多得多，而我们在登录时只请求其中一小部分。我们还需要此模型成为 `MySQLModel` 的子类，以便将其存储在数据库中。通过不公开使用此模型，我们还可以避免一些开销。

但如果你想在请求中输出完整的模型怎么办？这取决于模型的性质，你*可能*会通过内容扩展它并原样返回模型。然而，创建一个单独的输出模型会更整洁、更易于维护——即使它有 95% 的代码相同。考虑到你可能还需要适配某些客户端请求。例如，特定的客户端库（如 JavaScript 或 iOS）可能期望你在返回数据时采用特定格式。通过你的输出模型，你可以快速满足该要求。

那如果你想按前文所述输出用户登录信息呢？接下来让我们看看如何结合这两种类型的模型。

# 返回 DB 模型

返回用户模型相对简单：创建输出模型的新实例并为其赋值。得益于 Swift 的扩展，你可以这样做：

```
extension UserOutput {
    init(withUser user: User, accessToken: String, refreshToken: String) {
        self.userId = user.id
        self.email = user.email
        self.firstname = user.firstname
        self.lastname = user.lastname
        self.accessToken = accessToken
        self.refreshToken = refreshToken
    }
}
```

现在要返回用户，我们只需调用以下代码：

```
return UserOutput(withUser: user, ...)
```

当我们在 第 7 章 *编写用户服务* 中编写用户管理服务时，将完成这些模型。

# 预定义模型

你可能会想知道是否可以且应该为所有服务一起预定义模型，这意味着它们将共享相同的模型，即使服务独立运行。这没有简单的答案；这实际上取决于你的服务是否会共享大量信息。你的某些服务可能不需要任何共享模型，而另一些则需要。通常情况下，你不必复制 DB 模型。根据定义，它们应该在架构中保持唯一。否则，你可能需要重新考虑你的应用程序设计。

另一方面，定义一些可跨不同服务使用的 I/O 模型可能非常合理。一个你在大多数服务中肯定需要的模型是 **JSON Web Tokens**（**JWT**）的载荷。它可能如下所示：

```
struct Payload: Content {
    var userId: Int
    var permissionLevel: Int
    // ...
}
```

借助此模型，每个服务都可以访问 JWT 中的数据（在某些 JWT 包的帮助下）。你可以继续将该模型放在 `Sources/App/Models/` 文件夹中，并将其命名为 `Payload.swift`。

现在让我们看看控制器，它们将接收数据并返回你的应用（即客户端）可以处理的输出。

# 使用控制器

正如我们已经提到的，控制器函数是当请求进来并寻址特定路径时路由器调用的函数。它可以很简单：

```
router.get("hello") { req in
    return "Hello, world."
}
```

当然，我们可以指定更多内容。以下是一些示例：

```
router.post("hello", use: myPostFunction)
router.patch("hello", use: controllerClass.helloPatch)
```

你可以看到，我们只需要为路由器指定一个可被调用并返回 `ResponseEncodable` 对象的函数。

许多其他类都可以遵循 `ResponseEncodable` 协议。例如，一个简单的字符串就被扩展为有效的返回值——就像前面的例子一样。

从技术上讲，我们不需要控制器类，因为路由器只需要函数。然而，控制器类是将函数按逻辑片段分组在一起的常见且整洁的方式。此外，我们可以将路由本身的配置传递给控制器本身，从而拥有一个自我维护的类。

例如，考虑以下情况：

```
try router.register(collection: SampleController()) // 子 URL: "/"
// 或者我们想要一个子 URL
try router.grouped("sub").register(collection: SampleController()) 
// 子 URL: "/sub/"
```

并在控制器中按如下方式定义路由：

```
final class SampleController: RouteCollection {
    func boot(router: Router) throws {
        router.get(use: get)
        router.get(Int.parameter, use: getById)
        router.patch(Int.parameter, use: edit)
        router.post("new", use: add)
        router.delete(Int.parameter, use: delete)
    }
    // ...
}
```

在这里，我们注册了类函数 `get`、`getById`、`edit`、`add` 和 `delete`。所有函数均已在先前指定的子 URL 中注册。

# 与模型交互

Fluent 使与模型通信变得非常容易。对于我们的模板，我们只需记住以下几点：

*   每次数据库交互都发生在 `eventLoop` 上。
*   在 models 文件夹中定义所有模型。如果你想将某些模型分组，可以创建子文件夹。
*   通常，你需要两个模型——一个用于 DB，一个用于 I/O。将两者合并可能很危险。

根据我自己的经验，我再怎么强调第三点的重要性都不为过。你会想把两者合并，但在几乎所有情况下，你最终都不得不重写无数次模型，因为 I/O 的需求与 DB 的不同。

除此之外，只需坚持 Fluent 处理模型的方式，你应该没问题。

# 文件管理

微服务应该足够独立，不必依赖服务器。这意味着，如果你接收文件（如图片）并存储它们，则**始终**将它们存储在云存储系统中。**切勿**将它们存储在本地文件系统上（除非用于处理，但即使这样也几乎总是可以通过内存完成）。

如果你需要返回文件，请利用云分发系统（如 CloudFront 或 S3）。这些系统大多允许你在需要限制访问时自定义访问权限，并且它们也提供缓存功能。然而，最重要的是，它将微服务所需的可扩展性剥离出来，节省你的资源。它们在世界各地还拥有分布式数据中心，使它们能比你更快地提供文件服务。如果你确实需要直接自己返回文件，那么在通过你的服务返回之前，仍应从数据库或云存储提供商处接收它们。

实际上，大多数文件将是媒体文件，如图片或视频。考虑使用像 `imgix.com` 这样的服务帮助你在返回前处理它们。

由于并非每个微服务都需要此功能，我们不会将其添加到通用模板中。当我们进入产品阶段时，你将看到实现的样子。

# SendGrid

有时，你可能需要向用户发送电子邮件。你不应通过 SMTP 自己发送，而应使用像 SendGrid 这样的服务。发送符合反垃圾邮件法规的高质量邮件相当困难。SendGrid 通过为你提供易用的 API 和格式正确的邮件使这变得简单。他们的费用极低，尤其是当你刚开始一个项目时。Vapor 社区还提供了一个易用的包，帮助你与 SendGrid 集成。




类似于文件管理，我们没有在模板中包含 SendGrid，因为我们只在一个服务中需要它。

在较大的项目中，你会想要创建一个通用的“通知服务”来为你管理所有通知。无论这包括电子邮件、短信、WhatsApp 消息还是电报，你都可以管理各个用户的设置以及它们的格式。

我们现在已经使用所有需要的包设置好了模板。现在让我们来看看工作器。

# 使用工作器

工作器是一个为我们执行任务的类或结构体；它通常比控制器所做的事情稍微复杂一些。示例包括转换图像、处理订单和批量处理。在传统的服务器设置中，你会通过 `cron jobs` 来做同样的事情。

根据你的工作器将要执行的任务性质，你可以使用 `vapor/jobs` 包将其功能封装到一个作业中。这样，你就已经拥有了自动重试功能。你可以在这里查看该包：[`github.com/vapor/jobs`](https://github.com/vapor/jobs)

就这样——我们的模板准备好了！你可以在本章的 GitHub 仓库中找到完整的工作模板！

# 总结

在本章中，你已经完成了创建一个模板 API 服务的工作，我们将在即将到来的服务中复用它。你已经编写了该服务，现在了解你将需要哪些元素以及它们是如何连接的。

你已经了解了模板如何预配置常用包、如何设置 Fluent、如何处理信息以及如何处理凭据。这使你具备了编写可以协同工作的功能性微服务的能力。

有了这个模板，你现在已经准备好为我们的在线商店后端开发微服务了！在下一章中，我们将讨论应用程序结构和数据库设计。

# 问题

回答以下问题以验证你的理解：

1.  为什么我们要包含 SimpleJWTMiddleware？

2.  你最有可能如何运行你的微服务，以及你会使用什么服务器环境？

3.  你应该为所有服务一起定义模型，还是为每个服务单独定义？
