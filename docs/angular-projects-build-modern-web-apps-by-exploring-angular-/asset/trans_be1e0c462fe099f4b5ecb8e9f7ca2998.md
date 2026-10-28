

```
myContent = '';

constructor(private editorService: EditorService) {}

ngOnInit(): void {
}
}
```

7. 创建一个调用 editorService 变量的 getContent 方法并在 ngOnInit 方法内部执行它：

```ts
ngOnInit(): void {
  this.getContent();
}

private async getContent() {
  this.myContent = await
    this.editorService.getContent();
}
```

我们使用 async/await 语法，这允许我们在基于 promise 的方法调用中同步执行代码。

8. 创建一个调用 editorService 变量的 setContent 方法：

```ts
saveContent(content: string) {
  this.editorService.setContent(content);
}
```

9. 让我们将我们创建的这些方法与组件的模板连接起来。打开 editor.component.html 文件并添加以下绑定：

```html
<ngx-wig placeholder="Enter your content"
  [ngModel]="myContent"
  (contentChange)="saveContent($event)"></ngx-wig>
```

## 与文件系统交互

主进程使用内置于 Electron 框架中的 `fs` Node.js 库与文件系统交互。让我们看看如何使用它：

1. 打开 src/electron 文件夹中存在的 main.ts 文件并导入以下构件：

```ts
import { app, BrowserWindow, ipcMain } from 'electron';
import * as fs from 'fs';
import * as path from 'path';
```

`fs` 库负责与文件系统交互。`path` 库提供用于处理文件和文件夹路径的实用工具。`ipcMain` 对象允许我们使用 Electron 的主进程。

2. 创建一个变量来保存包含编辑器内容的文件的路径：

```ts
const contentFile = path.join(app.getPath('userData'), 'content.html');
```

保存编辑器内容的的文件是存在于保留的 userData 文件夹中的 content.html 文件。userData 文件夹是每个操作系统的特殊用途系统文件夹的别名，用于存储应用程序特定的文件，例如配置。您可以在 https://www.electronjs.org/docs/api/app#appgetpathname 找到有关 userData 文件夹以及其他系统文件夹的更多详细信息。

> **重要说明**
>
> `app` 对象的 `getPath` 方法跨平台工作，用于获取特殊文件夹的路径，例如用户的主目录或应用程序数据。

3. 调用 `ipcMain` 对象的 `handle` 方法开始监听 getContent 通道中的请求：

```ts
ipcMain.handle('getContent', () => {
    if (fs.existsSync(contentFile)) {
        const result = fs.readFileSync(contentFile);
        return result.toString();
    }
    return '';
});
```

当主进程在此通道中接收到请求时，它使用 `fs` 库的 `existsSync` 方法检查编辑器内容的 文件是否已存在。如果存在，它使用 `readFileSync` 方法读取它并将其内容返回给渲染进程。

4. 再次调用 `handle` 方法，但这次针对 setContent 通道：

```ts
ipcMain.handle('setContent', (event, content: string) => {
  fs.writeFileSync(contentFile, content);
});
```

在前面的代码片段中，我们使用 `fs` 库的 `writeFileSync` 方法将 content 属性的值写入文件中。

5. 打开 package.json 文件并更改 `@types/node` npm 包的版本：

```json
"@types/node": "^15.6.0"
```

现在我们已经连接了 Angular 和 Electron 应用程序，是时候预览我们的 WYSIWYG 桌面应用程序了：

1. 执行 start:desktop npm 脚本并按 F5 运行应用程序。

2. 使用编辑器及其工具栏输入一些内容，例如以下内容：

![](img/efea53d01337e7b223389b38dcdb0160_163_0.png)

图 5.5 – 编辑器内容

3. 关闭应用程序窗口并重新运行应用程序。如果一切正常，您应该在编辑器中看到您输入的内容。

恭喜！您已通过为其添加持久化功能来丰富了您的 WYSIWYG 编辑器。在下一节中，我们将朝着创建桌面应用程序的最后一步迈进，并学习如何打包和分发它。

## 打包桌面应用程序

Web 应用程序通常捆绑并部署到托管它们的 Web 服务器。另一方面，桌面应用程序被捆绑和打包为单个可执行文件，可以轻松分发。打包我们的 WYSIWYG 应用程序需要以下步骤：

- 为生产模式配置 webpack
- 使用 Electron 打包器

我们将在以下各节中更详细地介绍它们。

### 为生产配置 webpack

我们已经为开发环境创建了 webpack 配置文件。现在我们需要为生产环境创建一个新的。两个配置文件将共享一些功能，所以让我们先创建一个公共的：

1. 在 Angular CLI 工作区的根文件夹中创建 webpack.dev.config.js 文件，其内容如下：

```js
const path = require('path');
const baseConfig = require('./webpack.config');

module.exports = {
  ...baseConfig,
  mode: 'development',
  devtool: 'source-map',
  output: {
    path: path.join(process.cwd(), 'dist', 'my-editor'),
    filename: 'shell.js'
  }
};
```

2. 从 webpack.config.js 文件中删除 `mode`、`devtool` 和 `output` 属性。

3. 打开 `package.json` 文件并在 `start:desktop` 脚本中传递新的 webpack 开发配置文件：

```json
"start:desktop": "concurrently \"ng build --delete-output-path=false --watch\" \"webpack --config webpack.dev.config.js --watch\""
```

4. 在 Angular CLI 工作区的根文件夹中创建 webpack.prod.config.js 文件，其内容如下：

```js
const path = require('path');
const baseConfig = require('./webpack.config');

module.exports = {
  ...baseConfig,
  output: {
    path: path.join(process.cwd(), 'dist', 'my-editor'),
    filename: 'main.js'
  }
};
```

与开发环境的 webpack 配置文件的主要区别是我们将输出 bundle 的文件名更改为 `main.js`。Angular CLI 在生产环境中为 Angular 应用程序的 `main.js` 文件添加哈希号，因此不会有冲突。其他需要注意的是，当我们省略它时，mode 默认设置为 `production`，并且缺少 `devtool` 属性，因为我们不希望在生产模式下启用 source map。

5. 在 `package.json` 文件的 `scripts` 属性中添加一个新条目，用于在生产模式下构建我们的应用程序：

```json
"scripts": {
  "ng": "ng",
  "start": "ng serve",
  "build": "ng build",
  "watch": "ng build --watch --configuration development",
  "test": "ng test",
  "start:desktop": "concurrently \"ng build --delete-output-path=false --watch\" \"webpack --config webpack.dev.config.js --watch\"",
  "build:electron": "ng build && webpack --config webpack.prod.config.js"
}
```

build:electron 脚本同时在生产模式下构建 Angular 和 Electron 应用程序。

我们已经完成了打包桌面应用程序所需的所有配置。在下一节中，我们将学习如何将其转换为特定于每个操作系统的单个 bundle。

### 使用 Electron 打包器

Electron 框架拥有由开源社区创建和维护的各种工具。

您可以在以下链接的 Tools 部分看到可用项目的列表：

https://www.electronjs.org/community

其中一个工具是 electron-packager 库，我们可以使用它将桌面应用程序打包为每个操作系统的单个可执行文件（Windows、Linux 和 macOS）。让我们看看如何将其集成到我们的开发工作流程中：

1. 运行以下 npm 命令将 electron-packager 作为开发依赖项安装到我们的项目中：

```bash
npm install -D electron-packager
```

2. 在 `package.json` 文件的 `scripts` 属性中添加一个新条目用于打包我们的应用程序：

```json
"scripts": {
  "ng": "ng",
  "start": "ng serve",
  "build": "ng build",
  "watch": "ng build --watch --configuration development",
  "test": "ng test",
  "start:desktop": "concurrently \"ng build --
