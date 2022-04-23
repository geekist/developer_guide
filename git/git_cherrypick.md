

案例场景： 将develop分支上的commitID为7fcbede的提交合并到master分支

## step1： 切换到develop分支，找到该commitId；

git checkout develop

git log

## step2：切换到master分支，执行cherry-pick，将该提交在本地合并到master分支；


git checkout master

git cherry-pick 7fcbede

## step3：在master分支上执行git push，将master分支上的更新提交到远程。

git push


代码开发的时候，有时需要把某分支（比如develop分支）的某一次提交合并到另一分支（比如master分支），这就需要用到git cherry-pick命令。

首先，切换到develop分支，敲 git log 命令，查找需要合并的commit记录，比如commitID：7fcb3defff；

然后，切换到master分支，使用 git cherry-pick 7fcb3defff  命令，就把该条commit记录合并到了master分支，这只是在本地合并到了master分支；

最后，git push 提交到master远程，至此，就把develop分支的这条commit所涉及的更改合并到了master分支。
