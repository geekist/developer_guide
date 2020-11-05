## UI
 * textview如何多行显示，且带有滚动条？


```java
  <TextView
        android:inputType="textMultiLine"
        android:scrollbars="vertical"
        android:singleLine="false"
        
        />

```
然后在代码中加上：
```java
//      设置内容可滚动
        tv_dialog_flower_content.movementMethod = ScrollingMovementMethod.getInstance()
```

## Android stuido

* Android Studio升级后如何导入旧版本工程？

  1、首先在gradle/wapper下面打开gradle-wrapper.properties文件，将下面一行替换成当前工程的版本：

```java
#distributionUrl=https\://services.gradle.org/distributions/gradle-2.14.1-all.zip
distributionUrl=https\://services.gradle.org/distributions/gradle-6.5-bin.zip
```

2、修改library和app模块下面的build.gradle文件，将编译版本号修改为当前版本号。

3、在工程的build.gradle文件中，将classpath修改为当前的版本号

```java

 dependencies {
        classpath "com.android.tools.build:gradle:4.1.0"


```
4、添加google的仓库选择
```java

  repositories {
        google()
        jcenter()
    }
```

5、注意，如果有compile的关键字，改为implementation关键字

6、关闭工程重新打开即可。

* windows下如何彻底删除androidstudio？

    1、删除android studio，右键卸载即可。
    2、删除sdk文件夹。
    按道理理论上应该有两处：
    第一处，是你安装目录，在你当初选择的目录下的sdk文件，不大。
    第二处，是C盘->用户->《用户名目录》->AppData->Local->android->sdk文件。我的非常大，十几个G呢，但是直接删除还是很快的。
    3、删除sdk环境变量。
    右击我的电脑->属性->高级系统设置->环境变量->系统变量下的path->编辑或者双击打开path，找到关于android sdk的两项，移除该两项。（两项就是2步骤中sdk的路径）
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181223131821601.png)
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181223131910348.png)
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181223131939549.png)
    4、进入“C:\Users\<你的用户名下>”目录下，手动删除".android"、".AndroidStudioX.X"、".gradle"目录文件夹。

* android4.1更新后markdown无法用，下载的插件也无法用，该如何解决？

问题：

Plugin “Smalidea” is incompatible (supported only in IntelliJ IDEA).
Plugin “FindViewByMe” is incompatible (supported only in IntelliJ IDEA).
插件尚未更新，无法使用。但是在 Plugins 里也看不到这两个插件，无法卸载。

解决办法：

在 C:\Users\Administrator\AppData\Roaming\Google\AndroidStudio4.1\plugins 下找到对应插件。删除即可



## 其他