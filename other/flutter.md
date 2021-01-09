## 一：Flutter 介绍

## 什么是Flutter

Flutter is Google's UI toolkit for building beautiful, natively compiled applications for mobile, web, and desktop from a single codebase.

Flutter是谷歌主导研发的一个UI工具包，可以利用它使用非常简洁的代码开发出来漂亮的、原生的应用程序。无论是在移动端、web端、还是桌面端。

Flutter 主要是针对跨平台：Android and iOS 的解决方案，它性能优越、并且高效。

## Flutter 特点

* 1: 美观

使用Flutter内置美丽的Material Design and Cupertino Widget(组件), 丰富的motion API，平滑而自然的滑动效果和平台感知，为用户带来全新的体验。

* 2: 快速

Flutter 的UI 渲染性能很好，在生产环境下，Flutter将代码编译成机器码执行、并充分利用GPU的图形加速能力，因此使用Flutter开发的移动应用即使在低配手机上也能实现每秒60帧的UI渲染速度。
flutter 引擎使用C++编写、包括高效的Skia 2D渲染引擎，Dart运行时和文本渲染库。
这个引擎使得Flutter 框架可以自由、灵活、高效地绘制UI组件，而应用开发者则可以用Flutter框架来轻松实现各种设计语言和动画效果。

* 3: 高效

对于开发者来说，使用Flutter开发应用十分高效。
Flutter 广受好评的Hot Reload（热重载）功能可以在1秒内实现代码到UI的更新、使得开发操作周期被大幅度缩短。另外，热重载能够在执行的时候保留应用的当前状态（stateful），例如你可能在修改一个导航结构里的子页面，保留状态的热重载可以让你不需要重新从起始页一路点击回到这个子页面，而是在代码修改完成后即刻看到结果。

* 4: 开放

Flutter是开放的，它是一个完全的开源的项目，全球的开发者都可以免费使用和拓展Flutter的源代码，并为flutter的生态和文档作贡献，我们已经看到许多中国开发者（比如闲鱼开发团队）活跃在社区中，并为flutter做出了许多贡献。

![flutter架构](https://upload-images.jianshu.io/upload_images/3637091-e8b82ebd58b04be0.png?imageMogr2/auto-orient/strip|imageView2/2/w/805/format/webp)

## 3: 跨平台历史

### 3.1 平台独立开发

目前移动端有iOS和Android两大系统。很多公司为了扩散自己的产品，都需要在两大系统中跑自己的应用app。

* iOS系统

最初，如果希望在其上开发应用程序，所采用的是Objective-C
2014年，苹果在WWDC大会上发布Swift语言，Swift更加现代化，也更加接近其他语言，被认为是Objective-C的替代品（但是到现在还是没有替代，两个都在使用）
也就是说，现在开发iOS系统应用需要掌握两门语言: Objective-C and Swift.


* Android系统


最初，如果希望在其上开发应用程序，所采用的语言是Java
2011 年JetBrains 推出kotlin项目,在Google I/O2017中，Google宣布在Android上为Kotlin提供最佳支持
也就是现在开发Android系统上的应用需要掌握两门语言：Java and Kotlin


通常在一个公司很难让一个人同时去开发iOS and Android两个应用，所以一家公司就需要iOS组和Andorid组分别针对不同的系统进西行开发，但是对于一家小公司来说，这样的成本非常高。
在很长一段时间内，大家都在需求一种移动端跨平台的方案，希望可以通过一套代码来开发出可以同时运行在iOS 和 Android两个系统上的应用。

### 3.2 跨平台解决方案

* 基于JavaScript 和 WebView的跨平台


最早出现的跨平台框架是基于JavaScript和WebView，代表框架有PhoneGap、Apache Cordova、Ionic等。
主要是通过HTML来构建自己的界面，再将其显示在各个平台的WebView中。但是它默认不能调用本地的一些服务，比如相机、蓝牙等；所以需要通过JavaScript进行桥接来调用Native的一些代码来完成某些功能，但是它本身的体验并不理想、而且开发过程中的坑非常多。

* 备受欢迎的React Native.


在寻求最佳跨平台解决方案的过程中，无疑React Native是之前最优秀的一个。
React Native （简称RN）是facebook于2015年4月开源的跨平台移动应用开发框架，是facebook早先开源的JS框架，React 在原生移动应用平台的衍生产物，目前支持iOS 和 安卓两大平台。
RN使用javaScript 语言，类似于HTML的JSX，以及CSS来开发移动应用，因此熟悉Web前端开发的技术人员只需要很少的学习就可以进入移动应用开发领域。
并且在保留基本渲染能力的基础上，用原生自带的UI组件实现代替了核心的渲染引擎，从而保证了良好的渲染性能。
但是，由于RN的本质是通过javaScript VM调用远程接口，通信相对比较低效，而且框架本身不负责渲染，而是是间接通过原生进行渲染的。还有一个就是在进行iOS 和 Android适配的过程中，还要求开发者对两大系统本身有所熟悉才行。所以在RN上做出非常多的贡献的Airbnb之前就宣布放弃，而转向Native进行开发。

* 可能是终极的解决方案：Flutter


从flutter 出现到现在，我个人就一直非常看好，因为它可能才是我们很久以来所期待的跨平台的终极解决方案。

我们直接看下面这幅图来对比flutter - native - RN的区别？


flutter利用skia绘图引擎，直接通过CPU、GPU、进行绘制，不需要依赖任何原生的控件
Android操作系统中，我们编写的原生控件实际上也是依赖于Skia进行绘制，所以flutter在某些Android操作系统中的Skia必须随着操作系统进行更新、而flutter SDK中总是保持最新的。
而类似于RN的框架，必须通过某些桥接的方式先转成原生进行调用，之后在进行渲染。
## Flutetr 绘制原理

### 4.1. Flutter 渲染本质
问题一：一个图像到底是如何显示在屏幕上的？
首先你需要知道，我们在屏幕上可以看到的所有内容都是计算机绘制出来的图像，无论是视频还是GIF图片，还是操作系统给我们看到的图像化界面都是图像

问题二：但是我们为什么能看到类似于动画的效果呢？
这是因为播放的速度非常的快，研究表明：

当图片连续播放的频率超过16帧（16张图片），人眼就会感到非常流畅，当少于16帧时，会感到非常卡顿。
所以我们平时看到的电影，通常都是24帧或者30帧（李安导演之前拍摄过120帧的电影，目的就是让图片间隔更小，画面更加流畅）
我们说回到电脑、手机屏幕上的显示？
事实上显示器就是以固定的频率显示图像，比如iphone的60Hz，ipad pro的120Hz。一帧图像绘制完毕后，准备绘制下一帧时，显示器会发出一个垂直信号VSync。所以60Hz的频率就会一秒发出60次这样的信号。在计算机系统中，CPU、GPU、和显示器以一种特定的方式协作：

CPU将计算机好的显示内容提交给GPU
GPU渲染后放入帧缓冲区
视频控制器按照VSync信号从帧缓冲区取帧数据传递给显示器；
Flutter 跟 React Native的本质区别？
React Native 之类的框架，只是通过JavaScript虚拟机拓展调用系统组件，由Android 和 iOS系统进行组件的渲染。
Flutter是自己完成了组件渲染的闭环。

### 4.2. Dart语言的优势？
Flutter 为啥选择Dart语言作为开发语言？
早期Flutter团队评估了十多种语言，并选择了Dart语言，因为它符合他们的构建用户界面的方式。其实针对于前端开发者来说，选择JavaScript看起来更合适，因为大家的入门成本会更低，会有更多人选择学习和使用Flutter，但是Flutter团队从一开始就决定，不讲究！

Dart是AOT（Ahead of Time）编译的，编译成快速、可预测的本地代码，使Flutter几乎都可以使用Dart编写，这不仅使Flutetr变得更快，而且几乎所有的东西（包括所有的小组件）都可以定制。
Dart也可以JIT（Just in Time）编译，开发周期异常快，工作流颠覆常规（包括Flutter流行的亚秒级有状态热重载）。
Dart可以更轻松地构建以60fps运行的流畅动画和转场，Dart可以在没有锁的情况下进行对象分配和垃圾回收，就像JavaScript一样，Dart避免了抢占式调度和共享内存（因而也不许需要锁），由于Flutter应用程序被编译为本地代码，因此它们不需要在领域之间建立缓慢的桥接（例如JavaScript到本地代码)，它的启动速度也快的多。
Dart使Flutter不需要单独的声明式布局语言，如JSX或XML，或单独的可视化界面构建器，因为Dart的声明式编译布局易于阅读和可视化，所有的布局使用一种语言，聚集在一处，Flutter很容易提供高级工具，使布局更简单。
开发人员发发现Dart特别容易学习，因为它具有静态和动态语言用户都熟悉的特性，并非所有这些功能都是Dart独有的，但它们的组合却恰大好处，使Dart在实现Flutter方面独一无二，因为没有Dart，很难想象Flutter像这样强大。

### 4.3. 渲染引擎Skia


想要了解Flutter 本质，必须先了解它的底层图像渲染引擎Skia， 前面提到了Flutter只关心如何构建视图抽象结构，向GPU提供视图数据，Skia就是Flutter向GPU提供数据的途径。

Skia 全名Skia Graphics Library（SGL）是一个由C++编写的开源图形库，能在低端设备如手机上呈现高质量的2D图形，最初由Skia公司开发，后被Google收购，应用于Android、Google、Chrome、Chrome OS等等当中。
目前，Skia已然是Android官网的图像渲染引擎了，因此Flutter Android SDK无需内嵌Skia引擎就可以获得天然的Skia支持。
而对于iOS平台来说，由于Skia是跨平台的，因此它作为Flutter iOS渲染引擎嵌入到Flutter的iOS SDK中，替代了iOS闭源的Core Craphics/ Core Animation/ Core Text，这也正是Flutter iOS SDK 打包的App包体积比Android要大一些的原因。
底层渲染引擎统一了，上层开发接口和功能体验也就随即统一了，开发者再也不用操心平台相关的渲染特性了，也就是说，Skia保证了同一套代码调用在Android 和 iOS平台上的渲染效果就是完全一致的。

## 如何学习Flutter？

### 5.1. 大前端学不动了？

很多人看到Google 的flutter框架的时候，第一反应就是别出新东西了，是在学不动了。但是作为大前端开发者就是这样，各行折腾

客户端开发者：
从Android 到iOS， 或者从iOS 到 Android，到RN，甚至现在越来越多的客户端开发者接触到前端的相关知识，（Vue、React、Angular、小程序）

前端开发者:
从JQuery到AngularJS， 到三大框架并行：Vue、React、Angular，还有小程序，甚至现在也要接触客户端的开发（比如RN、Flutter）

大前端开发就是，不像服务器一样可能几年甚至几十年还是那一套的东西，新技术会层出不穷。
但是每一样技术的出现都会让人惊喜，因为他必然是解决了之前技术的某一个痛点、所以我们要学会拥抱这种变化。

并且很多东西在学习的过程中，你会发现它们都是相同的、并不是说都要从头到来、最重要的是建立属于自己的知识体系。

### 5.2: Flutter 学的会吗？

很多人对于学习望而却步、主要是基于两点考虑：

学习一门全新的语言：Dart、也就是你必须从你原来熟悉的JavaScript/Swift/Java或其他语言转向这门全新的语言。

Flutter是全新的跨平台技术、意味着自己需要取学习很多新的内容、开发模式、框架原理、底层原理渲染机制等等。

Dart语言并不复杂、而且非常现代化

首先、所有编程语言都是大同小异的、你花两天的时间去练习一定可以快速掌握它。

Dart语言几乎集结了现代语言所有好用的特性、并不复杂

Flutter 并没有非常多创新的概念

Flutter 从其他框架中借鉴了非常多的设计思想、框架原理、底层渲染机制、事件处理方式都是大同小异的。


声明式编程方式、组件化开发也是现代框架都有的特性，比如Vue、React。
