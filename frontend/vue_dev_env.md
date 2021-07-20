
* vue 为什么需要node.js的安装环境？


Node是一个基于Chrome V8引擎的JavaScript运行环境，一个让JavaScript 运行在服务端的开发平台。

Vue.js是一套用于构建用户界面的渐进式JavaScript框架。

那么为什么vue需要node？

vue是一个js框架，但是安装vue一般是通过npm进行安装，npm是Node.js官方提供的包管理工具，所以安装完node.js才可以使用npm了。

使用vue-cli搭建项目时也需要nodejs。当vue与第三方库或既有项目整合或构建vue项目时需要通过nodejs来实现。


npm是Node.js官方提供的包管理工具，他已经成了Node.js包的标准发布平台，用于Node.js包的发布、传播、依赖控制。npm提供了命令行工具，使你可以方便地下载、安装、升级、删除包，也可以让你作为开发者发布并维护包。

在node里面通过Npm安装vue，是为了方便进行模块化管理。这样你的整个项目就能实现模块化组件化，并且按需加载。

你也可以用script标签引入vue.min.js这样的，没人拦你，在js里实例化vue，也行。

使用node有几件事，打包部署，解析vue单文件组件，解析每个vue模块，拼在一起，转码es6，less等，启动测试服务器localhost8080, 帮你管理 vue-router，vue-resource这些插件，直接拿来用。

* 如何安装node.js的环境？
* 