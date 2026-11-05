
```
Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
Content root path: C:\Code\PracticalApps\Northwind.Web
```

Visual Studio 还会自动启动您选择的浏览器。如果您使用的是 Visual Studio Code，则需要手动启动 Chrome。

- 4. 让 Web 服务器继续运行。
- 5. 在 Chrome 中，打开开发者工具，并点击 Network 标签。
- 6. 输入地址 http://localhost:5000/，或分配给 HTTP 的任意端口号，并注意响应是来自跨平台 Kestrel Web 服务器的纯文本 Hello World!，如 图 14.6 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_607_0.png)

图 14.6：来自 http://localhost:5000/ 的纯文本响应

Chrome 还会自动请求 favicon.ico 文件以在浏览器标签中显示，但该文件缺失，因此显示为 404 Not Found 错误。

- 7. 输入地址 https://localhost:5001/，或分配给 HTTPS 的任意端口号，并注意如果您未使用 Visual Studio 或在提示信任 SSL 证书时点击了 No，则响应会显示隐私错误，如 图 14.7 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_608_0.png)

图 14.7：显示未使用证书启用 SSL 加密的隐私错误

当您未配置浏览器可信任以加密和解密 HTTPS 流量的证书时，您会看到此错误（因此如果您未看到此错误，则是因为您已配置了证书）。

在生产环境中，您希望向 Verisign 等公司付费获取 SSL 证书，因为它们提供责任保护和技术支持。

Linux 开发者： 如果您使用的是无法创建自签名证书的 Linux 变体，或您不介意每 90 天重新申请一次新证书，则可以从以下链接获取免费证书：https://letsencrypt.org

在开发期间，您可以让操作系统信任 ASP.NET Core 提供的临时开发证书。

- 8. 在命令行或在 TERMINAL 中，按 Ctrl + C 关闭 Web 服务器，并注意写入的消息，如以下输出中突出显示所示：

```
info: Microsoft.Hosting.Lifetime[0]
    Application is shutting down...
This executes after the web server has stopped!

C:\Code\PracticalApps\Northwind.Web\bin\Debug\net6.0\Northwind.Web.exe
(process 19888) exited with code 0.
```

- 9. 如果您需要信任本地自签名 SSL 证书，则在命令行或在 TERMINAL 中，输入 dotnet dev-certs https --trust 命令，并注意消息 Trusting the HTTPS development certificate was requested。系统可能会提示您输入密码，并且可能已存在有效的 HTTPS 证书。

## 启用更强的[content]
```
Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
Content root path: C:\Code\PracticalApps\Northwind.Web
```

Visual Studio 还会自动启动您选择的浏览器。如果您使用的是 Visual Studio Code，则需要手动启动 Chrome。

- 4. 让 Web 服务器继续运行。
- 5. 在 Chrome 中，打开开发者工具，并点击 Network 标签。
- 6. 输入地址 http://localhost:5000/，或分配给 HTTP 的任意端口号，并注意响应是来自跨平台 Kestrel Web 服务器的纯文本 Hello World!，如 图 14.6 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_607_0.png)

图 14.6：来自 http://localhost:5000/ 的纯文本响应

Chrome 还会自动请求 favicon.ico 文件以在浏览器标签中显示，但该文件缺失，因此显示为 404 Not Found 错误。

- 7. 输入地址 https://localhost:5001/，或分配给 HTTPS 的任意端口号，并注意如果您未使用 Visual Studio 或在提示信任 SSL 证书时点击了 No，则响应会显示隐私错误，如 图 14.7 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_608_0.png)

图 14.7：显示未使用证书启用 SSL 加密的隐私错误

当您未配置浏览器可信任以加密和解密 HTTPS 流量的证书时，您会看到此错误（因此如果您未看到此错误，则是因为您已配置了证书）。

在生产环境中，您希望向 Verisign 等公司付费获取 SSL 证书，因为它们提供责任保护和技术支持。

Linux 开发者： 如果您使用的是无法创建自签名证书的 Linux 变体，或您不介意每 90 天重新申请一次新证书，则可以从以下链接获取免费证书：https://letsencrypt.org

在开发期间，您可以让操作系统信任 ASP.NET Core 提供的临时开发证书。

- 8. 在命令行或在 TERMINAL 中，按 Ctrl + C 关闭 Web 服务器，并注意写入的消息，如以下输出中突出显示所示：

```
info: Microsoft.Hosting.Lifetime[0]
    Application is shutting down...
This executes after the web server has stopped!

C:\Code\PracticalApps\Northwind.Web\bin\Debug\net6.0\Northwind.Web.exe
(process 19888) exited with code 0.
```

- 9. 如果您需要信任本地自签名 SSL 证书，则在命令行或在 TERMINAL 中，输入 dotnet dev-certs https --trust 命令，并注意消息 Trusting the HTTPS development certificate was requested。系统可能会提示您输入密码，并且可能已存在有效的 HTTPS 证书。

## 启用更强的[content]
```
Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
Content root path: C:\Code\PracticalApps\Northwind.Web
```

Visual Studio 还会自动启动您选择的浏览器。如果您使用的是 Visual Studio Code，则需要手动启动 Chrome。

- 4. 让 Web 服务器继续运行。
- 5. 在 Chrome 中，打开开发者工具，并点击 Network 标签。
- 6. 输入地址 http://localhost:5000/，或分配给 HTTP 的任意端口号，并注意响应是来自跨平台 Kestrel Web 服务器的纯文本 Hello World!，如 图 14.6 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_607_0.png)

图 14.6：来自 http://localhost:5000/ 的纯文本响应

Chrome 还会自动请求 favicon.ico 文件以在浏览器标签中显示，但该文件缺失，因此显示为 404 Not Found 错误。

- 7. 输入地址 https://localhost:5001/，或分配给 HTTPS 的任意端口号，并注意如果您未使用 Visual Studio 或在提示信任 SSL 证书时点击了 No，则响应会显示隐私错误，如 图 14.7 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_608_0.png)

图 14.7：显示未使用证书启用 SSL 加密的隐私错误

当您未配置浏览器可信任以加密和解密 HTTPS 流量的证书时，您会看到此错误（因此如果您未看到此错误，则是因为您已配置了证书）。

在生产环境中，您希望向 Verisign 等公司付费获取 SSL 证书，因为它们提供责任保护和技术支持。

Linux 开发者： 如果您使用的是无法创建自签名证书的 Linux 变体，或您不介意每 90 天重新申请一次新证书，则可以从以下链接获取免费证书：https://letsencrypt.org

在开发期间，您可以让操作系统信任 ASP.NET Core 提供的临时开发证书。

- 8. 在命令行或在 TERMINAL 中，按 Ctrl + C 关闭 Web 服务器，并注意写入的消息，如以下输出中突出显示所示：

```
info: Microsoft.Hosting.Lifetime[0]
    Application is shutting down...
This executes after the web server has stopped!

C:\Code\PracticalApps\Northwind.Web\bin\Debug\net6.0\Northwind.Web.exe
(process 19888) exited with code 0.
```

- 9. 如果您需要信任本地自签名 SSL 证书，则在命令行或在 TERMINAL 中，输入 dotnet dev-certs https --trust 命令，并注意消息 Trusting the HTTPS development certificate was requested。系统可能会提示您输入密码，并且可能已存在有效的 HTTPS 证书。

## 启用更强的[content]
```
Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
Content root path: C:\Code\PracticalApps\Northwind.Web
```

Visual Studio 还会自动启动您选择的浏览器。如果您使用的是 Visual Studio Code，则需要手动启动 Chrome。

- 4. 让 Web 服务器继续运行。
- 5. 在 Chrome 中，打开开发者工具，并点击 Network 标签。
- 6. 输入地址 http://localhost:5000/，或分配给 HTTP 的任意端口号，并注意响应是来自跨平台 Kestrel Web 服务器的纯文本 Hello World!，如 图 14.6 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_607_0.png)

图 14.6：来自 http://localhost:5000/ 的纯文本响应

Chrome 还会自动请求 favicon.ico 文件以在浏览器标签中显示，但该文件缺失，因此显示为 404 Not Found 错误。

- 7. 输入地址 https://localhost:5001/，或分配给 HTTPS 的任意端口号，并注意如果您未使用 Visual Studio 或在提示信任 SSL 证书时点击了 No，则响应会显示隐私错误，如 图 14.7 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_608_0.png)

图 14.7：显示未使用证书启用 SSL 加密的隐私错误

当您未配置浏览器可信任以加密和解密 HTTPS 流量的证书时，您会看到此错误（因此如果您未看到此错误，则是因为您已配置了证书）。

在生产环境中，您希望向 Verisign 等公司付费获取 SSL 证书，因为它们提供责任保护和技术支持。

Linux 开发者： 如果您使用的是无法创建自签名证书的 Linux 变体，或您不介意每 90 天重新申请一次新证书，则可以从以下链接获取免费证书：https://letsencrypt.org

在开发期间，您可以让操作系统信任 ASP.NET Core 提供的临时开发证书。

- 8. 在命令行或在 TERMINAL 中，按 Ctrl + C 关闭 Web 服务器，并注意写入的消息，如以下输出中突出显示所示：

```
info: Microsoft.Hosting.Lifetime[0]
    Application is shutting down...
This executes after the web server has stopped!

C:\Code\PracticalApps\Northwind.Web\bin\Debug\net6.0\Northwind.Web.exe
(process 19888) exited with code 0.
```

- 9. 如果您需要信任本地自签名 SSL 证书，则在命令行或在 TERMINAL 中，输入 dotnet dev-certs https --trust 命令，并注意消息 Trusting the HTTPS development certificate was requested。系统可能会提示您输入密码，并且可能已存在有效的 HTTPS 证书。

## 启用更强的安全性和重定向到安全连接

启用更严格的安全性并自动将 HTTP 请求重定向到 HTTPS 是良好的做法。

![](img/c63b00dca7192cc58cfb9e1f1e64f483_609_0.png)

> 良好做法：HTTP 严格传输安全（HSTS）是一种可选的安全增强功能，您应始终启用它。如果网站指定它并且浏览器支持它，则它会强制所有通信通过 HTTPS 进行，并防止访客使用不受信任或无效的证书。

现在就这样做：

- 1. 在 Program.cs 中，添加一个 if 语句以在非开发环境下启用 HSTS，如以下代码所示：

```
if (!app.Environment.IsDevelopment())
{
    app.UseHsts();
}
```

- 2. 在调用 app.MapGet 之前添加一条语句以将 HTTP 请求重定向到 HTTPS，如以下代码所示：

```
app.UseHttpsRedirection();
```

- 3. 启动 Northwind.Web 网站项目。
- 4. 如果 Chrome 仍在运行，请关闭并重新启动它。
- 5. 在 Chrome 中，显示开发者工具，并点击 Network 标签。
- 6. 输入地址 http://localhost:5000/，或分配给 HTTP 的任意端口号，并注意服务器如何以 307 Temporary Redirect 响应到端口 5001，并且证书现在有效且受信任，如 图 14.8 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_610_0.png)

图 14.8：连接现在使用有效证书和 307 重定向得到保护

- 7. 关闭 Chrome。
- 8. 关闭 Web 服务器。

> **良好做法**：在完成网站测试后，请记得关闭 Kestrel Web 服务器。

## 控制托管环境

在 ASP.NET Core 的早期版本中，项目模板设置了一条规则，即在开发模式下，任何未处理的异常都将在浏览器窗口中显示，以便开发人员查看异常的详细信息，如以下代码所示：

```
if (app.Environment.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
}
```

在 ASP.NET Core 6 及更高版本中，此代码默认自动执行，因此未包含在项目模板中。

ASP.NET Core 如何知道我们正在开发模式下运行，以便 IsDevelopment 方法返回 true？让我们来了解一下。

ASP.NET Core 可以从环境变量中读取以确定要使用的托管环境，例如 DOTNET_ENVIRONMENT 或 ASPNETCORE_ENVIRONMENT。

您可以在本地开发期间覆盖这些设置：

- 1. 在 Northwind.Web 文件夹中，展开名为 Properties 的文件夹，打开名为 launchSettings.json 的文件，并注意名为 Northwind.Web 的配置文件将托管环境设置为 Development，如以下配置中突出显示所示：
- 2. 将 HTTP 的随机分配端口号更改为 5000，将 HTTPS 更改为 5001。
- 3. 将环境更改为 Production。可选地，将 launchBrowser 更改为 false 以防止 Visual Studio 自动启动浏览器。
- 4. 启动网站并注意托管环境为 Production，如以下输出所示：

```
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:56111",
      "sslPort": 44329
    }
  },
  "profiles": {
    "Northwind.Web": {
      "commandName": "Project",
      "dotnetRunMessages": "true",
      "launchBrowser": true,
      "applicationUrl": "https://localhost:5001;http://localhost:5000",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

```
info: Microsoft.Hosting.Lifetime[0]
  Hosting environment: Production
```

- 5. 关闭 Web 服务器。
- 6. 在 launchSettings.json 中，将环境改回 Development。

> launchSettings.json 文件还具有使用随机端口号使用 IIS 作为 Web 服务器的配置。在本书中，我们将只使用 Kestrel 作为 Web 服务器，因为它是跨平台的。

## 分离服务和管道的配置

将所有用于初始化简单 Web 项目的代码放在 Program.cs 中可能是一个好主意，尤其对于 Web 服务而言，因此我们将在第 16 章《构建和使用 Web 服务》中再次看到这种风格。

然而，对于比最基本的 Web 项目更复杂的项目，您可能更喜欢将配置分离到一个单独的 Startup 类中，该类包含两个方法：

- ConfigureServices(IServiceCollection services)：向依赖注入容器添加依赖服务，例如 Razor Pages 支持、跨源资源共享（CORS）支持，或用于操作 Northwind 数据库的数据库上下文。
- Configure(IApplicationBuilder app, IWebHostEnvironment env)：设置请求和响应流经的 HTTP 管道。在 app 参数上调用各种 Use 方法以按功能应处理的顺序构建管道。

![](img/c63b00dca7192cc58cfb9e1f1e64f483_612_0.png)

图 14.9：Startup 类 ConfigureServices 和 Configure 方法图

这两个方法都将由运行时自动调用。

现在让我们创建一个 Startup 类：

- 1. 向 Northwind.Web 项目添加一个名为 Startup.cs 的新类文件。
- 2. 修改 Startup.cs，如以下代码所示：

```
namespace Northwind.Web;

public class Startup
```
