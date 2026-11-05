
## 控制 XML 序列化

在 Program.cs 中，我们添加了 XmlSerializer，以便我们的 Web API 服务在客户端请求时可以返回 XML 以及 JSON。

然而，XmlSerializer 无法序列化接口，而我们的实体类使用 ICollection<T> 来定义相关的子实体。这会在运行时导致警告，例如针对 Customer 类及其 Orders 属性，如以下输出所示：

```
warn: Microsoft.AspNetCore.Mvc.Formatters.XmlSerializerOutputFormatter[1]
An error occurred while trying to create an XmlSerializer for the type 'Packt.Shared.Customer'.
System.InvalidOperationException: There was an error reflecting type 'Packt.Shared.Customer'.
---> System.InvalidOperationException: Cannot serialize member 'Packt.Shared.Customer.Orders' of type 'System.Collections.Generic.ICollection`1[[Packt.Shared.Order, Northwind.Common.EntityModels, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null]]', see inner exception for more details.
```

我们可以通过在序列化 Customer 到 XML 时排除 Orders 属性来防止此警告：

- 1. 在 Northwind.Common.EntityModels.Sqlite 和 Northwind.Common.EntityModels.SqlServer 项目中，打开 Customers.cs。
- 2. 导入 System.Xml.Serialization 命名空间，以便我们可以使用 [XmlIgnore] 属性。
- 3. 使用属性装饰 Orders 属性以在序列化时忽略它，如以下代码中突出显示所示：
- 4. 在 Northwind.Common.EntityModels.SqlServer 项目中，也用 [XmlIgnore] 装饰 CustomerCustomerDemos 属性。

```
[InverseProperty(nameof(Order.Customer))]
[XmlIgnore]
public virtual ICollection<Order> Orders { get; set; }
```

## 记录和测试 Web 服务

你可以通过使用浏览器发出 HTTP GET 请求轻松测试 Web 服务。要测试其他 HTTP 方法，我们需要更高级的工具。

## 使用浏览器测试 GET 请求

你将使用 Chrome 测试三种 GET 请求的实现——获取所有客户、获取指定国家/地区的客户，以及使用其唯一客户 ID 获取单个客户：

- 1. 启动 Northwind.WebApi Web 服务。
- 2. 启动 Chrome。
- 3. 导航到 https://localhost:5001/api/customers 并注意返回的 JSON 文档，其中包含 Northwind 数据库中全部 91 名客户（未排序），如图 16.3 所示：
- 4. 导航到 https://localhost:5001/api/customers/?country=Germany 并注意返回的 JSON 文档，其中仅包含德国的客户，如图 16.4 所示：
- 5. 导航到 https://localhost:5001/api/customers/alfki 并注意返回的 JSON 文档，其中仅包含名为 Alfreds Futterkiste 的客户，如图 16.5 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_719_0.png)

Figure 16.3: 来自 Northwind 数据库的客户作为 JSON 文档

![](img/c63b00dca7192cc58cfb9e1f1e64f483_719_1.png)

Figure 16.4: 来自德国的客户列表作为 JSON 文档

> 如果返回空数组，请确保使用正确的大小写输入国家名称，因为数据库查询区分大小写。例如，比较 uk 和 UK 的结果。

![](img/c63b00dca7192cc58cfb9e1f1e64f483_719_2.png)

Figure 16.5: 特定客户信息作为 JSON 文档

与国家名称不同，我们无需担心客户 id 值的大小写，因为在控制器类内部，我们在代码中将字符串值规范化为大写。

但是我们如何测试其他 HTTP 方法，例如 POST、PUT 和 DELETE？又如何记录我们的 Web 服务，以便任何人都能轻松理解如何与其交互？

为了解决第一个问题，我们可以安装一个名为 **REST Client** 的 Visual Studio Code 扩展。为了解决第二个问题，我们可以使用 **Swagger**，这是世界上最流行的用于记录和测试 HTTP API 的技术。但首先，让我们看看使用 Visual Studio Code 扩展可以实现什么。

![](img/c63b00dca7192cc58cfb9e1f1e64f483_720_0.png)

> 有许多用于测试 Web API 的工具，例如 **Postman**。尽管 Postman 很受欢迎，但我更喜欢 **REST Client**，因为它不会隐藏实际发生的事情。我觉得 Postman 太 GUI 化了。但我鼓励你探索不同的工具并找到适合你风格的工具。你可以在以下链接了解更多关于 Postman 的信息：https://www.postman.com/

## 使用 REST Client 扩展测试 HTTP 请求

REST Client 是一个扩展，允许你在 Visual Studio Code 中发送任何类型的 HTTP 请求并查看响应。即使你更喜欢使用 Visual Studio 作为代码编辑器，安装 Visual Studio Code 以使用像 REST Client 这样的扩展也是有用的。

## 使用 REST Client 发出 GET 请求

我们将首先创建一个用于测试 GET 请求的文件：

- 1. 如果你尚未安装由 Huachao Mao 创建的 REST Client（humao.rest-client），请现在在 Visual Studio Code 中安装它。
- 2. 在你喜欢的代码编辑器中，启动 Northwind.WebApi 项目 Web 服务。
- 3. 在 Visual Studio Code 中，在 PracticalApps 文件夹中创建 RestClientTests 文件夹，然后打开该文件夹。
- 4. 在 RestClientTests 文件夹中，创建一个名为 get-customers.http 的文件，并修改其内容以包含用于检索所有客户的 HTTP GET 请求，如以下代码所示：
- 5. 在 Visual Studio Code 中，导航到 View | Command Palette，输入 rest client，选择命令 Rest Client: Send Request，然后按 Enter，如图 16.6 所示：

```
GET https://localhost:5001/api/customers/ HTTP/1.1
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_721_0.png)

Figure 16.6: 使用 REST Client 发送 HTTP GET 请求

6. 注意 **Response** 在新的选项卡窗口窗格中垂直显示，并且你可以通过拖放选项卡将打开的选项卡重新排列为水平布局。

7. 输入更多 GET 请求，每个请求由三个井号分隔，以测试获取不同国家的客户以及使用其 ID 获取单个客户，如以下代码所示：

```
###
GET https://localhost:5001/api/customers/?country=Germany HTTP/1.1
###
GET https://localhost:5001/api/customers/?country=USA HTTP/1.1
Accept: application/xml
###
GET https://localhost:5001/api/customers/ALFKI HTTP/1.1
###
GET https://localhost:5001/api/customers/abcxy HTTP/1.1
```

8. 点击每个请求上方的 **Send Request** 链接以发送它；例如，GET 请求带有请求头以 XML 而非 JSON 请求美国的客户，如图 16.7 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_721_1.png)

Figure 16.7: 使用 REST Client 发送 XML 请求并获取响应

## 使用 REST Client 发出其他请求

接下来，我们将创建一个用于测试其他请求（如 POST）的文件：

- 1. 在 RestClientTests 文件夹中，创建一个名为 create-customer.http 的文件，并修改其内容以定义用于创建新客户的 POST 请求，注意在输入常见 HTTP 请求时 REST Client 会提供 IntelliSense，如以下代码所示：

```
POST https://localhost:5001/api/customers/ HTTP/1.1
Content-Type: application/json
Content-Length: 301

{
  "customerID": "ABCXY",
  "companyName": "ABC Corp",
  "contactName": "John Smith",
  "contactTitle": "Sir",
  "address": "Main Street",
  "city": "New York",
  "region": "NY",
  "postalCode": "90210",
  "country": "USA",
  "phone": "(123) 555-1234",
  "fax": null,
  "orders": null
}
```

- 2. 由于不同操作系统中的换行符不同，Content-Length 头的值在 Windows 和 macOS 或 Linux 上会不同。如果值错误，则请求将失败。要发现正确的正文长度，请选择请求正文，然后在状态栏中查看字符数，如图 16.8 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_722_0.png)

Figure 16.8: 检查正确的正文长度

3. 发送请求并注意响应为 201 Created。还请注意新创建的客户的位置（即 URL）为 https://localhost:5001/api/Customers/abcxy，并且响应正文中包含新创建的客户，如图 16.9 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_723_0.png)

Figure 16.9: 添加新客户

我将给你一个可选挑战，创建 REST Client 文件来测试更新客户（使用 PUT）和删除客户（使用 DELETE）。请在已存在的客户以及不存在的客户上尝试。解决方案在本书的 GitHub 仓库中。

现在我们已经看到了快速简便的测试服务的方法，这也恰好是学习 HTTP 的好方法，那么外部开发者呢？我们希望他们尽可能轻松地学习然后调用我们的服务。为此，我们将使用 Swagger。

## 了解 Swagger

Swagger 最重要的部分是 OpenAPI Specification，它为你的 API 定义了 REST 风格的契约，以人类可读和机器可读的格式详细说明其所有资源和操作，便于开发、发现和集成。

开发者可以使用 Web API 的 OpenAPI Specification 自动生成其首选语言或库中的强类型客户端代码。

对我们来说，另一个有用的功能是 Swagger UI，因为它会自动为你的 API 生成文档，并内置可视化测试功能。

让我们回顾一下如何使用 Swashbuckle 包为我们的 Web 服务启用 Swagger：

- 1. 如果 Web 服务正在运行，请关闭 Web 服务器。
- 2. 打开 Northwind.WebApi.csproj 并注意 Swashbuckle.AspNetCore 的包引用，如以下标记所示：

```
<ItemGroup>
    <PackageReference Include="Swashbuckle.AspNetCore" Version="6.1.5" />
</ItemGroup>
```
