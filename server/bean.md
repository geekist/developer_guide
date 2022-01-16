Java Bean 与Spring Bean的区别

>**什么是JavaBean:**


JavaBean是一种JAVA语言写的可重用组件。JavaBean符合一定规范写的Java类，是一种规范。它的方法命名，构造以及行为必须符合特定的要求：

1. 所有属性为private

2. 这个类必须具有一个公共的（public）无参构造函数

3. private属性必须提供public的getter和setter来给外部访问，并且方法的命名也必须遵循一定的命名规范

4. 这个类是可序列化的，要实现serializable接口

JavaBean，类必须是具体的和公共的，并且具有无参数的构造器。JavaBean 通过提供符合一致性设计模式的公共方法将内部域暴露成员属性。众所周知，属性名称符合这种模式，其他Java 类可以通过自身机制发现和操作这些JavaBean 的属性。


>**什么是SpringBean:**


SpringBean是受Spring管理的对象  所有能受Spring容器管理的对象都可以成为SpringBean。

Spring中的bean，是通过配置文件、javaconfig等的设置，有Spring自动实例化，用完后自动销毁的对象。让我们只需要在用的时候使用对象就可以，不用考虑如果创建类对象（这就是spring的注入）。

>**二者之间的区别：**


* 用处不同：


　　传统javabean更多地作为值传递参数，而spring中的bean用处几乎无处不在，任何组件都可以被称为bean。

* 写法不同：


　　传统javabean作为值对象，要求每个属性都提供getter和setter方法；但spring中的bean只需为接受设值注入的属性提供setter方法。

* 生命周期不同：

　　传统javabean作为值对象传递，不接受任何容器管理其生命周期；spring中的bean有spring管理其生命周期行为。

