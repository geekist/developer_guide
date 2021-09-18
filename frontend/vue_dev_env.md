



[使用vscode 创建第一个vue项目](https://www.jianshu.com/p/08e12cdeed82)

[Vue项目打包部署总结](https://segmentfault.com/a/1190000021530126)

* 
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

## step1：安装node.js环境


* 1

Node.js 安装包及源码下载地址为：https://nodejs.org/en/download/。


* 2 配置环境变量

3.配置环境变量
计算机->属性->高级系统配置->环境变量->用户变量->编辑path,添加`global“目录如下：

PATH: D:\node\nodejs\node_global\;

总结：
不需要添加系统环境变量NODE_PATH，只需编辑用户环境变量
包安装统一到node安装包目录，便于管理查询
只需修改.npmrc一个文件
之前path可能会产生影响，不生效请删除原环境path中node相关内容，尝试重启机器

* 3 使用npm install npm -g更新npm至最新版


## step2： 安装vue-cli脚手架构建工具

在命令行运行命令：

npm install -g vue-cli 

## step2: 安装vue插件：


### 安装 Vetur

点击左边的Extensions图标（快捷键Ctrl+Shift+X），输入vetur，找对对应版本然后点击install即可

### 按照同样方法查找ESLint插件，并安装。

## step3： 创建一个vue项目

选择菜单Terminal->New Terminal 打开一个新的命令行窗口（快捷键Ctrl+Shift+`)

选择你想要创建新项目的目录，然后执行命令

**vue init webpack vuedemo**

或者

**vue create vuedemo**

然后按照提示进行操作；




此过程会先进性一些配置，根据自己的情况进行配置

```xml
? Project name vuedemo
? Project description A Vue.js project
? Author fancyebai <lvsedehuanxiang@163.com>
? Vue build standalone
? Install vue-router? Yes
? Use ESLint to lint your code? Yes
? Pick an ESLint preset Standard
? Set up unit tests Yes
? Pick a test runner jest
? Setup e2e tests with Nightwatch? Yes
? Should we run `npm install` for you after the project has been created? (recommended) npm
```

安装过程会持续一段时间，如果最后出现Project initialization finished!，则说明安装成功

## step4 运行vue项目
切换到项目目录cd vuedemo
最后执行命令npm run dev

## step5： 构建生产包

其他命令，npm run build用户构建生产包

## step6： 运行生产包

打包后复制dist中所有文件，拷贝到tomcat中的webapps下,直接运行tomcat即可访问页面

## step7： 运行

打开浏览器输入http://localhost:8080，如果出现vue的欢迎页面则说明成功



## PS：

使用vscode的过程中，我们可能会用到终端，

虽然系统自带有，但是还要另外打开，有点不方便，vscode中就有这个功能，

打开方法

使用快捷键： ctrl + ·     即可；注意那个点是键盘上 esc 下面的那个；

或者：

选择vscode的 “查看”，然后选择“集成终端” ，打开即可；


* 无法打开vue.ps1文件


原因：首次在计算机上启动 Windows PowerShell 时，现用执行策略很可能是 Restricted（默认设置）。Restricted 策略不允许任何脚本运行，防止执行不信任的脚本。

解决办法：

重新打开编辑器，以管理员身份运行vscode，输入命令

set-executionpolicy remotesigned
再次执行命令创建vue项目就可以啦。

