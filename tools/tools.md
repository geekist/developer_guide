

## 如何在github中创建目录

github下生成目录不太方便，多方搜索后，发现可以使用[gh-md-toc](https://github.com/ekalinin/github-markdown-toc)工具来实现生成目录。


该工具在windows下面不太友好，但是提供了go语言的包，可以直接下载go语言的二进制文件，然后使用即可。

go语言的二进制包下面目录如下：

https://github.com/ekalinin/github-markdown-toc.go/releases




使用步骤：

* 1、 下载windows的压缩包

![](./assets/markdown_1.png)


* 2、用解压工具解压缩windows工具包，得到一个文件

解压缩后目录下有一个名称为gh-md-toc的文件

将该文件添加后缀，重命名为：  gh-md-toc.exe



* 3、使用命令可以创建toc目录

在该工具所在的文件夹菜单输入框中输入cmd，打开powershell界面

![](./assets/markdown_2.png)

输入
./gh-md-toc.exe README.md

![](./assets/markdown_3.png)

* 4、将创建的toc目录复制到文档中

接下来只需将这段话复制粘贴到README.md里面即可。

