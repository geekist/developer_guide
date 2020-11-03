
BottomNavigationView实现的效果就是常见的app底部导航栏的效果，且自带了badge的风格。



BottomNavigation + fragments实现多fragments的效果。

方法参照UDPserver的例子，但是注意有两个坑：
fragement的切换，用hide、show的方法，当系统回收fragments时，可能会有问题。
用replace方法，因为android原代码问题，在arraylist的遍历过程中删除，会删除不干净，7.0修复了这个问题。