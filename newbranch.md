This is newbranch.md

## git 一个分支完全覆盖另一个分支

```
方案一：【本作者测试过可行】
git push origin develop:newbranch -f 
就可以把本地的develop分支强制(-f)推送到远程newbranch



方案二：【本作者测试过可行】
//如：当前分支是develop分支，我想将develop分支上的代码完全覆盖newbranch分支，首先切换到newbranch分支。
git checkout newbranch 
    //切换到newbranch分支 
git reset -hard origin/develop 
    //将develop分支上的代码完全覆盖newbranch分支
git push -f 
    //推送到远程仓库
    //执行上面的命令后newbranch分支上的代码就完全被develop分支上的代码覆盖了，然后将本地分支强行推到远程分支

```