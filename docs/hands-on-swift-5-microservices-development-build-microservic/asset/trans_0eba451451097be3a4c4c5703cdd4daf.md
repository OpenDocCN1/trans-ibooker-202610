

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

