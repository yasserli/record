## 终端
### unix <div id='unix'></div>
* Unix/Linux基本哲学之一就是“一切皆文件”，都可以用“打开open –> 读写write/read –> 关闭close”模式来操作。

* 当进程不是守护进程时，不能简单地在命令行后添加一个&，当终端关闭时，该进程也随之关闭。
因为通常在终端起动的进程其父进程是终端进程。当终端关闭时，其所有子进程也随之关闭。
使进程在后台执行需要使用nohup命令：
    * nohup command > out.log

* 查看进程
    * ps -ef | grep nginx



















