## php
### php构架 <div id='php_framework'></div>
* php从下到上是一个4层体系
    * Zend：
        * 整体用纯c实现，是php的内核部分，它将php代码翻译（词法、语法解析等一系列编译过程）为可执行opcode的处理并实现相应的处理方法、
        实现了基本的数据结构（如hashtable、oo）、内存分配及管理、提供了相应的api方法供外部调用，是一切的核心，所有的外围功能均围绕zend实现。
    * Extensions：
        * 通过组件式的方式提供各种基础服务，我们常见的各种内置函数（如array系列）、标准库等都是通过extension来实现，用户也可以根据需要实现自己的extension以达到功能扩展、性能优化等目的。
    * Sapi：
        * 全称是Server Application Programming Interface，也就是服务端应用编程接口，sapi通过一系列钩子函数，使得php可以和外围交互数据。
    * 上层应用：
        * 编写的php程序，通过不同的sapi方式得到各种各样的应用模式。
* 引擎(Zend)+组件(ext)的模式降低内部耦合，中间层(sapi)隔绝web server和php

### apche <div id='php_apache'></div>
* apache与php
    * Apache对于php的解析，就是通过众多Module中的php Module来完成的。
        * 假定我们安装的版本是Apache2 和 php5，那么需要编辑Apache的主配置文件http.conf，在其中加入下面的几行内容：
        ```
            Unix/Linux环境下：
            
            LoadModule php5_module modules/mod_php5.so
            AddType application/x-httpd-php .php
            
            注：其中modules/mod_php5.so 是X系统环境下mod_php5.so文件的安装位置。
            
            Windows环境下：
            
            LoadModule php5_module d:/php/php5apache2.dll
            AddType application/x-httpd-php .php
            
            注：其中d:/php/php5apache2.dll 是在Windows环境下php5apache2.dll文件的安装位置。
            
            这两项配置就是告诉Apache Server，以后收到的Url用户请求，凡是以php作为后缀，就需要调用php5_module模块（mod_php5.so/ php5apache2.dll）进行处理。
        ```

* lamp架构
    * 从下往上四层：
    ```
        1、liunx 属于操作系统的底层
        2、apache服务器，属于次服务器，沟通linux和PHP
        3、php:属于服务端编程语言，通过php_module 模块 和apache关联
        4、mysql和其他web服务：属于应用服务，通过PHP的Extensions外 挂模块和mysql关联
    ```

### 运行 <div id='php_run'></div>
* php语言由zend编译成机器语言，操作cpu
* 对数据库的操作属于I/O操作，属于机械运动，也就是说一个网站的瓶颈再去对硬盘的读写造成的，解决办法就是减少I/O操作次数，
使用缓冲技术，就是在数据的操作放在mencache/redis里面，达到一定数量级的时候在一次性写入数据库，mencache/redis属于key-value关系
* 而nosql（非关系型数据）也是基于这个理念建设的，也是属于key-value关系
* 频繁读操作，放在缓存(mencache/redis)里面（读多写少，放在nosql里面，读取功能很强大！）


### 路由 <div id='php_router'></div>
* 什么是php的路由机制
```
1、路由机制就是把某一个特定形式的URL结构中提炼出来系统对应的参数。
    举个例子,如：http://main.wopop.com/article/1  其中：/article/1  -> ?_m=article&id=1。 
2、然后将拥有对应参数的URL转换成特定形式的URL结构，是上面的过程的逆向过程。
```

* PHP的URL路由方式 
    * 总体来说就是：获取路径信息->处理路径信息

* URL路由方式： 
    * 第一种是通过url参数进行映射的方式，一般是两个参数，分别代表控制器类和方法比如index.php?c=index&m=index映射到的是index控制器的index方法。
    * 第二种，是通过url-rewrite的方式，这样的好处是可以实现对非php结尾的其他后缀进行映射，
        当然通过rewrite也可以实现第一种方式，不过纯使用rewrite的也比较常见，一般需要配置apache或者nginx的rewrite规则，
        启用.htaccess，需要修改apache/conf/httpd.conf，启用AllowOverride，并可以用AllowOverride限制特定命令的使用。 
        ```
        apache配置文件conf/httpd.conf
        <Directory />
        AllowOverride All
        </Directory>
        ``` 
        ```
        网站的根目录.htaccess文件
        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteRule ^(.*)$ index.php/$1 [L]
        ```  
    * 第三种，就是通过pathinfo的方式，所谓的pathinfo，就是形如这样的url。xxx.com/index.php/c/index/aa/cc，
    apache在处理这个url的时候会把index.php后面的部分输入到环境变量$_SERVER['PATH_INFO']，
    它等于/c/index/aa/cc。然后我们的路由器再通过解析这个串进行分析就可以了，
    后面的部分放入到参数什么地方的，就依据各个框架不同而不同了。
    

### 知识点 <div id='php_work'></div>
* 下载文件
    ```php
    //下载文件方法
    function download_file($file_name)
    {
        header("Content-type:text/html;charset=utf-8");
    
        //对中文文件名进行转码
        $file_name = iconv("UTF-8", "GB2312", $file_name);
    
        //文件绝对路径，如：D:/web/static/xxx.jpg
        $filepath = $_SERVER['DOCUMENT_ROOT'] . $file_name;
    
        if (!file_exists($filepath)) { //检查文件是否存在
            echo "该文件不存在！";
            return;
        }
    
        $file_size = filesize($filepath);  //计算文件大小
    
        //HTTP头部信息
        header("Content-type: application/octet-stream");
        header("Accept-Ranges: bytes");
        header("Accept-Length: " . $file_size);
        header("Content-Disposition: attachment; filename=" . $file_name);
    
    }
    
    //调用
    download_file("xxx.jpg");
    ```

* 允许跨域
    ```php
    浏览器跨域策略起作用，阻止了跨域的请求。
    
    第一次请求后端时候，浏览器意识到是访问一个跨与资源，没有直接发送GET请求获取数据，
    而是发送了一个OPTIONS请求询问是否可以访问该资源。
    我们称之为Preflight请求，默认因为同源策略的存在，
    该请求返回的Header中没有'Access-Control-Allow-Origin'属性，所以访问失败。
    
    如果要实现跨域，关键在于服务器，客户端的代码按照正常的方式编写即可。
    对于服务器，只需要在收到OPTIONS请求的地方，返回的头信息中增加该属性即可，代码如下：
    //允许所有域名发起的跨域请求
    header('Access-Control-Allow-Origin: *');
        //允许指定发起的跨域请求如：www.test.com
        //header("Access-Control-Allow-Origin: http://www.test.com");
    header("Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept");
    header('Access-Control-Allow-Methods: GET, POST, PUT');
    ```

* 随机码
    ```php
    //生成6位包含0~9、a~z、A~Z的随机码
    substr(str_shuffle(implode(array_merge(range(0, 9), range('a', 'z'), range('A', 'Z')))), 0, 6);
    ```







 