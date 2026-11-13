# 开始使用 Vapor 框架

在了解了微服务和 Swift 之后，现在是时候专注于你将要使用的框架了。选择合适的框架是我们技术栈的重要组成部分。Swift 开源后，出现了几个可靠的 Web 开发框架选择。在本章中，你将探索 Vapor 框架，以及为什么本书选择它而不是其他框架。

首先，我们将了解 Vapor 框架及其脱颖而出的原因。我们将探索 Vapor 提供的功能，并查看一些代码示例。之后，我们将学习其他框架与 Vapor 的对比。

本章结构如下：

*   Vapor 框架
*   将 Vapor 与 Kitura、Perfect 和 Smoke 进行比较
*   安装 Vapor
*   Vapor 实战

让我们开始吧！

## Vapor 框架

Vapor 是目前最流行、最受喜爱的 Swift 框架。它在 GitHub 上拥有超过 18,000 颗星，并拥有一个正在构建该框架的活跃社区。在本节中，我们将深入了解该框架的设计和运作方式。

让我们看看以下内容：

*   Vapor 的历史：一切从何开始？
*   Hello World：一个简短易懂的 Vapor 应用程序
*   总体结构：了解 Vapor 的内部工作原理
*   SwiftNIO – Future 和 Promise：SwiftNIO 如何与 Vapor 集成？
*   控制器：逻辑构建块
*   数据库和模型：存储数据
*   视图：提供 HTML

### Vapor 的历史

Vapor 由 Tanner Nelson 于 2016 年发明，就在 Swift 开源后不久。初始版本依赖一些外部 C 库来处理套接字（响应 HTTP 请求），后来被重写为 Vapor 2，后者仅为 MySQL 扩展使用一个 C 库。Tanner 参考 Laravel（PHP）框架松散地编写了 Vapor，因为他从结构角度喜欢 Laravel。

Vapor 2 经历了一个强劲的增长期，期间一些企业赞助商加入支持该框架。与脚本框架类似，Vapor 2 在处理连接时采用同步方法。这成为用户的痛点，因为它可能导致应用冻结或出现其他网络问题。发生这种原因是应用没有明智地利用 CPU 时间，且可能存在竞争条件。因此，Vapor 3 从一开始就被设计为异步的。它甚至在 SwiftNIO 之前就拥有自己的异步层。2018 年初 Apple 发布 SwiftNIO 后，Vapor 在一周内就将其纳入。Vapor 3 于 2018 年 5 月正式发布。

Vapor 3 给框架带来了许多变化——主要变化是几乎每个事务现在都由 SwiftNIO 异步处理。经过几个月的错误修复，Vapor 4 的最初构想开始形成。

在撰写本文时，Vapor 4 目前处于测试阶段，是 Vapor 3 的更新和改进版本。Vapor 3 的反馈和经验被汇总到一个新版本中，包含一些破坏性更改，从而产生了 Vapor 4 版本。实际上，让 Vapor 3 应用程序在 Vapor 4 下运行并不需要太多工作。

### Hello World

Vapor 在结构和内部设置方面是有主见的。有主见的框架为你的结构提供了大量指导，例如文件格式、文件夹层次结构和命名约定。

然而，官方的“Hello World”只需七行代码：

```swift
import Vapor
let app = try Application()

app.get("hello") { req in
    return "Hello, world."
}

try app.run()
```

如你所见，Vapor 应用程序作为服务器运行只需要一个元素：应用程序对象。应用程序充当路由器，管理所有 URL 访问点，并将它们转发给控制器函数或类。

Vapor 是一个有主见的框架，这意味着它鼓励遵循某些标准来构建文件结构和文件内容。

### 总体结构

Vapor 为其应用推荐了一种结构，尽管这不是强制的。你可以在 <https://docs.vapor.codes/4.0/folder-structure/> 阅读更多关于结构的详细信息。

简化的 Vapor 应用程序结构如下：



# 项目结构

```
Public/
Sources/
Sources/Run/main.swift
Sources/App/Controllers/SomeController.swift
Sources/App/Models/AModel.swift
Sources/configure.swift
Sources/routes.swift
Tests/
Tests/LinuxMain.swift
Tests/AppTests/AppTests.swift
Package.swift
```

`Package.swift` 是 **Swift Package Manager** (**SPM**) 文件，包含所有目标和依赖项。请在此查看：

```swift
// swift-tools-version:5.1
import PackageDescription

let package = Package(
    name: "app",
    platforms: [
        .macOS(.v10_14)
    ],
    products: [
        .executable(name: "Run", targets: ["Run"]),
        .library(name: "App", targets: ["App"]),
    ],
    dependencies: [
        // 一个服务端 Swift Web 框架。
        .package(url: "https://github.com/vapor/vapor.git", from: "4.0.0-beta.2"),
        .package(url: "https://github.com/vapor/fluent.git", from: "4.0.0-beta.2"),
        .package(url: "https://github.com/vapor/fluent-sqlite-driver.git", from: "4.0.0-beta.2"),
    ],
    targets: [
        .target(name: "App", dependencies: ["Fluent", "FluentSQLiteDriver", "Vapor"]),
        .target(name: "Run", dependencies: ["App"]),
        .testTarget(name: "AppTests", dependencies: ["App", "XCTVapor"])
    ]
)
```

从技术上讲，Vapor 应用只需要一个依赖项：`Vapor`。该包包含服务器应用所需的所有通用功能；例如应用、路由、服务、JSON、辅助工具等。

不过，你经常会发现自己需要额外的包。在前面的例子中，应用使用了 SQLite，因此将其与 Vapor 一起引入。

Vapor 应用中有两个目标，如下所示：

1. `Run` 是包含 Swift 主函数的应用程序。
2. `App` 是作为依赖项使用的实际应用程序。

你可能会问为什么 Vapor 要以这种方式拆分。原因是你不能将测试目标与可执行目标放在一起。因此，为了能对你的应用程序运行单元测试，它必须是一个依赖项。

在 `Controllers` 文件夹中，你放置应用的所有控制器。对于微服务，你不需要太多控制器，因为你要尽量降低复杂性。`Models` 包含你交互的所有数据库模型。`Sources` 中的其他文件是通用和配置文件，让应用知道该做什么。

注意，Swift 并不规定文件结构，Vapor 也不规定。你可以把所有东西放在一个文件中。不过，按逻辑功能拆分类、结构体和枚举是有意义的，这样以后容易找到代码。Vapor 也鼓励这样做。

# 配置

Vapor 应用过去有配置文件。然而，在 Vapor 3 中它们被弃用了，原因有二——第一个是主要原因：

1. 它们解析缓慢并增加额外层级（如 YAML 或 JSON）。
2. 它们可能被暴露且构成风险因素；环境变量可在运行时传递给应用程序，而无需引入任何文件。

向 Vapor 应用传递配置设置的方法是设置系统环境变量。在 macOS 中，通过调用以下命令完成：

```
$ export VARIABLE='value'
```

所有必需的配置设置都应放在环境变量中。随后可通过操作系统或 Docker 传递给应用。稍后在本书中，我们将探讨配置工作原理的更复杂示例。

向 Vapor 传递环境变量的另一种方法是创建一个包含如下配置变量的 `.env` 文件：

```
VARIABLE1=value
VARIABLE2=value
```

该文件将在应用启动时加载。

# 中间件

中间件是一种特定的服务，它在控制器函数处理请求前后与传入和传出连接进行交互。它们对于执行常见任务非常有用，例如：

* 将所有输出转换为 JSON，包括错误
* 基于请求头、Cookie 等对请求进行身份验证
* 返回你可能需要的静态文件

我们也将设置其中一些中间件来协助我们。

# 服务

你可能从其他项目中熟悉服务。它们是为多个控制器执行常见任务的单实例类。在 Vapor 中，服务因其异步特性而扮演特殊角色。例如，大多数数据库驱动都是服务。Vapor 4 在 `application` 实例或 `request` 实例上提供服务对象。

例如，要访问数据库，你只需引用 `request.db`，它将返回正确的数据库实例对象。Vapor 使用扩展提供这些变量，从而节省大量时间。可以说 Vapor 没有传统意义上的服务（即通过依赖注入或静态链接），而是通过 Swift 的语言设计。

例如，让我们看看 Vapor 3 是如何工作的：

```swift
let mailService = container.make(MailService.self)
```

在这种传统方法中，服务按请求初始化，我们通过函数获取它们，这里是 `make()`。

现在，在 Vapor 4 中，我们可以简单地假设服务存在于 application 或 request 上，不需要调用函数，只需假设它在那里：

```swift
let mailService = request.mailService
```

在后台，通过 Swift 的语言设计，对象就在那里且可用。

# RoutesBuilder

路由器是应用中决定由谁（即哪个控制器）响应请求的逻辑。Vapor 的方法是拥有 `RoutesBuilder` 协议，使每个遵循该协议的类成为路由类。这非常强大，且——更重要的是——性能极高。HTTP 支持九种不同类型的请求：`GET`、`POST`、`OPTIONS`、`DELETE`、`PUT`、`HEAD`、`PATCH`、`COPY` 和 `SEARCH`。对我们而言，以下四种最相关：`GET`、`POST`、`DELETE` 和 `PUT`。

请看此代码示例：

```swift
public func routes(_ app: Application) throws {
    // Example 1
    app.get("hello") { req in
        return "Hello, world!"
    }
    // Example 2
    let someController = SomeController()
    app.get("models", use: comeController.index)
    app.post("models", use: someController.create)
    app.delete("models", AModel.parameter, use: someController.delete)

    // Example 3
    let group = app.grouped("v1")
    v1.get("user", use: someController.user)
}
```

`routes` 函数由 `configure.swift` 中的 `configure` 函数调用，并提供一个 `Application` 实例。你现在可以调用 `RoutesBuilder` 的函数，并为特定路径分配回调函数，定义 `Application` 的行为。

这里，`Example 1` 是处理路由器最基本的方式。对于特定 URL (`"/hello"`) ，我们向屏幕返回 `"Hello, world!"`。

`Example 2` 现在添加了控制器。路径（例如 `"/models"`）现在重定向到控制器的函数（`index`）。这使我们能将逻辑分散到多个控制器中。

`Example 3` 添加了分组功能。现在使用该组的所有内容都需要匹配组标识符。在这种情况下，URL 将是 `/v1/user`，并将调用控制器中的 `"user"` 函数。

# SwiftNIO – Future 和 Promise

SwiftNIO 是一个库，通过网络和数据流（包括文件）提供基于事件的数据流。重要的一部分是了解和理解事件循环，以及 Future 和 Promise 是什么及其扮演的角色。

让我们分析 NIO 的要素：

* EventLoop
* EventLoopFutures
* EventLoopPromises

## EventLoop



事件循环是一个持续运行的 `while` 循环，用于处理事件。它提供线程安全，并允许服务器同时响应多个请求，即使它们共享相同的资源。幸运的是，你不需要过多担心这部分。你只需知道 Vapor 和 NIO 正在为你管理这部分。但这意味着你需要注意以下几点：

*   你永远不知道何时会在循环中被处理。
*   每个输入/输出都必须经过事件循环。

输入和输出的处理方式是通过 `EventLoopFutures` 和 `EventLoopPromises`。让我们看看如何工作：

# EventLoopFutures

Future 是一个类的实例，它将在未来的某个时刻拥有一个值。因此，当你接收到一个 future 实例时，它还没有值。然而，一旦它被事件循环处理，它将拥有一个值。实际上，你很少会在事件循环之外处理 futures，这就是为什么 NIO 仅称它们为 `EventLoopFutures` 而非仅 `Futures`。Futures 是通过事件循环创建的，如下所示：

```
let intFuture: EventLoopFuture<Int> = eventLoop.newSucceededFuture(result: 10)
```

你可以看到，我们的 `EventLoopFuture` `intFuture` 将包含 `Int` 类型。在这种情况下，future 已经成功。通常，你接收的 futures 正在等待外部事件，例如数据库响应。

为了使用该值，你必须附加一个回调函数，该函数可以使用 `EventLoopFuture` 类型提供的方法之一来访问最终值：

```
intFuture.whenSuccess { i in 
    print("our value: ", i) // output: "our value: 10"
}
```

现在我们为此 future 附加了一个观察者，当 future 拥有值时会被调用。

实际上，你通常希望接收 future 的结果并将其转换为其他东西。NIO 提供了两个函数来实现这一点，我们接下来将进行探讨。

# map

如果我们想要返回一个具有不同类型和值的 future，可以使用 `map`：

```
let stringFuture: EventLoopFuture<String> = intFuture.map { i in
    return String(i)
}
```

你会注意到你需要经常进行这些转换，尤其是在处理数据库时。例如，你会接收到 `DatabaseResult` 类型的 future，但想要返回一个用户。`map` 是允许你执行此类转换的函数。

# flatMap

另一个重要的函数是 `flatMap`。它类似于 `map`，但返回另一个 future，允许你链接不同的操作。例如，你可以从数据库接收一个用户，更改名称，然后立即保存：

```
return User.query(on: request.db).filter(\.$id == 1).first().flatMap { (user: User?) in
    if let user = user {
        user.name = "Steve Jobs"
        return user.save(on: request.db)
    }
    else {
        return request.eventLoop.makeFailedFuture(Abort(.badRequest, reason: "No user found."))
    }
}
```

在这里，我们通过 `request` 实例上的数据库对象创建了一个 future。该 future 将包含 ID 为 1 的数据库用户，如果不存在则为 `nil`。因此，在第一行之后，我们在回调中接收到一个可选的 `User` 类。我们更改 `user` 的名称并返回一个保存用户的新 future。当用户保存时返回第二个 future，其类型为 `Void`。许多操作但不返回任何内容的函数类型为 `Void`。

如果遇到错误，例如用户不存在，我们可以返回一个失败的 future。这样，future 的监听器可以做出适当的反应。

你可以看到我们对第一个 `EventLoopFuture` 类型使用了 `query(on: request.db)`。如前所述，所有 futures 都在事件循环上运行。因此，我们传递了 request（它引用了事件循环），Vapor 将返回正确的 `EventLoopFuture` 类型给我们。`flatMap` 函数现在使用第一个 future 的结果并将其映射到另一个具有不同类型（`Void`）的 `EventLoopFuture`。

# EventLoopPromises

`EventLoopPromises` 与 `EventLoopFutures` 非常相似：它们要么成功，要么失败。这种可预测性至关重要，因为并非所有操作都总是成功的。如果你失去了与数据库的连接，该过程将失败——有了 promise，你可以在失败后选择拾起碎片。

创建 promise 与 futures 息息相关：

```
let testPromise: EventLoopPromise<String> = eventLoop.newPromise(String.self)
print(testPromise.futureResult)         // Future<String>
testPromise.succeed(result: "Hello")    // 成功完成 promise
testPromise.fail(error: ...)            // 使 promise 失败
```

如果你不确定返回值是否存在，promise 会很有帮助。例如，承诺我们将在这里有一个 `String`。这个 promise 要么被履行，要么被拒绝。与 `EventLoopFuture` 的区别在于，Future 是 Promise 的结果。我们承诺一个结果，Future 将用于履行这个承诺。

# Controllers

Controllers 是 Vapor 中的核心决策元素。它们是类或结构体，从路由器获取**输入**并将其转换为**输出**。输入包含来自请求的数据。大多数情况下，你将交互的数据是 POST 数据，它发送在请求体中、请求的 URL 参数或头部中。

从技术上讲，controllers 并非绝对必要。如果你只渲染模板，或者只需要一个函数，路由器的回调可以是一个函数——不依附于 controller。然而，对于比 Hello World 更复杂的应用程序，没有 controllers 属于糟糕的设计；缺乏结构会让读者困惑。

# Content

在 Vapor 中传递的数据通常源自 `Content` 类型。它是一个遵循 `Codable` 的类，这意味着它可以毫无问题地从 JSON、XML 或任何其他数据结构转换并转换为它们。请看以下结构体：

```
struct SomeData: Content {
    var stringValue: String
    var optionalValue: String?
    var anotherValue: Int
    var anotherContent: ClassThatIsContent
}
```

在 JSON 中，数据可能如下所示：

```
{
    "stringValue": "a random string",
    "anotherValue": 10,
    "anotherContent": { ... }
}
```

我们可以使用任何实现 `Codable` 协议的对象或类。所有标准类型（`String`、`Int` 和 `Bool`）默认遵循。但我们也可以实现自己的类型。这使我们能够通过构建所有必需的结构体（或类）轻松表示甚至复杂的 JSON 结构。

注意 Swift 是静态类型语言。如果你以前处理过动态类型语言，请注意 Swift 不容忍从 `String` 到 `Int` 以及反之的隐式转换。要转换它们，你需要使用提供的函数。

`Content` 是一个有用的协议，允许你为输入和输出编写结构体或类。

# Requests

在大多数情况下，你想要处理传入的请求。协议 `RoutesBuilder` 将其传递给我们的函数。一个示例 controller 类可能如下所示：

```
final class TestController {
    func doSomething(_ req: Request) throws -> String {
        return "we did something"
    }
}
```

如你所见，我们将 `Request` 对象作为函数的参数获取。现在我们可以与请求中的数据交互。例如，如果你想要解析传入的 body 以成为一个模型，你可以这样做：




```swift
struct InputData: Content {
    var someVariable: String
    var anotherVariable: Int?
}
func parseMe(_ req: Request) throws -> EventLoopFuture<HTTPStatus> {
    let inputData = try req.content.decode(InputData.self)
    print("our input data:", inputData)
    return req.eventLoop.makeSucceededFuture(.ok)
}
```

我们的函数接收 `Request` 对象并访问 `content` 变量，要求其将自身解码为 `InputData` 实例。这是异步发生的，这就是为什么我们在这里返回 `EventLoopFuture` 类型。

# 数据库与模型

模型代表存储或传递的数据。一般来说，模型不需要连接到数据库。在某些情况下，你可能只需要一个模型作为你已在另一个模型中拥有的数据的另一种表示形式。

另一方面，模型也是将数据加载进出数据库的绝佳工具。Vapor 最受期待甚至最强大的功能之一是其 `Object-Relational Mapping`（`ORM`，对象关系映射）：Fluent。

在接下来的两节中，你将探索 Fluent 及其支持的数据库。

## Fluent 和 FluentKit

`FluentKit` 是 Swift 的 ORM；Fluent 是其针对 Vapor 的特定适配。Fluent 将你的模型连接到数据库，并为你管理存储、删除和查询。FluentKit 设计为与框架无关，因此它也可以用于其他服务端框架，甚至客户端。

## 一个 Fluent 模型

让我们看看下面的示例模型：

```swift
final class User: Model {
    static let schema = "users"

    @ID(key: "id")
    var id: Int?

    @Field(key: "name")
    var name: String

    init() {
        self.name = ""
    }

    init(id: Int? = nil, name: String) {
        self.id = id
        self.name = name
    }
}
```

`Model` 类是一个协议，确保我们的类符合 Fluent 的要求，以便其能处理数据。

## 插入

将新模型插入数据库就像这样简单：

```swift
// .. 这必须发生在我们拥有 Worker 的某处
let user = User()
user.name = "Steve Jobs"
return user.create(on: request.db) // 返回 EventLoopFuture<User>
```

使用 Fluent 保存模型意味着该操作必须在事件循环中运行，并且必须异步发生。

## 更新

更新现有模型就像保存它一样简单，如下所示：

```swift
// .. "user" 是 User 的实例
user.name = "Tim Cook"
return user.update(on: request.db) // 返回 EventLoopFuture<User>
```

如果你不确定是创建还是更新模型，也可以使用 `save(on:)`；它将根据主键 ID 判断是更新还是创建。

## 删除

删除模型的工作方式——与更新和创建非常相似——如下所示：

```swift
user.delete(on: request.db) // 返回 EventLoopFuture<Void>
```

Vapor 区分硬删除和软删除。硬删除意味着从数据库中删除条目。软删除仅将条目标记为已删除——这意味着它可以被恢复。

要启用软删除，你必须在模型中像这样指定变量/列的键：

```swift
final class MyModel: Model {
    // ...
    @Field(key: "deletedAt")
    var deletedAt: Date?
}
```

通常最好至少暂时使用软删除。你稍后随时可以删除它们；然而，从实际角度来看，在条目被删除后至少保留几天已被证明是有益的。

## 查询

最常需要的功能之一是在数据库中查找条目。Fluent 提供了多种方法来实现这一点。你不会在本节学到所有方法，但会获得一个大致的概念。

所有查询都从模型开始，然后是 `query(on:)`，这会初始化 `QueryBuilder`。拥有 `QueryBuilder` 后，你可以根据需要应用任意数量的过滤器，确定顺序和偏移量，并获取结果。

如果你想查找单个模型，以下函数可以为你做到：

```swift
Model.find(1337, on: request.db) // 返回 EventLoopFuture<Model> 并查找 ID 为 1337 的模型
```

如果你想构建更复杂的查询，请看以下示例：

```swift
// 下一个查询等同于上面的第一个示例。
Model.query(on: request.db).filter(\.$id == 1337).all() 
// 返回 Future<[Model]>
// 这是如何实现 "或" 或 "与"
Model.query(on: request.db).or { or in
    or.filter(\.$id == 1337)
      .filter(\.$id == 1338)
}
```

`\.$id` 符号指的是 Swift 的反射特性，是 `\Model.$id` 的简写。它返回对该类中变量的引用。Fluent 随后将其转换为有效的 SQL 语句。

以下运算符默认可用：

| **运算符** | **类型** |
| --- | --- |
| `==` | 等于 |
| `!=` | 不等于 |
| `>` | 大于 |
| `<` | 小于 |
| `>=` | 大于或等于 |
| `<=` | 小于或等于 |
| `~~` | 包含；对于 `String`，这是 `LIKE %foo%`，对于 `Array`，这是 `IN (...)` |
| `!~` | 不包含；对于 `String`，这是 `NOT LIKE %foo%`，对于 `Array`，这是 `NOT IN (...)` |

现在，让我们看看支持的数据库。

## 支持的数据库

Fluent 目前支持以下数据库：

- MySQL
- PostgreSQL
- Redis
- SQLite

Vapor 没有原生的 NoSQL 驱动。然而，社区提供的包使用 `MongoKitten` 驱动。在本书中，你将使用 MySQL，但如果你想换成 PostgreSQL 也相对容易。NoSQL 是一种适合特定情况的方法；依我的经验，最好还是从关系型数据库开始。关系型数据库强迫你提前规划结构，当你拥有大量数据时，这最终会得到回报。另一方面，NoSQL 可能导致数据不一致。

# 视图

Vapor 提供了一个名为 `Leaf` 的模板系统。Leaf 允许你编写包含一些逻辑的模板并与控制器交互。Leaf 模板以 `*.leaf` 结尾，通常存储在 `/Resources/Views` 文件夹中。

方便的是，你可以直接将视图的渲染作为 `EventLoopFuture` 类型返回：

```swift
app.get("myTemplate") { req -> EventLoopFuture<View> in
    return req.view.render("example", ["name": "Vapor"])
}
```

在这种情况下，`/myTemplate` 返回 `/Resources/Views` 文件夹中 `example.leaf` 的内容，并传递一个设置为 `"Vapor"` 的 `"name"` 变量。模板应如下所示：

```
Hi #(name)!
```

如果你需要使用 Vapor 展示 HTML 页面，应该查看 `docs.vapor.codes` 下 Leaf 的大量文档。微服务可能直接提供 HTML，但它们真正的意图更偏向于提供 API。你在本书中不需要编写模板，因为大多数微服务不直接提供 HTML。

# Vapor 与 Kitura、Perfect 和 Smoke 的对比

在本节中，你将了解 Vapor 与其他 Swift 框架之间的一些关键区别。所有框架都被各行各业和参与者使用。Vapor 脱颖而出，因为它在 GitHub 上成为宠儿，拥有比其他框架多得多的星标和活跃贡献者。

三个替代的 Swift 框架如下：

- Kitura
- Perfect
- Smoke

让我们逐一探索它们。

## Vapor 和 Kitura




# Vapor 和 Kitura

Kitura 与 Vapor 大致在同一时间起步，但实际上是由 IBM 发起的。Vapor 和 Kitura 之间最明显且值得注意的区别在于，Kitura 大致模仿了 Express JavaScript 框架。框架内的其他 API 和库同样模仿了现有的流行框架。例如，模板系统 Stencil 模仿了 Django 和 Mustache。相比之下，Vapor 的整体设计最初某种程度上模仿了 Laravel（PHP）；因此，Vapor 的各个方面都被开发得尽可能原生于 Swift。

Kitura 以完全开源的形式在 Apache 2.0 许可证下发布，而 Vapor 则在 MIT 许可证下发布。

Kitura 和 Vapor 都使用 `SwiftNIO`，因此它们提供相似的功能。本书未选择 Kitura 的原因是 Vapor 提供了更原生的 Swift 体验，且拥有显著更活跃的社区。

# Vapor 和 Perfect

Perfect 是第一个允许 Swift 在服务器上运行的框架。它甚至在 Apple 将 Swift 开源之前就已发布。与 Kitura 和 Vapor 不同，Perfect 在风格和指南方面并不太有主见。

与 Kitura 类似，Perfect 模仿了 Express。

Perfect 和 Vapor 最大的区别在于 Perfect 尚未引入 `SwiftNIO`。至今，Perfect 仍使用现有的 C 库进行网络活动。这会使调试变得困难，并可能导致 API 不一致，这也是本书未选择 Perfect 的原因。

# Vapor 和 Smoke

Smoke 框架是 Swift 服务端框架中最新加入的成员，因为它直到 2019 年才发布。它由 Amazon 开发，旨在解决现有云提供商结构（如 AWS API Gateway）背后的微服务问题。Smoke 并不积极试图成为像 Vapor 或 Kitura 那样的竞争者。它主要面向 AWS 集成。本书未选择它是因为并非每个微服务应用都应该或将会运行在 AWS 上——因此，你应该选择一个云提供商无关的框架，而 Smoke 不是。

虽然总体而言 Smoke 的用途有限，但如果你需要在 AWS 中实现特定的小功能（如 Lambda），你可能会发现拥有一个能帮上忙的 Swift 框架非常有用（而不必转向 Java、Python 或其他语言）。

# 安装 Vapor

现在是时候在你的机器上安装 Vapor 了。让我们确保你拥有所需的一切。使用最新的系统至关重要，以便 Swift 和 Vapor 能正常运行。本书假设你在 macOS 电脑上工作。不过，所有与服务器相关的内容在 Linux 机器上也应能正常工作。

## 先决条件

请确保已具备以下设置：

* macOS Mojave 或更高版本
* Xcode 10.2 或更高版本
* Homebrew：如果你尚未安装 Homebrew，请前往此处按说明安装：[`brew.sh/`](https://brew.sh/)

你也可以使用 Linux（Ubuntu）；不过本书假设你使用的是 macOS。唯一的区别是 Linux 上没有 Xcode，但其他一切应该都一样。

请按以下步骤操作：

1. 安装 Xcode 后，你已安装 Swift 运行时和编译器。运行以下命令检查版本：

```
$ swift --version
```

2. 应显示 5.2 或更高版本。

3. 现在安装 Vapor；我们通过向 Homebrew 添加正确的 tap 并安装所有必要文件来实现：

```
$ brew tap vapor/tap
$ brew install vapor/tap/vapor
$ brew install vapor-beta
```

4. 完成后，验证安装是否成功。输入以下命令，你应该会看到参数列表：

```
$ vapor --help
```

Vapor 还提供了一个小工具箱，本质上是将 `SPM` 的常用控制组合在一起。

现在我们准备好运行第一个 Vapor 应用了！

## Vapor 实战

安装 Vapor 并了解其基本原理后，让我们安装 API 模板以开始。运行以下命令：

```
$ vapor new FirstApp
```

此命令告诉 Vapor 下载其 API 启动模板的最新版本。这是一组预配置的文件，是 API 应用的良好起点。请按以下步骤操作：

1. 输入以下命令切换到包含 FirstApp 的文件夹：

```
$ cd FirstApp
```

2. 现在，让我们创建一个 Xcode 项目：

```
$ open Package.swift
```

3. 上述命令将自动为你打开项目。你现在将看到项目及你所学的结构。

4. 要在 Xcode 中运行 Vapor 应用，你必须确保运行的是 `Run` 目标。它可能默认为其他目标。正确运行时应如下所示：

![](img/a971c22b-73fd-49af-99bc-5aabef2165c2.png)

本章到此结束。让我们进入总结。

## 总结

在本章中，你学习了 Vapor 的基本原理及其运作方式。你了解了 Vapor 相对于其他框架的优势，以及为什么它是微服务的绝佳选择。安装 Vapor 后，你运行了第一个 Vapor 应用并看到了结果。

请花些时间把玩 Vapor，以熟悉该框架。本章无法涵盖 Vapor 的所有功能，但应该是一个良好的起点。你可以访问 `docs.vapor.codes` 获取更多信息。本章向你介绍了编写微服务所需的 Vapor 相关部分。不过，你应该熟悉官方 Vapor 文档，因为本章无法涵盖所有内容，且框架在不断演进。

在下一章中，我们将详细规划我们的电子商务示例后端。

## 问题

当你感到自信时，回答以下问题以验证你的理解。本节的问题性质较广泛，旨在引导你更深入地学习 Vapor：

1. 向你的 FirstApp 应用添加一个与新控制器交互的新路由。
2. 向你的应用添加一个新模型并将其集成到控制器中。
3. 构建一个简单的分页应用，其中请求可以包含 `sortBy` 和 `offset` 等参数。
