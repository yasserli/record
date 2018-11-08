## git

* git
    * [详细教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
    * 是一个分布式版本控制软件，最初由林纳斯·托瓦兹创作，于2005年以GPL发布。最初目的是为更好地管理Linux内核开发而设计。

* 常用命令
    * git clone
        //克隆远程分支到本地仓库 
    * git add
        //修改文件添加到缓冲区
    * git commit 
        //缓冲区提交到本地仓库
    * git push 
        //推送到远程分支
    * git pull 
        //拉取最新分支
    * git status
        //查看状态 
    * git diff 
        //查看修改
    * git log 
        //查看提交记录
    * git reset --hard
        //撤回到某个记录 
    * git checkout 
        //检出分支，对分支的操作等

* 解决冲突
    * 所谓冲突
        * 2018-08-08 07:00:00发生事件：假设 远程仓库 debug.txt 只有唯一的一行代码 `hello,world`(此时远程仓库 debug.txt 的状态记录为 A1)，参与开发者有 研发员X 和 研发员Y 都克隆 A1 到本地仓库；
        
        * 2018-08-08 08:00:00发生事件：研发员Y 把 debug.txt 只有唯一的一行代码 `hello,world` 改为 `hello,Russia!`，但并没有推送到远程仓库
        
        * 2018-08-08 09:00:00发生事件：研发员X 把 debug.txt 只有唯一的一行代码 `hello,world` 改为 `hello,America!` 并 git add debug.txt -> git commit debug.txt -m 
        '提交备注'->git push ，结果是 成功推送到远程仓库；
        
        * 2018-08-08 09:00:00发生事件：当 研发员X 成功推送到远程仓库后，此时远程仓库 debug.txt 的状态记录为 A2；
        
        * 2018-08-08 10:00:00发生事件：此时，当 研发员Y 把他本地的 debug.txt 推送到远程仓库时，git add debug.txt -> git commit debug.txt -m '提交备注'->git push ，
        结果是 提示错误信息：error: failed to push some refs to 'git@github.com:ooooooooo.git'，这就是冲突
    
    * 解决修改、提交时，同一分支下的冲突
    ```
    
    方案一：
        研发员Y 根据冲突进行适当的修改，即兼容 研发员X 的提交代码
            git pull //拉取 A2 的最新代码
            git status //查看 A2 与 研发员Y 的冲突文件
            git diff //查看 A2 与 研发员Y 的具体冲突
            手动编辑解决（所有）冲突
            git add -all //添加修改的文件到缓冲区
            git commit -a -m 'resolve conflict' //提交缓冲区的文件到本地仓库，若不用 -a 则提示错误信息：fatal: cannot do a partial commit during a merge.
            当未解决冲突（即未 git push 成功前）时操作 git pull 或 git merge 会遇到类似下面的 提示错误信息：You have not concluded your merge (MERGE_HEAD exists).
            git push //本地仓库推送远程分支成功
    
    方案二：
        研发员Y 放弃本地仓库修改，直接获取远程仓库最新的 A2 ——相当于删除本地仓库重新克隆一次远程仓库到本地
            
            git reset --hard //还原本地仓库至最近一次没有冲突的提交
            git pull //拉取远程仓库最新git
            
            或
            
            把本地仓库目录删了，重新git clone
    ```

    * 解决合并不同分支下的冲突
        * [场景与解决](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840202368c74be33fbd884e71b570f2cc3c0d1dcf000)











