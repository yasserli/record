## Process
### Kafka <div id='kafka'></div>
* 描述
    * Kafka框架本身使用Scala编写，因其可水平扩展和高吞吐率而被广泛使用
    * Kafka是由LinkedIn开发的一个分布式的消息系统，同时支持离线和在线日志处理。
    
* [场景、优点](http://orchome.com/295)：
    * 场景：
        * 消息系统
        * 事件采集，网站活动追踪，指标监测
            * kafka也常常用于监测数据。分布式应用程序生成的统计数据集中聚合。
            * kafka原本的使用场景：用户的活动追踪，网站的活动（网页游览，搜索或其他用户的操作信息）发布到不同的话题中心，
            这些消息可实时处理，实时监测，也可加载到Hadoop或离线处理数据仓库。
            ```
            大数据时代，最重要的是数据的收集和分析，这些数据包括：
            
            1）.用户的行为数据
            
            2）.应用工程的性能数据
            
            3）.日志的用户活动数据等
            ```
        * 日志聚合
        * 流处理
            * 仅仅读，写和存储是不够的，kafka的目标是实时的流处理
        * 提交日志
    * 优点：
        * 支持多种语言
        * 高并发，高吞吐量
            * 主要是用来解决百万级别的数据中生产者和消费者之间数据传输的问题
        * 分布式、集群式
            * 可以将一条数据提供给多个接收这做不同的处理
            * 当两个系统是隔绝的，无法通信的时候，如果想要他们通信就需要重新构建其中的一个工程，而kafka实现了生产者和消费者之间的无缝对接
        * Kafka的持久化方案是写入磁盘，虽然内存读写速度明显快过磁盘读写速度，但Kafka却通过线性读写的方式实现快速读写。

* 提示：
    * 运行kafka需要使用Zookeeper，所以需要先启动一个Zookeeper服务器，
    如果没有Zookeeper，可以使用kafka自带打包和配置好的Zookeeper
    * [官网下载地址](https://kafka.apache.org/downloads)，使用Binary downloads
    * 新版开启消费者使用[--bootstrap-server](http://kafka.apache.org/documentation/#quickstart_consume)命令

* windows平台运行：
```
    zookeeper:【新版的Kafka自带有zookeeper，下载、安装、配置zookeeper可跳过】
        进入zookeeper设置目录，E:\soft\elk\zookeeper-3.4.8\conf
        将"zoo_sample.cfg" 复制一份，重命名为"zoo.cfg"
        打开并编辑 并编辑dataDir=/tmp/zookeeper为指定目录为E:/soft/elk/zookeeper-3.4.8/data
        编辑环境变量，Path加 E:\soft\elk\zookeeper-3.4.8\bin

    //开启kafka依赖Zookeeper
    zkserver
    
    //进入kafka安装目录
    cd D:\kafka_2.12-2.0.0
    
    //开启kafka
    bin\windows\kafka-server-start.bat config\server.properties
    
    //创建主题
    bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test1
    
    //查看主题
    bin\windows\kafka-topics.bat --list --zookeeper localhost:2181
    
    //生产者（写入）
    bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic test123
    
    //消费者（读取）【旧版使用--zookeeper，新版使用--bootstrap-server】
    bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test123 --from-beginning
    
    //查看版本
    find ./libs/ -name \*kafka_\* | head -1 | grep -o '\kafka[^\n]*'
        kafka_2.12-2.0.0-javadoc.jar
        如上所示，2.12是Scala 的版本，2.0.0就是kafka的版本
    或
    find ./libs/ -name 'kafka_*.jar.asc' |head -n1 | cut -d'/' -f3
        kafka_2.12-2.0.0-javadoc.jar.asc
        如上所示，2.12是Scala 的版本，2.0.0就是kafka的版本
            
    //查看消费group
    bin\windows\kafka-consumer-groups.bat --bootstrap-server 127.0.0.1:9092 --list
    
    
```

* linux平台运行：
```
    java:
        1.执行命令yum -y list java*查看可安装java版本。
        2.选择一个java版本进行安装，这里我们希望安装java1.8，因为我们的机器是64位的，所以选择安装java-1.8.0-openjdk-devel.x86_64
        这里有个地方要注意，上图中我用红框圈起来的两个java版本，要选择-devel的安装，因为这个安装的是jdk，而那个不带-devel的安装完了其实是jre。
        3.执行命令yum install -y java-1.8.0-openjdk-devel.x86_64
        4.输入java -version查看已安装的jdk版本，当出现如下输出表示安装成功。
        5.yum命令安装java在/usr/lib/jvm目录下
    建一个/usr/bin/java的java的超链接。默认情况下从/usr/bin/java路径使用java，yum安装的时候，这个超链接会自动创建，如果你自己下载包安装的话，这个超链接就需要你手动创建了。
    
    zookeeper:【新版的Kafka自带有zookeeper，下载、安装、配置zookeeper可跳过】
        进入zookeeper官网 http://zookeeper.apache.org/
            1.下载  http://mirror.bit.edu.cn/apache/zookeeper/
            2.解压  tar -zxvf  zookeeper.tar.gz 
            3.进入到conf目录
            4.拷贝zoo_samle.cfg为zoo.cfg
            5.编辑zoo.cfg文件
                tickTime=2000  
                initLimit=10  
                syncLimit=5 
                dataDir=/usr/zookeeper (你的安装路径)
                clientPort=2181 
            6.设置环境变量
                export ZOOKEEPER_HOME=/usr/zookeeper-3.4.12(你的安装路径)
                export PATH=$ZOOKEEPER_HOME/bin:
            
            此时安装成功，进行测试：
            1.进入zookeeper的bin目录，执行sh zkServer.sh start进行启动zookeeper
            2.查看状态   进入bin目录，执行sh zkServer.sh status
            3.停止    进入bin目录，执行sh zkServer.sh stop
    
    kafka:
        进入kafka官网 http://kafka.apache.org/downloads.html
            wget http://mirrors.hust.edu.cn/apache/kafka/2.0.0/kafka_2.11-2.0.0.tg 
            tar -zxvf kafka_2.11-2.0.0.tg 
            mv kafka_2.11-2.0.0 kafka
            rm kafka_2.11-2.0.0.tgz
        
        cd /root/kafka
        
        由于新版的Kafka自带有zookeeper，所以就直接使用了
        bin/zookeeper-server-start.sh config/zookeeper.properties
            //上面代码的意思是：运行脚本从读取zookeeper配置文件中数据以启动zookeeper
        
        nohup bin/kafka-server-start.sh config/server.properties
            //开启kafka
        
        jps
            //检查启动是否成功
        
        bin/kafka-server-stop.sh
        或者
        kill -9 端口号
            //关闭
        
        bin/kafka-console-producer.sh --broker-list 192.168.218.129:9092 --topic test123
            //启动Producer ,并向我们上面创建的名称为test123的Topic中生产消息
        
        bin/kafka-console-consumer.sh --bootstrap-server 192.168.218.129:9092 --from-beginning --topic test123
            //启动Consumer ，并订阅我们上面创建的名称为test123的Topic中生产的消息【旧版使用--zookeeper，新版使用--bootstrap-server】
                --property print.key=true //将消息的key也输出
        
        bin/kafka-topics.sh --list --zookeeper localhost:2181
            //查看创建的所有Topic
        
        bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic test123
            //查看创建的Topic (查看Topic的分区和副本情况)
        
        bin/kafka-topics.sh --zookeeper zk_host:port/chroot --delete --topic my_topic_name
            //删除一个主题
        Kafka目前不支持减少主题的分区数量。
```



### git <div id='git'></div>

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



### Nginx <div id='nginx'></div>
* 描述
    * 是异步框架的 Web服务器，也可以用作反向代理，负载平衡器 和 HTTP缓存。
* `nginx.conf`配置
```
#user  nobody;
worker_processes  1;

error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        # 监听的端口
        listen       80;

        # 被访问域名时触发 这个 server 块配置
        server_name  yun.service.consul;

        # 设置网站根目录
        root   D:\lys\saas;

        # 设置默认读取文件
        index  index.php index.html index.htm;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            # https://www.nginx.com/blog/installing-wordpress-with-nginx-unit/
            # 隐藏index.php
            try_files $uri $uri/ /index.php/$args;
            #try_files $uri $uri/ /index.php/$uri;
        }

        # https://www.nginx.com/resources/wiki/start/topics/examples/phpfcgi/
        # Connecting NGINX to PHP FPM
        # Now we must tell NGINX to proxy requests to PHP FPM via the FCGI protocol:
        location ~ [^/]\.php(/|$) {
        #location ~ \.php$ {
            #fastcgi_split_path_info ^(.+\.php)(/.+)$;

            # https://www.nginx.com/resources/wiki/start/topics/examples/phpfastcgionwindows/
            # 关联 php-cgi 来解析 php 文件
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            #root   html;
        }

    }

}
```
* 问题
    * 解析php文件变为自动下载
        * 解决：nginx.conf 新增 
        ```
        location ~ \.php$ {
            # 网站根目录
            root xxx;
            # 关联 php-cgi 来解析 php 文件
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
        ```
    * 无法解析php文件
        * 解决：NGINX can interface with PHP on Windows via a FastCGI daemon, which ships with PHP: php-cgi.exe. 
        You need to run php-cgi.exe -b 127.0.0.1:<port> and use fastcgi_pass 127.0.0.1:<port>; in the NGINX configuration file. 
        [LINK](https://www.nginx.com/resources/wiki/start/topics/examples/phpfastcgionwindows/)
    * "No input file specified."
        * 解决：因为 $document_root 的参数是由 root xxx 这一行定义的，所以 root xxx 设置不正确或没有设置就报错。



### gitbook <div id='gitbook'></div>

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



### 管理程序 <div id='manager_software'></div>
* 如今每个语言体系中都有一个包管理工具，
    * PHP 的 `composer`，
    * Ruby 的 `gem`，
    * Python 的 `pip`，
    * Java 的 `maven`，
    * Node.js 的 `npm`，



### Ngrok <div id='ngrok'></div>
* [详细描述](https://blog.csdn.net/zhangguo5/article/details/77848658)
```
简单来说内网穿透的目的是：让外网能访问你本地的应用
```

* 【简易操作，使用官方服务器、域名】[ngrok官网](https://ngrok.com/)，[ngrok简易操作](https://dashboard.ngrok.com/get-started)（即下载后运行就可以达到内网穿透效果）
```
简易操作，就是下载ngrok官方的客户端，然后会调用ngrok官方的服务达到穿透效果，步骤：

1、下载官方的客户端（无论windows、linux、mac os x从官网下载的都是压缩包，下载后解压）

2、设置authtoken（authtoken就是在ngrok官网注册登陆后所获得的token，如果不设置的话默认穿透时间限制是8小时）【可以不设置】

3、运行客户端（进入解压后的目录，运行 ./ngrok http 80，这里的80是指你要穿透的内网http协议的服务端口；
也可以指定内网ip和端口，windows例子：ngrok.exe http 172.16.1.2:8080，linux例子：./ngrok http 172.16.1.2:8080）

4、获取官方的穿透网址（会得到http和https协议的 xxx.ngrok.io 类似的ngrok官方域名）进行浏览
```


* 【复杂操作，使用自己搭建的服务器、域名】
[ngrok描述与教程1](https://www.jianshu.com/p/796c3411f8eb)，[ngrok描述与教程2](https://www.5yun.org/16287.html)，
[ngrok详细描述与教程3](https://blog.csdn.net/zhangguo5/article/details/77848658)，
[下载安装go环境](http://www.runoob.com/go/go-environment.html)
```
Ngrok是一个很有名的内网穿透神器，不少人都利用它大展身手，实现将本地电脑让公网来访问；

Ngrok1.7版源码在HTTP映射模式下有内存溢出BUG，目前已经停止更新。ngrok2.0已经作为商业用途，不再开放源码。
    目前网络上很多衍生版本，均是在ngrok1.7的源码基础上做修改的。另外也有很多开发者利用不同的语言开发了ngrok，
    C语言版的Ngrok目前用得最多，尤其是在路由系统里面。

ngrok是golang语言编写，所以需要安装GO语言环境和git环境。



使用自己的服务器、域名（当然的，服务器和域名随便定义）搭建步骤：

第一步：解析自己的域名到自己的公网服务器ip

第二步：服务端搭建——VPS上操作【本作者是Linux环境】
        
        零、下载安装go环境
        
        一、下载ngrok源码
            命令是
                git clone https://github.com/inconshreveable/ngrok.git
        
        二、设置ngrok域名证书（然后替换ngrok源码的证书，域名设置为自己的域名）
            命令是
                #设置环境变量，直接export是临时设置，重启机器后会消失；永久设置是添加到/etc/profile文件（这里保存后要source /etc/profile）
                export NGROK_DOMAIN="abc.yasserli.top"
                #打开ngrok源码目录
                cd xxx/ngrok
                #生成证书
                openssl genrsa -out rootCA.key 2048
                openssl req -x509 -new -nodes -key rootCA.key -subj "/CN=$NGROK_DOMAIN" -days 5000 -out rootCA.pem
                openssl genrsa -out server.key 2048
                openssl req -new -key server.key -subj "/CN=$NGROK_DOMAIN" -out server.csr
                openssl x509 -req -in server.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out server.crt -days 5000
                #替换证书
                cp rootCA.pem assets/client/tls/ngrokroot.crt
                cp server.crt assets/server/tls/snakeoil.crt
                cp server.key assets/server/tls/snakeoil.key
        
        三、生成ngrok服务端程序（使用go命令生成，因为ngrok源码是go编写的，生成的程序默认在ngrok源码目录的bin目录下）
            命令是
                GOOS=linux GOARCH=amd64 make release-server
        
        四、生成ngrok客户端程序（使用go命令生成，因为ngrok源码是go编写的，生成的程序默认在ngrok源码目录的bin目录下）
            命令是
                # 苹果客户端
                GOOS=darwin GOARCH=amd64 make release-client
                # windows客户端
                GOOS=windows GOARCH=amd64 make release-client
                # linux客户端
                GOOS=linux GOARCH=amd64 make release-client
                #成功之后在xxx/ngrok/bin目录下会生成对应的目录
        
        五、调试模式运行ngrok服务端
            命令是 
                #服务域名设置为abc.yasserli.top，http端口为8091，https端口为8092（这些都是客户端将调用服务器资源的设置）
                #如果不想每次启动都这样启动则，
                    #方案一：写脚本启动；
                    #方案二：修改ngrok源码的src/ngrok/server/cli.go文件的parseArgs()里面的配置，然后要重新编译，
                        #即重新生成ngrok服务端程序和客户端程序，然后就可以直接 ./ngrokd 运行了。
                ./ngrokd -domain="abc.yasserli.top" -httpAddr=":8091" -httpsAddr=":8092"

第三步：客户端运行——在内网环境操作（你也可以在外网，不过外网都能直接浏览了，所以在外网环境使用穿透没什么意义）【本作者是windows环境】
        
        零、在D:/ngrok目录（存放ngrok客户端的目录，目录名随便定义）下新建一个ngrok.cfg配置文件
                #目录名、文件名随便定义，内容是下面的就可以，运行时-config根据你自己定义的弄，否则就跟着步骤走
                #写入ngrok.cfg配置文件的内容
                server_addr: "abc.yasserli.top"
                trust_host_root_certs: false
        
        一、启动、运行ngrok客户端（是指服务端ngrok源码编译生成的ngrok客户端程序）
            命令是
                #进入存放客户端的目录运行 windows客户端 ngrok.exe
                #内网穿透出去的使用域名前缀是xxx，并读取ngrok.cfg配置文件来运行，同时写入日志ngrok_log.txt 
                ngrok.exe -log=ngrok_log.txt -subdomain="xxx" -config="ngrok.cfg" 80
```



