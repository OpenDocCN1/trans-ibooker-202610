# Vue.js 快速入门





当需要借助现有代码来提供上下文时，为了避免重复展示全部代码，会以 … 代替显示：

```
function animate() {
    ...
    new_variable = "Hello";
}
```

有些代码行本应在一行内输入，但由于页面限制，我们不得不将其换行。➜ 表示仅为排版需要而存在的换行，阅读时应忽略该换行：

```
URL.open("http://www.sitepoint.com/responsive-web-design-real-user-testing/?responsive1");
```

你会注意到，全书采用了一些特定的版式来标示不同类型的信息。请留意下面这些形式。

## 提示、注意与警告

> **嘿，你！**
>
> 提示提供一些有用的小建议。

> **嗯，打扰一下……**
>
> 注意是一些有用的补充说明，与当前主题相关，但并非关键。可以把它们看作额外的信息花絮。

> **请务必始终……**
>
> ……注意这些重要之处。

![](img/eaf78d0df5a5560ef972f68b7d6e48cc_14_0.png)

在上面的示例中，`v-model` 指令用于将 `text` 属性绑定到 `<input>` 元素。当 Vue 首次渲染模板时，输入框会预先填充 `text` 的内容。修改输入字段将使 `text` 属性得到更新，并重新渲染 `<p>` 元素内的输出。

## v-on

我们可以使用 `v-on:<event>` 语法为元素和组件触发的事件附加监听器：

```
<button v-on:click="heading = 'Hello World!'">Click Me</button>
```

在上面的示例中，当按钮被点击时，实例数据上的 `heading` 属性将被设置为“Hello, World!”。与插值一样，传递给指令的值可以包含任何有效的 JavaScript 表达式，并且可以直接引用实例数据属性，就像它们是局部变量一样。

Vue 允许你使用一种简写形式来绑定事件处理程序，用 `@` 前缀代替较冗长的 `v-on:`。在本书后续部分，我们将使用这种更简短的语法。

除了修改数据属性之外，你还可以调用 Vue 实例上的自定义方法。该方法将接收事件对象作为第一个参数。

## JavaScript

```javascript
new Vue({
  el: "#main",
  data: {
    status: ""
  },
  methods: {
    updateStatus(event) {
```

```
},
"extends": [
"plugin:vue/essential",
"eslint:recommended"
],
"rules": {},
"parserOptions": {
"parser": "babel-eslint"
}
},
"postcss": {
"plugins": {
"autoprefixer": {}
}
},
"browserslist": [
"> 1%",
"last 2 versions",
"not ie <= 8"
]
}
```

如你所见，ESLint 已设置为使用推荐的规则集，并启用了 vue/essential 插件（你可以在 ESLint 插件的仓库中了解更多信息）。Babel 也已配置为支持 browserslist 选项中涵盖的所有浏览器。

虽然默认预设是学习 Vue 应用构建工具以及快速构建原型的一个不错的起点，但对于正式应用程序，你可能会希望通过选择所需功能来自定义项目。

## Webpack 配置

Vue CLI 构建在 webpack 之上，但抽象掉了所有配置，以便更轻松地启动和运行。你可以随时运行 `vue inspect` 命令查看 webpack 配置。

运行 Vue CLI 并选择手动选择功能时，你会看到以下屏幕：

自闭合形式，除非我们想让它包裹任何子元素（我们稍后会讨论）。

## 生命周期方法

如果你以前使用过 React，可能对**组件生命周期方法**的概念很熟悉。其背后的思想是，在组件的生命周期中会发生各种事件——从组件创建到销毁。生命周期方法在这些时间点被调用，使你能够执行代码——例如，在组件初始化时执行 Ajax 请求。

Vue 文档中有一张有用的图表，列出了所有不同的方法以及它们在生命周期中的调用时机。

你可能会发现自己最常用的方法有：

- **created**。这是启动任何用于获取数据的 Ajax 请求的地方。
- **mounted**。当这个钩子被触发时，组件已被渲染并插入到 DOM 中。此时你可以操作 DOM 元素——例如，如果你正在封装某种非 Vue 库。
- **beforeDestroy**。就在组件从 DOM 中移除之前，会调用此方法。你可以在此进行任何清理工作。

如果我们想通过 API 为组件加载员工数据，可以执行类似以下操作。

### src/StaffDirectory.vue

```
<script>
  export default {
    name: 'StaffDirectory',
    data() {
      return {
        heading: "Staff Directory",
        sortBy: 'department',
        employees: []
```
