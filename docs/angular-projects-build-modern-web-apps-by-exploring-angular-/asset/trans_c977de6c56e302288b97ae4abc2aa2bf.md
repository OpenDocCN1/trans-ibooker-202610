

+   3. 打开 `contact.component.html` 文件，并添加以下 HTML 内容：

```
```html
<div class="card mx-auto text-center border-light"
     style="width: 18rem;">
  <img src="assets/angular.png" class="card-img-top"
       alt="Angular logo">
  <div class="card-body">
    <h5 class="card-title">Angular Projects</h5>
    <p class="card-text">
      A personal blog created with the Angular
      framework and the Scully static site generator
    </p>
    <a href="https://angular.io/" target="_blank"
       class="card-link">Angular</a>
    <a href="https://scully.io/" target="_blank"
       class="card-link">Scully</a>
  </div>
</div>
```
```

在前面的代码中，我们使用了 `angular.png` 图像。你可以在随附 GitHub 仓库项目的 `src\assets` 文件夹中找到该图像。

> **提示**
> 
> Angular CLI 项目中的 assets 文件夹用于存放图像、字体或 JSON 文件等静态内容。

我们已经创建了 contact 功能。下一步是将其添加到 Angular 应用程序的主页中：

+   1. 打开 `app-routing.module.ts` 文件，并在 `routes` 属性中添加一个新的路由配置对象：

```
```typescript
import { ContactComponent } from './contact/contact.component';

const routes: Routes = [
  { path: 'contact', component: ContactComponent }
];
```
```

前面的代码表示，当浏览器的 URL 指向 contact 路径时，应用程序将激活 `ContactComponent`，并在屏幕上显示它。路由模块的 routes 属性包含相应功能模块的路由配置。它是一个路由配置对象数组，每个对象都会定义激活该路由的组件类和 URL 路径。

+   2. 将 ContactModule 添加到 AppModule 的 @NgModule 装饰器的 imports 数组中，以便使用它：

```
```typescript
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    CoreModule,
    SharedModule,
    ContactModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
```
```

不要忘记在文件顶部添加 ContactModule 相应的导入语句。

+   3. 路由组件和 ContactComponent 一样，需要一个可以加载它们的位置。打开 app.component.html 文件，并添加 router-outlet 指令：

```
```html
<app-header></app-header>
<div class="container">
  <router-outlet></router-outlet>
</div>
<app-footer></app-footer>
```
```

## 使用 Scully 和 Angular Router 构建 SPA 应用程序

现在，我们需要将创建的路由配置与 header 组件中的实际链接连接起来：

+   1. 打开 `header.component.html` 文件，并将 `routerLink` 指令添加到相应的锚点 HTML 元素：

```
```html
<li class="nav-item">
  <a routerLink="/contact" routerLinkActive="active"
     class="nav-link">Contact</a>
</li>
```
```

在前面的代码片段中，`routerLink` 指令指向路由配置对象的 path 属性。我们还添加了 `routerLinkActive` 指令，当特定路由被激活时，它会为锚点元素设置 active 类。

> **重要提示**
> 
> 请注意，`routerLink` 指令的值包含一个前导 `/`，而我们定义的路由配置对象的 path 属性并不包含。根据具体情况，省略 `/` 会赋予该路由不同的含义。

+   2. `routerLink` 和 `routerLinkActive` 指令属于 Angular router 软件包。要使用它们，我们需要在核心模块中导入 `RouterModule`：

```
```typescript
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { HeaderComponent } from './header/header.component';
import { RouterModule } from '@angular/router';

@NgModule({
  declarations: [
    HeaderComponent
  ],
  imports: [
    CommonModule,
    RouterModule
  ],
  exports: [
```
```
```
HeaderComponent
]
})
export class CoreModule { }
```

现在，我们已经准备好预览新的 contact 页面了！如果使用 `ng serve` 运行应用程序，然后点击 **Contact** 链接，应该会看到以下输出：

![](img/efea53d01337e7b223389b38dcdb0160_55_0.png)

*图 2.3 – Contact 页面*

在下一节中，我们将为博客页眉中的 **Articles** 链接构建相应功能。

## 添加 articles 页面

负责在博客中显示文章的功能将由 articles 模块实现。它还将负责连接 Angular 和 Scully。我们将使用 Angular CLI 的 generate 命令创建该模块：

```
ng generate module articles --route=articles --module=app-routing
```

在前面的命令中，我们传入了一些额外的路由选项：

+   - `--route`：定义功能的 URL 路径
+   - `--module`：指定将定义用于激活该功能的路由配置对象的路由模块

执行该命令时，Angular CLI 除了创建模块之外，还会执行以下操作：

+   - 在 `src\\app\\articles` 文件夹中创建一个路由组件，该组件默认会由路由导航对象激活。它是该功能的着陆页，并将显示博客文章列表，具体内容将在 *在主页上显示博客数据* 一节中介绍。
+   - 创建一个名为 `articles-routing.module.ts` 的路由模块，其中包含 articles 模块的路由配置。
+   - 在主应用程序模块的路由配置中添加一个新的路由配置对象，用于激活 articles 模块。

`articles-routing.module.ts` 文件包含 articles 模块的路由配置：

### articles-routing.module.ts

```
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { ArticlesComponent } from './articles.component';

const routes: Routes = [{ path: '', component: ArticlesComponent }];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class ArticlesRoutingModule { }
```

前面介绍的方法称为**懒加载**，可以改善 Angular 应用程序的启动速度和整体性能。它会为每个懒加载模块创建一个单独的软件包，并在需要时加载，从而减小最终软件包的大小并降低应用程序的内存消耗。现在，让我们将新路由连接到 header 组件：

+   1. 打开 `header.component.html` 文件，并将以下 `routerLink` 和 `routerLinkActive` 指令添加到 Articles 锚点 HTML 元素中：
+   
+   ```html
+   <li class="nav-item">
+       <a routerLink="/articles" routerLinkActive="active"
+          class="nav-link">Articles</a>
+   </li>
+   ```
+   
+   2. 运行 `ng serve`，并使用你喜欢的浏览器预览应用程序。
+   3. 打开浏览器的开发者工具，点击 **Articles** 链接，然后检查 **Network** 选项卡：

![](img/efea53d01337e7b223389b38dcdb0160_58_0.png)

在其他请求中，你应该会看到一个名为 **articles-articles-module.js** 的请求。它就是在点击 **Articles** 链接时加载的懒加载 articles 模块的软件包。

现在，我们已经准备好将这个出色的 Angular 应用程序转换为专业的博客网站了。在继续之前，让我们向 `app-routing.module.ts` 文件添加一些其他路由：

```
```typescript
const routes: Routes = [
  { path: 'contact', component: ContactComponent },
  { path: 'articles', loadChildren: () =>
      import('./articles/articles.module').then(m =>
      m.ArticlesModule) },
  { path: '', pathMatch: 'full', redirectTo: 'articles' },
  { path: '**', redirectTo: 'articles' }
];
```
```

我们添加了一个默认路由，以便博客用户访问博客时自动重定向到 `articles` 路径。此外，我们还创建了一个新的路由配置对象，并将其 path 设置为 `**`，该路由同样会导航到 `articles` 路径。`**` 语法称为**通配符路由**，当路由器无法将请求的 URL 与已定义的路由匹配时，就会触发该路由。

> **提示**
> 
> 先定义最具体的路由，然后再添加默认路由和通配符路由等通用路由。Angular 路由器会按照我们定义的顺序解析路由配置，并遵循 *第一个匹配项获胜* 的策略来选择路由。

我们已经在 Angular 应用程序中启用并配置了路由。在下一节中，我们将建立向应用程序添加博客功能所需的基础设施。

## 使用 Scully 添加博客功能

目前，我们的应用程序还没有任何与博客文章相关的特定逻辑。它只是一个使用路由的典型 Angular 应用程序。不过，通过添加路由配置，我们已经为使用 Scully 添加博客支持奠定了基础。

> **重要提示**
> 
> Scully 至少需要在 Angular 应用程序中定义一个路由，才能正常工作。

首先，我们需要在应用程序中安装 Scully。

