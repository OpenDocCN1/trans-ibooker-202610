# 产品管理服务

好的——您已经在第 7 章《编写用户服务》中编写了第一个微服务，并在第 8 章《测试微服务》中学习了如何测试微服务。现在，我们将编写下一个微服务：**产品管理服务 (PMS)**！

PMS 将是我们设置中的重要服务，但它将与**用户管理服务 (UMS)** 截然不同。在本章中，您将学习如何编写验证身份并提供有限管理员管理功能的微服务。这是我们示例商店应用程序中的第二个微服务，将使我们更接近让应用程序完全运行。它还将向您展示微服务之间交互的另一种方式，因为该服务比 UMS 简单。在本章结束时，您将构建第二个微服务，它在应用程序上下文中交互，并作为产品的内容提供者。

在本章中，我们将涵盖以下部分：

* 设置项目
* 开发路由
* 编写所需的模型
* 编写所需的控制器
* 测试服务

让我们开始吧！

# 技术要求

本节介绍您的机器上需要运行的内容，以便您编写此微服务。需要安装以下组件：

* Vapor Toolbox
* Swift 5.2+
* Xcode 11+
* macOS/Linux

我们还假设您已经学习了前几章，并知道如何编写 Vapor 微服务。创建此微服务的过程将侧重于*新*元素，而不包括第 7 章《编写用户服务》中涵盖的内容。本章中的所有代码均可在 macOS 和 Linux 上运行。

本章的 GitHub 地址是 [`github.com/PacktPublishing/Hands-On-Swift-5-Microservices-Development/tree/master/Chapter 9/ProductService`](https://github.com/PacktPublishing/Hands-On-Swift-5-Microservices-Development/tree/master/Chapter%209/ProductService)。

现在，让我们开始设置项目。

# 设置项目

在本节中，您将了解在开始编写主要元素之前如何设置此服务。设置此服务比之前的服务 UMS 容易得多。我们不发送电子邮件，也不签发 **JSON Web Tokens (JWT)**。请记住，JWT 由我们的 UMS 签发，因此我们只需验证它们是否正确签发且未被篡改。我们需要一个数据库来存储产品和类别信息，这将像以前一样使用 MySQL。

我们基本上将执行以下操作：

* 管理代表产品和类别的模型。
* 验证用户是否有权修改产品或类别。

本节涵盖以下内容：

* 设置模板
* 设置 JWT 验证
* 设置数据库

让我们从模板开始。

## 设置模板

让我们开始设置在第 5 章《创建您的第一个微服务》中创建的模板。我们不需要重写之前创建的内容，只需将其作为该服务的基础。请遵循以下步骤：

1. 进入产品管理器的文件夹，如下所示：

```
$ cd ~/Shop\ Backend/ProductService
```

2. 现在，将模板从原始文件夹复制到我们的新文件夹：

```
$ cp -r ../template/. . && rm -rf .build
```

3. 现在，打开 `Package.swift` 文件，如下所示：

```
$ open Package.swift
```

4. 将名称更改为 `ProductService`。

5. 现在，安装所有依赖并构建项目，如下所示：

```
$ swift package update
```

6. 在 Xcode 中打开项目，如下所示：

```
$ xed .
```

现在我们已经设置好模板，可以开始使用它了。PMS 不需要任何额外的包。我们确实想要验证某些请求，以允许管理员用户管理产品，所以让我们继续设置 JWT 验证。

## 设置 JWT 验证

得益于我们随模板安装的 JWT 包，我们在这里不需要做太多工作。为了方便，让我们创建一个设置文件，就像我们在第 7 章《编写用户服务》中所做的那样。为此，请遵循以下步骤：

1. 在文件夹中创建一个设置文件，如下所示：

```
$ open .env
```

2. 打开您的 **JSON Web Key Set (JWKS)** 文件，该文件应保存在项目文件夹上方的文件夹中。

3. 将整个 JWKS 内容复制到一行并粘贴到 `.env` 文件中，如下所示：

```
JWKS='<copy the value from the jwks>'
```

就这样。我们不需要私钥，因为我们只是验证 JWT。每当有请求进来并需要验证时，`JWTMiddleware` 将自动使用公钥检查 JWT 负载是否正确签名。如果您需要熟悉这一点，可以回顾第 3 章《Vapor 框架入门》。

现在我们已经设置好 JWT，让我们配置数据库。

## 设置数据库

继续在您的本地数据库设置中创建一个新用户和一个数据库。如果您需要提醒如何操作，请查看第 3 章《Vapor 框架入门》。将用户命名为 `service_products`，数据库也应命名为 `service_products`。

要设置数据库，我们只需执行以下操作：

将以下行添加到 `.env` 文件中：

```
MYSQL_CRED='mysql://service_products:password@localhost:3306/service_products?ssl=false'
```

请确保您输入了正确的密码。

我们现在已经完成了所有设置：模板、JWT 和数据库。这完成了我们的准备工作，以便我们现在可以着手处理路由。

# 开发路由



这些服务需要通过 URL 访问，就像用户服务一样。请记住，Vapor 解决此问题的方式是使用路由器（`Router`）。路由器会将传入的请求 URL 与我们告诉它要做的事情进行比较，并执行所需的函数。我们需要为每个作为 **Representational State Transfer** (**REST**) API 的服务执行此操作。

对于此服务，我们将需要涵盖以下功能的路由：

*   获取分类
*   获取产品
*   更新、创建和删除产品
*   更新、创建和删除分类

最后两项功能（更新、创建和删除产品及分类）当然是为管理员保留的，并需要具有正确用户级别的 JWT 验证。前两项功能（获取/读取分类和产品）则面向公众开放。

请注意，对于前两项功能，我们还需要以下函数。

对于分类：

*   基于名称和 ID 进行筛选
*   分页：每页仅 10 个分类

对于产品：

*   基于名称、分类、价格和 ID 进行筛选
*   分页：每页仅 10 个产品

你可能会想知道为什么需要分页。即使我们最初只提供少量的分类和产品，我们仍然希望能够在需要时支持数千个条目。我们通过从不交付完整列表，而只交付其中的一部分来实现这一点。这是一个好的实践，因为它为未来做好了充分准备。

路由列表见下表：

| **路径** | **方法** | **参数** | **输出/描述** |
| --- | --- | --- | --- |
| `/*/products/categories` | `GET` | 选项：`name`、`id`、`page` | 返回分类列表，受页面和筛选设置限制。 |
| `/*/products/categories` (Auth Required) | `POST` | Name | 创建新分类。 |
| `/*/products/categories/:id` (Auth Required) | `DELETE` | (none) | 删除由 ID 标识的分类。 |
| `/*/products/categories/:id` (Auth Required) | `PATCH` | Name | 基于 ID 更新分类并设置名称。 |
| `/*/products` | `GET` | 选项：`categoryId`、`name`、`id`、`page` | 返回与筛选条件匹配的所有产品列表。 |
| `/*/products` | `POST` | Name Description Category ID Price | 创建新产品。 |
| `/*/products/:id` | `PATCH` | Name Description Category ID Price | 使用给定信息更新位于该 ID 下的产品。 |
| `/*/products/:id` | `DELETE` | (none) | 删除该 ID 下的产品。 |

请注意，与我们之前的服务相比，路由结构简单得多。

为了使路由生效，我们需要创建控制器并正确链接它们。请执行以下步骤：

1.  进入 `Sources/App/Controllers` 文件夹。
2.  创建 `ProductsController.swift` 文件。
3.  创建 `CategoriesController.swift` 文件。
4.  打开 `Routes.swift` 并输入以下内容：

```swift
import Fluent
import Vapor
import SimpleJWTMiddleware

func routes(_ app: Application) throws {
    let root = app.grouped(.anything, "users")
    let authorized = root.grouped(SimpleJWTMiddleware())

    root.get("health") { request in
        return "All good!"
    }
    
    let productsController = ProductsController()
    let categoriesController = CategoriesController()
    
    root.get("categories", use: categoriesController.get)
    authorized.post("categories", use: categoriesController.new)
    authorized.patch("categories/:id", use: categoriesController.edit)
    authorized.delete("categories/:id", use: categoriesController.delete)
    
    root.get("", use: productsController.get)
    authorized.post("", use: productsController.new)
    authorized.patch(":id", use: productsController.edit)
    authorized.delete(":id", use: productsController.delete)
}
```

让我们快速浏览一下该函数。

我们正在创建控制器的实例。与 UMS 不同，我们不仅将控制器作为 `RouteCollection` 传递给 `Router`，而是自己定义路由。原因是我们有一些路由需要认证，而另一些则不需要。

接下来，我们定义了前表中的所有路由。需要用户权限的路由使用 `authorized` 变量，强制请求通过我们的 `SimpleJWTMiddleware`。

现在，我们已经声明了路由。用户现在可以调用这些路由，我们的应用程序至少知道它们的存在，并知道在技术上将它们转发到何处。在我们能够编写控制器中的逻辑之前，让我们先看看我们需要的模型。

## 编写所需模型

正如我们在 Chapter 2 *Understanding Server-Side Swift* 中讨论的那样，我们处理两种类型的模型：数据库模型和 **input/output** (**I/O**) 模型。你现在将创建这两种模型，并学习如何将它们连接到 Fluent。

此 PMS 服务包含两个模型：

*   Products（产品）
*   Categories（分类）

产品属于分类，但在该（简化）服务中，产品一次只能属于一个分类。如果你有足够的信心，欢迎扩展此功能，使产品可以属于多个分类。

在接下来的两节中，我们将执行以下操作：

*   创建数据库模型：存储数据库中信息所需的模型。
*   创建 I/O 模型：接收数据或向客户端返回数据所需的模型。
*   创建载荷模型：处理 JWT 所需的模型。

让我们先创建数据库模型。

## 创建数据库模型

要创建数据库模型，请执行以下步骤：

1.  在 `Sources/App/Models/Database` 中创建 `Category.swift` 文件。
2.  插入以下代码：

```swift
import Vapor
import Fluent

final class Category: Model {
    
    static let schema = "categories"
    
    @ID(key: "id")
    var id: Int?
    
    @Field(key: "name")
    var name: String
    
    init() {
    }
    
    init(name: String) {
        self.name = name
    }
}
```

你可以看到，这个类非常简单，仅包含分类的名称和 ID。

3.  在 `Sources/App/Models/Database` 中创建 `Product.swift` 文件。
4.  插入以下代码：

```swift
import Vapor
import Fluent

final class Product: Model {
    
    static let schema = "products"
    
    @ID(key: "id")
    var id: Int?
    
    @Field(key: "name")
    var name: String
    
    @Field(key: "description")
    var description: String
    
    @Field(key: "price")
    var price: Int
    
    @Field(key: "categoryId")
    var categoryId: Int
    
    @Timestamp(key: "createdAt", on: .create)
    var createdAt: Date?
    
    @Timestamp(key: "updatedAt", on: .update)
    var updatedAt: Date?
    
    @Timestamp(key: "deletedAt", on: .delete)
    var deletedAt: Date?
    
    init() {
    }
    
    init(name: String, description: String, price: Int, categoryId: Int) {
        self.name = name
        self.description = description
        self.price = price
        self.categoryId = categoryId
    }
}
```

产品也保持简单。在实际服务中，我们会添加更多属性，但对于此示例，我们涵盖了所有所需内容。

请注意，我们将 `price` 存储为 `Int`。原因在于，虽然 `price` 看起来像 `Float` 或 `Double` 更直观，但现实是货币永远不会小于分（0.01）。因此，以最小可能单位存储有助于防止保存到数据库时的舍入问题。此外，请注意我们没有定义货币；目前我们假设为 USD。存储 `Double` 或 `Float` 可能比较棘手；JSON、系统和数据库在如何解释数字方面并不总是完全一致。一般来说，存储为 `Int` 或 `String` 更安全。



对于最后一个函数，我们将 `categoryId` 连接到数据库中的 `Category` 表。这将防止我们拥有没有分类的产品。

现在我们已经创建了数据库模型，让我们看看如何将它们与我们的对象模型一起使用。

# 创建 I/O 模型

I/O 模型与数据库模型一样简单。请按以下步骤操作：

1. 在 `Sources/App/Models/Inputs` 中创建 `CategoryInput.swift` 文件。

2. 在该文件中输入以下内容：

```swift
import Vapor

struct CategoryInput: Content {
    let name: String
}
```

3. 现在在 `Sources/App/Models/Inputs` 中创建 `ProductInput.swift` 文件，内容如下：

```swift
import Vapor

struct ProductInput: Content {
    let name: String
    let description: String
    let price: Int
    let categoryId: Int
}
```

我们只需要两种输入：`Categories` 和 `Products`，即我们正在处理的两个数据库模型。无论它们是新建还是更新，都不会影响这里的模型，所以总共两个模型就足够了。注意，我们从模型中省略了 `ID`，因为它要么不存在（新模型），要么将通过 URL 传递（请参阅前面的 *Developing routes* 部分）。

4. 在 `Sources/App/Models/Responses` 中创建 `CategoryResponse.swift` 文件。

5. 在该文件中输入以下内容：

```swift
import Vapor

struct CategoryResponse: Content {
    let id: Int
    let name: String
}
```

6. 在 `Sources/App/Models/Response` 中创建 `ProductResponse.swift` 文件，内容如下：

```swift
import Vapor

struct ProductResponse: Content {
    let id: Int
    let name: String
    let description: String
    let price: Int
}
```

在这里，我们也只需要两种响应：一种用于分类，一种用于产品。这两种响应都包含其类中所有的变量。

设置完成后，我们只需在配置中注册模型，如下所示：

7. 打开 `Sources/App/` 中的 `Configure.swift`。

8. 调整文件以反映以下内容：

```swift
    app.migrations.add(CreateCategory())
    app.migrations.add(CreateProduct())
```

9. 最后，我们需要创建刚才引用的迁移。在 `Sources/App/Migrations` 中创建 `CreateCategory.swift` 文件，内容如下：

```swift
import Fluent

struct CreateCategory: Migration {
    func prepare(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("categories")
            .field("id", .int, .identifier(auto: true))
            .field("name", .string, .required)
            .create()
    }

    func revert(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("categories").delete()
    }
}
```

10. 本节的最后一个文件是 `Sources/App/Migrations` 中的 `CreateProduct.swift`，内容如下：

```swift
import Fluent

struct CreateProduct: Migration {
    func prepare(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("products")
            .field("id", .int, .identifier(auto: true))
            .field("name", .string, .required)
            .field("description", .string, .required)
            .field("price", .int, .required)
            .field("categoryId", .int, .required)
            .field("createdAt", .datetime)
            .field("updatedAt", .datetime)
            .field("deletedAt", .datetime)
            .create()
    }

    func revert(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("products").delete()
    }
}
```

这是数据库模型所需的全部内容。接下来让我们处理载荷模型。

# 载荷模型

载荷模型是从我们可能接收到的 JWT 生成的模型。该模型随 `SimpleJWTMiddleware` 一起提供，因此我们不需要在此自行定义。该代码涵盖的字段与我们来自 第 7 章 *Writing the User Service* 的 UMS 的载荷相同。自然，其他服务创建的 JWT 需要与我们在这里接收到的内容相对应。然后 `SimpleJWTMiddleware` 将调用 `verify` 函数。因为我们在该服务中仅需要管理员的 JWT，所以我们检查用户的状态，如果不是 1（即管理员用户），则会抛出错误。这有效地防止了非管理员用户修改产品。

就这样！我们已经创建了该服务所需的所有模型——包括数据库模型和 I/O 模型。现在，我们可以开始编写所需的控制器。由于控制器依赖于模型，它们是最后编写的部分。

# 编写所需的控制器

控制器是 Vapor 应用程序的逻辑部分。控制器将输入转换为输出。我们刚刚定义了 I/O 模型和数据库模型，因此我们拥有编写控制器所需的一切。

我们已经创建了所需的两个控制器文件。现在，让我们逐个处理它们，如下所示：

- 编写 `CategoriesController`：该控制器将管理分类。
- 编写 `ProductsController`：该控制器将管理产品。

我们将从 `CategoriesController` 开始。

# 编写 CategoriesController

请按以下步骤编写我们的 `CategoriesController`：

1. 打开 `Sources/App/Controllers` 中的 `CategoriesController.swift`。

2. 编写以下代码：

```swift
import Fluent
import Vapor

final class CategoriesController {
}
```

由于此控制器不自行注册路由，因此它不需要是 `RouteCollection` 实例，所以我们只使用一个简单的类。

有些人更喜欢使用 `structs` 而不是 `classes`。事实上，对于这种类型的控制器，这不会有太大区别，但你可能想了解更多关于 `classes` 和 `structs` 的知识。

现在让我们填写路由的函数。

3. 在类中输入以下函数：

```swift
func get(_ request: Request) throws -> EventLoopFuture<[CategoryResponse]> {
    return Category.query(on: request.db).all().map { cats in
        return cats.map { CategoryResponse(id: $0.id!, name: $0.name) }
    }
}
```

我们只是从数据库加载所有 `Category` 条目，将其转换为我们的响应模型，然后返回给请求者。不需要过滤器或其他操作。

4. 在类中输入以下代码作为 `new` 函数：

```swift
func new(_ request: Request) throws -> EventLoopFuture<CategoryResponse> {
    let input = try request.content.decode(CategoryInput.self)
    let category = Category(name: input.name)
    return category.save(on: request.db).map { _ in
        return CategoryResponse(id: category.id!, name: category.name)
    }
}
```

这对你来说应该不陌生，因为我们在 第 7 章 *Writing the User Service* 中编写过类似的代码。我们创建一个新的 `Category` 实例，然后保存它并将其作为 `CategoryResponse` 返回。

5. 现在让我们处理 `edit` 函数。输入以下代码：

```swift
func edit(_ request: Request) throws -> EventLoopFuture<CategoryResponse> {
    let id = try request.query.get(Int?.self)
    let input = try request.content.decode(CategoryInput.self)
    
    return Category.query(on: request.db).filter(\.$id == id).all().flatMap { categories in
        if categories.count == 0 {
            return request.eventLoop.makeFailedFuture(Abort(.badRequest, reason: "No product. found!"))
        }
        let category = categories.first!
        
        category.name = input.name
        
        return category.save(on: request.db).map { _ in
            return CategoryResponse(id: category.id!, name: category.name)
        }
    }
}
```



在此函数中，我们现在首先获取 `id` 参数。利用它，我们可以获取正确的分类。请注意，我们要确保实际找到了该分类，否则返回错误。随后，我们设置分类名称，保存它，并返回一个新的 `CategoryResponse`。

6. 通过添加以下代码来添加 `delete` 函数：

```
func delete(_ request: Request)throws -> EventLoopFuture<HTTPStatus> {
        let id = try request.query.get(Int.self)
        
        return Category.query(on: request.db).filter(\.$id ==
         id).delete().map { _ in
            return .ok
        }
    }
```

这里，我们再次需要 `id`，然后使用 `Fluent` 的 `delete` 函数删除数据库中的条目。我们在这里只返回一个状态码，因为条目已被删除，返回 `CategoryResponse` 将毫无意义。

分类所需的一切就这些。

# 编写 ProductsController

`ProductsController` 在本质上与 `CategoriesController` 相似，但拥有一个截然不同的函数来返回产品列表。我们希望能够根据产品的名称、分类或 `ID` 来筛选产品。但让我们先从基础开始，如下所示：

1. 打开 `Sources/App/Controllers` 中的 `ProductsController.swift`。

2. 将空类输入到该文件中，如下所示：

```
import Fluent
import Vapor

final class ProductsController {
}
```

到目前为止一切顺利。现在，让我们开始编写主函数。

3. 将以下函数输入到类中：

```
func get(_ request: Request)throws -> EventLoopFuture<[ProductResponse]> {
        let querybuilder = Product.query(on: request.db)
        
        return querybuilder.all().map { products in
            return products.map { ProductResponse(id: $0.id!,
             name: $0.name, description: $0.description,
             price: $0.price) }
        }
    }
```

这对你来说应该很眼熟。到目前为止，这与我们所做的一切唯一的区别在于，我们在使用之前将 `QueryBuilder` 实例（`Product.query(on: request)` 返回该实例）保存在一个变量中。

4. 在创建 `querybuilder` 实例后添加以下行：

```
if let categoryId = try request.query.get(Int?.self,
 at: "categoryId") {
            querybuilder.filter(\.$categoryId == categoryId)
        }
```

我们检查是否存在 `categoryId` 参数（`GET` 参数），如果存在，我们向 `QueryBuilder` 添加一个过滤器。现在，如果存在 `categoryId` 参数，我们将只查找该分类 ID 下的产品。

5. 在前面的代码后添加以下行：

```
if let query = try request.query.get(String?.self, at: "query") {
            querybuilder.group(.or) {
                $0.filter(\Product.$name ~~ query)
                    .filter(\Product.$description ~~ query)
            }
        }
```

这里，我们检查是否有 `query` 参数，如果有，我们按名称筛选。如果你不熟悉 `SQL`，我们本质上是让数据库搜索所有名称中包含 `query` 的模型。

6. 最后，在前面的代码后添加以下行：

```
if let idsString = try request.query.get(String?.self, at: "ids") {
            let ids:[Int?] = idsString.split(separator: ",").map {
             Int(String($0)) }
            querybuilder.filter(\.$id.field ~~ ids)
        }
```

这段代码使用了 `MySQL` 的 `IN` 函数。该过滤器将只返回 ID 在所提供数组中的产品。对于该数组，我们使用 `ids` 参数，并在每次出现逗号时进行分割以将其转换为数组。

你可能会想知道为什么我们不直接请求单个 `ID`。我们可以，但稍后我们将在另一个微服务中需要此功能。每当你想要验证或检查多个产品时，在一次调用中完成非常有帮助，而不是进行多次调用。

现在让我们处理其他创建、读取、更新和删除（`CRUD`）函数。

7. 添加 `new` 函数，如下所示：

```
func new(_ request: Request)throws -> EventLoopFuture<ProductResponse> {
      let input = try request.content.decode(ProductInput.self)
      let product = Product(name: input.name,
       description: input.description, price: input.price,
       categoryId: input.categoryId)
      return product.save(on: request.db).map { _ in
          return ProductResponse(id: product.id!,
           name: product.name, description: product.description,
           price: product.price)
      }
  }
```

这与前面分类的 `new` 函数非常相似。我们获取所有参数，将它们传入 `Fluent` 模型，让 `Fluent` 保存模型，并将其作为 `ProductResponse` 返回。

8. 同样地，添加 `edit` 函数，如下所示：

```
func edit(_ request: Request)throws -> EventLoopFuture<ProductResponse> {
      let input = try request.content.decode(ProductInput.self)
      let id = try request.query.get(Int?.self)
        
      return Product.query(on: request.db).filter(\.$id ==
       id).all().flatMap { products in
          if products.count == 0 {
              return request.eventLoop.makeFailedFuture(
               Abort(.badRequest, reason: "No product. found!"))
          }
          let product = products.first!
            
          product.name = input.name
          product.description = input.description
          product.price = input.price
          product.categoryId = input.categoryId
            
          return product.save(on: request.db).map { _ in
            return ProductResponse(id: product.id!,
             name: product.name, description: product.description,
             price: product.price)
          }
      }
  }
```

此时没有什么应该让你感到惊讶的。我们正在用新信息更新产品并返回更新后的模型。

9. 最后但同样重要的是，添加 `delete` 函数，如下所示：

```
func delete(_ request: Request)throws -> EventLoopFuture<HTTPStatus> {
        let id = try request.query.get(Int?.self)
        
        return Product.query(on: request.db).filter(\.$id ==
         id).delete().map { _ in {
            return .ok
        }
    }
}
```

同样，这与我们用于分类的函数几乎相同，但这没关系：代码简单并没有错，特别是微服务依靠简洁性蓬勃发展。

好了——控制器已准备就绪，这完成了我们服务在逻辑和模型方面的编写！现在，我们想应用关于编写测试的知识来确保其正常工作。

# 测试服务

测试此服务相对简单。我们可以跳过单元测试，因为我们没有编写适合单元测试的函数。编写一个 `Postman` 测试套件来测试 `API` 端点更有意义。在 第 8 章 *Testing Microservices*（测试微服务）中，你学习了如何做到这一点。所以，让我们在这里应用它，如下所示：

1. 打开 `Postman` 并创建一个新的测试套件。

2. 设置身份验证，以便我们也可以测试管理员功能。

3. 遍历前面的路由，并为每一个添加一个测试。

4. 让 `Postman` 测试此服务并验证其是否按预期工作。

你可以在本章的 `GitHub` 存储库中找到完整的测试套件：[`github.com/PacktPublishing/Hands-On-Microservices-with-Swift-5/tree/master/Chapter 9/ProductService`](https://github.com/PacktPublishing/Hands-On-Microservices-with-Swift-5/tree/master/Chapter%209/ProductService)

现在我们已经测试了此服务，让我们总结一下我们学到了什么。

# 总结

在本章中，我们为示例应用程序编写了第二个微服务。与第一个服务不同，我们仅验证 `JWT` 而不创建它。你已经确定了所需的路由。然后，你创建了 `I/O` 模型以及数据库模型。随后，你为我们的路由编写了相应的控制器并开发了所需的函数。最后，你为此服务编写了功能测试并验证了其功能。



