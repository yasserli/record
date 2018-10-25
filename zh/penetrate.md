##内网穿透
### 穿透 <div id='penetrate'></div>
* [详细描述](https://blog.csdn.net/zhangguo5/article/details/77848658)
```
简单来说内网穿透的目的是：让外网能访问你本地的应用
```

### ngrok <div id='ngrok'></div>
* [ngrok描述与教程1](https://www.jianshu.com/p/796c3411f8eb)，[ngrok描述与教程2](https://www.5yun.org/16287.html)，
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

第二步：服务端搭建——VPS上操作
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
        三、生成ngrok服务端程序（使用go命令生成，因为ngrok源码是go编写的，生成的程序默认在xxx/ngrok/bin目录下）
            命令是
                GOOS=linux GOARCH=amd64 make release-server
        四、生成ngrok客户端程序（使用go命令生成，因为ngrok源码是go编写的，生成的程序默认在xxx/ngrok/bin目录下）
            命令是
                # 苹果客户端
                GOOS=darwin GOARCH=amd64 make release-client
                # window客户端
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

第三步：客户端运行——在内网环境操作（你也可以在外网，不过外网都能直接浏览了，所以在外网环境使用穿透没什么意义）
        零、在~/ngrok目录（存放ngrok客户端的目录，目录名随便定义）下新建一个ngrok.cfg配置文件
                #写入ngrok.cfg配置文件的内容
                server_addr: "abc.yasserli.top"
                trust_host_root_certs: false
        一、启动、运行ngrok客户端（是指服务端生成的ngrok客户端程序）
            命令是
                #内网穿透出去的使用域名前缀是xxx，并读取ngrok.cfg配置文件来运行，同时写入日志ngrok_log.txt 
                ngrok.exe -log=ngrok_log.txt -subdomain="xxx" -config="ngrok.cfg" 80
```

* [ngrok官网](https://ngrok.com/)，[ngrok简易操作](https://dashboard.ngrok.com/get-started)（即下载后运行就可以达到内网穿透效果）
```
简易操作，就是下载ngrok官方的客户端，然后会调用ngrok官方的服务达到穿透效果，步骤：

1、下载官方的客户端（无论window、linux、mac os x从官网下载的都是压缩包，下载后解压）

2、设置authtoken（authtoken就是在ngrok官网注册登陆后所获得的token，如果不设置的话默认穿透时间限制是8小时）【可以不设置】

3、运行客户端（进入解压后的目录，运行 ./ngrok http 80，这里的80是指你要穿透的内网http协议的服务端口；
也可以指定内网ip和端口，win例子：ngrok http 172.16.1.2:8080，linux例子：./ngrok http 172.16.1.2:8080）

4、获取官方的穿透网址（会得到http和https协议的 xxx.ngrok.io 类似的ngrok官方域名）
```














