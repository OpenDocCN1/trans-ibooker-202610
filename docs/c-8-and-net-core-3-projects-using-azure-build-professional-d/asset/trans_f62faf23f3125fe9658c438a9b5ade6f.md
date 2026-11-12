

## 创建控制器

控制器是构建 ASP.NET Core MVC 应用程序的基本构建块。控制器内部的方法称为操作。因此，我们可以说控制器定义了一组操作。操作处理请求，这些请求通过路由映射到特定的操作。

有关控制器和操作主题的更多信息，请参阅 https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/actions 上的 Microsoft 文档。有关路由的更多信息，请参阅 https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/routing 上的 Microsoft 文档。

按照以下步骤操作：

- 1. 右键单击 Controller 文件夹，然后选择“添加 | 控制器”。
- 2. 在脚手架屏幕上，选择“使用 Entity Framework 的带视图的 MVC 控制器”，然后单击“添加”：

![](img/762492a8287f369cc1ac4621a212f193_183_0.png)

- 3. 在下一屏幕上，为 **模型类** 选择我们的 **Research** 模型，为 **数据上下文类** 选择 **ResearchContext**。除非您想更改 **控制器名称**，否则其余部分保持不变：

![](img/762492a8287f369cc1ac4621a212f193_184_0.png)

简单查看创建的控制器，我们现在已具备基本的 **创建、读取、更新和删除 (CRUD)** 任务。现在，是主环节的时候了。

## 运行应用程序

在运行应用程序之前，让我们确保新页面易于访问。最简单的方法是将其设置为默认主页：

- 1. 查看 `Startup.cs` 中的 `Configure` 方法。您会注意到默认路由指定为 `Home` 控制器。

- 2. 只需将控制器更改为您的 `Research` 控制器，如下所示：

```
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Research}/{action=Index}/{id?}");
    endpoints.MapRazorPages();
});
```

如果您熟悉 ASP.NET 2.x，您可能会期望在此处看到 UseMvc。事实上，UseMvc 已弃用，转而支持手动构建您所需的 MVC 中间件。

- 3. 最后，确保您的 Main 方法如下所示：

```
public static void Main(string[] args)
{
    var host = BuildWebHost(args);
    using (var scope = host.Services.CreateScope())
    {
        var services = scope.ServiceProvider;
        try
        {
            var context = services.GetRequiredService<ResearchContext>();
            DbInitializer.Initialize(context);
        }
        catch (Exception ex)
        {
            var logger =
            services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred while seeding the database.");
        }
    }
    host.Run();
}
```

- 4. 现在，按 Ctrl + F5 运行应用程序，查看您的成果：

![](img/762492a8287f369cc1ac4621a212f193_185_0.png)

- 5. 如您所见，我们的测试条目可供我们使用。让我们快速查看可用功能：
- 6. 单击 **创建新项** 以查看我们链接的条目表单：

![](img/762492a8287f369cc1ac4621a212f193_186_0.png)

- 7. 输入一些有趣的数据，然后单击 **创建** 按钮。您将被重定向到列表视图，并看到新条目已添加到列表底部：

![](img/762492a8287f369cc1ac4621a212f193_187_0.png)

每个项目旁边，您可以选择 **编辑**、查看 **详细信息** 或 **删除**。大胆尝试功能。我们可以做很多事情来改善用户体验，例如自动填充日期字段。我将把改善用户体验的创意留给您，按您觉得合适的方式进行。

## 测试应用程序

如果您选择扩展此项目的功能，您可能想要添加的一项内容是一些自动化测试。先厘清一些术语：本节涉及自动化测试，但不涉及单元测试。关于什么是测试，很容易产生争论和讨论，但出于本节的目的，我们正在添加自动化测试。

由于此应用程序基本上是一个基本的数据访问应用程序，自动化测试常常被省略。当然可以争辩说，由于逻辑很少，您实际上是在测试 EF Core。不测试基本 CRUD 应用程序的另一个主要原因一直是其难度。测试以数据库为中心的应用程序一直是一项困难、耗时且感恩不尽的任务。

首先，许多单元测试纯粹主义者会争辩说，您无法在测试中包含数据访问，因此应该模拟数据访问。这通常通过创建存储库模式来抽象数据访问，甚至通过多层抽象来完成，但一旦我们这样做，就必须模拟对数据库的调用将返回的内容，尽管这符合单元测试的原则，但并不能让我们太放心我们的应用程序能够正常工作。

其次，数据库操作通常不具备幂等性；也就是说，一旦在真实数据库上运行过测试，再次运行通常会产生不同的结果，因为数据已发生变化。在过去，人们通过每次重新创建数据库、编写脚本回滚操作或在事务中运行测试然后回滚来绕过这一问题。所有这些方法的共同点是它们都非常缓慢。

Entity Framework 本身就是一种存储库模式，您当然可以抽象掉 `DbContext`；但是，使用 EF Core 时，您有一个称为 **内存数据库** 的概念。已经有许多开源项目试图通过将数据更改保存在字典或类似结构中，为 EF6 提供此功能。

让我们创建几个自动化测试（我不会将它们称为单元测试，因为它们显然不测试单个功能单元）：

- 1. 第一步是创建一个 `Test` 项目：

![](img/762492a8287f369cc1ac4621a212f193_188_0.png)

目前我们将项目创建为类库。对于这个特定的测试，我们将使用 XUnit，但您应该能够毫无困难地将其替换为任何主要测试库。

- 2. 项目创建完成后，可以使用包管理器控制台安装测试库（确保选择了正确的 Test 项目）：

```
Install-Package XUnit -ProjectName WebResearch.Test
```

我们还需要一些其他库：

```
Install-Package Microsoft.EntityFrameworkCore -ProjectName WebResearch.Test Install-Package XUnit.Runner.VisualStudio -ProjectName WebResearch.Test
```

- 3. 接下来，我们需要添加对要测试的项目的引用：

![](img/762492a8287f369cc1ac4621a212f193_189_0.png)

因为控制器是数据访问前面的逻辑，让我们测试 ResearchController。在测试库中创建（或重命名）类为 ResearchControllerTests：

![](img/762492a8287f369cc1ac4621a212f193_189_1.png)

- 4. 最后，我们需要向测试项目添加一个 NuGet 包，以便我们可以使用内存数据库：

```
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

我们将在本章后面讨论这究竟是什么。

让我们来看看测试的代码。然后，我们可以逐步了解它的作用：

```
[Fact]
public async Task RetrieveDetails_DetailsCorrect()
{
    // Arrange
    var testUrl = "www.pichaels.net";
    var options = new DbContextOptionsBuilder<ResearchContext>()
        .UseInMemoryDatabase(Guid.NewGuid().ToString())
        .EnableSensitiveDataLogging()
        .Options;
    var researchContext = new ResearchContext(options);
    var researchController = new ResearchController(researchContext);
    var research = new Research()
    {
        Id = 1,
        DateSaved = new DateTime(2018, 10, 24),
        Note = "Useful site for programming and tech information",
        Read = false,
        Url = testUrl
    };
    var createResult = await researchController.Create(research);
    // Act
    var detailsResult = await researchController.Details(1);
    // Assert
    var viewResult = (ViewResult)detailsResult;
    var resultsModel = (Research)viewResult.Model;
    Assert.Equal(testUrl, resultsModel.Url);
}
```

