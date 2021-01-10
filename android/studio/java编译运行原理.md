
# Java编译运行原理

## 1、Java程序编译运行过程

* 编写一个java文件：hello.java

```java
public class Hello{
	public static void main(String[] args) {
		System.out.println("Hello World");
	}
}
```

* 在cmd模式下执行：javac hello.java

 javac是编译命令，将hello.java源文件编译成hello.class字节码文件。

* 在cmd模式下执行：java hello

java：是运行字节码文件；由java虚拟机对字节码进行解释和运行。

![](https://img-blog.csdn.net/20160724134932709)

## 2、Java程序编译原理

### Java 源码编译由以下三个过程组成：

**分析和输入到符号表**

**注解处理**

**语义分析和生成class文件**

流程图如下所示：

![java编译过程](https://img-blog.csdnimg.cn/20210110203124811.png)
 
最后生成的class字节码文件由以下部分组成：

* 结构信息:

包括class文件格式版本号及各部分的数量与大小的信息

* 元数据:

对应于Java源码中声明与常量的信息。包含类/继承的超类/实现的接口的声明信息、域与方法声明信息和常量池

* 方法信息:

对应Java源码中语句和表达式对应的信息。包含字节码、异常处理器表、求值栈与局部变量区大小、求值栈的类型记录、调试符号信息

编译后的字节码文件格式主要分为两部分：

**常量池和方法字节码。常量池记录的是代码出现过的所有token(类名，成员变量名等等)以及符号引用（方法引用，成员变量引用等等）；方法字节码放的是类中各个方法的字节码**


## 3、Java程序执行原理



假设有一个java文件MainApp：
```java
//MainApp.java  
public class MainApp {  
    public static void main(String[] args) {  
        Animal animal = new Animal("Puppy");  
        animal.printName();  
    }  
}  
//Animal.java  
public class Animal {  
    public String name;  
    public Animal(String name) {  
        this.name = name;  
    }  
    public void printName() {  
        System.out.println("Animal ["+name+"]");  
    }  
}  

```

### 3.1JVM加载Java程序装载原理：

JVM的类加载是通过ClassLoader及其子类来完成的，类的层次关系和加载顺序可以由下图来描述：

![](https://img-blog.csdnimg.cn/2021011020493653.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lhbmd3dTAwNw==,size_16,color_FFFFFF,t_70)

* 1）Bootstrap ClassLoader
负责加载 $JAVA_HOME中jre/lib/rt.jar里所有的class，由C++实现，不是ClassLoader子类

* 2）Extension ClassLoader
负责加载java平台中扩展功能的一些jar包，包括$JAVA_HOME中jre/lib/*.jar或-Djava.ext.dirs指定目录下的jar包

* 3）App ClassLoader
负责记载classpath中指定的jar包及目录中class

* 4）Custom ClassLoader
属于应用程序根据自身需要自定义的ClassLoader，如tomcat、jboss都会根据j2ee规范自行实现ClassLoader

加载过程中会先检查类是否被已加载，检查顺序是自底向上，从Custom ClassLoader到BootStrap ClassLoader逐层检查，只要某个classloader已加载就视为已加载此类，保证此类只所有ClassLoader加载一次。而加载的顺序是自顶向下，也就是由上层来逐层尝试加载此类。

### 3.2 java程序运行的过程

* 在编译好java程序得到MainApp.class文件后，在命令行上敲java AppMain。系统就会启动一个jvm进程，jvm进程从classpath路径中找到一个名为MainApp.class的二进制文件，**将MainApp的类信息加载到运行时数据区的方法区内**，这个过程叫做MainApp类的加载。

* 然后JVM找到AppMain的主函数入口，开始执行main函数。


* main函数的第一条命令是Animal animal = new Animal(“Puppy”);就是让JVM创建一个Animal对象，但是这时候方法区中没有Animal类的信息，所以JVM马上**加载Animal类，把Animal类的类型信息放到方法区中**。


* 加载完Animal类之后，Java虚拟机做的第一件事情就是**在堆区中为一个新的Animal实例分配内存**, 然后调用构造函数初始化Animal实例，这个Animal实例持有着指向方法区的Animal类的类型信息（其中包含有方法表，java动态绑定的底层实现）的引用。


* 当使用animal.printName()的时候，JVM根据animal引用找到Animal对象，然后根据Animal对象持有的引用定位到方法区中Animal类的类型信息的方法表，获得printName()函数的字节码的地址。
* 开始运行printName()函数。

![](https://img-blog.csdnimg.cn/20210110204317434.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lhbmd3dTAwNw==,size_16,color_FFFFFF,t_70)






