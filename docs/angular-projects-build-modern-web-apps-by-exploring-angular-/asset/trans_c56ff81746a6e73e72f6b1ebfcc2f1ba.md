
+ 4. 打开新生成组件的模板文件 `editor.component.html`，并将其内容替换为以下 HTML 片段：

```html
<ngx-wig placeholder="Enter your content"></ngx-wig>
```

NgxWigModule 提供了一组可在我们的应用程序中使用的 Angular 服务和组件。该模块的主要组件是 `ngx-wig` 组件，它显示实际的所见即所得（WYSIWYG）编辑器。它暴露了一系列我们可以设置的输入属性，例如编辑器的占位符。你可以在 https://github.com/stevermeister/ngx-wig 找到更多选项。

+ 5. 打开 `app.component.html` 文件，并将其内容替换为 `app-editor` 选择器：

```html
<app-editor></app-editor>
```

+ 6. 打开 `styles.css` 文件（该文件包含 Angular 应用程序的全局样式），并添加以下样式，使编辑器可停靠并占据整个页面：

```css
html, body {
  margin: 0;
  width: 100%;
  height: 100%;
}

.ng-wig, .nw-editor-container, .nw-editor {
  display: flex !important;
  flex-direction: column;
  height: 100% !important;
  overflow: hidden;
}
```

+ 7. 打开 Angular 应用程序的主 HTML 文件 `index.html`，并从 `head` 元素中移除 `base` 标签。浏览器使用 `base` 标签来引用具有相对 URL 的脚本和 CSS 文件。若保留 `base` 标签，我们的桌面应用程序将无法运行，因为它会直接从本地文件系统加载所有必要的资源。我们将在“*将 Angular 与 Electron 集成*”部分进一步了解。

让我们看看目前已经实现了什么。运行 `ng serve` 并导航到 http://localhost:4200 来预览应用程序：

![](img/efea53d01337e7b223389b38dcdb0160_148_0.png)

图 5.1 – 应用程序输出

我们的应用程序由以下部分组成：

- 一个带有按钮的工具栏，允许我们对编辑器内容应用不同的样式
- 一个文本区域，用作向编辑器添加内容的主要容器

至此，我们已经使用 Angular 创建了一个包含功能完备的所见即所得编辑器的 Web 应用程序。在下一节中，我们将学习如何使用 Electron 将其转换为桌面应用程序。

## 在工作区中集成 Electron

Electron 框架是一个 npm 包，我们可以使用以下命令安装它：

```bash
npm install -D electron
```

上述命令会将最新版本的 Electron npm 包安装到 Angular CLI 工作区中。它还会在我们的项目的 `package.json` 文件的 `devDependencies` 部分中添加一个相应的条目。

> **重要说明**
>
> Electron 被添加到 `package.json` 文件的 `devDependencies` 部分，因为它是我们应用程序的开发依赖项。它仅用于准备和构建桌面版应用程序，而不在运行时使用。

Electron 应用程序在 Node.js 运行时上运行，并使用 Chromium 浏览器进行渲染。Node.js 应用程序至少有一个 JavaScript 文件，通常称为 `index.js` 或 `main.js`，它是应用程序的主入口点。由于我们使用 Angular 和 TypeScript 作为开发技术栈，我们首先创建一个相应的 TypeScript 文件，该文件最终将被编译为 JavaScript：

+ 1. 在 Angular CLI 工作区的 `src` 文件夹中创建一个名为 `electron` 的文件夹。`electron` 文件夹将包含与 Electron 相关的所有源代码。

> 提示
>
> 我们可以将应用程序视为两个不同的平台。Web 平台是 Angular 应用程序，位于 `src\app` 文件夹中。桌面平台是 Electron 应用程序，位于 `src\electron` 文件夹中。这种方法有很多好处，包括在我们的应用程序中强制实现关注点分离，并且允许每个平台独立开发。从现在开始，我们将它们分别称为 Angular 应用程序和 Electron 应用程序。

+ 2. 在 `electron` 文件夹中创建一个 `main.ts` 文件，内容如下：

```typescript
import { app, BrowserWindow } from 'electron';

function createWindow () {
    const mainWindow = new BrowserWindow({
        width: 800,
        height: 600
    });

    mainWindow.loadFile('index.html');
}

app.whenReady().then(() => {
    createWindow();
});
```

我们首先从 `electron` npm 包中导入 `BrowserWindow` 和 `app` 构件。`BrowserWindow` 类用于为我们的应用程序创建桌面窗口。我们通过在其构造函数中传入一个选项对象来定义窗口尺寸，该对象设置窗口的 `width` 和 `height` 值。然后我们调用 `loadFile` 方法，将我们想要在窗口中加载的 HTML 文件作为参数传入。

> **重要说明**
>
> 我们在 `loadFile` 方法中传入的 `index.html` 文件是 Angular 应用程序的主 HTML 文件。它将通过 `file://` 协议加载，这就是我们在“*添加一个所见即所得的 Angular 库*”部分中移除 `base` 标签的原因。

`app` 对象是桌面应用程序的全局对象，就像网页上的 `window` 对象一样。它暴露了一个 `whenReady` Promise，当该 Promise 被解析时，意味着我们可以运行应用程序的任何初始化逻辑，包括创建窗口。

+ 3. 在 `electron` 文件夹中创建一个 `tsconfig.json` 文件，并添加以下内容：

```json
{
  "extends": "../../tsconfig.json",
  "compilerOptions": {
    "importHelpers": false
  },
  "include": [
    "**/*.ts"
  ]
}
```

`main.ts` 文件需要被编译为 JavaScript，因为浏览器目前不理解 TypeScript。这个编译过程称为**转译**，并且需要一个 TypeScript 配置文件。该配置文件包含驱动 TypeScript **转译器**的选项，转译器负责转译过程。

前面的 TypeScript 配置文件使用 `include` 属性定义了 Electron 源代码文件的路径，并将 `importHelpers` 属性设置为 false。

> **重要说明**
>
> 如果我们启用 `importHelpers` 标志，它将把 `tslib` 库中的辅助函数包含到我们的应用程序中，从而导致更大的打包体积。

4. 运行以下命令来安装 **webpack 命令行工具**：

```bash
npm install -D webpack-cli
```

webpack 命令行工具用于从命令行调用 **webpack**——一个流行的模块打包器。我们将使用 webpack 来构建和打包我们的 Electron 应用程序。

5. 使用以下 npm 命令安装 ts-loader npm 包：

```bash
npm install -D ts-loader
```

ts-loader 库是一个 webpack 插件，可以加载 TypeScript 文件。

现在，我们已经创建了使用 Electron 将 Angular 应用程序转换为桌面应用程序所需的各个部分。我们只需要将它们组合在一起，以便能够构建和运行桌面应用程序。编排 Electron 应用程序的主要部分是 webpack 配置文件，我们需要在 Angular CLI 工作区的根文件夹中创建该文件：

## webpack.config.js

```javascript
const path = require('path');
const src = path.join(process.cwd(), 'src', 'electron');

module.exports = {
  mode: 'development',
  devtool: 'source-map',
  entry: path.join(src, 'main.ts'),
  output: {
    path: path.join(process.cwd(), 'dist', 'my-editor'),
    filename: 'shell.js'
  },
  module: {
    rules: [
      {
        test: /\.ts$/,
        loader: 'ts-loader',
        options: {
          configFile: path.join(src, 'tsconfig.json')
        }
      }
    ]
  },
  target: 'electron-main'
};
```

前面的文件使用以下选项在我们的应用程序中配置 webpack：

- `mode`：表示我们当前正在开发环境中运行。
- `devtool`：启用 source map 文件生成，用于调试。
- `entry`：表示 Electron 应用程序的主入口点，即 `main.ts` 文件。
- `output`：定义 webpack 将生成的 Electron 包的路径和文件名。`path` 属性指向与 Angular CLI 用于生成 Angular 应用程序包相同的文件夹。`filename` 属性设置为 `shell.js`，因为 webpack 生成的默认文件名是 `main.js`，这将会与 Angular 应用程序生成的 `main.js` 文件冲突。
- `module`：指示 webpack 加载 ts-loader 插件来处理 TypeScript 文件。
- `target`：表示我们当前正在 Electron 的主进程中运行。

现在 webpack 包含了构建和打包 Electron 应用程序所需的全部信息。另一方面，Angular CLI 负责构建 Angular 应用程序。让我们看看如何将它们组合起来并运行我们的桌面应用程序：

+ 1. 运行以下 npm 命令来安装 concurrently npm 包：

```bash
npm install -D concurrently
```

concurrently 库使我们能够并发执行多个进程。在我们的场景中，它将使我们能够并行运行 Angular 和 Electron 应用程序。

