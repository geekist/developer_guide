

## 如何在github中创建目录

使用[gh-md-toc](https://github.com/ekalinin/github-markdown-toc)工具来实现生成目录。

该工具在windows下面不太友好，但是提供了go语言的包，可以直接下载go语言的二进制文件，然后使用即可。

go语言的二进制包下面目录如下：

https://github.com/ekalinin/github-markdown-toc.go/releases

![](./assets/markdown_1.png)


下载下来之后，发现没有后缀名无法识别，实际上这是个exe文件，所以只需要暴力地在后面加上.exe就可以开始愉快使用了。

首先将README.md文档复制到gh-md-toc.exe的根目录下。

接着按住shift键同时右击。



打开Powershell窗口后，直接键入。

./gh-md-toc.exe README.md
1


接下来只需将这段话复制粘贴到README.md里面即可。

