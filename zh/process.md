## process
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

