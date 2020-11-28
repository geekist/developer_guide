
# developer_guide

本文档包含以下几个部分：

## Android编程规范

为使代码规范、可读性强，提高开发效率和方便沟通，参照官方的文档，并结合自身开发的经验，总结出适合个人和团队使用的编程规范

[Android-Java编程规范][]

[Kotin编程规范][]

## Android项目基本框架

[Android项目基本框架][]


[Android开发中流行的框架库][]

[Android开发中流行的框架库][]

[Android开发中优秀的第三方控件或工具][]

[Android功能组件导入][]

[Android基础类库][]

[Android基础知识学习][]

[][]

**本文档为个人开发过程中笔记整理，目的是为了唤醒记忆，达到以最快的速度保持知识栈的目的**



在Android开发过程中，为达到代码规范、可读性强为了有利于项目维护、增强代码可读性、提升 Code Review 效率以及规范团队安卓开发，故提出以下安卓开发规范，该规范结合本人多年的开发经验并吸取多家之精华，可谓是本人的呕心沥血之作，称其为当前最完善的安卓开发规范一点也不为过，如有更好建议，欢迎到 GitHub 提 issue，原文地址：**[Android 开发规范（完结版）][Android 开发规范（完结版）]**。相关 Demo，可以查看我的 Android 开发工具类集合项目：**[Android 开发人员不得不收集的代码][Android 开发人员不得不收集的代码]**。后续可能会根据该规范出一个 CheckStyle 插件来检查是否规范，当然也支持在 CI 上运行。




## [Kotlin][Kotlin]

## Java

## [Android][android]

### 四大组件
#### Activity
* [添加一个Activity到工程中][添加一个Activity到工程中]
* [添加一个Fragment到工程中][添加一个Fragment到工程中]

### UI

* [BottomNavigationView][BottomNavigationView]

* [BottomNavigationView+fragments实现activity中多个fragments][BottomNavigationView+fragments]

* [DrawerLayout+NavigationView实现Drawer侧滑][drawerlayout]

* **(Jetpack)**[RecyclerView实现元素滚动列表][recyclerview]

* **(Jetpack)**[Activity+Fragments+TableLayout+ViewPager实现activity中多个fragment绑定TableLayout侧滑效果][Activity+Fragments+TableLayout+ViewPager]

* **(Jetpack)**[Activity+Fragments+TableLayout+ViewPager2实现activity中多个fragment绑定TableLayout侧滑效果][Activity+Fragments+TableLayout+ViewPager2]

### 线程

* [Timer和Handler配合使用][handler]
* [线程池介绍][threadpool]

### 网络编程

* [TCP/IP协议简介][tcp]
* [UDP协议简介][udp]

### Koltin


### Open Source Libraries

- [LitePal][litepal]

- [Glide][glide]

- [Retrofit][]

- [Gson][]

- [pgyersdk][]

- [jmdns][]

- [multidex][]


### Bluetooth
- [Bluetooth][Bluetooth]


## [硬件开发][IoT]

## [疑难问题][QA]

## [项目分析][yy]





[Activity+Fragments+TableLayout+ViewPager]: https://github.com/geekist/developer_guide/blob/main/ui/Activity+Fragments+TableLayout+ViewPager.md

[Activity+Fragments+TableLayout+ViewPager2]: https://github.com/geekist/developer_guide/blob/main/ui/Activity+Fragments+TableLayout+ViewPager2.md

[添加一个Activity到工程中]:https://github.com/geekist/developer_guide/blob/main/activity/添加一个activity到工程中.md

[添加一个Fragment到工程中]:https://github.com/geekist/developer_guide/blob/main/activity/添加一个fragment到工程中.md

[recyclerview]:https://github.com/geekist/developer_guide/blob/main/ui/RecyclerView.md

[IoT]:https://github.com/geekist/developer_guide/blob/main/IoT/IoT.md

[litepal]:https://github.com/geekist/developer_guide/blob/main/libraries/LitePal.md


[glide]:https://github.com/geekist/developer_guide/blob/main/libraries/Glide.md

[drawerlayout]:https://github.com/geekist/developer_guide/blob/main/ui/DrawerLayout+NavigationView实现Drawer侧滑.md

[Bluetooth]:https://github.com/geekist/developer_guide/blob/main/bluetooth/bluetooth.md

[handler]:https://github.com/geekist/developer_guide/blob/main/thread/handler.md

[threadpool]:https://github.com/geekist/developer_guide/blob/main/thread/threadpool.md

[tcp]:https://github.com/geekist/developer_guide/blob/main/network/tcp.md

[udp]:https://github.com/geekist/developer_guide/blob/main/network/udp.md

[BottomNavigationView]:https://github.com/geekist/developer_guide/blob/main/ui/BottomNavigationView.md

[BottomNavigationView+fragments]:https://github.com/geekist/developer_guide/blob/main/ui/BottomNavigationView+fragments.md

[QA]:https://github.com/geekist/developer_guide/blob/main/QA.md
[Kotlin]:https://github.com/geekist/developer_guide/blob/main/kotlin/kotlin.md

[android]:https://github.com/geekist/developer_guide/blob/main/android/android.md

[yy]:https://github.com/geekist/developer_guide/blob/main/yy/yy.md