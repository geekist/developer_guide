
# Vue 开发环境配置


[使用vscode 创建第一个vue项目](https://www.jianshu.com/p/08e12cdeed82)

[Vue项目打包部署总结](https://segmentfault.com/a/1190000021530126)

## vue.js 开发环境需求

vue.js所有的环境基础包括：

* Node.js ：这是基于Chrome的V8 JavaScript引擎构建的JavaScript运行时，(运行时：可以理解为运行环境)

* npm：这是node.js的包管理工具  *cnpm：npm的淘宝镜像*

* webpack：前端资源模块化管理和打包工具

* vue-cli：脚手架构建工具

## 一、安装node.js
 
### 1.1 vue 为什么需要node.js的安装环境？


* JavaScript

传统的JavaScript是运行在浏览器上的，因为浏览器的内核分为两个部分

渲染引擎: 渲染html&&css

JavaScript引擎：运行 JavaScript，随着技术的发展， Chrome 使用的 JavaScript 引擎是 V8，它的速度非常快且性能好，同时由2009年5月Ryan Dahl开发的Node.js 诞生。


* 什么是Node.js

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，运行在服务端的JavaScript。Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效。

nodejs是一个很强大的js 运行环境，类似于jvm之于java。因此对js的支持非常好，催生了基于js的一系列应用开发。源于各js的应用的成长壮大，继而催生出了 npm。

NPM是基于node js环境的一个包管理器。试问 为什么单纯的 jsp/php里面没有NPM？因为没有一个类似于nodejs的强大的js运行环境的支撑。由于nodejs 催生了js的兴盛，又进而催生出NPM来打包管理这些基于js的应用。

随着前端开发的网页元素不断丰富和复杂化，催生出webpack 来进一步规划js应用的打包部署。前端目标页面资源，通过webpack来打包压缩出来。

可以看出vue.js 就是遵循的webpack 的方式来部署的，我们使用npm run build之后，会生成一个目标dist文件。这即是目标静态web资源，放在nginx下面即可通过网页访问。

**综上所述，vue.js 是通过 webpack来打包，而webpack 又基于 npm, npm需要nodejs环境。这就是为什么vue.js 还需要安装nodejs环境。**

将目标dist文件夹拷贝到一台未安装nodejs的 nginx服务器上，访问页面可以正常响应逻辑。这时跟nodejs没有任何关系，服务器又不是nodejs在担当，而是nginx。如果你用nodejs来部署服务器，则需要在目标机上安装nodejs.

简单的说：你既可以开发nodejs的服务程序，亦可以用基于nodejs的npm && webpack来打包 目标前端页面。vue.js 使用webpack来打包，故而需要nodejs环境。


### 1.2 安装node.js环境


* Node.js 安装包及源码下载地址为：https://nodejs.org/en/download/。

下载后按照提示安装即可。

>安装过程中，不要选择安装node工具，否则后续会比较混乱；
>
>使用下载的安装包安装后，不需要配置环境变量，因为已经配置好了；

* Node.js配置环境变量

计算机->属性->高级系统配置->环境变量->用户变量->编辑path,添加`global“目录如下：

PATH: D:\node\nodejs

总结：
不需要添加系统环境变量NODE_PATH，只需编辑用户环境变量

![](./assets/vue-1.png)

## 二、安装npm和cnpm

npm是随同Node.js一起安装的包管理工具,直接在命令行敲出npm -v就可以查看是否安装成功：


### 2.1 更新npm到最新版本

使用：

**npm install npm -g**

更新npm至最新版

### 2.2 cnpm

因为npm包管理器需要依赖包的服务器地址是国外的，资源下载及访问会很慢，所以我们需要安装淘宝的国内镜像来提高速度。

安装淘宝的国内镜像的指令：

```

npm install -g cnpm --registry=http://registry.npm.taobao.org
 
```

安装完成后，使用cnpm即可完成依赖安装


**cnpm install [依赖的name]**

## 三、安装webpack

WebPack可以看做是模块打包机：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其打包为合适的格式以供浏览器使用。

Webpack 本质上是一个打包工具，它会根据代码的内容解析模块依赖，帮助我们把多个模块的代码打包。

![](./assets/webpack.awebp)

webpack安装：

**npm install webpack webpack-cli -g**

## 四、安装vue-cli

Vue CLI 致力于将 Vue 生态中的工具基础标准化，它确保了各种构建工具能够基于智能的默认配置即可平稳衔接，这样你可以专注在撰写应用上，而不必花更多的时间去纠结配置的问题。也就是可以通过简单的命令行或者图形界面创建一个vue应用。

安装：

**npm install  vue-cli -g**
 

## 五、vscode环境下的vue插件：

* Vetur

Vetur支持.vue文件的语法高亮显示，除了支持template模板以外，还支持大多数主流的前端开发脚本和插件，比如Sass和TypeScript。

安装：

点击左边的Extensions图标（快捷键Ctrl+Shift+X），输入vetur，找对对应版本然后点击install即可

* ESLint 

ESLint是一个用来识别 ECMAScript 并且按照规则给出报告的代码检测工具，使用它可以避免低级错误和统一代码的风格。如果每次在代码提交之前都进行一次eslint代码检查，就不会因为某个字段未定义为undefined或null这样的错误而导致服务崩溃，可以有效的控制项目代码的质量。


