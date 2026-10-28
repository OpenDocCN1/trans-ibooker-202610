

## 创建 HTTP 服务

为了构建我们的原理图库，我们将创建一个用于搭建 Angular 服务的原理图。它会生成一个导入内置 HTTP 客户端的服务，并且包含 CRUD 操作中每个 HTTP 请求对应的一个方法。

我们将要构建的生成原理图不会独立运行。相反，我们会将其与 Angular CLI 现有的服务生成原理图结合起来。因此，我们不需要单独的 JSON schema。

让我们从创建原理图开始：

- 1. 执行以下命令，向我们的集合中添加一个新的原理图：

  ```
  schematics blank crud-service
  ```

- 2. 打开 collection.json 文件，并为该原理图提供描述性说明：

  ```
  "crud-service": {
    "description": "Generate a CRUD HTTP service",
    "factory": "./crud-service/index#crudService"
  }
  ```

- 3. 在工作区的 crud-service 文件夹内创建一个名为 files 的文件夹。

- 4. 在 files 文件夹内创建一个名为 `__name@dasherize__.service.ts.template` 的文件，并添加以下代码：

  ```
  import { Injectable } from '@angular/core';
  import { HttpClient } from '@angular/common/http';
  import { Observable } from 'rxjs';

  @Injectable({
    providedIn: 'root'
  })
  export class <%= classify(name) %>Service {
    constructor(private http: HttpClient) { }
  }
  ```

上面的文件是我们的原理图将要生成的 Angular 服务文件的模板。它默认在类的构造函数中注入 `HttpClient` 服务。

- 2. 重命名 `tree` 参数，并将其从 return 语句中移除，因为我们不会用到它。由此产生的工厂函数应如下所示：

  ```
  export function crudService(_options: any): Rule {
    return (_tree: Tree, _context: SchematicContext) =>
      {};
  }
  ```

- 3. 在 `crudService` 函数中添加以下代码片段：

  ```
  const templateSource = apply(url('./files'), [
    applyTemplates({
      ..._options,
      classify: strings.classify,
      dasherize: strings.dasherize
    }),
    move(normalize(_options.path ??
      normalize('src/app/')))
  ]);
  ```

上面的代码片段与我们用于组件原理图的代码片段看起来完全相同。主要区别在于默认路径是 `src/app` 文件夹，并且我们通过 `_options` 参数将所有可用的选项传递给原理图。

> **重要说明**
>
> 我们无法预先知道哪些选项会被用来生成 Angular 服务。因此，我们使用*展开*运算符将所有可用选项传递给 `templateSource` 方法。这也是 `_options` 参数类型为 `any` 的原因。

- 4. 在函数末尾添加以下 return 语句：

  ```
  return chain([
    externalSchematic('@schematics/angular', 'service',
      _options),
    mergeWith(templateSource, MergeStrategy.Overwrite)
  ]);
  ```

在上述语句中，我们首先使用 `externalSchematic` 方法调用内置的生成原理图来创建 Angular 服务。然后，我们将执行该原理图得到的结果与我们的 `templateSource` 变量合并。我们还使用 `MergeStrategy.Overwrite` 定义了合并操作的策略，以便我们原理图所做的任何更改都会覆盖默认的更改。

我们用于创建 CRUD 服务的原理图现已完成。让我们在示例应用程序中使用它：

- 1. 执行以下命令来构建原理图库：

  ```
  npm run build
  ```

  > **提示**
  >
  > 我们不需要再次链接原理图库。只要我们重新构建原理图，我们的应用程序就会自动更新。

- 2. 导航到我们的应用程序所在的 `my-app` 文件夹。

- 3. 执行以下命令，使用我们的新原理图生成一个 Angular 服务：

  ```
  ng generate my-schematics:crud-service --name=customers
  ```

  我们使用 Angular CLI 的 generate 命令，再次传入我们原理图集合的名称，但这次目标是 crud-service 原理图。

- 4. 新的 Angular 服务创建在 `src/app` 文件夹中，终端窗口中的输出如下所示：

  ```
  CREATE src/app/customers.service.spec.ts (372 bytes)
  CREATE src/app/customers.service.ts (545 bytes)
  ```

图 10.2 —— 生成 Angular 服务

请注意，该原理图自动为我们生成了一个单元测试文件。这怎么可能呢？嗯，回想一下，我们将自己的原理图与 Angular CLI 内置的生成原理图合并了。所以，默认原理图所做的任何操作，都会直接反映到自定义原理图的执行中。

我们刚刚为原理图集合添加了一个有用的新命令。我们可以生成一个与 HTTP 端点交互的 Angular 服务。此外，我们还添加了与端点通信所需的基本方法。

![](img/efea53d01337e7b223389b38dcdb0160_333_0.png)

Packt.com

订阅我们的在线数字图书馆，即可完整访问超过 7,000 本图书和视频，以及行业领先的工具，帮助你规划个人发展并推进职业生涯。如需了解更多信息，请访问我们的网站。

## 为什么订阅？

- 通过来自 4,000 多位行业专业人士的实用电子书和视频，减少学习时间，增加编码时间
- 通过专为你打造的 Skill Plans 改进学习
- 每月免费获得一本电子书或视频
- 完全可搜索，便于快速访问重要信息
- 复制、粘贴、打印和书签内容

你知道吗，Packt 提供每本已出版图书的电子书版本，并附有 PDF 和 ePub 文件？你可以升级到 packt.com 上的电子书版本，作为纸质书客户，你还有权享受电子书折扣。如需更多详情，请通过 customercare@packtpub.com 与我们联系。

在 www.packt.com 上，你还可以阅读一系列免费技术文章，注册各种免费新闻通讯，并享受 Packt 图书和电子书的独家折扣和优惠。

![](img/efea53d01337e7b223389b38dcdb0160_335_0.png)

## 学习 Angular（第三版）

Aristeidis Bampakos, Pablo Deeleman

ISBN: 978-1-83921-066-2

- 使用 Angular CLI 搭建、构建和部署新的 Angular 应用程序
- 构建组件，即 Angular 应用程序的基本构建块
- 探索使 Angular 组件相互交互的技术
- 理解 Angular 支持的不同类型的模板
- 创建 HTTP 数据服务以访问 API 并向组件提供数据
- 使用 Angular Material 增强应用程序的用户体验
- 将最佳实践和编码约定应用于大规模 Web 开发项目

## 索引

- **符号**
  - `@schematics/angular` npm 包 300
- **A**
  - add 原理图 300
  - Angular
    - 与 Electron 对比 137
    - 为 127-130 添加 WYSIWYG 编辑器库
  - Angular 应用程序
    - 使用 GitHub API 构建 196
    - 使用 Nx Console 构建 17-20
    - 安装 Clarity 59, 60
    - 创建 11-13
    - 构建仪表板 196-201
    - 特性 196
    - 与文件系统集成 144, 145
    - 与 WYSIWYG 编辑器集成 140-143
    - 可视化组织成员身份 212-216
    - 概述 10
    - 显示个人信息 201-205
  - 设置响应式表单 67, 68
  - 设置路由 25-27
  - 设置 95-97
  - 工具 10
  - 列出用户仓库 207-212
  - Angular CLI
    - 关于 4, 5, 11, 127, 272
    - 配置选项 96
    - 创建库 273-276
    - 参考链接 127
  - 使用 Schematics 自定义 Angular CLI 命令
    - 背景理论 300
    - 上下文 300
    - 概述 300
    - 软件要求 300
  - Angular CLI 工作区
    - 配置 138, 139
  - Angular Component Dev Kit (CDK) 272
  - Angular elements
    - 使用组件作为 293-297
  - Angular Evergreen 8, 9
  - AngularFire 库
    - 集成 174-179

