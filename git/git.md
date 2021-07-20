
# Git

## Git介绍 

* [git介绍](https://www.cnblogs.com/tugenhua0707/p/4050072.html)


# Git常见命令

* [git基本操作](https://www.runoob.com/git/git-basic-operations.html)


## 常用命令

### 如何移除远程代码仓库中的无用的分支？

  git remote prune 
   
  git remote prune 命令可以删除本地版本库上那些失效的远程追踪分支，具体用法如下：

   step1： 查看那些分支需要清理：

   git remote prune origin --dry-run

step2: 清理无效的分支：

  git remote prune origin 清除本地存在但是远程服务器已经删除的分支。

 * 1、进入代码仓库的目录中，输入
git remote show origin   //显示本地仓库追踪的远程仓库状态

* 2、输入：
git rimote prune origin   //清除所有失效的远程分支或根据提示杀手拿出特定失效的分支。


# Gitee

* [Gitee介绍](https://github.com/geekist/developer_guide/blob/main/git/gitee.md)

