
## .jar与.aar的区别

从名称上来讲，一个是java application resource；一个是android application resource；



.jar 中只包含java的class文件和清单文件
.aar 中包含了java的class文件和其他所有资源文件，包括res中资源文件
比如你的lib库是一个自定义view封装，里面不仅有class文件并且包含了资源文件，那么此时你就需要将这个库以.aar的形式来提供给使用。


## Android studio 如何生成.jar和.aar

在android studio中，新建一个module，选中android library。

![](./assets/1319564-b1d8c7e935775fb7.webp)


## Android studio 如何引用 .jar和.aar
