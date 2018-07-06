##gitbook

* gitbook
    * [nvm](https://github.com/coreybutler/nvm-windows/releases)
    * [nodejs](https://nodejs.org/en/download/)
        * [n](https://github.com/tj/n)

* gitbook命令
    * ``gitbook --version`` 查看版本
    * ``gitbook --help`` 查看帮助
    * ``gitbook init`` 初始化当前目录为gitbook图书目录
    * ``gitbook serve`` 内置服务器浏览本地gitbook图书，默认http://localhost:4000
    * ``gitbook build`` 导入当前目录并在当前目录格式化后导出(生成)``_book``目录，默认格式html（3.0.0后gitbook版本的html无法跳转）
        * ``gitbook build . my_book --gitbook=2.6.7`` ``.``：要导入的图书目录路径，``my_book``：格式化后导出的文件路径，``--gitbook=2.6.7``：指定使用gitbook的2.6.7版本生成
        * 如果需要导出本地可跳转html，建议使用nvm管理nodejs（如已安装但又不懂nodejs则把原有的卸载了）
            * [nvm](https://github.com/coreybutler/nvm-windows/releases)，[教程](https://segmentfault.com/a/1190000007612011)
        * 走过的路：
            * [n](https://github.com/tj/n)是node的一个模块， 是一个需要全局安装的 npm package，安装命令：npm install -g n
                * ``n latest`` 安装最新的版本
                * ``n stable`` 安装稳定版本
                * ``n 6.9.1`` 安装或使用某个版本，例如6.9.1版
    * ``gitbook pdf d:/aabb abc.pdf`` gitbook目录生成本地图书，格式为pdf，``d:/aabb``：要导入的图书目录路径，``abc.pdf``：格式化后导出的文件路径
        * 导出pdf格式需要下载安装[calibre](https://calibre-ebook.com/download)插件

* gitbook安装
    * 下载

