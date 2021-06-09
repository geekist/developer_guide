

## JAR 包是什么

JAR 文件的全称 Java Archive File（Java 档案文件）,通常 JAR 文件是一种压缩格式，和 ZIP 格式兼容，与 ZIP格式不同的是它 包含了一个名为 META-INF/MANIFEST.MF的清单文件,这个清单文件是由生成 JAR 包的时候系统自动创建的,这个清单文件与我们可以不做关注.

## JAR 包的用途

当我们开发了一个程序以后，程序中有很多的类，如果需要提供给别人使用,发给对方一堆的源文件是非常不好的，通常需要把这些类打包成一个 JAR 包,把这个 JAR 包提供给别人使用,只需要别人在CLASSPATH 环境变量中添加这个 JAR 包,则 Java 虚拟机就可以在内存中解析这个 JAR 包了，这个 JAR 包就是一个路径,就像我们电脑访问普通文件一样, Java 虚拟机会根据路径查找相应的文件.

## JAR 包的优点

体积小,安全,可移植性强，封装好等优点.

## jar命令的使用详解：

jar 命令是随jdk 自动安装的,在 jdk 安装目录的 bin 目录中,我们可能需要先将 tools.jar配置到classpath环境变量中,才能正常使用.(关于为什么需要配置环境变量才能使用,在文末会给出解释),下面让我们了解一下常用的 jar 命令吧!

### 1.创建JAR 文件

jar cvf test.jar test

这里参数 c 创建，v 显示创建过程文件的详细信息

该命令的意思是将 test 目录下的所有文件打包成一个 test.jar文件,如果 test.jar 文件已经存在则覆盖. cf 参数是必须的.


### 2.查看 JAR 文件内容列表

jar tvf test.jar

这里参数 t 显示列表 ,v 显示文件的详细信息,如果不想显示详细信息可去掉参数 v,tf参数是必须的.
当 jar 包文件特别多的时候，我们可以使用命令 jar tvf test.jar > a.txt 将列表结果保存在 a.txt 文本中进行查看.

### 3.解压JAR 文件

jar xvf test.jar

将 test.jar 文件解压到当前的目录下,加上参数 v,显示详细解压缩信息

### 4.更新 JAR 文件

jar uvf test.jar Hello.class

如果已经存在该文件,则替换该文件,如果不存在则添加该文件.

## 程序的发布方式

我们的程序执行都是从 main 函数为入口的，但我们的程序开发完毕以后我们怎么方便的让别人使用那？一般有两种方式：

### 1、制作一个批处理文件。

以 windows 为例,我们可以在创建一个 start.bat 文件中定义如下命令 **java 主类的路径(含有 main 函数的类) ** ,例如 ：

java C://user/test/MainTest

用户点击 start.bat 文件的时候，将会执行文件内的java 命令,这样 MainTest 类的 main 函数就会运行.如果不想保留运行的 java 命令行窗口,可在批处理文件中定义如下命令：

start java package.MainClass

### 2.制作可执行 jar 包

制作可执行 jar 包的关键在于我们要让执行命令知道 jar 包那个类是主类,因此在制作 jar 包的时候有一个 e ，该选项用于指定程序入口主类的类名,因此在制作一个可执行 jar 报的时候需要添加一个e选项

jar cfe test.jar test.Test test

jar -cvfe 可执行文件名 入口 需要打包文件

该命令的意思是将 test 目录下的所有文件都压缩到 test.jar 文件内,并且指定 test 目录下 Test类为程序的主类 作为程序的主类,如果主类带有包名,这必须制定完整的包名
命令中参数等对应顺序不能颠倒


运行可执行 jar 包的方式有两种(仅仅指可执行 jar 包)：

使用 java 命令, java -jar test.jar

使用 javaw 命令, javaw test.jar

jar包添加到环境变量后有什么用?

在 windows 系统中,需要将tools.jar 和dt.jar添加到环境变量,我们平时使用的 jdk bin目录下的 java javac 等命令其实都是调用tools.jar里面的类来执行的;dt.jar主要是包含swing类库,如果在开发的时候用不到 swing 类库,则可以不添加.

