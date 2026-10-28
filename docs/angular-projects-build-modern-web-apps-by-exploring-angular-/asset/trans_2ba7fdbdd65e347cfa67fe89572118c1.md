

-   4. 打开 `index.html` 文件，再次查看 `app-root` 元素。

这次主组件不是空的了。Angular Universal 已经向 GitHub API 发出了所有 HTTP 请求，并在运行时预获取了应用的内容。所有组件模板和样式都已预渲染到主 HTML 文件中，这实质上意味着即使不启用 JavaScript，我们也可以在浏览器中查看应用！

-   5. 执行以下命令，启动我们的 GitHub 作品集应用的预渲染版本：

```
npm run serve:ssr
```

上述命令将在 http://localhost:4000 启动应用。

-   6. 在浏览器设置中禁用 JavaScript，然后访问 http://localhost:4000。

我们创建的 GitHub 作品集应用在未启用 JavaScript 的情况下仍能完全正常运行。应用的主页面也能即时渲染，用户无需等待应用加载。

上述场景非常适合那些无法在设备上启用 JavaScript 的用户。但是，当启用了 JavaScript 的用户使用同一个预渲染版本的应用时，会发生什么呢？让我们进一步了解：

-   1. 在浏览器中启用 JavaScript，并切换开发者工具。

-   2. 访问 http://localhost:4000。乍一看似乎没有任何变化。不过，由于预渲染内容的存在，应用会立即加载。

-   3. 检查 Network 标签页，你会注意到以下内容：

![](img/efea53d01337e7b223389b38dcdb0160_239_0.png)

图 7.6 – Network 标签页（Google Chrome）

我们的应用会向 GitHub API 发起所有 HTTP 请求，就像它是从浏览器渲染出来的一样。它实质上重复了应用所需的所有 HTTP 请求，即使数据已经预渲染在 HTML 页面中。这是为什么呢？

应用会为浏览器渲染版本发起一个 HTTP 请求，并为 SSR 应用发起另一个请求，因为这两个版本的状态不同。我们可以通过在服务器和浏览器之间共享状态来避免上述行为。更具体地说，我们可以使用 Angular Universal 库中一个名为 TransferHttpCacheModule 的专用 Angular 模块，将服务器状态传输到浏览器。

如果我们使用 TransferHttpCacheModule，服务器将缓存来自 GitHub API 的响应，浏览器将使用缓存而不是发起新请求。TransferHttpCacheModule 通过在 Angular 应用中安装一个 HTTP 拦截器来解决该问题，该拦截器会忽略服务器最初已处理过的 HTTP 请求。

> **重要说明**
> HTTP 拦截器是一种 Angular 服务，用于拦截源自 Angular 框架内置 HTTP 客户端的 HTTP 请求和响应。

要在我们的 GitHub 作品集应用中安装 TransferHttpCacheModule，请按照以下步骤操作：

-   1. 打开 Angular 应用的主模块文件 `app.module.ts`，并从 @nguniversal/common npm 包中导入 TransferHttpCacheModule：

```
import { TransferHttpCacheModule } from '@nguniversal/common';
```

-   2. 将 TransferHttpCacheModule 类添加到 @NgModule 装饰器的 imports 数组中：

```
@NgModule({
  declarations: [
    AppComponent,
    PersonalInfoComponent,
    PanelComponent,
    RepositoriesComponent,
    OrganizationsComponent
],
imports: [
  BrowserModule.withServerTransition({ appId:
  'serverApp' }),
  HttpClientModule,
  TransferHttpCacheModule
],
providers: [],
bootstrap: [AppComponent]
})
```

-   3. 打开 SSR 应用的主模块文件 `app.server.module.ts`，并从 @angular/platform-server npm 包中导入 ServerTransferStateModule：

```
import { ServerModule, ServerTransferStateModule }
from '@angular/platform-server';
```

-   4. 将 ServerTransferStateModule 类添加到 @NgModule 装饰器的 imports 数组中：

```
@NgModule({
  imports: [
    AppModule,
    ServerModule,
    ServerTransferStateModule
  ],
  bootstrap: [AppComponent],
})
```

-   5. 执行以下命令来预渲染你的应用：

```
npm run prerender
```

-   6. 运行以下命令来启动你预渲染后的应用：

```
npm run serve:ssr
```

如果你预览作品集应用并检查浏览器的 **Network** 标签页，你会注意到它不会发起额外的 HTTP 请求。`TransferHttpCacheModule` 拦截了所有 HTTP 请求，并将它们存储在我们应用的 `TransferState` 存储中。TransferState 是一个键值存储，可以从服务器传输到浏览器。应用的浏览器版本之后可以直接从存储中读取 HTTP 响应，而无需额外发起调用。

我们现在拥有了 GitHub 作品集的完整预渲染版本。但如何进一步优化，以便能在社交媒体平台上分享呢？我们将在下一节中了解更多 SEO 优化技术。

## 增强 SEO 能力

**SEO** 是优化网站以便被网络爬虫正确索引的过程。网络爬虫是一种专用软件，存在于大多数搜索引擎中，能够识别网站并为其建立索引，使其易于通过搜索引擎平台被发现和链接。

Angular Universal 通过在构建时预渲染内容，在 SEO 方面做得非常出色。有些网络爬虫无法执行 JavaScript，也无法构建 Angular 应用的动态内容。使用 Angular Universal 进行预渲染消除了对 JavaScript 的需求，从而使网络爬虫能够尽其所能识别 Web 应用。

我们还可以通过在 Angular 应用的主 `index.html` 文件的 `head` 元素中定义几个标签来帮助 SEO，例如 `title`、`viewport` 和 `charset`：

## index.html

```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>GhPortfolio</title>
  <base href="/">
  <meta name="viewport" content="width=device-width,
    initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
</head>
<body>

<app-root></app-root>
</body>
</html>
```

你可以在 https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta/name 找到可用标签的列表。

然而，仅在 index.html 文件中设置标签并不足够，尤其是当 Angular 应用启用了路由并包含多个路由时。Angular 框架提供了几个方便的服务，我们可以用它们以编程方式设置标签。首先，让我们看看如何在应用中设置 title 标签：

-   1. 打开 `app.component.ts` 文件，并导入 Title 和 OnInit 这两个 Angular 构件：

```
import { Component, OnInit } from '@angular/core';
import { environment } from
'../environments/environment';
import { Title } from '@angular/platform-browser';
```

-   2. 将 Title 服务注入到 AppComponent 类的构造函数中：

```
export class AppComponent {
  username = environment.username;

  constructor(private title: Title) {}
}
```

-   3. 将 OnInit 接口添加到组件的实现接口列表中，并在 ngOnInit 方法中调用 title 变量的 setTitle 方法：

```
export class AppComponent implements OnInit {
  username = environment.username;

  constructor(private title: Title) {}

  ngOnInit() {
    this.title.setTitle('GitHub portfolio app');
  }
}
```

-   4. 运行 `npm run dev:ssr` 预览应用，你应该能在浏览器标签页中看到标题：

![](img/efea53d01337e7b223389b38dcdb0160_244_0.png)

图 7.7 – 浏览器标签页标题

与 Title 服务类似，我们可以使用 Meta 服务为应用设置 meta 标签：

-   1. 打开 `app.component.ts` 文件，并从 `@angular/platform-browser` npm 包中导入 `Meta`：

```
import { Meta } from '@angular/platform-browser';
```

-   2. 将 `Meta` 服务注入到 `AppComponent` 类的构造函数中：

```
constructor(private title: Title, private meta: Meta) {}
```

-   3. 使用 `meta` 变量的 `addTags` 方法向 `ngOnInit` 方法中添加一些 meta 标签：

```
ngOnInit(): void {
  this.title.setTitle('GitHub portfolio app');
  this.meta.addTags([
    {
      name: 'description',
      content: `${this.username}'s GitHub portfolio`
    },
    {
      name: 'author',
      content: this.username
    }
  ]);
}
```

在上述代码中，我们添加了两个 meta 标签。第一个设置 description（描述），其中包含当前 GitHub 个人资料的用户名。第二个将 author（作者）标签设置为与 GitHub 个人资料的用户名相同。

