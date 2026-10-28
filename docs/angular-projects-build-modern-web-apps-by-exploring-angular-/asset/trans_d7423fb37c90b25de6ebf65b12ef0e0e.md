
我们的应用程序现在为 GitHub 用户的个人资料提供了完整的作品集功能。它显示以下内容：

- 个人信息、简短传记和社交媒体链接
- 用户公开仓库的列表，其中包含指向每个仓库的链接以获取更多信息
- 用户所属组织的列表，其中包含指向每个组织的链接以查看更多详细信息

在下一节中，我们将学习如何集成 Angular Universal 并在服务器上渲染我们的应用程序。

## 集成 Angular Universal

Angular Universal 是一个 Angular 库，它使 Angular CLI 应用程序能够在服务器上渲染。SSR 应用会提高 Angular 应用的加载速度，并改善首屏加载。

要在现有的 Angular CLI 应用程序中安装 Angular Universal，我们将使用 Angular CLI 的以下命令：

```
ng add @nguniversal/express-engine
```

前面的命令使用 Angular CLI 的 `ng add` 命令来安装 `@nguniversal/express-engine` npm 包。`@nguniversal/express-engine` 包是 Angular Universal 库的核心，其核心是一个 **Node.js Express** Web 服务器。

当我们执行上述命令安装 Angular Universal 时，我们不仅在安装该库，还会使用以下文件修改我们的 Angular CLI 工作区：

- `angular.json`：这会在 architect 部分创建新条目，以构建和启用我们的 Angular Universal 应用程序。其中一个条目是 `server` 属性，它负责使用 SSR 构建我们的应用程序。它会将生成的 bundle 输出到 Angular CLI 应用标准输出文件夹内的单独 `server` 文件夹中：

```
"options": {
  "outputPath": "dist/gh-portfolio/server",
  "main": "server.ts",
  "tsConfig": "tsconfig.server.json",
  "inlineStyleLanguage": "scss"
}
```

原始应用程序的 bundle 现在会生成到标准输出文件夹内的 browser 文件夹中：

```
"options": {
  "outputPath": "dist/gh-portfolio/browser",
  "index": "src/index.html",
  "main": "src/main.ts",
  "polyfills": "src/polyfills.ts",
  "tsConfig": "tsconfig.app.json",
  "inlineStyleLanguage": "scss",
  "assets": [
    "src/favicon.ico",
    "src/assets"
  ],
  "styles": [
    "src/styles.scss"
  ],
  "scripts": []
}
```

因此，Angular Universal 应用程序会生成同一 Angular 应用的两个版本：一个用于服务器，另一个用于浏览器。

- `package.json`：这会添加所有必要的 npm 依赖，并创建一组 npm 脚本，用于开始使用 Angular Universal 构建：

```
"scripts": {
  "ng": "ng",
  "start": "ng serve",
  "build": "ng build",
  "watch": "ng build --watch --configuration development",
  "test": "ng test",
  "dev:ssr": "ng run gh-portfolio:serve-ssr",
  "serve:ssr": "node dist/gh-portfolio/server/main.js",
  "build:ssr": "ng build && ng run gh-portfolio:server",
  "prerender": "ng run gh-portfolio:prerender"
}
```

包含 :ssr 后缀的脚本与构建和提供 Angular Universal 应用程序有关。prerender 脚本将在构建期间创建 Angular 应用的预渲染版本。我们将在“在构建期间预渲染内容”一节中了解 prerender 脚本。

- `server.ts`：这包含 Node.js Express 应用程序，它将托管我们作品集的服务器端渲染版本。
- `main.server.ts`：这是我们 Angular Universal 应用程序的主入口点。
- `app.server.module.ts`：这是服务器端渲染应用程序的主应用模块。
- `tsconfig.server.json`：这是我们 Angular Universal 应用程序的 TypeScript 配置。
- `main.ts`：我们 Angular 应用的主入口点仅在 DOM 完全加载后才加载主应用模块：

```
document.addEventListener('DOMContentLoaded', () => {
  platformBrowserDynamic().bootstrapModule(AppModule)
    .catch(err => console.error(err));
});
```

- `app.module.ts`：这调用 BrowserModule 的 withServerTransition 方法，将我们的应用程序作为服务器端渲染的应用加载：

```
@NgModule({
  declarations: [
    AppComponent,
    PersonalInfoComponent,
    PanelComponent
    RepositoriesComponent,
    OrganizationsComponent
  ],
  imports: [
    BrowserModule.withServerTransition({ appId:
      'serverApp' }),
    HttpClientModule
  ],
  providers: [],
```

```
bootstrap: [AppComponent]
})
export class AppModule { }
```

> **重要提示**
> 在服务器上渲染 Angular 应用时，由于没有浏览器，`window` 和 `document` 等全局 JavaScript 对象不可用。Angular 为其中一些对象提供了抽象 API，例如 `DOCUMENT` 注入令牌。如果您需要按条件启用它们，可以注入 `PLATFORM_ID` 令牌，并使用 `@angular/common` npm 包中的 `isPlatformServer` 或 `isPlatformBrowser` 方法来检查您的应用程序当前运行在哪个平台上：

```
import { Inject, PLATFORM_ID } from '@angular/core';
import { isPlatformBrowser } from '@angular/common';

export class CheckPlatformComponent {
  isBrowser: boolean;

  constructor( @Inject(PLATFORM_ID) platformId: any) {
    this.isBrowser = isPlatformBrowser(platformId);
  }
}
```

现在我们可以使用以下 npm 命令在服务器上运行我们的 GitHub 作品集应用：

```
npm run dev:ssr
```

要在服务器上预览您的 GitHub 作品集应用，请在浏览器中打开 http://localhost:4200。

通常情况下，您会看到与之前一样的应用。那么，我们在这里获得了什么呢？Angular Universal 应用在功能强大的处理器和大内存的开发机上运行时，并不会展现出全部潜力。相反，我们需要在真实场景（如慢速网络）中运行和预览它们。我们可以使用 **Google Chrome 开发者工具** 在开发环境中模拟慢速网络：

- 1. 打开 Google Chrome 浏览器。
- 2. 切换开发者工具并选择 **网络** 选项卡。
- 3. 从 **节流** 下拉菜单中选择 **慢速 3G** 选项。
- 4. 在浏览器地址栏中输入 `http://localhost:4200`。

如果在页面加载时点击任何链接，您会注意到没有任何反应。这是为什么呢？服务器首先加载应用的一个静态版本显示给用户，同时在后台等待实际的 Angular 应用加载。在此 FCP 期间，除 `routerLink` 指令外，所有链接都没有响应。当 Angular Universal 在后台完全加载后，它会切换到完整应用。

稍后，在 *使用 preboot 重放事件* 一节中，我们将了解如何处理这些事件。与此同时，我们将在下一节中研究如何使用预渲染来进一步提高应用程序的加载速度。

## 在构建期间预渲染内容

我们的 Angular CLI 工作区的 `package.json` 文件包含 prerender npm 脚本，我们可以使用它来改善应用程序的首次加载。该脚本会运行 `angular.json` 配置文件中 architect 部分的 prerender 命令，并在构建期间预渲染应用程序的内容。让我们看看预渲染会对我们的 GitHub 作品集应用产生什么效果：

- 1. 执行以下 npm 命令以生成应用的预渲染版本：

```
npm run prerender
```

> **重要提示**
> 
> 生产环境文件中的 username 和 apiUrl 属性应该正确设置。否则，该命令将在 VSCode 的终端窗口中输出错误。

上述命令会将应用程序的生产版 bundle 输出到 `dist\gh-portfolio\browser` 文件夹中。

- 2. 导航到 `dist\gh-portfolio\browser` 文件夹，您应该会看到两个 HTML 文件：`index.html` 和 `index.original.html` 文件。
- 3. 打开 `index.original.html` 文件并找到 `app-root` HTML 元素。这是我们 Angular 应用的主组件，Angular 将在浏览器中渲染我们应用的内容。

