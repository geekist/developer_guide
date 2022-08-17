
# 一、Vue路由介绍

## 1.1 客户端路由

客户端 vs. 服务端路由

服务端路由时指的是服务器根据用户访问的 URL 路径返回不同的响应结果。当我们在一个传统的服务端渲染的 web 应用中点击一个链接时，浏览器会从服务端获得全新的 HTML，然后重新加载整个页面。

然而，在单页面应用中，客户端的 JavaScript 可以拦截页面的跳转请求，动态获取新的数据，然后在无需重新加载的情况下更新当前页面。这样通常可以带来更顺滑的用户体验，尤其是在更偏向“应用”的场景下，因为这类场景下用户通常会在很长的一段时间中做出多次交互。

在这类单页应用中，“路由”是在客户端执行的。一个客户端路由器的职责就是利用诸如 History API 或是 hashchange 事件 这样的浏览器 API 来管理应用当前应该渲染的视图。

## 1.2 Vue Router

Vue Router 是 Vue.js 的官方路由。它与 Vue.js 核心深度集成，让用 Vue.js 构建单页应用变得轻而易举。功能包括：

嵌套路由映射

动态路由选择

模块化、基于组件的路由配置

路由参数、查询、通配符

展示由 Vue.js 的过渡系统提供的过渡效果

细致的导航控制

自动激活 CSS 类的链接

HTML5 history 模式或 hash 模式

可定制的滚动行为

URL 的正确编码

# 二、vue-router安装


可以通过直接下载的方式安装vue router
https://unpkg.com/vue-router@4

Unpkg.com 提供了基于 npm 的 CDN 链接。上述链接将始终指向 npm 上的最新版本。

也可以通过像 https://unpkg.com/vue-router@4.0.15/dist/vue-router.global.js 这样的 URL 来使用特定的版本或 Tag。

也可以直接通过npm命令下载安装vue-router

```shell
npm
npm install vue-router@4
yarn
yarn add vue-router@4
```





