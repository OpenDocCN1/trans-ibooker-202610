
若要测试此行为，请向 ValuesController 中添加另一个方法，如下所示：

```
[HttpGet("error")]
public IActionResult Error()
{
    return NotFound();
}
```

运行应用程序，并使用 Swagger UI 执行新的 Error 终结点。结果仍然是 404（NotFound）状态码，但响应正文中会返回额外信息。以下显示了一个示例响应（你的 traceId 会不同）：

```
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  "title": "Not Found",
  "status": 404,
  "traceId": "00-9a609e7e05f46d4d82d5f897b90da624-a6484fb34a7d3a44-00"
}
```

可以通过 Startup.cs 类的 ConfigureServices() 方法中的配置来禁用此行为。

```
services.AddControllers()
    .ConfigureApiBehaviorOptions(options =>
    {
        options.SuppressMapClientErrors = true;
    });
```

当此行为被禁用时，对 Error 终结点的调用会返回 404，且不包含任何额外信息。

## 更新 Swagger/OpenAPI 设置

Swagger（也称为 OpenAPI）是一种用于记录 RESTful API 的开放标准。在 ASP.NET Core API 中添加 Swagger 的两个主要选择是 Swashbuckle 和 NSwag。ASP.NET Core 5 现在将 Swashbuckle 作为新项目模板的一部分包含在内。为 AutoLot.Api 生成的 swagger.json 文档包含站点、每个终结点以及终结点中涉及的任何对象的信息。

Swagger UI 是一个基于 Web 的 UI，它提供了一个交互式界面，用于检查应用程序的终结点以及测试这些终结点（正如你在本章前面所做的那样）。通过将文档添加到生成的 swagger.json 文件中，可以增强这种体验。

### 更新 Startup 类中的 Swagger 调用

默认 API 模板已添加代码，用于在 Startup.cs 的 ConfigureServices() 方法中生成 swagger.json 文件。

```
services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo { Title = "AutoLot.Api", Version = "v1" });
});
```

与默认代码相比，第一个更改是向 OpenApiInfo 添加元数据。将 AddSwaggerGen() 调用更新为以下内容，这会更新标题并添加描述和许可证信息：

```
services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1",
        new OpenApiInfo
        {
            Title = "AutoLot Service",
            Version = "v1",
            Description = "Service to support the AutoLot dealer site",
            License = new OpenApiLicense
            {
                Name = "Skimedic Inc",
                Url = new Uri("http://www.skimedic.com")
            }
        });
});
```

下一步是将 UseSwagger() 和 UseSwaggerUI() 从仅开发环境块中移到 Configure() 的主执行路径中。此外，将标题从“AutoLot.Api v1”更新为“AutoLot Service v1”。

```
public void Configure(IApplicationBuilder app, IWebHostEnvironment env, ApplicationDbContext context)
{
    if (env.IsDevelopment())
    {
        //If in development environment, display debug info
        app.UseDeveloperExceptionPage();
        //Original code
        //app.UseSwagger();
        //app.UseSwaggerUI(c => c.SwaggerEndpoint("/swagger/v1/swagger.json", "AutoLot.Api v1"));
        //Initialize the database
        if (Configuration.GetValue<bool>("RebuildDataBase"))
        {
            SampleDataInitializer.ClearAndReseedDatabase(context);
        }
    }

    // Enable middleware to serve generated Swagger as a JSON endpoint.
    app.UseSwagger();
    // Enable middleware to serve swagger-ui (HTML, JS, CSS, etc.),
    // specifying the Swagger JSON endpoint.
    app.UseSwaggerUI(c => { c.SwaggerEndpoint("/swagger/v1/swagger.json", "AutoLot Service v1"); });
    ...
}
```

上述代码选择使用 Swagger（app.UseSwagger()）和 Swagger UI（app.UseSwaggerUI()）。它还配置了 swagger.json 文件的终结点。

### 添加 XML 文档文件

.NET Core 可以通过检查方法上的三斜杠（///）注释，从你的项目生成 XML 文档文件。若要使用 Visual Studio 启用此功能，请右键单击 AutoLot.Api 项目并打开“属性”窗口。选择“生成”页面，选中“XML 文档文件”复选框，并在文件名中输入 AutoLot.Api.xml。另外，在“抑制警告”文本框中输入 1591，如图 30-3 所示。

![](img/587021f4d5ba039be70a11107130377b_1273_0.png)

图 30-3. 添加 XML 文档文件并抑制 1591

也可以直接将相同的设置输入到项目文件中。下面显示了要添加的 PropertyGroup：

```
<PropertyGroup Condition="'$(Configuration)|$(Platform)'=='Debug|AnyCPU'">
  <DocumentationFile>AutoLot.Api.xml</DocumentationFile>
  <NoWarn>1701;1702;1591;</NoWarn>
</PropertyGroup>
```

NoWarn 1591 设置会关闭对没有 XML 注释的方法的编译器警告。

> 注意 1701 和 1702 警告是经典 .NET 早期遗留内容，由 .NET Core 编译器暴露出来。

要查看此过程的实际效果，请将 ValuesController 的 Get 方法更新为以下内容：

```
/// <summary>
/// This is an example Get method returning JSON
/// </summary>
/// <remarks>This is one of several examples for returning JSON:
/// <pre>
/// [
///   "value1",
///   "value2"
/// ]
/// </pre>
/// </remarks>
/// <returns>List of strings</returns>
[HttpGet]
public IActionResult Get()
{
    return Ok(new string[] { "value1", "value2" });
}
```

当你生成项目时，会在项目的根目录中创建一个名为 AutoLot.Api.xml 的新文件。打开该文件即可看到你刚刚添加的注释。

```
<?xml version="1.0"?>
<doc>
  <assembly>
    <name>AutoLot.Api</name>
  </assembly>
  <members>
    <member name="M:AutoLot.Api.Controllers.ValuesController.Get">
      <summary>
        This is an example Get method returning JSON
      </summary>
<remarks>This is one of several examples for returning JSON:
<pre>
[
"value1",
"value2"
]
</pre>
</remarks>
<returns>List of strings</returns>  </member>
</members>
</doc>
```

> 注意 使用 Visual Studio 时，如果在类或方法定义之前输入三个斜杠，Visual Studio 会为你生成初始的 XML 注释。

下一步是将 XML 注释合并到生成的 swagger.json 文件中。

### 向 SwaggerGen 添加 XML 注释

必须将生成的 XML 注释添加到 swagger.json 的生成过程中。首先向 Startup 类添加以下 using 语句：

```
using System.IO;
using System.Reflection;
```

通过在 AddSwaggerGen() 方法中调用 IncludeXmlComments() 方法，可以将 XML 文档文件添加到 Swagger。导航到 Startup 类的 ConfigureServices() 方法，并将 AddSwaggerGen() 方法更新为以下内容以添加 XML 文档文件：

```
services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1",
        new OpenApiInfo
        {
            Title = "AutoLot Service",
            Version = "v1",
            Description = "Service to support the AutoLot dealer site",
            License = new OpenApiLicense
            {
                Name = "Skimedic Inc",
                Url = new Uri("http://www.skimedic.com")
            }
        });
    var xmlFile = $"{Assembly.GetExecutingAssembly().GetName().Name}.xml";
    var xmlPath = Path.Combine(AppContext.BaseDirectory, xmlFile);
    c.IncludeXmlComments(xmlPath);
});
```

运行应用程序并检查 Swagger UI。注意已集成到 Swagger UI 中的 XML 注释，如图 30-4 所示。

![](img/587021f4d5ba039be70a11107130377b_1276_0.png)

图 30-4. 集成到 Swagger UI 中的 XML 文档

除了 XML 文档之外，对应用程序终结点进行附加配置也可以改进文档。

### API 终结点的其他文档选项

还有一些附加属性可以增强 Swagger 文档。若要使用它们，请首先将以下 using 语句添加到 ValuesController.cs 文件中：

```
using Microsoft.AspNetCore.Http;
using Swashbuckle.AspNetCore.Annotations;
```

Produces 特性指示终结点的内容类型。ProducesResponseType 特性使用 StatusCodes 枚举来指示终结点可能的返回代码。更新 ValuesController 的 Get() 方法，以指定 application/json 作为返回类型，并指定操作结果将返回 200 OK 或 400 Bad Request。

```
[HttpGet]
[Produces("application/json")]
[ProducesResponseType(StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
public ActionResult<IEnumerable<string>> Get()
{
    return new string[] { "value1", "value2" };
}
```

