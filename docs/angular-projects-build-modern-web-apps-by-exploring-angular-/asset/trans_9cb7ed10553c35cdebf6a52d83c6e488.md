

7. 实现一个 ngDoBootstrap 钩子方法来为 CopyButtonComponent 类创建自定义元素，如下所示：

```
export class AppModule {

  constructor(private injector: Injector) {}

  ngDoBootstrap() {
    const el =
      createCustomElement(CopyButtonComponent,
        { injector: this.injector });
    customElements.define('copy-button', el);
  }
}
```

ngDoBootstrap 方法用于挂钩 Angular 应用的手动启动过程。我们使用来自 @angular/elements npm 包的 createCustomElement 方法来创建一个自定义元素，传入组件的类和注入器。最后，我们使用 customElements 对象的 define 方法来声明自定义元素，传入我们想要使用的 HTML 选择器和自定义元素作为参数。

现在我们已经实践了将 Angular 组件转换为 Angular 元素的全部流程，是时候构建它，以便在 Web 应用中使用它了。

构建 Angular 元素与构建标准的 Angular 应用不同。当我们构建 Angular 应用时，Angular CLI 会生成包含应用源代码、Angular 框架和任何第三方库的不同 JavaScript 包。在 Angular 元素场景中，我们只想生成一个包含我们组件的包文件。为此，我们将使用 ngx-build-plus 库，它可以生成单个包等。让我们看看如何安装它并在我们的应用中使用它，如下所示：

1. 执行以下 Angular CLI 命令来安装 ngx-build-plus 包：

```
ng add ngx-build-plus --project=ui-elements
```

前面的命令将修改 Angular CLI 工作区的 angular.json 文件，以使用 ngx-build-plus 库来构建 ui-elements 应用。

2. 运行以下 Angular CLI 构建命令来构建应用：

```
ng build ui-elements --single-bundle
```

前面的命令将构建 ui-elements 应用并为所有应用代码生成一个单一包。

3. 将 dist\ui-elements 文件夹复制到硬盘上你选择的另一个位置并使用你的集成开发环境（IDE）打开 index.html 文件。

4. 从 head 元素中移除 <base> 标签并使用内容分发网络（CDN）添加 Bulma CSS 压缩文件，如下所示：

```
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.2/css/bulma.min.css">
```

5. 在 body 元素中用以下 HTML 片段替换 app-root 选择器：

```
<div class="container is-fluid">
<h1 class="title">My Angular Element</h1>
<copy-button></copy-button>
</div>
```

在前面的片段中，我们添加了一个使用 Bulma CSS 类样式化的 div 元素，以及我们在 AppModule 中定义的 Angular 元素的选择器。

6. 在 div 元素之后插入以下 JavaScript 代码：

```
<script>
const el = document.getElementsByTagName(
'copy-button')[0];
el.setAttribute('data', 'Some data');
el.addEventListener('copied', () => alert('Copied to
clipboard'));
</script>
```

在前面的脚本中，我们使用原生 JavaScript 与隐藏在 Angular 元素后面的组件进行通信。首先，我们查询全局 document 对象以获取对 Angular 元素的引用。然后，我们使用元素的 setAttribute 方法设置 data 输入属性。最后，我们通过使用 addEventListener 方法附加事件监听器来监听 copied 输出事件。

7. 使用你的浏览器打开 `index.html` 文件，你应该会看到以下输出：

![](img/efea53d01337e7b223389b38dcdb0160_315_0.png)

图 9.6 – Angular 元素

8. 点击 **Copy** 按钮，你应该会看到以下警告对话框：

![](img/efea53d01337e7b223389b38dcdb0160_315_1.png)

图 9.7 – 警告对话框

我们已经成功地将我们 UI 组件库中的一个 Angular 组件作为原生 HTML 元素用在一个与 Angular 无关的 Web 应用中！自定义元素的外观和行为与它的 Angular 对应版本相同。唯一的区别是我们如何使用纯 JavaScript 在 Web 应用中设置和配置自定义元素。

## Summary

在本项目中，我们构建了一个组件 UI 库，可以在我们的 Angular 应用中使用。最初，我们学习了如何使用 Angular CLI 创建 Angular 库。我们搭建了一个新的 Angular CLI 工作区，其中包含我们的 Angular 库，以及用于测试它的 Angular 应用。

然后我们使用 Angular CDK 和 Bulma CSS 框架构建了我们库的 UI 组件。我们创建了一个可以使用拖放功能重新排序的卡片列表，以及一个用于将内容复制到剪贴板的按钮。

我们还看到了如何将我们的库发布到 npm 仓库以在其他 Angular 项目中使用它。最后，我们使用 Angular Elements 将其转换为自定义元素以分发给非 Angular 应用。

在下一项目中，即本书的最后一个项目，我们将学习如何自定义 Angular CLI 来创建我们的生成 schematics。

## Essential background theory and context

Angular schematics 是可以使用 npm 安装的库。它们用于各种情况，包括创建具有标准用户界面的组件，甚至在组织内部强制执行约定和编码指南。schematic 可以独立使用，也可以作为现有 Angular 库的伴侣。

Angular schematics 被打包到集合中，它们位于 @schematics/angular npm 包中。当我们使用 Angular CLI 运行 ng add 或 ng build 命令时，它会运行该包中的相应 schematic。Angular CLI 目前支持以下类型的 schematics：

- **Add schematic**：这用于使用 ng add 命令在 Angular CLI 工作区中安装 Angular 库。
- **Update schematic**：这用于使用 ng update 命令更新 Angular 库。
- **Generate schematic**：这用于使用 ng generate 命令在 Angular CLI 工作区中生成 Angular 工件。

在本项目中，我们将专注于 generate schematics，但相同的规则适用于所有其他命令。

## Project overview

在本项目中，我们将学习如何使用 schematics API 构建自定义 Angular CLI generate schematics 来创建组件和服务。首先，我们将构建一个用于创建在模板中使用 Tailwind CSS 框架的 Angular 组件的 schematic。然后，我们将创建一个 schematic 来生成一个 Angular 服务，该服务默认注入内置 HTTP 客户端并为 CRUD 操作中的每个 HTTP 请求创建一个方法。

构建时间：1 小时

## Getting started

完成本项目需要以下先决条件和软件工具：

- Angular CLI：Angular 的命令行界面，可在 https://angular.io/cli 找到。
- GitHub 资料：本章的代码可在 https://github.com/PacktPublishing/Angular-Projects-Second-Edition 的 Chapter10 文件夹中找到。

## Installing the schematics CLI

schematics CLI 是一个命令行界面，我们可以使用它与 schematics API 交互。要安装它，运行以下 npm 命令：

```bash
npm install -g @angular-devkit/schematics-cli
```

前面的命令将在我们的系统上全局安装 @angular-devkit/schematics-cli npm 包。然后我们可以使用 schematics 可执行文件创建一个新的 schematics 集合：

```bash
schematics blank my-schematics
```

前面的命令将生成一个名为 my-schematics 的 schematics 项目。它默认在 src 文件夹内包含一个同名的 schematic。schematic 包含以下文件：

- `collection.json`：描述属于 my-schematics 集合的 schematics 的 JSON 模式
- `my-schematics\index.ts`：my-schematics schematic 的主要入口点
- `my-schematics\index_spec.ts`：my-schematics schematic 主要入口点的单元测试文件

集合的 JSON 模式文件为与该集合关联的每个 schematic 包含一个条目：

### collection.json

```json
{
  "$schema": "../node_modules/@angular-devkit/schematics/collection-schema.json",
  "schematics": {
    "my-schematics": {
      "description": "A blank schematic.",
      "factory": "./my-schematics/index#mySchematics"
    }
  }
}
```

