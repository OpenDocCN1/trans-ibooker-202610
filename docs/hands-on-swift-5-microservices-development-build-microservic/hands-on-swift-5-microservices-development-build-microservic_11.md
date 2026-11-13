# 订单管理服务

编写完**用户管理服务**（**UMS**）和**产品管理服务**（**PMS**）后，现在让我们将两者连接起来，编写**订单管理服务**（**OMS**）。

在本章中，你将学习如何编写一个连接到另一个微服务以验证信息，然后异步处理数据的微服务。你将应用第 10 章《理解微服务通信》中的知识，让该服务验证数据。在本章结束时，你将编写完我们示例应用的最后一块拼图：一个连接其他两个服务的微服务。

本章将涵盖以下部分：

* 入门
* 创建路由
* 创建模型
* 创建控制器
* 连接到 PMS
* 添加支付方式

让我们开始吧！

# 技术要求

本节介绍你需要在机器上运行的组件，以便编写此微服务。需要安装以下组件：

* `Vapor Toolbox`
* `Swift 5.2+`
* `XCode 11+`
* `macOS/Linux`

我们还假设你已经完成了前几章，并知道如何编写 Vapor 微服务。创建此微服务的过程将重点关注*新*元素，而不涉及第 7 章《编写用户服务》中已介绍的内容。此外，我们将假设你正在使用 macOS。所有功能在 Linux 下也以相同方式工作。你可以在 GitHub 上找到本章的代码：[`github.com/PacktPublishing/Hands-On-Swift-5-Microservices-Development/tree/master/Chapter 11/OrderService`](https://github.com/PacktPublishing/Hands-On-Swift-5-Microservices-Development/tree/master/Chapter%2011/OrderService)。

现在让我们开始设置项目。

# 入门

在本节中，你将学习如何在开始编写主要元素之前设置此服务。设置此服务比之前的服务 UMS 容易得多。我们要发送电子邮件，但不发布 JWT。我们基本上将执行以下操作：

* **接收用户的订单：** 服务需要接收来自用户的订单，通常通过你的应用程序或前端。订单请求包含用户想要购买的商品以及发货和账单信息。

* **验证订单中的价格：** 在微服务中，数据库不共享。因此 OMS 不知道产品的价格，因为它们在 PMS 中。为了验证订单，我们必须在收到订单后连接到 PMS 并获取价格列表。

* **等待付款到账：** 订单下单后，我们需要等待付款。某些付款可能几乎是即时的，如信用卡。其他付款，如寄送支票或电汇，可能需要几天时间。

* **付款完成后处理订单：** 订单完成后，我们需要开始发货。在我们的例子中，我们只是将订单标记为已发货，但将来扩展此服务时可能涉及更多内容。

* **让用户查看订单历史：** 用户通常想查看过去订购的内容，因此应该提供该功能。

本节涵盖以下内容：

* 设置模板
* 设置 JWT 验证
* 设置数据库
* 配置

在每一节中，我们都将准备微服务模板以供此服务使用。我们将自定义设置，添加所需的中间件，并配置数据库。在最后一节中，我们将配置 Vapor 调用的 `configure` 函数。

让我们从模板开始。

## 设置模板

让我们开始设置上一章创建的模板。第 5 章《创建你的第一个微服务》中创建的模板是此服务的一个很好的起点。但我们需要对其进行自定义。我们只需将之前创建的内容作为该服务的基础。请遵循以下步骤：

1. 进入产品管理器的文件夹，如下所示：

```
$ cd ~/Shop\ Backend/OrderService
```

2. 现在使用以下命令将模板从原始文件夹复制到我们的新文件夹：

```
$ cp -r ../template/. . && rm -rf .build
```

3. 现在使用以下命令打开 `Package.swift` 文件：

```
$ open Package.swift
```

4. 将名称更改为 `OrderService`。

5. 现在 Xcode 将安装所有依赖项，我们可以构建项目。

现在我们已经设置好模板，可以开始使用它了。产品管理服务不需要任何额外的包。我们确实想验证特定请求，以允许管理员用户管理产品，所以让我们继续设置 JWT。

## 设置 JWT 验证

为了让此服务将正确的用户与订单关联，我们需要能够识别发送请求的用户。正如我们在第 7 章《编写用户服务》中讨论的，我们使用 JWT 进行识别。在此服务中，我们只需使用提供的包将负载与其签名进行验证。

得益于随模板安装的 JWT 包，我们在这里不需要做太多工作。为了方便，让我们像在第 7 章《编写用户服务》中那样创建一个设置文件。请遵循以下步骤：

1. 编辑文件夹中的 `.env` 文件：

```
$ open .env
```

2. 打开你的 `jwks.json` 文件，它应该保存在项目文件夹上方的文件夹中。它看起来像这样：

![](img/464a7938-a47e-4ec7-9786-6df729a896f6.png)

3. 将 `jwks.json` 复制为一行。

4. 将其粘贴到设置文件中，如下所示：

```
JWKS='<从 jwks 复制值>'
```

就这样。我们不需要私钥，因为我们只是验证 JWT。每当有请求进来并需要验证时，JWT 中间件将自动使用公钥检查 JWT 负载是否正确签名。如果你需要熟悉这一点，可能需要回到第 3 章《Vapor 框架入门》。

现在我们已经设置好 JWT，让我们配置数据库。

## 设置数据库

继续在本地数据库设置中创建一个新用户和数据库。如果需要提醒，请查看第 3 章《Vapor 框架入门》。将用户命名为 `service_orders`，数据库也命名为 `service_orders`。

要设置数据库，我们只需执行以下操作：

1. 将以下行添加到 `.env` 文件中：

```
MYSQL_CRED='mysql://service_orders:password@localhost:3306/service_orders?ssl=false'
```

确保你输入了正确的密码。

让我们接下来看看配置。

## 配置 Vapor

此服务的配置非常简单。你只需要添加迁移并设置数据库。




## 创建路由

打开 `Configure.swift` 并将函数调整为如下所示：

```swift
import Fluent
import FluentMySQLDriver
import Vapor

func configure(_ app: Application) throws {
    guard
        let jwksString = Environment.process.JWKS
    else {
        fatalError("No value was found at the given public key environment 'JWKS'")
    }

    guard
        let urlString = Environment.process.MYSQL_CRED
    else {
        fatalError("No value was found at the given public key environment 'MYSQL_CRED'")
    }

    guard
        let url = URL(string: urlString)
    else {
        fatalError("Cannot parse: \(urlString) correctly.")
    }

    app.databases.use(try DatabaseDriverFactory.mysql(url: url), as: DatabaseID.mysql)
    app.middleware.use(CORSMiddleware())
    app.middleware.use(ErrorMiddleware())
    try app.jwt.signers.use(jwksJSON: jwksString)
    app.server.configuration.supportCompression = true

    app.migrations.add(CreateOrder())
    app.migrations.add(CreateOrderItem())
    app.migrations.add(CreateOrderPayment())

    try routes(app)
}
```

请注意，我们在此处注册了三个 `migrations`？你将在本章稍后创建相应的类。此外，还调用了 `routes` 函数，我们将在本节之后立即对其进行配置。

此处所需的就是这些。

你已成功设置服务，配置了数据库和中间件，并创建了迁移和路由的引用。现在让我们开始处理路由。

与其他服务一样，我们的 OMS 需要可被公开调用。我们通过设置路由来实现这一点，允许用户和其他服务调用此服务。本节设置的路由将为我们完成此任务。

该服务的大部分工作是在幕后完成的，而非公开进行。对于最终用户（或消费者），以下两个操作很重要：

*   提交新订单。
*   检索订单历史记录。

对于管理员，我们需要执行以下操作：

*   获取所有订单列表
*   为订单添加付款

下表列出了订单相关路由：

| 路径 | 参数 | 输出/描述 |
| --- | --- | --- |
| `/*/orders` (需要认证) | `POST` | 预期订单项总数 | 从最终用户/消费者提交订单 |
| `/*/orders` (需要认证) | `GET` | - | 返回 JWT 中用户的所有订单 |
| `/*/orders` (需要认证，管理员级别) | `GET` | - | 返回所有用户 |
| `/*/orders/payment/:id` | `POST` | 付款信息 | 为订单提交付款 |

因此，路由并不多。让我们来实现它们。请遵循以下步骤：

1.  转到 `Sources/App` 并打开 `Routes.swift`。
2.  输入以下代码：

```swift
import Fluent
import Vapor
import SimpleJWTMiddleware

func routes(_ app: Application) throws {
    let currentVersion = app.grouped(.anything, "orders")

    currentVersion.get("health") { req in
        return "All good."
    }

    let protected = currentVersion.grouped(SimpleJWTMiddleware())

    let orderController = OrderController()
    protected.post("", use: orderController.post)
    protected.get("", use: orderController.listMine)
    protected.get("all", use: orderController.list)
    protected.post("payment", ":id", use: orderController.postPayment)
}
```

前几行对你来说应该非常熟悉。然后我们利用 `SimpleJWTMiddleware` 来保护路由。在该服务中，我们没有公开路由。之后，我们定义了上表中的路由。代码执行时只会注册路由，但它会告诉 Vapor Router 将传入特定 URL 端点的调用导向何处。

好的，我们已经创建了允许用户调用此服务的路由。要使该服务开始工作，我们需要处理控制器和模型。控制器需要模型才能工作，因为模型将包含我们所有的信息。所以让我们开始处理模型。

## 创建模型

在该服务中，我们需要将传入的订单存储在数据库中。本节创建的模型通过 Fluent 用 Swift 表示该信息。此外，我们还需要创建输入和输出模型，帮助我们将信息从用户传入和传出。

我们有以下模型需要考虑：

*   `Order`：代表消费者的订单。
*   `OrderItem`：订单中的一项商品。
*   `OrderPayment`：为订单记录的付款。

谈到付款时，请务必为不同的付款类型和金额提供足够的灵活性。人们通常不像我们希望的那样精通我们的服务。

让我们浏览以下章节：

*   **配置数据库模型**：创建与数据库对应的模型。
*   **编写输入模型**：将所有输入放入结构体中。
*   **编写输出模型**：模型对应数据库模型，以便用户按我们指定的格式获取它们。

开始吧！

## 配置数据库模型

我们的数据库模型位于其惯常文件夹中：`Sources/App/Models`。请遵循以下步骤：

1.  在 Finder 或 Xcode 中打开 `Sources/App/Models/Database` 文件夹，添加一个新的 `Order.swift` 文件，并为该文件输入以下代码：

```swift
import Fluent
import Vapor

final class Order: Model {
    static let schema = "orders"

    @ID(key: "id")
    var id: Int?

    @Field(key: "totalAmount")
    var totalAmount: Int

    @Field(key: "paidAmount")
    var paidAmount: Int

    @Field(key: "userId")
    var userId: Int

    @Field(key: "status")
    var status: Int

    @Timestamp(key: "createdAt", on: .create)
    var createdAt: Date?

    @Timestamp(key: "updatedAt", on: .update)
    var updatedAt: Date?

    @Timestamp(key: "deletedAt", on: .delete)
    var deletedAt: Date?

    @Field(key: "firstname")
    var firstname: String

    @Field(key: "lastname")
    var lastname: String

    @Field(key: "street")
    var street: String

    @Field(key: "zip")
    var zip: String

    @Field(key: "city")
    var city: String

    @Children(for: \OrderItem.$order)
    var items: [OrderItem]

    init() { }

    init(
        id: Int? = nil,
        totalAmount: Int,
        paidAmount: Int = 0,
        status: Int = 0,
        firstname: String,
        lastname: String,
        street: String,
        zip: String,
        city: String
    ) {
        self.id = id
        self.totalAmount = totalAmount
        self.firstname = firstname
        self.lastname = lastname
        self.street = street
        self.paidAmount = paidAmount
        self.status = status
        self.zip = zip
        self.city = city
    }
}
```

你所看到的代码是 Fluent 模型中完整的订单模型。我们首先将其声明为模型子类，然后定义属性：

*   `id`：这是数据库中的 ID。
*   `totalAmount`：这是订单的总金额。该数字以**美元分**为单位。
*   `paidAmount`：这是已支付的总金额。
*   `status`：这是订单的状态。
*   `firstname`：这是客户的名。
*   `lastname`：这是客户的姓。
*   `street`：这是客户的街道。
*   `zip`：这是客户的邮政编码。
*   `city`：这是客户的城市。

地址以及客户信息当然是**过度简化**的，因此你需要添加更多信息（如国家、州等）才能使其良好运行。

订单包含我们存储订单所需的一切。你可能会想知道为什么我们也要在这里保存地址，因为它也在用户服务中。有两个原因：





## 创建订单项模型

### 2. 创建 `OrderItem.swift`

创建另一个文件 `OrderItem.swift`，并输入以下内容：

```
import Fluent
import Vapor

final class OrderItem: Model, Content {
    static let schema = "orderItems"
    
    @ID(key: "id")
    var id: Int?

    @Field(key: "unitPrice")
    var unitPrice: Int

    @Field(key: "totalAmount")
    var totalAmount: Int
    
    @Field(key: "productId")
    var productId: Int

    @Field(key: "quantity")
    var quantity: Int
    
    @Parent(key: "orderId")
    var order: Order

    @Timestamp(key: "createdAt", on: .create)
    var createdAt: Date?
    
    @Timestamp(key: "updatedAt", on: .update)
    var updatedAt: Date?
    
    @Timestamp(key: "deletedAt", on: .delete)
    var deletedAt: Date?
    
    init() { }

    init(
        id: Int? = nil,
        totalAmount: Int,
        unitPrice: Int,
        quantity: Int,
        order: Order) {
        self.id = id
        self.totalAmount = totalAmount
        self.unitPrice = unitPrice
        self.quantity = quantity
        self.order = order
    }
}
```

与前面的 `Order` 类类似，我们存储了单个项所需的所有信息：

*   `id`：数据库中的 ID
*   `totalAmount`：该行的总金额（`quantity` x `unitPrice`）
*   `unitPrice`：该商品的单价
*   `quantity`：数量
*   `order`：对应的订单

我们存储了在给定时间点重建订单所需的一切。如果产品价格将来发生变化，我们仍然保存了该订单的价格。

## 创建订单支付模型

### 3. 创建 `OrderPayment.swift`

创建第三个也是最后一个数据库模型，创建另一个文件 `OrderPayment.swift`，并输入以下内容：

```
import Fluent
import Vapor

final class OrderPayment: Model, Content {
    static let schema = "orderPayments"
    
    @ID(key: "id")
    var id: Int?

    @Field(key: "totalAmount")
    var totalAmount: Int
    
    @Parent(key: "orderId")
    var order: Order
    
    @Field(key: "method")
    var method: String

    @Timestamp(key: "createdAt", on: .create)
    var createdAt: Date?
    
    @Timestamp(key: "updatedAt", on: .update)
    var updatedAt: Date?
    
    @Timestamp(key: "deletedAt", on: .delete)
    var deletedAt: Date?
    
    init() { }

    init(
        id: Int? = nil,
        totalAmount: Int,
        method: String,
        order: Order) {
        self.id = id
        self.totalAmount = totalAmount
        self.method = method
        self.order = order
    }
}
```

支付条目保存以下属性：

*   `id`：数据库中的 ID
*   `totalAmount`：该支付的总金额
*   `method`：描述支付方式的字符串，例如 "cash"
*   `order`：对应的订单

我们不指定支付方式；我们将其留空，以便管理员可以输入 "credit card"、"cash" 或其他方式。

现在我们还需要创建迁移文件，以便 Vapor 自动创建我们的表。

## 创建迁移文件

### 4. 创建 `CreateOrder.swift`

在 `Sources/App/Migrations` 中创建一个新的 `CreateOrder.swift` 文件，并输入以下代码：

```
import Fluent

struct CreateOrder: Migration {
    func prepare(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("orders")
            .field("id", .int, .identifier(auto: true))
            .field("totalAmount", .int, .required)
            .field("paidAmount", .int, .required)
            .field("userId", .int, .required)
            .field("status", .int, .required)
            .field("firstname", .string, .required)
            .field("lastname", .string, .required)
            .field("street", .string, .required)
            .field("zip", .string, .required)
            .field("city", .string, .required)
            .field("createdAt", .date)
            .field("updatedAt", .date)
            .field("deletedAt", .date)
            .create()
    }

    func revert(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("orders").delete()
    }
}
```

该迁移创建订单的所有属性。

### 5. 创建 `CreateOrderItem.swift`

创建一个新的 `CreateOrderItem.swift` 文件，并输入以下代码：

```
import Fluent

struct CreateOrderItem: Migration {
    func prepare(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("orderItems")
            .field("id", .int, .identifier(auto: true))
            .field("totalAmount", .int, .required)
            .field("unitPrice", .int, .required)
            .field("productId", .int, .required)
            .field("quantity", .int, .required)
            .field("orderId", .int, .required)
            .field("createdAt", .date)
            .field("updatedAt", .date)
            .field("deletedAt", .date)
            .create()
    }

    func revert(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("orderItems").delete()
    }
}
```

此代码包含创建上述订单项的迁移。

### 6. 创建 `CreateOrderPayment.swift`

创建另一个文件 `CreateOrderPayment.swift`，包含以下代码：

```
import Fluent

struct CreateOrderPayment: Migration {
    func prepare(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("orderPayments")
            .field("id", .int, .identifier(auto: true))
            .field("totalAmount", .int, .required)
            .field("method", .string, .required)
            .field("orderId", .int, .required)
            .field("createdAt", .date)
            .field("updatedAt", .date)
            .field("deletedAt", .date)
            .create()
    }

    func revert(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("orderPayments").delete()
    }
}
```

这个最后的迁移创建订单支付表，包含前面对应的字段。所有三个迁移指导 Fluent 创建哪些字段。正如你所见，这些字段与前面的模型 100% 对应。

数据库模型部分到此结束。让我们继续输入模型。

# 编写输入模型

输入模型定义服务从用户或客户端接收的用户输入。将其与数据库模型区分开很重要，因为它们包含的内容可能不同。就像我们为产品管理服务和用户管理服务定义输入模型一样，我们需要在这里定义正确的模型。

我们需要为以下目的编写输入模型：

*   订单提交：订单的内容
*   支付提交：支付所需的信息
*   产品信息：从 PMS 检索的信息

按照以下步骤创建输入模型：

### 1. 创建 `OrderItemInput.swift`

打开 `Sources/App/Models/Input` 文件夹，创建一个 `OrderItemInput.swift` 文件，并插入以下代码：

```
import Vapor

struct OrderItemInput: Content {
    var productId: Int
    var unitPrice: Int
    var quantity: Int
}
```

这非常简单，我们拥有接收订单项所需的一切。

### 2. 创建 `OrderInput.swift`

创建一个 `OrderInput.swift` 文件，并插入以下代码：

```
import Vapor

struct OrderInput: Content {
    var totalAmount: Int
    var firstname: String
    var lastname: String
    var street: String
    var zip: String
    var city: String
    var items: [OrderItemInput]
}
```

这现在包含关于订单的更多通用信息。注意我们引用了第一步创建的 `OrderItemInput`。

### 3. 创建 `PaymentInput.swift`

接下来，创建一个 `PaymentInput.swift` 文件，包含以下内容：

```
import Vapor

struct PaymentInput: Content {
    var orderId: Int
    var method: String
    var totalAmount: Int
}
```

这里只需要三个字段。





4.  最后，我们需要一个来自 PMS 的输入。创建 `Product.swift` 文件，并输入以下代码：

```swift
import Vapor

struct Product: Content {
    var id: Int
    var unitPrice: Int
}
```

PMS 返回给我们的信息比这个 `struct` 中定义的要多，但我们只关心价格。我们想要验证用户需要支付的金额，所以我们需要价格和产品 ID。

创建完输入模型后，现在让我们开始处理输出模型。

# 编写输出模型

输出模型是输入模型的对应物；它们定义了我们服务的输出应该是什么样子。

与输入模型的一个区别是，我们不需要那么多输出模型。当我们执行以下操作时，会返回信息：

*   订单成功提交。
*   支付已添加。
*   以列表形式返回订单。

第一点和最后一点可以合并，因为它们包含大致相同的信息。所以让我们从以下步骤开始：

1.  打开 `Sources/App/Models/Output`。
2.  创建 `OrderItemResponse.swift` 文件并输入以下内容：

```swift
import Vapor

struct OrderItemResponse: Content {
    var productId: Int
    var unitPrice: Int
    var quantity: Int
    var totalAmount: Int
}
```

请注意，这个文件与 `OrderItemInput.swift` 文件几乎相同。区别在于我们不包含总金额，因为该金额将被计算得出，不需要作为输入的一部分。

3.  创建一个 `OrderResponse.swift` 文件，并输入以下内容：

```swift
import Vapor

struct OrderResponse: Content {
    var id: Int
    var totalAmount: Int
    var paidAmount: Int
    var userId: Int
    var status: Int
    var createdAt: Date?
    var items: [OrderItemResponse]
}
```

请注意，这个文件看起来也非常像 `OrderInput.swift`。但我们在这里也添加了订单项。

4.  在最后一个 `}` 大括号之前输入以下函数：

```swift
init(order: Order, items: [OrderItem]) {
    self.id = order.id!
    self.totalAmount = order.totalAmount
    self.paidAmount = order.paidAmount
    self.userId = order.userId
    self.status = order.status
    self.createdAt = order.createdAt
    
    self.items = []

    for item in items {
        let orderItemResponse = OrderItemResponse(productId: item.productId, unitPrice: item.unitPrice, quantity: item.quantity, totalAmount: item.totalAmount)
        self.items.append(orderItemResponse)
    }
}
```

这使我们能够创建一个完整的订单输出。

我们现在已经创建了数据库模型、输入模型和输出模型。我们的服务只缺少将不同模型相互连接的逻辑。所以，让我们来处理控制器，它们将包含服务的逻辑。

# 创建控制器

该服务在后台使用用户提供的输入进行操作。为了在逻辑上分离这两个领域，我们将创建两个控制器：

*   `OrderController`：管理与真实用户相关的所有内容。
*   `ProcessingController`：管理后台的所有内容，无需人工干预。

让我们从 `OrderController` 开始。

## 编写 OrderController

该控制器与用户交互，并将引用我们的输入和输出。

1.  首先在 `Sources/App/Controllers` 中创建一个 `OrderController.swift` 文件，并输入以下代码：

```swift
import Fluent
import Vapor

final class OrderController {
}
```

这只是一个空控制器，所以让我们添加几个函数。

2.  添加前面 `routes.swift` 文件中的函数：

```swift
func list(req: Request) throws -> EventLoopFuture<[Order]> {
    
}

func listMine(req: Request) throws -> EventLoopFuture<[Order]> {
}

func postPayment(req: Request) throws -> EventLoopFuture<AddedPaymentResponse> {
}

func post(req: Request) throws -> EventLoopFuture<OrderResponse> {
}
```

3.  现在我们可以填充 `list(req: Request)` 函数的代码：

```swift
return Order.query(on: req.db).all().map { orders in
    return orders.map { return OrderResponse(order: $0, items: $0.items) }
}
```

4.  `listMine(req: Request)` 函数的代码看起来非常熟悉：

```swift
return Order.query(on: req.db).all().map { orders in
    return orders.map { return OrderResponse(order: $0, items: $0.items) }
}
```

这两个函数根据请求者是管理员还是消费者返回订单。接下来的重要函数将保存订单和支付。

5.  填充 `post(req: Request)` 函数：

```swift
let orderInput = try req.content.decode(OrderInput.self)

let order = Order(totalAmount: orderInput.totalAmount, firstname: orderInput.firstname, lastname: orderInput.lastname, street: orderInput.street, zip: orderInput.zip, city: orderInput.city)

return order.save(on: req.db).flatMap {
    var saving: [EventLoopFuture<Void>] = []
    var items: [OrderItem] = []
    
    for inputItem in orderInput.items {
        let item = OrderItem(totalAmount: inputItem.unitPrice * inputItem.quantity, unitPrice: inputItem.unitPrice, quantity: inputItem.quantity, order: order)
        saving.append(item.save(on: req.db).map {
            items.append(item)
        })
    }
    
    return saving.flatten(on: req.eventLoop).map {
        return OrderResponse(order: order, items: items)
    }
}
```

虽然该函数为用户做了很多事，但代码本身简单直接。我们从 POST 请求中获取订单输入。然后根据该输入创建订单。我们将其保存到数据库以获取 ID，然后将订单项添加到订单中。订单项被保存，然后我们返回订单。

6.  填充 `postPayment(req: Request)` 函数：

```swift
let paymentInput = try req.content.decode(PaymentInput.self)

return Order.query(on: req.db).filter(\Order.$id == paymentInput.orderId).all().flatMap { orders in
    if orders.count == 0 {
        return req.eventLoop.makeFailedFuture(OrderError.orderNotFound)
    }
    
    let payment = OrderPayment(id: nil, totalAmount: paymentInput.totalAmount, method: paymentInput.method, order: orders.first!)
    
    return payment.save(on: req.db).transform(to: AddedPaymentResponse())
}
```

这接收支付输入。然后将输入保存到订单，并返回 `AddedPaymentResponse`。

这处理了用户所需的一切。现在让我们看看内部控制器。

## 编写 ProcessingController

`ProcessingController` 是一个仅限内部使用的控制器。它将用于执行用户无法参与的订单操作。

按照以下步骤创建我们的控制器：

1.  打开 `Sources/App/Controllers`，创建一个 `ProcessingController.swift` 文件，并将以下内容输入到文件中：

```swift
import Fluent
import Vapor

final class ProcessingController {
}
```

这段代码只是类的容器。我们将需要以下三个函数：

*   `getOrders`：根据状态返回订单列表。
*   `processOrderInformation`：根据 PMS 检查订单的价格。
*   `getProductInformation`：从 PMS 返回价格。

2.  创建三个函数模板：

```swift
func getOrders(status: Int = 0, on app: Application) throws -> EventLoopFuture<[Order]> {
}

func processOrderInformation(_ order: Order, on app: Application) -> EventLoopFuture<Bool> {
}
    
func getProductInformation(productIds: [Int], client: Client) -> EventLoopFuture<[Product]> {
}
```




到目前为止，所有这些函数尚未填充；我们稍后将处理此事。注意我们将 `Application` 传递给了两个函数。原因在于当我们不是从用户请求运行这些函数时，通常没有可用的请求实例。不过 `Application` 为我们提供了所需内容。

接下来，我们需要确保获取 `PSM URL`，以便知道从哪里获取价格。

3. 在第一个 `{` 大括号下方输入以下内容：

```
let productServiceUrl:String
    
init(_ productServiceUrl: String) {
    self.productServiceUrl = productServiceUrl
}
```

初始化此控制器时，我们期望传入 `PSM URL`。稍后我们将调用此 URL 以检索产品信息。

4. 按如下方式填充 `getProductInformation` 函数：

```
func getProductInformation(productIds: [Int], client: Client) ->
 EventLoopFuture<[Product]> {
       return client.get(URI(string: self.productServiceUrl),
        headers: ["Content-Type": "application/json"]).
        flatMapThrowing { (response:ClientResponse) in
            return try response.content.decode([Product].self)
        }
    }
```

你可以看到这部分出奇地简单，不需要太多额外逻辑。我们从预先定义的 URL 发起请求，并将信息解析到我们的数据模型中。

5. 使用刚创建的函数按如下方式处理订单：

```
func processOrderInformation(_ order: Order, on app: Application) -> EventLoopFuture<Bool> {
        let productIds:[Int] = order.items.map { $0.productId }
        
        let expectedTotal = order.totalAmount
        
        return self.getProductInformation(productIds: productIds,
         client: app.make(Client.self)).flatMap {
          (products:[Product]) -> EventLoopFuture<Bool> in
            var total = 0
            for item in order.items {
                for product in products {
                    if product.id == item.productId {
                        total = total + product.unitPrice *
                         item.quantity
                    }
                }
            }
            if total != expectedTotal {
                return app.make(EventLoop.self).makeFailedFuture(
                 OrderError.totalsNotMatching)
            }
            order.totalAmount = total
            order.status = 1
            return order.save(on: app.databases.default()).
             transform(to: true)
        }
    }
```

在第一行中，我们为自己创建了一个产品 ID 数组（`productIds`）。然后我们可以将此数组传递给 `getProductInformation` 函数。该函数的结果是一个包含产品的 `EventLoopFuture`。我们遍历产品以计算我们自己的总价。

循环结束后，我们比较计算出的总价（`total`）是否与用户提供的预期总价（`expectedTotal`）匹配，如果不匹配则返回一个失败的 future。否则，一切正常，我们为订单分配新状态并保存它。

6. 现在剩下的就是创建我们的获取订单函数：

```
func getOrders(status: Int = 0, on app: Application) throws -> EventLoopFuture<[Order]> {
        return Order.query(on: app.databases.default()).
         filter(\Order.$status == status).all()
    }
```

此代码仅返回带有状态过滤器的所有订单。我们将状态作为参数保留，因为我们希望提供一种在其他场景中也能使用这些函数的方式。

最后，我们现在可以着手处理运行控制器的应用部分。为了调度任务，我们可以使用 SwiftNIO 中的一些函数。

在此使用 SwiftNIO 的函数非常适合我们的需求。根据你的需要，你可能想要使用包含恢复和重试机制的更复杂的作业队列系统。

7. 按如下方式填充 `boot.swift` 中的 boot 函数：

```
import Vapor
import NIO

private var verifyOrdersTask: RepeatedTask? = nil

public func boot(_ app: Application) throws {
    
    let url = Environment.get("PRODUCT_SERVICE_URL") ??
     "http://localhost:8080/v1/products"

    let processingController = ProcessingController(url)
    
    verifyOrdersTask = app.make(EventLoop.self).
     scheduleRepeatedAsyncTask(
        initialDelay: .seconds(0),
        delay: .seconds(60)
    ) { [weak app] task -> EventLoopFuture<Void> in
        let app = app!
        
        var returnFuture:EventLoopFuture<Void>
        
        do {
            returnFuture = try processingController.
             getOrders(status: 0, on: app).flatMap {
             orders -> EventLoopFuture<Void> in
                var processingFutures:[EventLoopFuture<Void>] = []
                
                for order in orders {
                    processingFutures.append(processingController.
                    processOrderInformation(order, on: app).
                    transform(to: ()))
                }
                return processingFutures.flatten(on: app.make(
                 EventLoop.self))
            }
        }
        catch let error {
            returnFuture = app.make(EventLoop.self).
             makeFailedFuture(error)
        }
        
        return returnFuture
    }
}
```

在此代码中，我们创建了一个 `ProcessingController` 实例（服务中唯一的实例），然后使用 SwiftNIO 启动一个重复任务。在此任务中，我们仅请求所有未处理订单并进行处理。我们通过调用控制器的相应函数来实现这一点。

注意我们是如何从环境中获取 `PSM URL` 的。然后我们仅调用 `ProcessingController` 中的函数。

至此，这部分已完成。我们现在已经编写了控制器和模型，以及一个处理传入订单的例程。接下来让我们看看如何扩展此服务。

# 扩展订单管理服务

当然，此服务过于简化，无法在真实购物平台的上下文中完全工作。在本节中，我们将探讨你可以做些什么以使其准备好投入生产。你可能有未在此列出的扩展想法，这完全没问题。本节旨在展示如何扩展服务的示例，以便你应用自己的扩展。

请记住每个微服务都是独特的。如果你计划将此微服务用于多个商店，请为每个商店复制一份并进行定制。它并非旨在通用。

我们将查看以下部分：

*   添加税费
*   添加支付方式
*   添加退款和优惠券

让我们从税费开始。

## 添加税费

税费是真实商店需要考虑和实现的。你会注意到我们尚未在此服务中实现任何税费。你添加的税费将取决于法律框架。但假设你想添加简单的销售税。你会在 PSM 中为 `Product` 扩展一个税率百分比。然后订单管理服务的更改将涉及以下步骤：

1. 按如下方式编辑 `Models` 中的 `Order.swift` 文件：

```
final class Order: Model, Content {
...
    @Field(key: "taxAmount")
    var taxAmount: Int
...
```

这会将总税额保存到订单中。

2. 按如下方式编辑 `Models` 中的 `OrderItem.swift` 文件：

```
final class OrderItem: Model, Content {
...
    @Field(key: "taxAmount")
    var taxAmount: Int
...
```

这里我们也为每个项目添加了税额。自然，你也需要验证税额。因此你可以通过添加以下代码来编辑 `processOrderInformation` 函数：



```
...            
var total = 0
var totalTax = 0
            for item in order.items {
                for product in products {
                    if product.id == item.productId {
                        total = total + product.unitPrice *
                         item.quantity
                        totalTax = totalTax + (product.unitPrice *
                         product.tax * item.quantity)
                    }
                }
            }
            if total != expectedTotal {
                return app.make(EventLoop.self).makeFailedFuture(
                 OrderError.totalsNotMatching)
            }
            order.totalTax = totalTax
            order.totalAmount = total + totalTax
...
```

我们所做的就是添加税务代码并将其加到总计中。

注意，我们现在需要将 `totalTax` 添加到 `totalAmount`。

3. 要使其工作，你需要向 `Product` 添加税务字段，以便从 PSM 获取：

```
...
    var tax: Decimal
...
```

这将添加一个税务字段。

要确定实际税率，值得研究像 taxjar.com 这样的服务；根据你的国家和法规，正确计算这可能需要相当多的工作。

这只是一个示例，展示如何用此功能扩展服务。接下来让我们看看添加支付方式。

# 添加支付方式

一般来说，添加支付方式并非易事。每种方法都独一无二，通常涉及几个步骤。例如，信用卡提供商采取的措施可能因所用卡提供商而异。PayPal 有自己的流程，其他提供商也是如此。一些支付方式通常需要预授权。这就是信用卡被请求但尚未扣款的时候。要在你的服务中实现这一点，你可以向 `OrderPayment` 添加一些状态，如下所示：

```
final class OrderPayment: Model, Content {
...
    // 0 = initiated
    // 1 = authorized
    // 2 = rejected
    // 3 = charged
    @Field(key: "status")
    var status: Int
...
```

当然，你还需要在其他地方修改系统，才能使其合理。

如果你想查看一个包含支付方式的完整订单系统实现，应该查看 [`github.com/SwiftCommerce/OrderSystem.`](https://github.com/SwiftCommerce/OrderSystem)

最后，让我们看看如何添加优惠券和退款。

# 添加退款和优惠券

就像其他两个主题一样，没有更多上下文就无法可靠地实现这一点。优惠券可以基于百分比或固定金额；它们也可能仅适用于某些产品。类似于我们添加税务的方式，我们也可以添加 `couponAmount`。

同样，对于退款，创建另一个 `refundAmount` 字段将使你能够记录订单中退款的金额。该交易本身在技术上可以在 `OrderPayment.swift` 中注册为负交易。

正如你所见，有很多工作可以扩展此服务并使其更易用。本节应该让你了解如何实现以及大致样子。现在让我们总结整章内容。

# 总结

你做到了！订单管理服务现在已准备就绪并可运行。你设置了模板并为该框架配置了它。然后你指定了路由。之后，你创建了所需的模型，并为路由创建了公共控制器。随后是内部控制器，我们讨论了如何实现例程。

最后，我们探讨了如何扩展服务以包含税务、添加额外支付方式，以及如何添加优惠券和退款。

你了解到 OMS 与其他两个服务不同，但学会了如何将其连接到 PMS 以及如何实现异步价格检查。

现在我们基本完成了后端服务，下一章我们将探讨 Swift 微服务开发的最佳实践。

# 问题

为加深理解，请完成以下练习：

1. 你能为这个微服务编写所有必要的测试吗？
2. 你能完成本章的税务设置并用 10% 固定税率测试吗？
3. 你能实现一个例程，取消 30 天内未支付的订单吗？
4. 你能编写一个函数，在订单发货时通知用户吗？
