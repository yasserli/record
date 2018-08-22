##命令
### windows <div id='windows'></div>
* dos
    * wmic //进入命令行系统管理执行脚本界面
    * cls //清空屏幕显示
    * netstat -ano | findstr "80" //列出进程极其占用的端口，且包含 80
    * tasklist | findstr 2000 //列出进程号为2000的进程信息
    * taskkill -PID <进程号> -F //强制关闭某个进程 
    * ntoskrnl
        ```
        Apache无法启动是因为win10的ntoskrnl.exe进程占用80端口，导致Apache无法正常启动运行。
         TCP    [::]:80                [::]:0                 LISTENING       4
        通过任务管理器找到PID 4的进程为ntoskrnl.exe
        
        禁用World Wide Web Publishing Service等服务
        Win+X，打开命令提示符（管理员）
            禁用ntoskrnl服务：
                net stop http //停止 HTTP Service 服务
                Y //确认禁用
                Sc config http start= disabled //保存服务配置
        ```
    


### mac <div id='mac'></div>



### ubuntu <div id='ubuntu'></div>



### linux <div id='linux'></div>
* 网络
    * 重启网络
        * service network restart (本质是/etc/init.d/network)
    * 修改host
        * /etc/hosts
* 系统
    * 查看系统信息
        * uname -a
* 时间
    * 同步
         ```
        ntpdate -u 210.72.145.44 :网络时间同步命令
            注意：若不加上-u参数， 会出现以下提示：no server suitable for synchronization found
            -u：从man ntpdate中可以看出-u参数可以越过防火墙与主机同步；
            210.72.145.44：中国国家授时中心的官方服务器。
        ntp常用服务器：
            中国国家授时中心：210.72.145.44
            NTP服务器(上海) ：ntp.api.bz
            美国：time.nist.gov 
            复旦：ntp.fudan.edu.cn
        ```
    * 设置时间
    ```
    date命令：
        date :查看当前时间，结果如下：Wed Feb  4 16:29:51 CST 2015
        date -s 16:30:00 :设置当前时间，结果如下：Wed Feb  4 16:30:00 CST 2015
        date -s "YYYY-MM-DD hh:mm[:ss]" 如date -s "2015-02-04 16:30:00"
        hwclock -w（将时间写入bios避免重启失效）
    即时生效：
        cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
        hwclock
    重启生效：
        修改/etc/sysconfig/clock文件，把ZONE的值改为Asia/Shanghai，UTC值改为false
    ```
* 编辑
    * vi
        * 默认显示行号
        ```
        vi ~/.vimrc //在 ~ 目录添加或编辑 .vimrc 文件
        set number //设置默认显示行号
        ```
    ```
    使用 vi 打开 /abc.txt 文件时提示 Found a swap file by the name......
        此问题是因为之前的编辑 /abc.txt 此文件的时候没有正常退出引起的。同时在当前目录下产生了一个 .xxx(abc.txt).swp 的隐藏文件。
    ls -a //查看所有文件
    rm -rf .abc.txt.swp //强制删除
    ```
* 环境变量
    ```
    编辑你的 PATH 声明，其格式为：
        PATH=$PATH:<PATH 1>:<PATH 2>:<PATH 3>:------:<PATH N>
    你可以自己加上指定的路径，中间用冒号隔开。环境变量更改后，在用户下次登陆时生效，如果想立刻生效，则可执行下面的语句：$ source .bash_profile
    需要注意的是，最好不要把当前路径 “./” 放到 PATH 里，这样可能会受到意想不到的攻击。完成后，可以通过 $ echo $PATH 查看当前的搜索路径。
    
    可用 export 命令查看PATH值
    
    添加PATH环境变量(临时)，可用：
        export PATH=/mysql/bin:$PATH
    永久添加环境变量(影响当前用户)：
        vim ~/.bashrc
            export PATH="/mysql/bin:$PATH"
    永久添加环境变量(影响所有用户)：
        vim /etc/profile
            export PATH="/mysql/bin:$PATH"
        source /etc/profile
    问题： 
    1. 做了各实验，在/etc/profile, ~/.profile, ~/.bashrc中加入新PATH，重启都没有效果，只有使用source才可以，ubunt12.04
     找到原因，~/.zshrc导致的，因为在zshrc中直接对PATH重新赋值，而没有继承之前的$PATH，导致启动加载完/etc/profile后，PATH又被重新赋值。
    ```

* [composer](https://getcomposer.org/)
    * [install](https://getcomposer.org/download/)
    ```
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
    php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
    php composer-setup.php
    php -r "unlink('composer-setup.php');" 
    ```
    * [换源](https://pkg.phpcomposer.com/)，加快下载速度
        * 有两种方式启用本镜像服务：
          ```
          系统全局配置： 即将配置信息添加到 Composer 的全局配置文件 config.json 中。
          单个项目配置： 将配置信息添加到某个项目的 composer.json 文件中。
          ```
* [lnmp](https://lnmp.org/install.html)
    * 卸载
        * cd /home/yasserli/lnmp1.5 //进入下载lnmp包后的解压目录
        * ./uninstall.sh //卸载
        * 本人卸载完后network无法使用，所以重启系统后恢复正常
    * LNMP相关配置文件位置
        ```
        Nginx主配置(默认虚拟主机)文件：/usr/local/nginx/conf/nginx.conf
        添加的虚拟主机配置文件：/usr/local/nginx/conf/vhost/域名.conf
        MySQL配置文件：/etc/my.cnf
        PHP配置文件：/usr/local/php/etc/php.ini
        php-fpm配置文件：/usr/local/php/etc/php-fpm.conf
        PureFtpd配置文件：/usr/local/pureftpd/pure-ftpd.conf 1.3及更高版本：/usr/local/pureftpd/etc/pure-ftpd.conf
        PureFtpd MySQL配置文件：/usr/local/pureftpd/pureftpd-mysql.conf
        Proftpd配置文件：/usr/local/proftpd/etc/proftpd.conf 1.2及之前版本为/usr/local/proftpd/proftpd.conf
        Proftpd 用户配置文件：/usr/local/proftpd/etc/vhost/用户名.conf
        Redis 配置文件：/usr/local/redis/etc/redis.conf
        ```
    * LNMP状态管理命令：
        ```
        LNMP 1.2+状态管理: lnmp {start|stop|reload|restart|kill|status}
        LNMP 1.2+各个程序状态管理: lnmp {nginx|mysql|mariadb|php-fpm|pureftpd} {start|stop|reload|restart|kill|status}
        Nginx状态管理：/etc/init.d/nginx {start|stop|reload|restart}
        MySQL状态管理：/etc/init.d/mysql {start|stop|restart|reload|force-reload|status}
        Memcached状态管理：/etc/init.d/memcached {start|stop|restart}
        PHP-FPM状态管理：/etc/init.d/php-fpm {start|stop|quit|restart|reload|logrotate}
        PureFTPd状态管理： /etc/init.d/pureftpd {start|stop|restart|kill|status}
        ProFTPd状态管理： /etc/init.d/proftpd {start|stop|restart|reload}
        Redis状态管理： /etc/init.d/redis {start|stop|restart|kill}
        ```
* php
    * php -i //直接输出phpinfo()函数的信息
        * php -i > /home/yasserli/phpinfo.txt //信息写入文件
    * php -m //查看包含的扩展
    * php -v //查看php版本信息
* [laravel](https://laravel.com/)
    * [版本5.5](https://laravel-china.org/docs/laravel/5.5/installation/1282)
        * 安装 Laravel
            * composer create-project --prefer-dist laravel/laravel blog "5.5.*"
        * 安装后注意
        ```
        配置
        Public 目录
        安装 Laravel 之后，你要将 Web 服务器的根目录指向 public 目录。该目录下的 index.php 文件将作为所有进入应用程序的 
        HTTP 请求的前端控制器。
        
        配置文件
        Laravel 框架的所有配置文件都放在 config 目录中。每个选项都有注释，方便你随时查看文件并熟悉可用的选项。
        
        目录权限
        安装完 Laravel 后，你可能需要给这两个文件配置读写权限：storage 目录和 bootstrap/cache 目录应该允许 Web 服务器写入，
        否则 Laravel 将无法运行。如果你使用的是 Homestead 虚拟机，这些权限已经为你设置好了。
        
        应用密钥
        安装 Laravel 之后下一件应该做的事就是将应用程序的密钥设置为随机字符串。如果你是通过 Composer 或 Laravel 安装器安装
        的 Laravel，那这个密钥已经为你通过 php artisan key:generate 命令设置好了。
        
        通常来说，这个字符串长度为 32 个字符。密钥可以在 .env 环境文件中设置。前提是你要将 .env.example 文件重命名为 .env。
        如果应用程序密钥没有被设置，就不能确保你的用户会话和其他加密数据的安全！
        
        更多的配置
        除了以上的配置，Laravel 几乎就不需要再配置什么了。你随时就能开发！但是，可能的话，还是希望你查看 config/app.php 
        文件及其注释。它包含几个你可能想要根据你的应用来更改的选项，比如 timezone 和 locale。
        ```
        * Web 服务器配置
        ```
            优雅链接
              Apache
              Laravel 使用 public/.htaccess 文件来为前端控制器提供隐藏了 index.php 的优雅链接。如果你的 Laravel 使用了 Apache 
              作为服务容器，请务必启用 mod_rewrite模块，让服务器能够支持 .htaccess 文件的解析。
              如果 Laravel 附带的 .htaccess 文件不起作用，就尝试用下面的方法代替：
              Options +FollowSymLinks
              RewriteEngine On
              RewriteCond %{REQUEST_FILENAME} !-d
              RewriteCond %{REQUEST_FILENAME} !-f
              RewriteRule ^ index.php [L]
              
              Nginx
              如果你使用的是 Nginx，在你的站点配置中加入以下内容，它将会将所有请求都引导到 index.php 前端控制器：
              location / {
                  try_files $uri $uri/ /index.php?$query_string;
              }
        ```
        
        * 提示：php_network_getaddresses: getaddrinfo failed: Name or service not known
        ```
        一般在调用外部服务请求时候（例：file_get_contents远程请求url时），有时由于配置问题无法访问。
        解决：配置好网络，联通外网
        ```
        * 开启debug模式，显示、记录错误
        ```
        1、.env文件或者config->app.php 设置APP_DEBUG=TRUE
        2、php配置文件是否没有开启display_error选项，或者错误级别未打开
        3、php配置文件是否将错误直接存入到日志，不在运行时显示？
        ```
        * 提示：could not be opened: failed to open stream: Permission denied
        ```
        赋予项目目录权限
        chmod -R 777 /xxx
        ```
        * 提示：No application encryption key has been specified
            * 进入项目目录输入[php artisan key:generate](https://stackoverflow.com/questions/44839648/no-application-encryption-key-has-been-specified-new-laravel-app)
    * 
    ```
    * /config
        * Laravel 框架的所有配置文件都保存在 config 目录中
    * /.env
        * 在新安装好的 Laravel 应用程序中，其根目录会包含一个 .env.example 文件。如果是通过 Composer 安装的 Laravel，
    该文件会自动更名为 .env。否则，需要你手动更改一下文件名。
        * 你的 .env 文件不应该提交到应用程序的源代码控制系统中，因为每个使用你的应用程序的开发人员 / 服务器可能需要有一个不同的环境配置。
        此外，在入侵者获得你的源代码控制仓库的访问权的情况下，这会成为一个安全隐患，因为任何敏感的凭据都被暴露了。
        * 如果是团队开发，则可能希望应用程序中仍包含 .env.example 文件。因为通过在示例配置文件中放置占位值，
        团队中的其他开发人员可以清楚地看到哪些环境变量是运行应用程序所必需的。你也可以创建一个 .env.testing 文件，
        当运行 PHPUnit 测试或以 --env=testing 为选项执行 Artisan 命令时，该文件将覆盖 .env 文件中的值。
          
        * {tip} .env 文件中的所有变量都可被外部环境变量（比如服务器级或系统级环境变量）所覆盖。
    ```




