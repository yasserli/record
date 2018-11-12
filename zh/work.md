## work
以下都是windows系统：

* 软件
    * 加密 `Shadowsocks.exe`，特点：科学学习
    * 加密 `ProxifierSetup.exe`，特点：配合`Shadowsocks.exe`全系统科学学习
    * 火绒安全软件 `sysdiag-full-4.0.74.5.exe`，优点：无广告
    * 编程语言 `go1.11.2.windows-amd64.msi`，特点：编程语言，可快速创建小应用
    * 数据库 `mysql-installer-web-community-8.0.11.0.msi`，特点：小巧便捷数据库
    * 数据库管理 `navicat120_premium_cs_x64.exe`，特点：操作数据库功能多
    * 浏览器 `ChromeStandaloneSetup.exe`，描述：谷歌浏览器离线安装包；特点：简易web开发调试
    * ftp `FileZilla_3.21.0_win64-setup.exe`，特点：可同步目录（即本地与远程目录同步，方便修改提交）
    * 缓存 `Redis-x64-3.2.100.msi`，特点：高效缓存
    * 虚拟机管理 `vagrant_2.1.2_x86_64.msi`，特点：快速构建php网站虚拟系统
    * 虚拟机 `VMware-workstation-full-14.1.2-8497320.exe`，特点：虚拟机
    * 目录树 `tree-1.5.2.2-setup.exe`，特点：显示目录的树结构
    * 效率启动器 `Wox-1.3.524.exe`，特点：类mac的`Alfred`，而`Alfred`它是增强版的`Spotlight`
    * 搜索 `Everything-1.3.4.686.x64-Setup.exe`，描述：好像与`Wox`配合，忘了
    * js `node-v10.5.0-x64.msi`【待续】，特点：JavaScript的开放源代码、跨平台JavaScript 运行环境
    * 管理器 `nvm-setup.exe`【待续】，特点：管理nodejs，NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题
    * 调试 `Postman-win64-6.0.10-Setup.exe`，特点：接口调试
    * ftp和远程 `SecureCRT.exe`，特点：ftp与远程
    * 内网穿透 `ngrok.exe`，特点：内网穿透
    * 编程语言 `python-3.5.1.exe`，特点：专注ai等
    * 消息日志 `kafka_2.12-2.0.0.tgz`，特点：消息、日志、监控等
    * gitbook依赖 `calibre-64bit-3.26.1.msi`，特点：gitbook生成pdf等电子书需要，生成html就不需要
    * 编辑器 `npp_6.9.2_Installer.exe`，特点：轻量编辑器
    * 编辑器 `PhpStorm-2018.2.5.exe`，特点：专注php开发
    * 管理器 `Git-2.19.1-64-bit.exe`，特点：git管理
    


* bat批量
    * copy_phpgen.bat
    ```
    @echo off
    
    ECHO open D disk...
    d:
    
    ECHO cd protocols...
    cd D:\lys\protocols
    
    ECHO rmdir php-gen...
    rmdir /s /q php-gen
    
    ECHO git checkout :/
    git checkout :/
    
    ECHO git pull..
    git pull
    
    ECHO xcoppy file...
    xcopy php-gen d:\lys\saas\php-gen /y /e /q /i
    
    EXIT
    
    ::暂停
    PAUSE
    ```
    
    * run_software.bat
    ```
    echo start running software...
    
    start /min "" "D:/lys/Shadowsocks.exe"
    start /min "" "C:\Program Files (x86)\Google\Chrome\Application\chrome.exe"
    start /min "" "D:\Apache24\bin\ApacheMonitor.exe"
    start /min "" "D:\Program Files (x86)\Tencent\TIM\Bin\QQScLauncher.exe"
    start /min "" "C:\Users\yasserli\AppData\Local\youdao\dict\Application\YoudaoDict.exe"
    ::start /min "" C:\Users\yasserli\AppData\Local\Postman\Update.exe --processStart "Postman.exe"
    start /min "" "D:\Program Files\JetBrains\PhpStorm 2018.1.2\bin\phpstorm64.exe"
    
    echo end!!!
    ```
    
    * git_pull.bat
    ```
    @echo off
    
    ECHO open D disk...
    d:
    
    ECHO pull ui...
    cd d:\lys\ui
    ::更新ui的git
    git pull
    
    ECHO pull template...
    cd d:\lys\saas_frontend
    git pull
    
    ECHO pull template1...
    cd d:\lys\saas_frontend_v0.1
    git pull
    
    ECHO pull source...
    cd D:\lys\product
    git pull
    
    
    ECHO pull grpc...
    cd D:\lys\protocols
    git pull
    
    
    ECHO exit...
    exit
    
    
    ::暂停
    PAUSE
    ```
    
    * apache.bat
    ```
    echo start...
    
    d: & cd Apache24\bin & httpd -k start
    
    echo end...
    ```







