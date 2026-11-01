
![](img/e276a43da8123ca0974f10d5c85948b9_156_0.png)

图 9-4. 父组件与子组件之间的通信

如图 9-4 所示，如果你想让父组件与子组件通信，可以使用 props 或 ref 属性。

## 什么是 Props？

Props 是可以在组件上注册的自定义属性。当值传递给 prop 属性时，它会成为该组件实例上的一个属性。你可以在清单 9-12 中看到基本结构。

### 清单 9-12. 在 Vue 组件中声明 Props

```
const app = createApp({...})
app.component('some-item', {
  props: ['somevalue'],
  template: '<div>{{ somevalue }}</div>'
})
```

现在你可以像清单 9-13 中那样传递值。

### 清单 9-13. 在 Vue 组件中使用 Props

```
<some-item somevalue="value for prop"></some-item>
```

## 什么是 Ref 属性？

ref 属性用于注册对元素或子组件的引用。引用将注册在父组件的 $refs 对象下。¹ 你可以在清单 9-14 中看到基本结构。

### 清单 9-14. 在 Vue 组件中使用 Ref

```
<input type="text" ref="email">

<script>
    const input = this.$refs.email;
</script>
```

## 发射事件

如果你想从子组件向父组件通信，可以使用 $emit()，这是传递信息的推荐方法。调用 prop 函数是传递信息的另一种方法（图 9-5），但这是一种不好的做法，因为当项目增长时可能会令人困惑，因此我将跳过这一点。

![](img/e276a43da8123ca0974f10d5c85948b9_157_0.png)

图 9-5. 使用父组件与子组件通信

在 Vue 中，$emit 方法可用于向父组件发送数据。在清单 9-15 中，你可以看到发射事件的基本结构。

### 清单 9-15. 在 Vue 组件中使用 $emit

```
const app = createApp({...})
app.component('child-custom-component', {
  data: function () {
    return {
      customValue: 0
    }
  },
  methods: {
    giveValue: function () {
      this.$emit('give-value', this.customValue++)
    }
  },
  template: `
    <button v-on:click="giveValue">
      Click me for value
    </button>
  `
})
```

## 使用双向数据绑定

在组件之间促进通信的一种简单方法是使用双向数据绑定。在以下场景中，Vue.js 为我们处理组件之间的通信（图 9-6）。

![](img/e276a43da8123ca0974f10d5c85948b9_158_0.png)

图 9-6. 组件之间的双向通信

双向数据绑定意味着 Vue.js 为你同步数据属性和 DOM。数据属性的更改会更新 DOM，而对 DOM 所做的修改会更新数据属性。因此，数据双向流动。在清单 9-16 中，你可以看到使用双向数据绑定的基本结构。

### 清单 9-16. 在 Vue 组件中使用 v-model

```
const app = Vue.createApp({})
app.component('my-component', {
  props: {
    myProp: String
  },
  template: `
    <input
      type="text"
      :value="myProp"
      @input="$emit('update:myProp', $event.target.value)">
  `
})
<my-component v-model:myProp="Some Value"></my-component>
```

## Material Web 组件

在第 8 章中，我们使用 VanillaJS 和 Polymer 构建了一些 Web 组件。这是在这些技术基础上学习基础知识的绝佳方式。尽管如此，对于我们的 VueNoteApp，我们将使用 Google 维护的更强大的 Web 组件目录，该目录实现了 Material Design 并使用 LitElement 构建。你可以在 https://github.com/material-components/material-components-web-components 找到这些组件。

使用这些组件有助于提高我们的生产力，因为我们拥有 Material Design 的正确指南，并且提升质量，因为它们经过更充分的测试，并且在用户界面中有更多使用选项。

## 构建 VueNoteApp

在本节中，我们将使用 Vue 并使用 Material Design 构建一个完整的笔记应用，如图 9-7 所示。

![](img/e276a43da8123ca0974f10d5c85948b9_160_0.png)

图 9-7. VueNoteApp 设计

## 创建新的 Vue 项目

首先，我们将使用 Vue CLI 在 Vue 3 中创建一个新项目。请确保你使用的是最新版本的 Vue CLI 并运行以下命令：

```
$npm update -g @vue/cli
```

要创建项目，请在终端中运行以下命令：

```
$vue create note-app
```

选择 Vue 3，如图 9-8 所示。

![](img/e276a43da8123ca0974f10d5c85948b9_161_0.png)

图 9-8. 在 Vue CLI 中选择 Vue 3

安装完所有依赖项后，进入项目文件夹。

```
$cd note-app
```

## 添加 Material Web 组件

首先，我们将安装要在应用中使用的 Web 组件。

安装 mwc-button（图 9-9）。

![](img/e276a43da8123ca0974f10d5c85948b9_161_1.png)

图 9-9. mwc-button 组件

然后在终端中运行以下命令：

```
$npm install --save @material/mwc-button
```

你可以在 https://github.com/material-components/material-components-web-components/tree/master/packages/button 查看其所有属性。

安装 mwc-dialog（图 9-10）。

![](img/e276a43da8123ca0974f10d5c85948b9_162_0.png)

图 9-10. *mwc-dialog* 组件

然后在终端中运行以下命令：

```
$npm install --save @material/mwc-dialog
```

你可以在 https://github.com/material-components/material-components-web-components/tree/master/packages/dialog 查看其所有属性。

安装 mwc-fab（图 9-11）。

![](img/e276a43da8123ca0974f10d5c85948b9_162_1.png)

图 9-11. *mwc-fab* 组件

然后在终端中运行以下命令：

```
$npm install --save @material/mwc-fab
```

你可以在 https://github.com/material-components/material-components-web-components/tree/master/packages/fab 查看其所有属性。

## 第 9 章 使用 VUE.JS

安装 mwc-icon-button（图 9-12）。

![](img/e276a43da8123ca0974f10d5c85948b9_163_0.png)

图 9-12. mwc-icon-button 组件

然后在终端中运行以下命令：

```
$npm install --save @material/mwc-icon-button
```

你可以在 https://github.com/material-components/material-components-web-components/tree/master/packages/icon-button 查看其所有属性。

安装 mwc-list（图 9-13）。

![](img/e276a43da8123ca0974f10d5c85948b9_163_1.png)

图 9-13. mwc-list 组件

然后在终端中运行以下命令：

```
$npm install --save @material/mwc-list
```

你可以在 https://github.com/material-components/material-components-web-components/tree/master/packages/list 查看其所有属性。

安装 mwc-textfield（图 9-14）。

![](img/e276a43da8123ca0974f10d5c85948b9_164_0.png)

图 9-14. mwc-textfield 组件

然后在终端中运行以下命令：

```
$npm install --save @material/mwc-textfield
```

你可以在 https://github.com/material-components/material-components-web-components/tree/master/packages/textfield 查看其所有属性。

安装 mwc-top-app-bar（图 9-15）。

![](img/e276a43da8123ca0974f10d5c85948b9_164_1.png)

登录

图 9-15. mwc-top-app-bar 组件

然后在终端中运行以下命令：

```
$npm install --save @material/mwc-top-app-bar
```

你可以在 https://github.com/material-components/material-components-web-components/tree/master/packages/top-app-bar 查看其所有属性。

现在我们将安装 Polyfills 以支持旧版 Web 浏览器。

安装以下依赖项：

```
$npm install --save-dev copy-webpack-plugin @webcomponents/webcomponentsjs
```

将 copy-webpack-plugin 添加到 Vue 的 Webpack 配置文件。为此，我们必须创建一个新文件 vue.config.js，并添加清单 9-17 中的代码。

### 清单 9-17. 使用 copy-webpack-plugin 与 webcomponentsjs

```
const CopyWebpackPlugin = require('copy-webpack-plugin');

module.exports = {
    configureWebpack: {
        plugins: [
            new CopyWebpackPlugin({
                patterns: [{
                    context: 'node_modules/@webcomponents/webcomponentsjs',
                    from: '**/*.js',
                    to: 'webcomponents'
                }]
            })
        ]
    }
};
```

构建时，copy-webpack-plugin 现在会将所有必需的 JS 文件复制到 webcomponents 目录中。

现在，在 public/index.html 中，我们将添加几行代码来检查 Web 浏览器是否支持 customElements 或使用 Polyfills（见清单 9-18）。

### 清单 9-18. 在 VueNoteApp 中添加 webcomponentjs

```
<!DOCTYPE html>
<html lang="en">
<head>
...
    <script src="webcomponents/webcomponents-loader.js"></script>
    <script>
        if (!window.customElements) { document.write('<!--'); }
    </script>
    <script src="webcomponents/custom-elements-es5-adapter.js"></script>
    <!-- ! DO NOT REMOVE THIS COMMENT -->
...
</body>
</html>
```
