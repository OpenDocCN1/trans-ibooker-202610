
只需知道，当你在代码中设置绑定时，第一个参数几乎总是要求你指定需要绑定的类名（此处是 Label），随后调用带 Property 后缀的基础属性。无论如何，运行应用程序可以看到 Label 只打印出整数。

## 构建 DataGrid 选项卡

前面的数据绑定示例演示了如何配置两个（或更多）控件参与数据绑定操作。虽然这很有用，但也可以绑定来自 XML 文件、数据库数据以及内存对象的数据。为了完成本示例，你将设计选项卡控件的最后一个选项卡，使其显示从 AutoLot 数据库的 Inventory 表获取的数据。

与其他选项卡一样，你首先将当前的 Grid 改为 StackPanel。为此，请使用 Visual Studio 直接更新 XAML。然后在新 StackPanel 中定义一个名为 gridInventory 的 DataGrid 控件，如下所示：

```
<TabItem x:Name="tabDataGrid" Header="DataGrid">
    <StackPanel>
        <DataGrid x:Name="gridInventory" Height="288"/>
    </StackPanel>
</TabItem>
```

使用 NuGet 包管理器将以下包添加到项目中：

- Microsoft.EntityFrameworkCore
- Microsoft.EntityFrameworkCore.SqlServer
- Microsoft.Extensions.Configuration
- Microsoft.Extensions.Configuration.Json

如果你更愿意使用 .NET Core 命令行接口(CLI)来添加这些包，请在解决方案目录下输入以下命令：

```
dotnet add WpfControlsAndAPIs package Microsoft.EntityFrameworkCore
dotnet add WpfControlsAndAPIs package Microsoft.EntityFrameworkCore.SqlServer
dotnet add WpfControlsAndAPIs package Microsoft.Extensions.Configuration
dotnet add WpfControlsAndAPIs package Microsoft.Extensions.Configuration.Json
```

接下来，右键单击解决方案，选择“添加”▶“现有项目”，从第 23 章添加 AutoLot.Dal 和 AutoLot.Dal.Models 项目，并添加对这些项目的项目引用。你也可以使用 CLI 通过以下命令添加引用（你需要根据项目的位置和计算机的操作系统调整路径）：

```
dotnet sln .\Chapter25_AllProjects.sln add ..\Chapter_23\AutoLot.Models
dotnet sln .\Chapter25_AllProjects.sln add ..\Chapter_23\AutoLot.Dal
dotnet add WpfControlsAndAPIs reference ..\Chapter_23\AutoLot.Models
dotnet add WpfControlsAndAPIs reference ..\Chapter_23\AutoLot.Dal
```

确认 AutoLot.Dal 到 AutoLot.Dal.Models 的项目引用仍然存在。将以下命名空间添加到 MainWindow.xaml.cs 中：

```
using System.Linq;
using AutoLot.Dal.EfStructures;
using AutoLot.Dal.Repos;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
```

在 MainWindow.cs 中添加两个模块级属性，用于保存 IConfiguration 和 ApplicationDbContext 的实例。

```
private IConfiguration _configuration;
private ApplicationDbContext _context;
```

添加一个名为 GetConfigurationAndContext() 的新方法来创建这些实例，并在构造函数中调用它。完整方法如下：

```
private void GetConfigurationAndDbContext()
{
_configuration = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appsettings.json", true, true)
    .Build();
var optionsBuilder =
    new DbContextOptionsBuilder<ApplicationDbContext>();
var connectionString =
    _configuration.GetConnectionString("AutoLot");
optionsBuilder.UseSqlServer(connectionString,
    sqlOptions => sqlOptions.EnableRetryOnFailure());
_context = new ApplicationDbContext(optionsBuilder.Options);
}
```

向项目添加一个名为 appsettings.json 的新 JSON 文件，并将其生成状态设置为“始终复制”。这可以通过在解决方案资源管理器中右键单击该文件，选择“属性”，然后在“复制到输出目录”设置中输入“始终复制”来完成。你也可以将此添加到项目文件中以实现相同的效果：

```
<ItemGroup>
    <None Update="appsettings.json">
        <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </None>
</ItemGroup>
```

将 JSON 文件更新为以下内容（更新你的连接字符串以匹配你的环境）：

```
{
    "ConnectionStrings": {
        "AutoLotFinal": "server=.,5433;Database=AutoLot;User Id=sa;Password=P@ssw0rd;"
    }
}
```

打开 MainWindow.xaml.cs，添加一个名为 ConfigureGrid() 的最终辅助函数，并在配置完 ApplicationDbContext 后从构造函数中调用它。你只需添加几行代码，如下所示：

```
private void ConfigureGrid()
{
    using var repo = new CarRepo(_context);
    gridInventory.ItemsSource = repo
        .GetAllIgnoreQueryFilters()
        .ToList()
        .Select(x=> new {
            x.Id,
            Make=x.MakeName,
            x.Color,
            x.PetName
        });
}
```

现在，当你运行项目时，会看到数据填充到网格中。如果你想让网格更漂亮一些，可以使用 Visual Studio 的“属性”窗口编辑网格，使其更具吸引力。

这结束了当前的示例。在后面的章节中，你会看到其他一些控件的实际使用；不过，此时你应该已经熟悉了在 Visual Studio 中构建 UI 以及手动使用 XAML 和 C# 代码的过程。

## 理解依赖属性的作用

与任何 .NET Core API 一样，WPF 在其实现中使用了 .NET Core 类型系统的每个成员（类、结构、接口、委托、枚举）以及每个类型成员（属性、方法、事件、常量数据、只读字段等）。不过，WPF 还支持一种称为依赖属性的独特编程概念。

与“普通”.NET Core 属性（在 WPF 文献中通常称为 CLR 属性）一样，依赖属性可以使用 XAML 声明式设置，也可以在代码文件中以编程方式设置。此外，依赖属性（如 CLR 属性）最终用于封装类的数据字段，并且可以配置为只读、只写或读写。

更有趣的是，在几乎每种情况下，你都不会意识到自己实际上设置（或访问）的是依赖属性而不是 CLR 属性！例如，WPF 控件从 FrameworkElement 继承的 Height 和 Width 属性，以及从 ControlContent 继承的 Content 成员，实际上都是依赖属性。

```
<!-- Set three dependency properties! -->
<Button x:Name = "btnMyButton" Height = "50" Width = "100" Content = "OK"/>
```

鉴于所有这些相似之处，为什么 WPF 要为这样一个熟悉的概念定义一个新术语？答案在于依赖属性在类中的实现方式。稍后你会看到一个编码示例；但从高层来看，所有依赖属性都是按以下方式创建的：

- 首先，定义依赖属性的类必须在其继承链中包含 DependencyObject。
- 单个依赖属性表示为类中类型为 DependencyProperty 的公共静态只读字段。按照约定，该字段的命名方式是在 CLR 包装器名称后添加 Property 后缀（参见最后一点）。
- DependencyProperty 变量通过对 DependencyProperty.Register() 的静态调用进行注册，这通常发生在静态构造函数中或在声明变量时内联进行。
- 最后，该类将定义一个对 XAML 友好的 CLR 属性，该属性调用 DependencyObject 提供的方法来获取和设置值。

一旦实现，依赖属性就提供许多强大的功能，各种 WPF 技术（包括数据绑定、动画服务、样式、模板等）都会使用这些功能。简而言之，依赖属性的动机是提供一种基于其他输入的值来计算属性值的方法。以下是其中一些关键优势的列表，这些优势远远超出了 CLR 属性的简单数据封装：

- 依赖属性可以从父元素的 XAML 定义中继承其值。例如，如果在 Window 的开始标记中为 FontSize 属性定义了一个值，则该 Window 中的所有控件默认都会具有相同的字体大小。
- 依赖属性支持由 XAML 作用域内的元素设置值的能力，例如 Button 设置 DockPanel 父元素的 Dock 属性。（请记住，附加属性就是这样做的，因为附加属性是依赖属性的一种形式。）
- 依赖属性允许 WPF 基于多个外部值计算值，这对于动画和数据绑定服务可能很重要。
- 依赖属性为 WPF 触发器提供基础结构支持（在处理动画和数据绑定时也经常使用）。

现在请记住，在许多情况下，你将以与普通 CLR 属性相同的方式与现有依赖属性交互（这要归功于 XAML 包装器）。在上一节关于数据绑定的内容中，你了解到如果需要在代码中建立数据绑定，必须在作为操作目标的对象上调用 SetBinding() 方法，并指定它将操作的依赖属性，如下所示：

```
private void SetBindings()
{
    Binding b = new Binding
    {
        // Register the converter, source, and path.
        Converter = new MyDoubleConverter(),
        Source = this.mySB,
        Path = new PropertyPath("Value")
    };
    // Specify the dependency property!
    this.labelSBThumb.SetBinding(Label.ContentProperty, b);
}
```

在第 27 章中，你将看到类似的代码，了解如何在代码中启动动画。

```
// Specify the dependency property!
rt.BeginAnimation(RotateTransform.AngleProperty, dblAnim);
```

