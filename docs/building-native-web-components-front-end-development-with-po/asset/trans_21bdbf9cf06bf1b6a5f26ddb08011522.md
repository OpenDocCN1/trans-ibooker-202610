
### 列表 6-15. 创建 style.css

```
.fab {
  width: 50px;
  height: 50px;
  background-color: black;
  border-radius: 50%;
  border: 1px solid black;
  transition: all 0.1s ease-in-out;

  font-size: 30px;
  color: white;
  text-align: center;
  line-height: 50px;

  position: fixed;
  right: 20px;
  bottom: 20px;
}

.fab:hover {
  box-shadow: 0 6px 14px 0 #666;
  transform: scale(1.05);
}
```

现在我们的 NoteApp 已完成，看起来像图 6-9。

![](img/e276a43da8123ca0974f10d5c85948b9_97_0.png)

*图 6-9. Google Chrome 中的 NoteApp*

你可以访问本书的源代码 (https://github.com/carlosrojaso/apress-book-web-components) 在 `$git checkout chap-6`。

## 摘要

在本章中，你学习了

- 如何在真实的 Web 应用中设计组件
- 如何让所有组件通过属性和自定义事件协同工作
- 如何将我们的应用连接到 API

## 第 7 章

### 分发 Web 组件

在本章中，你将学习如何将我们的 Web 组件发布到 npm。你还将了解 Web Components API 提供的浏览器支持，如何添加 polyfill 以支持更多 Web 浏览器，以及如何添加 Webpack 和 Babel 来处理和准备我们的 Web 组件以供发布。

### 发布到 npm

在第 6 章中，我们创建了三个组件：`<simple-form-modal-component>`、`<note-list-component>` 和 `<note-list-item-component>`。现在我们将通过 www.npmjs.com 使这些组件可用。npm 是一个包仓库，我们可以用命令 `$npm install <package>` 轻松地将其添加到项目中。

首先，我们需要一个账户来发布包（参见图 7-1）。

![](img/e276a43da8123ca0974f10d5c85948b9_99_0.png)

图 7-1. 在 npmjs.com 创建用户账户

接下来，我们必须使用 $npm adduser 将我们的账户与终端连接起来（图 7-2）。

```
carlosrojaso@Carloss-MacBook-Pro apress-book-web-components % npm adduser
Username: carlosrojaso
Password:
Email: (this IS public) ing.carlosandresrojas@pm.me
Logged in as carlosrojaso on https://registry.npmjs.org/.
carlosrojaso@Carloss-MacBook-Pro apress-book-web-components %
```

图 7-2. 将我们的终端与 npm 连接

现在我们必须分离我们的组件并使它们成为模块，其结构如图所示 7-3。

![](img/e276a43da8123ca0974f10d5c85948b9_100_0.png)

### 图 7-3. 发布组件的结构

我们为组件创建一个 package.json 文件，如列表 7-1 所示。

### 列表 7-1. simple-form-modal-component 的 package.json

```
{
  "name": "apress-simple-form-modal-component",
  "version": "1.0.1",
  "description": "simple form modal component",
  "main": "src/index.js",
  "module": "src/index.js",
  "directories": {
    "src": "src"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/carlosrojaso/apress-book-web-components.git"
  },
  "author": "Carlos Rojas",
  "license": "MIT",
}
```

```
"bugs": {
    "url": "https://github.com/carlosrojaso/apress-book-web-components/issues"
},
"homepage": "https://github.com/carlosrojaso/apress-book-web-components#readme"
}
```

在这个 package.json 文件中，我们定义了组件在 npmjs.com 中的名称以及文件的结构。npm 包名称应该是唯一的。因此，你必须确保你在 package.json 中为 name 属性选择的名称在 npmjs.com 中尚未被占用。

现在我们将创建一个名为 src 的目录，在其中放置我们的代码源。然后，我们将把 simple-form-modal-component.js 移到那里并创建一个新的文件 index.js，如列表 7-2 所示。

### 列表 7-2. simple-form-modal-component 的 index.js

```
export * from './simple-form-modal-component';
```

这只是一行代码，我们用它导入 simple-form-modal-component.js 中的所有内容。因此，在我们的文件 simple-form-modal-component.js 中，我们必须添加 export 关键字，以使我们的组件作为模块可用，如列表 7-3 所示。

### 列表 7-3. 将 simple-form-modal-component 转换为模块

```
export class SimpleFormModalComponent extends HTMLElement {
...
}
customElements.define('simple-form-modal-component', SimpleFormModalComponent);
```

好的，现在我们的组件已准备就绪。下一步是运行

```
$npm publish
```

现在我们已发布了我们的组件（参见图 7-4）。

![](img/e276a43da8123ca0974f10d5c85948b9_102_0.png)

### 图 7-4. 发布 simple-form-modal-component

你可以访问本书的代码 (https://github.com/carlosrojaso/apress-book-web-components) 在 $git checkout chap-7-1。

这个过程对于 <note-list-component> 和 <note-list-item-component> 也是类似的。如果想查看修改内容，可以在 $git checkout chap-7-2 和 $git checkout chap-7-3 查看。

现在我们可以在项目中轻松使用我们的组件。目前，我们将使用服务 unpkg.com。unpkg 是 npm 包的 CDN，以便轻松插入我们的包，如列表 7-4 所示。

### 列表 7-4. simple-form-modal-component 的 package.json

```
<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<script async type="module" src="http://unpkg.com/apress-simple-form-modal-component@1.0.1/src/simple-form-modal-component.js"></script>
```

```
<script async type="module" src="http://unpkg.com/apress-book-web-components-note-list@1.0.1/src/note-list-component.js"></script>
<script async type="module" src="http://unpkg.com/apress-note-list-item-component@1.0.1/src/note-list-item-component.js"></script>

<script async type="module" src="./app.js"></script>

<link rel="stylesheet" type="text/css" href="./style.css">

</head>
<body>
<h2>Notes App</h2>

<button class="fab" id="myBtn">+</button>

<simple-form-modal-component></simple-form-modal-component>

<note-list-component></note-list-component>

</body>
</html>
```

你可以访问代码 (https://github.com/carlosrojaso/apress-book-web-components) 在 $git checkout chap-7-4。

通过这些修改，我们不需要在项目中引入组件文件，因为我们是从 unpkg.com 导入的。在下一节中，你将学习添加其他工具，以便从本地环境运行所有内容，使用 npm 而不使用 unpkg.com。

### 旧版 Web 浏览器支持

Web Components 在主要浏览器、WebKit 以及所有基于 Chrome 的 Web 浏览器中都有出色的支持（参见图 7-5）。但是如果我们必须支持像 IE 11 这样的 Web 浏览器会怎样？

| Specification | Chrome | Firefox | Safari | Edge | Opera |
|---|---|---|---|---|---|
| Custom Elements | ✓ | ✓ | ✓ | ✓ | ✓ |
| HTML Templates | ✓ | ✓ | ✓ | ✓ | ✓ |
| Shadow DOM | ✓ | ✓ | ✓ | ✓ | ✓ |
| ES Modules | ✓ | ✓ | ✓ | ✓ | ✓ |

### 图 7-5. 支持 Web Components 主要规范的主要浏览器

如果你访问 Can I use (caniuse.com) 并搜索我们使用 Web Components 所需的每个规范，你会发现对于 ES6，IE 11 的支持有限（图 7-6）。

| Support Level | IE | Chrome | Firefox | Safari | Edge |
|---|---|---|---|---|---|
| Supported |  | 4-20 | 2-5 | 3.1-7 | 12-14 |
| Supported |  | 21-50 | 6-53 | 7.1-9.1 | 15-18 |
| Partial | 6-10 | 51-81 | 54-79 | 10-13 | 79-83 |
| Not Supported | 11 | 83 | 80 | 13.1 | 84 |
|  |  | 84-86 | 81-82 | 14-TP |  |

### 图 7-6. 支持 ES6 的 Web 浏览器版本¹

IE 11 不提供对自定义元素的支持（图 7-7）。

¹https://caniuse.com/es6

![](img/e276a43da8123ca0974f10d5c85948b9_105_0.png)

图 7-7. 支持自定义元素的 Web 浏览器版本²

IE 11 不提供对 HTML 模板的支持（图 7-8）。

![](img/e276a43da8123ca0974f10d5c85948b9_105_1.png)

图 7-8. 支持 HTML 模板的 Web 浏览器版本³

IE 11 不提供对 Shadow DOM 的支持（图 7-9）。

²https://caniuse.com/custom-elementsv1
³https://caniuse.com/template
