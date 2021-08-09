
## JNA介绍

### JNI回顾

JNI(Java Native Interface)允许Java代码和其他语言（尤其C/C++）写的代码进行交互，只要遵守调用约定即可。首先看下JNI调用C/C++的过程，注意写程序时自下而上，调用时自上而下。

![](https://github.com/geekist/developer_guide/blob/main/3rd/assets/jni.png)

步骤非常的多，很麻烦，使用JNI调用.dll/.so共享库都能体会到这个痛苦的过程。如果已有一个编译好的.dll/.so文件，如果使用JNI技 术调用，我们首先需要使用C语言另外写一个.dll/.so共享库，使用SUN规定的数据结构替代C语言的数据结构，调用已有的 dll/so中公布的函 数。然后再在Java中载入这个库dll/so，最后编写Java  native函数作为链接库中函数的代理。经过这些繁琐的步骤才能在Java中调用 本地代码。因此，很少有Java程序员愿意编写调用dll/.so库中原生函数的java程序。这也使Java语言在客户端上乏善可陈，可以说JNI是 Java的一大弱点！

### JNA介绍

JNA(Java Native Access)框架是一个开源的Java框架，是SUN公司主导开发的，建立在经典的JNI的基础之上的一个框架。只需要下载一个jar包，就可以使用JNA的强大功能方便地调用动态链接库中的C函数。

JNA项目地址：https://jna.dev.java.net/

![](https://github.com/geekist/developer_guide/blob/main/3rd/assets/jna.png)

### JNA使用简介：

#### 1、用idea创建一个java工程，导入jna.jar。

#### 2、创建一个接口类，导入系统的dll库。

定义一个接口，继承自Library或StdCallLibrary。默认的是继承Library，如果动态链接库里的函数是以stdcall方式输出的，那么就继承StdCallLibrary，比如众所周知的kernel32库。比如上例中的接口定义：

```java

package com.ytech;

import com.sun.jna.Library;
import com.sun.jna.Native;
import com.sun.jna.Platform;

public interface WindowsSDK extends Library {
    WindowsSDK INSTANCE = (WindowsSDK)
            Native.loadLibrary((Platform.isWindows() ? "msvcrt" : "c"),
                    WindowsSDK.class);

    void printf(String format, Object... args);
}

```

接口内部定义

接口内部需要一个公共静态常量：INSTANCE，通过这个常量，就可以获得这个接口的实例，从而使用接口的方法，也就是调用外部dll/so的函数。

该常量通过Native.loadLibrary()这个API函数获得，该函数有2个参数：

第一个参数是动态链接库dll/so的名称，但不带.dll或.so这样的后缀，这符合JNI的规范，因为带了后缀名就不可以跨操作系统平台了。

搜索动态链 

接库路径的顺序是：先从当前类的当前文件夹找，如果没有找到，再在工程当前文件夹下面找win32/win64文件夹，找到后搜索对应的dll文件，如果 找不到再到WINDOWS下面去搜索，再找不到就会抛异常了。

比如上例中printf函数在Windows平台下所在的dll库名称是msvcrt，而在 其它平台如Linux下的so库名称是c。

第二个参数是本接口的Class类型。JNA通过这个Class类型，根据指定的.dll/.so文件，动态创建接口的实例。该实例由JNA通过反射自动生成。

```java
CLibrary INSTANCE = (CLibrary)
            Native.loadLibrary((Platform.isWindows() ? "msvcrt" : "c"),
                               CLibrary.class);
```
接口中只需要定义你要用到的函数或者公共变量，不需要的可以不定义，如上例只定义printf函数：

```java
void printf(String format, Object... args);
```

注意参数和返回值的类型，应该和链接库中的函数类型保持一致。


#### 3、创建一个类，调用接口

```java
package com.ytech;

public class HKTest {
    public static void main(String[] args) throws InterruptedException {
        WindowsSDK.INSTANCE.printf("Hello, World\n");
        for (int i = 0; i < args.length; i++) {
            WindowsSDK.INSTANCE.printf("Argument %d: %s\n", i, args[i]);
        }
    }
}

```

#### 4、运行HKTest，即可看到调用了c语言的DLL库。










