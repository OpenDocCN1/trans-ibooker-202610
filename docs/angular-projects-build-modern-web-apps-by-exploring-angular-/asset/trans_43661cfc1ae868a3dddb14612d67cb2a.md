

## 安装 Scully 库

我们将使用 Angular CLI 的 add 命令在 Angular 应用中安装 Scully：

```
ng add @scullyio/init
```

上述命令会下载并安装所有必要的 npm 包，使 Scully 能够在我们的 Angular 应用中正常工作。它还会修改 Angular 项目的结构，以满足其特定需求。

它会在我们的主应用模块中导入 `ScullyLibModule`：

### app.module.ts

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { ContactModule } from './contact/contact.module';
import { CoreModule } from './core/core.module';
import { SharedModule } from './shared/shared.module';
import { ScullyLibModule } from '@scullyio/ng-lib';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    CoreModule,
    SharedModule,
    ContactModule,
    ScullyLibModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

ScullyLibModule 是 Scully 库的主模块，其中包含 Scully 所需的各种 Angular 服务和指令。

它还会在 Angular CLI 工作区的根文件夹中为 Scully 库创建一个配置文件：

### scully.my-blog.config.ts

```typescript
import { ScullyConfig } from '@scullyio/scully';
export const config: ScullyConfig = {
  projectRoot: "./src",
  projectName: "my-blog",
  outDir: './dist/static',
  routes: {
  }
};
```

配置文件包含关于我们 Angular 应用的信息，Scully 在后续过程中会用到这些信息：

- projectRoot：包含 Angular 应用源代码的路径
- projectName：Angular 应用的名称
- outDir：Scully 生成文件的输出路径

> **重要提示**
>
> Scully 的输出路径必须与 Angular CLI 输出 Angular 应用包的路径不同。后者可以在 `angular.json` 文件中配置。

- routes：它包含用于访问我们博客文章的路由配置。Scully 会自动填充它，我们将在下一节中看到。

既然我们已经在 Angular 应用中成功安装了 Scully，现在可以配置它来初始化我们的博客了。

## 初始化我们的博客页面

Scully 提供了一个特定的 Angular CLI 原理图（schematic），用于通过 Markdown（`.md`）文件初始化 Angular 应用（如博客）：

```
ng generate @scullyio/init:markdown
```

上述命令将通过一系列问题来启动我们博客的配置过程（括号内显示的是默认值）：

1. 输入 `posts` 作为博客模块的名称：

   ```
   ? What name do you want to use for the module? (blog)
   ```

   *图 2.5 – 博客模块名称*

   这将创建一个名为 `posts` 的新 Angular 模块。

2. 将 slug 选项留空，然后按 Enter 接受默认值：

   ```
   ? What slug do you want for the markdown file? (id)
   ```

   *图 2.6 – Markdown slug*

   slug 是每篇文章的唯一标识符，它定义在模块的路由配置对象中。

3. 输入 `mdfiles` 作为 Scully 用来存储我们实际博客文章文件的路径：

   ```
   ? Where do you want to store your markdown files?
   ```

   *图 2.7 – Markdown 文件路径*

   这将在我们 Angular CLI 项目的根路径下创建一个 `mdfiles` 文件夹。默认情况下，它还会为我们创建一个博客文章。我们将在“在主页上显示博客数据”一节中学习如何创建我们自己的博客文章。

4. 输入 `posts` 作为访问我们博客文章的路由名称：

   ```
   ? Under which route do you want your files to be requested?
   ```

   *图 2.8 – 博客路由名称*

   路由名称是即将创建的路由配置对象的 `path` 属性。

在执行上述命令时，Scully 会执行多种操作，包括创建 posts 模块的路由配置：

### posts-routing.module.ts

```typescript
import {NgModule} from '@angular/core';
import {Routes, RouterModule} from '@angular/router';

import {PostsComponent} from './posts.component';

const routes: Routes = [
  {
    path: ':id',
    component: PostsComponent,
  },
  {
    path: '**',
    component: PostsComponent,
  }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule],
})
export class PostsRoutingModule {}
```

第一条路由的路径设置为 `:id`，并激活 `PostsComponent`。冒号字符表示 `id` 是一个路由参数。`id` 参数与前面在 Scully 配置中定义的 slug 属性相关。Scully 的工作方式是为我们创建的每篇博客文章生成一条路由。它使用 posts 模块和主应用模块的路由配置来构建 Scully 配置文件中的 `routes` 属性：

```javascript
routes: {
  '/posts/:id': {
    type: 'contentFolder',
    id: {
      folder: "./mdfiles"
    }
  }
}
```

PostsComponent 是用于渲染每篇博客文章详细信息的 Angular 组件。该组件的模板文件可以根据你的需求进一步自定义：

### posts.component.html

```html
<h3>ScullyIo content</h3>
<hr>

<!-- This is where Scully will inject the static HTML -->
<scully-content></scully-content>
<hr>
<h4>End of content</h4>
```

你可以自定义上述模板文件中的所有内容，除了 `<scully-content></scully-content>` 这一行，该行由 Scully 内部使用。

至此，我们已经在 Angular 应用中完成了 Scully 的安装和配置。现在是时候进行项目的最后一部分了！在下一节中，我们将让 Angular 与 Scully 协作，在 Angular 应用中显示博客文章。

## 在主页上显示博客文章

我们希望用户在进入我们的博客网站后，立即看到可用博客文章的列表。根据我们定义的默认路由路径，`ArticlesComponent` 是我们博客的着陆页。Scully 提供了 `ScullyRoutesService`，这是一个 Angular 服务，我们可以在组件中使用它来获取 Scully 将根据博客文章创建的路由信息。让我们在着陆页上使用这个服务：

1. 导航到 `articles.component.ts` TypeScript 类文件。
2. 从 `@scullyio/ng-lib` 包中导入 `ScullyRoute` 和 `ScullyRoutesService`：

   ```typescript
   import { ScullyRoute, ScullyRoutesService } from '@scullyio/ng-lib';
   ```

3. 在 `ArticlesComponent` 类的构造函数中注入 `ScullyRoutesService`：

   ```typescript
   constructor(private scullyService: ScullyRoutesService) {
   }
   ```

4. 创建一个 `ScullyRoute` 数组类型的组件属性：

   ```typescript
   posts: ScullyRoute[] = [];
   ```

5. 编辑组件的 `ngOnInit` 方法并添加以下代码：

   ```typescript
   ngOnInit(): void {
       this.scullyService.available$.subscribe(posts => {
           this.posts = posts.filter(post => post.title);
       });
   }
   ```

6. 打开 `articles.component.html` 文件并添加以下 HTML 代码：

   ```html
   <div class="list-group mt-3">
       <a *ngFor="let post of posts"
           [routerLink]="post.route" class="list-group-item
           list-group-item-action">
           <div class="d-flex w-100 justify-content-between">
               <h5 class="mb-1">{{post.title}}</h5>
           </div>
           <p class="mb-1">{{post.description}}</p>
       </a>
   </div>
   ```

前面的步骤涉及许多 Angular 技术，让我们逐一拆解。

当我们在组件中需要使用 Angular 服务时，只需向 Angular 框架请求它即可。怎么做？通过将它作为组件构造函数中的一个属性添加进去。组件不需要了解该服务是如何实现的。

`ngOnInit` 方法是 `OnInit` 接口的一部分，我们的组件实现了该接口。当组件被初始化时，Angular 框架会调用它，并为我们提供了一个钩子，用于添加要执行的自定义逻辑。

