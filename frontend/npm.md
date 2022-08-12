
# 一、npm介绍

npm 是 Node.js 官方提供的包管理工具，他已经成了 Node.js 包的标准发布平台，用于 Node.js 包的发布、传播、依赖控制。npm 提供了命令行工具，使你可以方便地下载、安装、升级、删除包，也可以作为开发者发布并维护包。

所谓的包通常是一些模块的集合，相当于提供了一些固定接口的函数库。包是在模块基础上更深一步的抽象。Node.js 的包类似于 C/C++ 的函数库或者 Java、.Net 的类库。它将某个独立的功能封装起来，用于发布、更新、依赖管理和版本控制。Node.js 根据 CommonJS 规范实现了包机制，开发了 npm 来解决包的发布和获取需求。

npm 之于 Node.js ，就像 pip 之于 Python， gem 之于 Ruby， pear 之于 PHP 。

# 二、npm安装

npm 不需要单独安装。在安装 Node 的时候，会连带一起安装 npm 。但是，Node 附带的 npm 可能不是最新版本，可以用下面的命令，更新到最新版本。

```shell
#linux
$ sudo npm install npm@latest -g

#Window系统
npm install npm -g
```
也就是使用 npm 安装自己。之所以可以这样，是因为 npm 本身与 Node 的其他模块没有区别。

然后，运行下面的命令，查看各种信息。

```
# 查看 npm 命令列表
$ npm help

# 查看各个命令的简单用法
$ npm -l

# 查看 npm 的版本
$ npm -v

# 查看 npm 的配置
$ npm config list -l
```
# 三、使用npm

## 3.1 初始化npm init

```
npm init
```

npm init用来初始化npm，执行后会在当前目录下生成一个package.json文件。


## 3.2 使用npm安装项目需要的依赖 npm install

```shell
npm install
```

运行npm install，会从当前目录中的package.json文件中读取依赖，下载依赖，并安装到node_modules目录下。

一个典型的json文件如下所示：
```json
{
  "name": "yuya",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "build": "vue-cli-service build",
    "serve": "vue-cli-service serve",
    "build:test": "vue-cli-service build --modern --mode test",
    "build:pro": "vue-cli-service build --modern --mode production"
  },
  "dependencies": {
    "axios": "^0.21.4",
    "core-js": "^3.6.5",
    "dayjs": "^1.11.2",
    "echarts": "^5.2.1",
    "element-ui": "^2.15.6",
    "js-cookie": "^3.0.1",
    "moment": "^2.29.1",
    "nprogress": "^0.2.0",
    "qiniu-js": "^3.3.3",
    "uglifyjs-webpack-plugin": "^2.2.0",
    "vue": "^2.6.11",
    "vue-pdf": "^4.3.0",
    "vue-router": "^3.2.0",
    "vuex": "^3.4.0"
  },
  "devDependencies": {
    "@vue/cli-plugin-babel": "^4.5.0",
    "@vue/cli-service": "^4.5.0",
    "compression-webpack-plugin": "^9.1.2",
    "node-sass": "^4.12.0",
    "sass-loader": "^8.0.2",
    "vue-template-compiler": "^2.6.11"
  }
}
```

## 3.3 设置环境变量npm set

npm set 用来设置环境变量

```
$ npm set init-author-name 'Your name'
$ npm set init-author-email 'Your email'
$ npm set init-author-url 'http://yourdomain.com'
$ npm set init-license 'MIT'
```
上面命令等于为 npm init 设置了默认值，以后执行 npm init 的时候，package.json 的作者姓名、邮件、主页、许可证字段就会自动写入预设的值。这些信息会存放在用户主目录的 ~/.npmrc文件，使得用户不用每个项目都输入。如果某个项目有不同的设置，可以针对该项目运行 npm config。


## 3.4 查看npm每个模块的具体信息

```
npm info
```
npm info 命令可以查看每个模块的具体信息。

```
$ npm info underscore
```
查看 underscore 模块的信息。上面命令返回一个 JavaScript 对象，包含了 underscore 模块的详细信息。这个对象的每个成员，都可以直接从 info 命令查询。

## 3.5 搜索npm模块
```
npm search
```

npm search 命令用于搜索 npm 仓库，它后面可以跟字符串，也可以跟正则表达式。

## 3.6 列出项目的所有依赖npm list

npm list 命令以树形结构列出当前项目安装的所有模块，以及它们依赖的模块。

## 3.7 运行项目脚本 npm run 

npm可以用于执行脚本。package.json 文件有一个 scripts 字段，可以用于指定脚本命令，供 npm 直接调用。

```json
{
  "name": "myproject",
  "devDependencies": {
    "jshint": "latest",
    "browserify": "latest",
    "mocha": "latest"
  },
  "scripts":
    "dev": "node build/dev-server.js",
    "build": "node build/build.js",
    "docs": "node build/docs.js",
    "build-docs": "npm run docs & git checkout gh-pages & xcopy /sy dist\\* . & git add . & git commit -m 'auto-pages' & git push & git checkout master",
    "build-publish": "rmdir /S /Q lib & npm run build &git add . & git commit -m auto-build & npm version patch & npm publish & git push",
    "lint": "eslint --ext .js,.vue src"
}
```
例如在这个 package.json 的文件夹下使用 npm run dev 就相当于运行了 node build/dev-server.js 这一段代码。使用 scripts 的目的就是为了把一些要执行的代码合并到一起，使用 npm run 来快速的运行，方便省事。
npm run 是 npm run-script 的缩写，一般都使用前者，但是后者可以更好的反应这个命令的本质。





