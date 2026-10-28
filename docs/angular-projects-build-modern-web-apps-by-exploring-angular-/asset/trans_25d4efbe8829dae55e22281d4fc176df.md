
- 4. 运行 `npm run dev:ssr` 启动应用程序，并导航到 http://localhost:4200。

- 5. 使用浏览器检查页面，你应该会在页面的 head 元素中看到以下 meta 标签：

```
<head>
  <meta charset="utf-8">
  <title>GitHub portfolio app</title>
  <base href="/">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
  <link rel="stylesheet" href="styles.css">
  <style>...</style>
  <style>...</style>
  <style>...</style>
  <style>...</style>
  <style>...</style>
  <meta name="description" content="<Your GitHub login>'s GitHub portfolio">
  <meta name="author" content="<Your GitHub login>">
```

图 7.8 – 应用程序 head 元素

诸如 Twitter、Facebook 和 LinkedIn 等热门社交平台都需要各自的 meta 标签，以便 SSR 应用程序的 URL 能够在这些平台上正确显示。如果你不想使用 Angular 框架的 Meta 服务手动添加这些标签，可以使用 ngx-seo 库，该库为每个平台提供了内置方法。你可以从位于 https://www.npmjs.com/package/ngx-seo 的 npm 仓库中获取该库。

在下一节中，我们将学习如何重放 SSR 应用程序不支持的事件，从而完成我们的项目。

## 使用 preboot 重放事件

在*集成 Angular Universal*一节中，我们了解到 SSR 应用程序在被完全引导之前，除了使用 routerLink 指令进行导航外，不会响应用户事件。现在，我们将学习如何使用 preboot 库将这些事件排队，并在应用程序完全加载后重放它们。让我们看看如何使用它：

- 1. 执行以下 npm 命令来安装该库：

```
npm install preboot
```

- 2. 打开 `app.module.ts` 文件，并从 `preboot` npm 包中导入 `PrebootModule`：

```typescript
import { PrebootModule } from 'preboot';
```

- 3. 将 `PrebootModule` 类添加到 `@NgModule` 装饰器的 imports 数组中：

```typescript
@NgModule({
  declarations: [
    AppComponent,
    PersonalInfoComponent,
    PanelComponent,
    RepositoriesComponent,
    OrganizationsComponent
  ],
  imports: [
    BrowserModule.withServerTransition({ appId: 'serverApp' }),
    HttpClientModule,
    TransferHttpCacheModule,
    PrebootModule.withConfig({ appRoot: 'app-root' })
  ],
  providers: [],
  bootstrap: [AppComponent]
})
```

我们使用 `PrebootModule` 的 `withConfig` 方法来定义 Angular 应用程序的主组件。它接受一个 `PrebootOptions` 类型的选项对象，其中包含各种属性，你可以在 https://github.com/angular/preboot#PrebootOptions 找到这些属性。其中一个选项是 `appRoot` 属性，它定义了主组件的选择器。

preboot 库的目的是在应用程序状态从服务器传输到浏览器的过程中保持一致的用户体验。

要验证其功能，请运行 `npm run dev:ssr` 启动 SSR 应用程序，并使用*集成 Angular Universal*一节中描述的步骤来模拟慢速网络。如果在应用程序加载时尝试点击任何链接，你会注意到在应用程序完全加载到浏览器后，浏览器会导航到所选链接。

## 总结

在本项目中，我们为 GitHub 个人资料构建了一个作品集应用程序。最初，我们学习了如何在一个新的 Angular 应用程序中与 GitHub API 交互。我们还使用了 Bootstrap CSS 和 Bootstrap Icons，为我们的作品集应用程序提供了漂亮的用户界面。

随后，我们了解了如何使用 Angular Universal 将 Angular 应用程序转换为 SSR 应用程序。我们学习了当用户使用低端且性能较慢的设备时，如何从预渲染内容中获益，以及这项技术的一些潜在缺陷。

我们使用了 Angular 框架提供的一些可用 SEO 技术来提高应用程序的可发现性。最后，我们安装了 preboot 库，以改善应用程序在加载期间的用户体验。

在下一章中，我们将了解 monorepo 架构，以及如何管理 Angular 应用程序的状态。

## 练习题

让我们来看几个练习题：

- 1. Angular 应用程序中环境文件的用途是什么？
- 2. 我们如何在组件的模板中订阅 observable？
- 3. 我们使用哪个命令来安装 Angular Universal？
- 4. 我们如何以编程方式区分浏览器平台和服务器平台？
- 5. 哪个命令会生成 SSR 应用程序的预渲染版本？
- 6. 我们使用哪个 Angular 模块将状态从服务器传输到浏览器？
- 7. 我们使用哪个 Angular 服务来设置 Angular 应用程序的标题？
- 8. 我们使用哪个 Angular 服务来在 Angular 应用程序中设置 meta 标签？
- 9. preboot 库的用途是什么？
- 10. 我们如何在 SSR 应用程序中启用 preboot？

## 延伸阅读

以下是一些链接，供你基于本章所学内容进一步学习：

- Angular Universal 指南：`https://angular.io/guide/universal`
- GitHub REST API：`https://docs.github.com/en/rest`
- Bootstrap CSS：`https://getbootstrap.com/`
- Bootstrap Icons：`https://icons.getbootstrap.com/`
- Angular HTTP 指南：`https://angular.io/guide/http`
- TransferHttpCacheModule：`https://github.com/angular/universal/blob/master/docs/transfer-http.md`
- preboot：`https://github.com/angular/preboot`

# 8

# 使用 Nx Monorepo 工具和 NgRx 构建企业门户

典型的企业应用程序通常由后端系统和前端系统组成。后端负责与数据库交互以实现数据持久化，并对外提供 REST API。前端通过 REST 接口与后端系统通信以交换数据。在某些情况下，前端系统可以由多个应用程序组成，包括 Web 界面或移动应用程序。将所有这类应用程序和系统分别放在独立的源代码控制仓库中，扩展性不佳，并且不易于维护和构建。或者，对于此类大型企业应用程序，我们可以采用 **monorepo** 架构，即每个应用程序都位于同一仓库内的不同位置。

Angular 生态系统中一个采用 monorepo 架构的流行工具是 **Nx Dev Tools**。将 Nx monorepo 应用程序与状态管理库相结合，可以显著提升你的应用程序。**NgRx** 是 Angular 应用程序中流行的状态管理库，可以帮助我们在全局范围内保持一致且可管理的状态。

## 使用 Nx 创建 monorepo 应用程序

Nx Dev Tools 为开发人员提供了用于处理 monorepo 的工具，包括以下内容：

- **create-nx-workspace**：一个用于搭建新 Nx monorepo 应用程序的 npm 包。
- **Nx CLI**：一个针对 monorepo 应用程序运行命令的命令行界面。Nx CLI 扩展了 Angular CLI 以提供更多命令，并且由于分布式缓存机制，其速度更快。

> **提示**
> 在使用 Nx monorepo 时，建议使用 VSCode 的*快速打开*功能。生成的文件夹和文件数量会显著增加，浏览它们将变得很有挑战性。你可以在 https://code.visualstudio.com/docs/editor/editingevolved#_quick-file-navigation 了解更多信息。

当我们使用上述 npm 包创建新的 Nx monorepo 工作区时，它会检查 Nx CLI 是否已安装。要安装 Nx CLI，请在终端中运行以下命令：

```
npm install -g nx
```

上述命令将在我们的系统上全局安装 nx npm 包。现在，我们可以使用以下命令搭建一个新的 Nx monorepo 工作区：

```
npx create-nx-workspace packt --appName=tour --preset=angular --style=css --linter=eslint --nx-cloud=false --routing
```

上述命令使用了 npx，这是一个 npm 命令行工具，允许我们在不预先安装的情况下执行 npm 包。它执行 create-nx-workspace 包，并传递以下选项：

- `packt`：Nx monorepo 工作区的名称。在大型企业环境中，我们通常使用组织名称。
- `--appName=tour`：应用程序的名称。
- `--preset=angular`：Nx 支持使用各种 JavaScript 框架构建的应用程序。preset 选项定义了我们想要构建的应用程序类型。
