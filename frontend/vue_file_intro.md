- [一、vue-cli3 项目目录结构](#一vue-cli3-项目目录结构)
  - [1.1 vue项目目录结构](#11-vue项目目录结构)
  - [1.2 vue cli3 和vue cli2 目录区别](#12-vue-cli3-和vue-cli2-目录区别)
- [二、 vue自动生成文件介绍(main.js,index.html,app.vue)](#二-vue自动生成文件介绍mainjsindexhtmlappvue)
  - [2.1 public/index.html](#21-publicindexhtml)
  - [2.2 App.vue根组件](#22-appvue根组件)
  - [2.3 HelloWorld.vue](#23-helloworldvue)
  - [2.4 main.js](#24-mainjs)
- [三、一个更加合适的vue项目目录结构](#三一个更加合适的vue项目目录结构)
  - [3.1 Assets](#31-assets)
  - [3.2 Common](#32-common)
  - [3.3 Layouts](#33-layouts)
  - [3.4 Middlewares](#34-middlewares)
  - [3.5 Modules](#35-modules)
  - [3.6 Plugins](#36-plugins)
  - [3.7 Services](#37-services)
  - [3.8 Static](#38-static)
  - [3.9 Router](#39-router)
  - [3.10 Store](#310-store)
  - [3.11 Views](#311-views)

# 一、vue-cli3 项目目录结构

![](./assets/vue-5.png)

## 1.1 vue项目目录结构

node_modules：依赖包

public:页面入口文件

src:项目核心文件（我们所写的代码都放在这个文件夹下）

.gitignore：git 上传需要忽略的文件配置

balel.config.js： 使用一些预设

package-lock.json：版本锁定文件

package.json：包管理文件

README.md：声明文档

vue.config.js：自己加 vue-cli3 的配置文件

src 文件夹

assets:静态资源（样式类文件，如 css，less，sass，以及一些外部 js 文件）

components:公共组件（多个组件同时用的可以放这里）

router：配置项目路由

views：视图

App.vue：根组件入口

main.js：入口文件（程序的主入口）

## 1.2 vue cli3 和vue cli2 目录区别

* vue-cli3移除了配置文件目录：config 和 build 文件夹，增加了vue.config.js文件

* vue-cli3移除了 static 静态文件夹，vue-cli3新增了 public 文件夹，将index.html 移动到 public 中

# 二、 vue自动生成文件介绍(main.js,index.html,app.vue)

## 2.1 public/index.html

index.html是总的入口文件，vue是单页面应用，挂在id为app的div下然后动态渲染路由模板.

pubilc/index.html是一个模板文件，作用是生成项目的入口文件，vue通过html-webpack-plugin插件读取模板(index.html)，webpack打包的js,css也会自动注入到该页面中, 进行模板绑定之后再输出到outut目录的index.html。我们浏览器访问项目的时候默认打开的文件就是dist/index.html。

```html
<!DOCTYPE html>
<html lang="">

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title>
        <%= htmlWebpackPlugin.options.title %>
    </title>
</head>

<body>
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <div id="app"></div>
    <!-- built files will be auto injected -->
</body>

</html>
```

## 2.2 App.vue根组件

```vue
<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Geekist Vue.js App"/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

App.vue文件是一个单文件组件，包含了`<template>`、`<script>` 和`<style>` 三部分组成，分别包含了组件的模板、脚本和样式相关的内容。所有的单文件组件都是这种类似的基本结构。

`<template>` 包含了所有的标记结构和组件的展示逻辑。template 可以包含任何合法的 HTML，以及一些我们接下来要讲的 Vue 特定的语法。

`<script>` 包含组件中所有的非显示逻辑.

`<script>` 标签需要默认导出一个JS对象。该对象是您在本地注册组件、定义属性、处理本地状态、定义方法等的地方。在构建阶段这个包含 template 模板的对象会被处理和转换成为一个有 render() 函数的 Vue 组件。

对于App.vue，我们的默认导出将组件的名称设置为app ，并通过将HelloWorld 组件添加到components属性中来注册它HelloWorld组件。这种注册组件的方式称为本地注册。本地注册的组件只能在注册它们的组件内部使用，因此您需要将其导入并注册到使用它们的每个组件文件中。

## 2.3 HelloWorld.vue

在components目录中定义的hellowrold.vue是一个单文件组件，展示了index.html页面的主要内容。

```html
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

```
## 2.4 main.js

main.js是项目的入口文件，项目中所有的页面都会加载main.js。

main.js主要有三个作用：

 1. 实例化Vue。

 2. 放置项目中经常会用到的插件和CSS样式。

 3. 存储全局变量。


在项目运行中，main.js作为项目的入口文件，运行中，找到其实例需要挂载的位置，即index.html,将实例中的组件模块内容替代index.html的相关内容。

```js

//引入vue库中的createApp函数
import { createApp } from 'vue'

//从app.vue中引入App组件
import App from './App.vue'

/*使用 createApp函数来创建应用，
传入基组件生成vm，
mount('#app') 将 Vue 应用 HelloVueApp 挂载到 <div id="app"></div> 的DOM元素中。
*/
createApp(App).mount('#app')
```

# 三、一个更加合适的vue项目目录结构

```
src
|--assets
|--common
|--layouts
|--middlewares
|--modules
|--plugins
|--router
|--services
|--static
|--store
|--views
```

## 3.1 Assets

静态文件目录：包含字体、图标、图片、样式等静态资源，不做赘述。

## 3.2 Common

公共文件夹：通常来说，它又能被拆分成多个子目录：components、mixins、directives，又或者是单个的文件：functions.ts、helpers.ts、constants.ts、config.ts，亦或者其它。但它们有共同的特点：Common 文件夹下的文件都是在多出被引用的。

举例：在 src/common/components 文件夹下，你可以设置 Button.vue 在全局共享的组件；在 helpers.ts 文件中写公共方法以供多处调用。

## 3.3 Layouts

你可以在 Layouts 文件夹下放整个应用的布局文件。比如 AppLayout.vue.，关于布局的更多问题可以见 这篇文章 - Vue tricks: smart layouts for VueJS

## 3.4 Middlewares

“中间件” 这个文件夹有点类似 vue router，你可以在之下放置你的关于路由跳转判断文件。这里有个简单的例子：

export default function checkAuth(next, isAuthenticated) {
  if (isAuthenticated) {
    next('/')
  } else {
    next('/login');
  }
}
在 vue-router 中这样使用

import Router from 'vue-router'
import checkAuth from '../middlewares/checkAuth.js'
const isAuthenticated = true
const router = new Router({
  routes: [],
  mode: 'history'
})
router.beforeEach((to, from, next) => {
  checkAuth(next, isAuthenticated)
});
此例意在做权限校验。更多关于中间件的讨论，在这篇文章 - Vue tricks: smart router for VueJS

## 3.5 Modules

Modules 文件夹是咱们应用的核心！

此文件夹关于应用的业务逻辑部分，它有以下类：

业务组件 components

测试单元 **tests**

数据持久 store

其它本业务相关的文件

这里有个很棒的例子：订单业务模块

```
src
--modules
----orders
------__tests__
------components
--------OrdersList.vue
--------OrderDetails.vue
------store
--------actions.ts
--------getters.ts
--------mutations.ts
--------state.ts
------helpers.ts
-----
-types.ts
```
包括：测试文件、组件（订单列表、订单详情）、Vuex 数据、相关文件。

它又像是一个小的 src 目录～

## 3.6 Plugins

Plugins 文件夹当然是用来放 plugin。在 Vue2 中，我们这样调用

import MyPlugin from './myPlugin.ts'
Vue.use(MyPlugin, { someOption: true })
在 Vue3 中，我们也可以在 main.ts 中调用，更多可见 v3-using-a-plugin。

## 3.7 Services

Services 文件夹是放请求库和 API 的地方，也包括对 localStorage 的管理等。


## 3.8 Static

通常来说，我们不需要 Static 这个文件夹，但也可以放一些 dummy data （虚拟数据）。

## 3.9 Router

Router 文件夹放置你的路由文件，太过常见、无需赘述。你也可以根据需要只在根目录设置 router.ts。但是更推荐你将路由进行一个划分以便阅读和扩展。vue-tricks-smart-router

## 3.10 Store

Store 文件夹放置你的 Vuex 相关文件。在这个目录下主要是一些全局的持久数据及方法：state 、 actions 、 mutations 、 getters，同时也和 modules 文件夹下的 Vuex 进行关联。

## 3.11 Views

Views 文件夹是我们应用中第二重要的文件夹了。我们都知道它包含的也是业务组件。但其实它更应该是路由的一种映射，比如 /home /about /orders 这个路由，在 Views 文件夹下就应该有 Home.vue、 About.vue 、Orders.vue 这三个文件！

你一定会问为什么要拆分业务部分为 Views 和 Modules 这两个目录，而不是像 Vue CLI 那样放在一起？

有以下优点：

更清晰的目录结构

更快速的了解路由

更直观看到根文件、根页面、以及它们与子组件、子业务是如何关联的。







