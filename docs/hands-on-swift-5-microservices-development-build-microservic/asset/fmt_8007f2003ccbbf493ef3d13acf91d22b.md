
# Git 和版本控制

这甚至不应该成为一个讨论点，但为了完整起见，*始终* 将代码保存在版本控制系统中，例如 `git`。虽然存在替代方案，但我强烈建议如果你还不熟悉 `git`，请熟悉它。要托管 `git` 仓库，你应该使用一个你选择的在线云服务提供商。`GitHub` 是最流行的，拥有有吸引力的定价模型（包括免费模型！），但其他提供商如 `GitLab` 或 `Bitbucket` 也同样好用。你不应该只将代码保存在本地机器上：硬盘可能会损坏，从云端恢复会让你更安心。

`GitHub` 是最流行的 `git` 提供商。大多数服务端 Swift 项目目前托管在 `GitHub` 上。除非所有者（即 `Microsoft`）以戏剧性的方式改变政策，否则我不指望这会改变。`GitHub` 为开源项目提供免费计划。

## 关于微服务和仓库

大多数云提供商提供某种方式自动部署后端应用程序。我建议以下存储代码的准则：

*   **一个仓库仅用于一个微服务。**
*   **至少拥有 `master` 和 `development` 分支。**
*   **使用 Pull Request 更新 `master` 分支。**
*   **`master` 是部署的分支。**
*   **从仓库（`master`）自动部署到服务器。**
*   **每个项目都应有自己的仓库。**

使用一个仓库管理多个项目可能很诱人。在某些非常通用的用例中，这可能行得通。然而，根据我的经验，你经常需要稍微定制一下。此外，除非仓库是开放的，否则你可能需要处理各种权限设置，如果每个项目都有自己的仓库，这会容易得多。

所以，继续设置你的账户并准备仓库。完成后，我们可以继续了解如何使用 `Vapor` 的标准 API 模板。

# 使用 API 模板

如果一切设置正确，你可以通过在命令行中输入以下命令来创建模板：

## 1. 创建一个新的 Vapor 应用程序

```
$ vapor-beta new template
```

撰写本文时，`Vapor` 的 CLI 仅在 Beta 版中支持 `Vapor 4`。一旦 `Vapor 4` 完全发布，你也可以执行以下操作：

```
$ vapor new template
```

`Vapor` 的 CLI 现在会自动复制标准 API 模板。你可以在 [https://github.com/vapor/api-template](https://github.com/vapor/api-template) 找到该模板；它是 API 的良好起点。

## 2. 进入新创建的文件夹

```
$ cd template
```

模板原样即可编译和构建，但我们想稍微修改一下，使其成为我们自己用途的良好起点。因此，我们将执行以下操作：

*   移除 `SQLite`。
*   添加 `MySQL`。
*   添加 `JWT` 包。
*   添加 `CORS` 中间件。

通过添加这些包，我们为微服务配备了在微服务架构中运行所需的一切。你将在后续章节学习如何添加和移除包。

## 3. 编译项目。这也将加载所有当前依赖项

```
$ swift build
```

## 4. 在 Xcode 中打开项目，输入以下命令

```
$ xed .
```

或者，你也可以通过打开 `Package.swift` 来打开 Xcode。在终端中，可以这样做：

```
$ open Package.swift
```

现在你已成功编译模板，让我们看看 `Vapor` 应用程序的组件。

# 通用文件文件夹

我们可以保持模板的文件结构不变；它已经提供了一个组织良好的层次结构。你唯一想添加的文件夹是 `Workers`，与 `Controllers` 同级，因此你应该将其添加到 `Sources/App/` 中。在此文件夹中，我们可以放置充当 *Workers* 的类。在后面的 *Workers* 部分，你将详细了解为什么需要这样一个文件夹。

此外，在 `Sources/App/` 中创建另一个 `Helpers` 文件夹。此文件夹用于不绑定到我们自己类的扩展和函数，但它们要么是通用的（全局的），要么是来自其他框架的扩展类。

你可以保留其余的文件夹和文件不变。不过，我们将坚持一些准则，以保持一致性：

*   文件名应大写。
*   每个文件应包含一个类或结构体。
*   类、函数或结构体名称应与文件名相同。
*   扩展文件使用 `+` 符号标记原始类。例如，`Application+ImageRequest.swift` 文件将使用与图像请求相关的函数扩展 `Application` 类。

继续将任何非大写的文件名改为大写（`main.swift` 除外；编译器要求它必须小写）。Swift 对文件夹或文件名没有严格要求，但这将使以后接手该项目或让其他人使用它变得容易得多。

在活跃开发期间，打破其中一些规则完全是人之常情。只需确保在提交到仓库之前清理它们。

现在你已设置好文件夹，让我们设置依赖项。

# 依赖管理

对于服务端 Swift 项目，经常需要添加或移除依赖包。接下来让我们探讨一个如何操作的示例。

## 添加和移除包

我们需要移除 `SQLite`，因为我们不会使用它：

### 1. 继续从 `Package.swift` 中移除以下行

```
.package(url: "https://github.com/vapor/fluent-sqlite-driver.git", from: "4.0.0"
```

### 2. 然后，在同一部分添加一行

```
.package(url: "https://github.com/vapor/fluent-mysql-driver.git", from: "4.0.0"),
```

这将 `MySQL Fluent` 包添加到项目中。注意，这也添加了通用的 `Fluent` 包，因此无需单独添加。

### 3. 现在你需要更改以下行

```
.target(name: "App", dependencies: ["Fluent", "FluentSQLiteDriver", "Vapor"]),
```

你可以将前面的行更改为以下内容：

```
.target(name: "App", dependencies: ["Fluent", "FluentMySQLDriver", "Vapor"]),
```

这使 `Fluent` 和 `FluentMySQL` 可用于项目。

### 4. 你现在可以以相同方式添加以下包

*   [https://github.com/proggeramlug/SimpleJWTMiddleware](https://github.com/proggeramlug/SimpleJWTMiddleware) 的 `master` 分支

你应该最终得到一个如下所示的 `.target` 行：

```
.target(name: "App", dependencies: ["FluentMySQLDriver", "Vapor", "SimpleJWTMiddleware"]),
```

### 5. 完成此设置后，我们可以运行以下命令

```
$ swift package update
```

请注意，如果你在 Xcode 中完成了所有这些操作，Xcode 将自动更新你的依赖项。

这将导致 Swift 更新你的依赖项。请注意，加载所有包可能需要一两分钟。如果遇到任何错误，请确保你正确拼写了包名称。

在本章的 GitHub 文件夹中，你将找到此服务的完全工作示例。使用它与你自己的版本进行比较。

### 6. 如果你尚未打开 Xcode，现在可以打开

```
$ open Package.swift
```

设置好模板后，我们现在准备调整配置。

# 配置

API 标准模板附带一个 `configuration.swift` 文件，其中包含一个函数，如下所示：

```
public func configure(_ app: Application) throws {
```

此函数在应用程序启动时调用。我们用它来添加应用程序所需的所有配置，包括中间件、服务、配置、服务器等。

我们想要添加一些我们肯定需要的中间件、服务和配置。接下来让我们做这件事。

## 中间件

将以下中间件添加到你的配置中：

```
app.middleware.use(CORSMiddleware())
```

我们需要的中间件如下：

*   **CORS**：响应浏览器 CORS 请求。
