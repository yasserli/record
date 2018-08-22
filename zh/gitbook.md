##gitbook

* gitbook
    * [nvm](https://github.com/coreybutler/nvm-windows/releases)
    * [nodejs](https://nodejs.org/en/download/)
        * [n](https://github.com/tj/n)
        
* gitbook使用
    * ``book.json`` gitbook配置文件
        ```
        title - 标题
        author - 作者信息
        description - 书本描述
        language - 使用的语言
        links - 在侧边栏添加链接
        styles - 自定义样式
        plugins - 插件
        pluginsConfig - 插件配置
        gitbook - 指定gitbook版本
        ```
    * Gitbook需要2个基本文件：README.md，SUMMARY.md
        * README.md是关于你的书的介绍，相当于一本Gitbook的简介
        * SUMMARY.md中则包含了书目，使用Markdown语法，即章节结构，它的格式如下：
          ```
          bui# Summary
          * [第1章](c1.md) 
            * [第1节](c1s1.md) 
            * [第2节](c1s2.md)
          * [第2章](c2.md)
            * [第1节](c2s1.md) 
            * [第2节](c2s2.md)
          ```

* gitbook命令
    * ``gitbook --version`` 查看版本
    * ``gitbook --help`` 查看帮助
    * ``gitbook init`` 初始化当前目录为gitbook图书目录
    * ``gitbook serve`` 内置服务器浏览本地gitbook图书，默认http://localhost:4000
    * ``gitbook build`` 导入当前目录并在当前目录格式化后导出(生成)``_book``目录，默认格式html（3.0.0后gitbook版本的html无法跳转）
        * ``gitbook build . my_book --gitbook=2.6.7`` 
            * ``.``：要导入的图书目录路径，
            * ``my_book``：格式化后导出的文件路径，
            * ``--gitbook=2.6.7``：指定使用gitbook的2.6.7版本生成
        * 如果需要导出本地可跳转html，建议使用nvm管理nodejs（如已安装但又不懂nodejs则把原有的卸载了）
            * [nvm](https://github.com/coreybutler/nvm-windows/releases)，[教程](https://segmentfault.com/a/1190000007612011)
        * 走过的路：
            * 提示nodejs缺少模块
            * 需要降级nodejs（其实只需要使用低版本gitbook就可以）
            * [n](https://github.com/tj/n)是node的一个模块， 是一个需要全局安装的 npm package，安装命令：npm install -g n
                * ``n latest`` 安装最新的版本
                * ``n stable`` 安装稳定版本
                * ``n 6.9.1`` 安装或使用某个版本，例如6.9.1版
    * ``gitbook pdf d:/aabb abc.pdf`` gitbook目录生成本地图书，格式为pdf，``d:/aabb``：要导入的图书目录路径，``abc.pdf``：格式化后导出的文件路径
        * 导出pdf格式需要下载安装[calibre](https://calibre-ebook.com/download)插件
    * ``gitbook uninstall 2.0.1`` 卸载对应的gitbook版本
    * ``gitbook fetch 2.1.0`` 安装对应的gitbook版本，可以使用标签(例：beta)或者版本号(例：2.1.0)安装

* gitbook安装
    * 方案一：
        * 下载nodejs
        * 进入命令行：
        ```
            npm install -g gitbook ##安装gitbook
            npm install -g gitbook-cli ##安装gitbook客户端
            cd d:/xxx/yyy ##进入本地图书目录
            gitbook serve ##使用内置服务浏览图书
        ```
    * 方案二：
        * 本人使用[nvm管理](https://segmentfault.com/a/1190000007612011)下载，其他步骤结合方案一

