
# React Native

## 1、什么是React Native?

React Native是Facebook在F8大会开源的JavaScript框架,(2015年9月15日发布)可以让广大开发者使用JavaScript和React开发跨平台的移动应用.

在短短不到一年的时间里,它成为手机端必不可少的开发模式之一。 它充分利用了Facebook现有的业务轮子既拥有Native的用户体验、又保留React的开发效率,目前，React Native基本完成了对多端的支持，实现了真正意义上的面向配置开发: 开发者可以灵活的使用HTML和CSS布局,使用React语法构建组件,实现：Android, iOS 两端代码的复用。

核心设计理念: 既拥有Native的用户体验,又保留React的开发效率.


![react原理图](https://img-blog.csdnimg.cn/20210109151935848.png)

二: React Native的特点:
使用了 Virtual DOM(虚拟DOM)

提供了响应式(Reactive)和组件化(Composable)的视图组件

将注意力集中保持在核心库，伴随于此，有配套的路由和负责处理全局状态管理的库.


## React Native的优势:

* 跨平台开发
运用React Native，我们可以使用同一份业务逻辑核心代码来创建原生应用运行在Web端，Android端和iOS端；

* 追求极致的用户体验
实时热部署

* 社区活跃,除了Facebook之外，GitHub上有很多第三方的团队、个人、公司开发贡献了很多非常优秀的第三方组件，它的社区是非常健康、非常活跃的。

React Native不强求一份原生代码支持多个平台,Java的是(Write once, run anywhere) 
learn once, write everywhere

## React Native的劣势:

*  RN框架原生并不支持Web端;

*  RN框架官方并不支持热更新;

* Facebook给出的官方RN API不能完全满足业务快速的发展,它只给了一些很基础的API，但业务中经常会用到的一些多媒体，比如录音、录像、视频播放文件以及文件上传、压缩、加密等等，这些都没有提供。
 
* 尽管RN框架性能非常不错，比H5好很多。实际上经过真正的业务开发后，发现90%的场景下RN的性能非常棒，可以满足我们的业务需求；但是在另外的10%的场景下，特别是一些交互非常复杂、页面非常复杂、需要频繁的更新、需要一些手势交互的场景，RN仍有些内存跟性能的瓶颈。
