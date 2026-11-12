

```
{
    Product product = dbContext.Products.FirstOrDefault(a => a.Id
    == id);
    if (product == null) return NotFound();
    return Ok(product.StockCount);
}
```

您的 StockController 现在应该如下所示：

```
[Route("api/[controller]")]
[ApiController]
public class StockController : ControllerBase
{
    private readonly IDbContext dbContext;

    public StockController(IDbContext dbContext)
    {
        this.dbContext = dbContext;
    }

    [HttpGet("{id}")]
    public int Get(int id)
    {
        Product product = dbContext.Products.FirstOrDefault(a =>
        a.Id == id);
        if (product == null) return NotFound();
        return Ok(product.StockCount);
    }
}
```

- 4. 我们现在已经将一个依赖项注入到控制器中；但是，我们需要某种东西来为我们完成注入。这时 IoC 容器就派上用场了。在 ASP.NET Core 之前，你可能曾使用过类似 Unity 的东西。如果你愿意，仍然可以使用，但 ASP.NET Core 3 自带了一个 IoC 容器：

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();
}
```

```
var connection =
@"Server=(localdb) \\MSSQLLocalDb;Database=StockCheckerDB;Trusted_Connection=True;ConnectRetryCount=0";
services.AddDbContext<StockContext>
(options => options.UseSqlServer(connection));
services.AddTransient<IDbContext, StockContext>();
}
```

有许多免费的 IoC 容器可供选择。如果你选择使用第三方容器，那么我建议你有使用它的理由；虽然目前某些选项确实提供了内置版本所没有的功能，但你应该考虑该功能是否真的是你需要的。此外，除了实验之外，我不建议你编写自己的 IoC 容器，因为这会让你未来有维护它的责任。

因此，我们现在应该能够运行此代码并获取库存数字；让我们试一下。执行 API；它应该会启动一个浏览器并显示 404 错误（没关系——基地址处没有任何内容）。导航到以下地址：
https://localhost:44371/api/stock/1

你的端口可能不同——如果不同，只需替换为你的端口即可。

你现在应该能够导航到以下地址：
https://localhost:44371/api/stock/1

浏览器应该正确返回库存数字（即 0）。

## 更新库存水平

既然我们可以读取库存水平，现在让我们添加更改它们的功能。既然检索已完成，更新就相对简单；你的 Update 方法应该类似于下面这样：

```
[HttpPut("{id}")]
public IActionResult Update(int id, [FromBody]int stockCount)
{
    Product product = dbContext.Products.FirstOrDefault(a => a.Id == id);
    if (product == null) return NotFound();
    product.StockCount = stockCount;
    dbContext.SaveChanges();
    return NoContent();
}
```

如你所见，代码与检索非常相似；我们只是在 dbContext 上更新库存计数并调用 SaveChanges()。

为了测试此功能，我们需要使用 Postman（如果你尚未安装，请参阅技术要求部分）：

![](img/762492a8287f369cc1ac4621a212f193_277_0.png)

你会发现这将项目 1 的库存水平更新为 6 个数量。你可以通过查看数据库来证明这一点，或者你可以直接导航到端点以检查库存水平（使用 Postman 或浏览器，就像你之前做的那样）。

> 你可能已经注意到，我们没有构建任何添加新产品的功能。这是有意为之；如果你希望作为扩展来做此操作，我强烈建议你考虑它是否适合放在 `Stock` 控制器中，或者是否应该单独处理。

## 权限

目前为止，一切都好。我们现在有了库存检查器应用程序的基本功能；也就是说，它检查库存水平并允许我们更新库存水平。但是，这里存在一个问题。在我们的示例中，任何用户都可以轻松更新库存水平。这可以（而且应该）在客户端上锁定；但是，我们也应该在 API 上实现此安全性；毕竟，访问 Postman，甚至网络浏览器，并非专属，而我们最不希望看到的是未经授权的人更新我们的库存水平。

让我们接入 IdentityServer 4，并确保访问 API 的人至少经过授权。

## 客户端应用程序

在我们引入 IdentityServer 以验证正确的人员和应用程序可以访问 API 之前，我们需要有一个可以说可以合法访问 API 的应用程序；否则，我们最好的办法就是完全阻止访问 API。我们的客户端应用程序将使用 **通用 Windows 平台 (UWP)** 构建。

> UWP 是 Microsoft 构建桌面应用程序的首选方法。WPF 和 WinForms 仍然受支持（如果你阅读过前面的章节，你会发现它们正在焕发新生）。但是，对于新应用程序，建议使用 UWP。事实上，XAML Islands 是连接旧技术和新技术的一种方式。

我们的应用程序将非常简单：我们只想要一个单屏，包含库存水平查找和更新库存水平的选项。

> 我从未声称自己是 UX 设计师，所以如果你觉得你能设计出更好的屏幕，那很可能是因为你能（而且更改屏幕布局不会改变任何功能）！

让我们在解决方案中创建一个新项目：

![](img/762492a8287f369cc1ac4621a212f193_278_0.png)

我们的客户端应用程序将是一个 C# UWP 应用程序：

![](img/762492a8287f369cc1ac4621a212f193_279_0.png)

在这个项目中，我们将利用 UWP 的数据绑定功能；但是，说这代表 MVVM 架构是错误的。数据绑定虽然是 MVVM 架构的重要组成部分，但与 MVVM 不等同。对于这个项目，我故意不引入任何 MVVM 框架，以演示项目是如何构建的；但是，除了提供学习机会之外，这实际上是重复造轮子。外面有几个优秀的 MVVM 框架：例如 MVVM Cross 或 MVVM Light。它们都会提供内置的命令、消息传递和依赖注入帮助程序。

UWP 允许你简单地编写事件处理程序；因此，从理论上讲，我们可以处理按钮的点击事件，检查屏幕，然后调用 API。事实上，我们的 UI 层非常小，这可能代表最佳解决方案；但是，我们将使用 UWP 提供的开箱即用的内置数据绑定。这种方法也使解决方案更具可扩展性。

让我们先创建一个 `ViewModels` 文件夹，并为我们的主视图添加一个 `ViewModel`：

```
public class MainPageViewModel : INotifyPropertyChanged
```

我们稍后将解释为什么实现这个特定的接口。ViewModel 的目的是在代码中提供视图的表示：也就是说，所有功能，但没有任何视觉元素。

> 你可以随意命名 ViewModel；但是，如果你选择使用特定的 MVVM 框架，其中一些框架采用约定，即 ViewModel 应该与 View 具有相同的词干；例如，`MainPageView`/`MainPageViewModel`。

我们在 ViewModel 中首先要声明的是我们将显示和更新的字段；在我们的案例中，我们实际上只显示两个：

```
private int _productId;
private int _quantity;
private int _originalQuantity;

public int ProductId
{
    get => _productId;
    set
    {
        if (UpdateField(ref _productId, value))
        {
            RefreshQuantity();
        }
    }
}

public int Quantity
{
    get => _quantity;
    set
    {
        if (UpdateField(ref _quantity, value))
        {
            UpdateQuantity.RaiseCanExecuteChanged();
        }
    }
}
```

