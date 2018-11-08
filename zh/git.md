## git

* git

* 解决冲突
    * 解决修改、提交时，同一分支下的冲突
    ```
    当 git pull 或 git merge 的时候遇到类似下面的错误：You have not concluded your merge (MERGE_HEAD exists).
    方案一：
        
    方案二：
        放弃本地仓库修改，直接获取远程仓库最新的git ——相当于删除本地仓库重新克隆一次远程仓库到本地
            git reset --hard //还原本地仓库至最近一次提交
            或
            把本地仓库目录删了，重新git clone
    ```

    * 解决合并不同分支下的冲突











