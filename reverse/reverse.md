# Android 反编译apk逆向分析

计算机软件反向工程（Reversepengineering）也称为计算机软件还原工程，是指通过对他人软件的目标程序（可执行程序）进行“逆向分析、研究”工作，以推导出他人的软件产品所使用的思路、原理、结构、算法、处理过程、运行方法等设计要素，作为自己开发软件时的参考，或者直接用于自己的软件产品中。高级语言源程序经过编译变成可执行文件，反编译就是逆过程，但是通常不能把可执行文件变成高级语言源代码，只能转换成汇编程序。反编译是一个复杂的过程,所以越是高级语言,就越难于反编译。

## 反编译工具 dex2jar、JD-GUI、apktool、AXMLPrinter2.jar


- dex2jar

将dex转化成jar 下载地址： 

https://sourceforge.net/projects/dex2jar/files/?source=navbar


- JD-GUI

反编译jar中的源码 下载地址：http://jd.benow.ca/

- apktool


资源文件的还原 下载地址：apktool.jar http://ibotpeaches.github.io/Apktool/


- AXMLPrinter2.jar

AndroidManifest.xml的反编译。


## 将dex文件转换为jar文件，然后在jd-gui里面可以查看或保存。


- 步骤1：将apk文件后缀改为zip，并解压。
 
- 步骤2：反编译classes.dex文件：执行 d2j-dex2jar.bat classes.dex

- 步骤3：在jd-gui里面查看或保存。

## 用AXMLPrinter.jar反编译xml文件：执行java -jar AXMLPrinter2.jar A.xml > B.xml


## 用apktool反编译apk
- apktool d xxx.apk





