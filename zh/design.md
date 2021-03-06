## 设计、架构、思想
### 单例模式（构造 construct） <div id='single_case'></div>
    * 结构：
        * $_instance必须声明为静态的私有变量
        * __construct()构造函数必须声明为私有，防止外部程序new类从而失去单例模式的意义
        * __clone()函数声明为私有，阻止用户复制对象实例
        * getInstance()方法必须设置为公有的，必须调用此方法以返回实例的一个引用
        * ::操作符只能访问静态变量和静态函数
        * new对象都会消耗内存
    * 场景：
        * 最常用的地方是数据库连接。
        * 使用单例模式生成一个对象后，该对象可以被其它众多对象所使用。



### 工厂模式（抽象 abstract extends） <div id='factory_case'></div>
    * 结构：
        * 抽象基类：类中定义抽象一些方法，用以在子类中实现
        * 继承自抽象基类的子类：实现基类中的抽象方法
        * 工厂类：用以实例化所有相对应的子类
    * 场景：
        * 通过采用面向对象的继承特性，我们可以很容易就能对原有程序进行扩展，
        比如：‘乘方’，‘开方’，‘对数’，‘三角函数’，‘统计’等，
        以还可以避免加载没有必要的代码。
        * 如果我们现在需要增加一个求余的类，会非常的简单
            * 我们只需要另外写一个类（该类继承虚拟基类），在类中完成相应的功能（比如：求乘方的运算），
            而且大大的降低了耦合度，方便日后的维护及扩展
    * 让程序根据用户输入的操作符实例化相应的对象，使用一个单独的类来实现实例化的过程，这个类就是工厂



### 观察者模式（接口 interface implements） <div id='observer_case'></div>
    * 结构：
        * 观察者模式属于行为模式，是定义对象间的一种一对多的依赖关系，以便当一个对象的状态发生改变时，
        所有依 赖于它的对象都得到通知并自动刷新。它完美的将观察者对象和被观察者对象分离。可以在独立的
        对象（主体）中维护一个对主体感兴趣的依赖项（观察器）列表。 
    * 场景：
        * 让所有观察器各自实现公共的 Observer 接口，以取消主体和依赖性对象之间的直接依赖关系。

### 策略模式（分离 interface implements） <div id='strategy_case'></div>
    * 结构：
        * 在此模式中，算法是从复杂类提取的，因而可以方便地替换。例如，如果要更改搜索引擎中排列页的方法，
       则策略模式是一个不错的选择。思考一下搜索引擎的几个部分，一部分遍历页面，一部分对每页排列，
       另一部分基于排列的结果排序。在复杂的示例中，这些部分都在同一个类中。通过使用策略模式，
       您可将排列部分放入另一个类中，以便更改页排列的方式，而不影响搜索引擎的其余代码。
   * 场景：
        * 非常适合复杂数据管理系统或数据处理系统，二者在数据筛选、搜索或处理的方式方面需要较高的灵活性。



### Codeigniter <div id='codeigniter'></div>
* CodeIgniter.php
    * 一、设置版本号
    * 二、加载常量
        * 引入ci默认constants.php
        * 引入用户配置constants.php，用户配置constants.php覆盖ci默认constants.php
    * 三、加载全局函数库
        * core/Common.php
    * 四、（判断是否大于等于php5.4版本）如果低于php5.4版本，将进行全局变量安全处理
    * 五、自定义错误、异常和程序完成的函数
        * 知识点：
            ```
            1、设置错误处理：set_error_handler('_error_handler')。处理函数原型：function _error_handler($severity， $message， $filepath， $line)。
            程序本身原因或手工触发trigger_error("A custom error has been triggered");
            2、设置异常处理：set_exception_handler('_exception_handler')。处理函数原型：function _exception_handler($exception)。
            当用户抛出异常时触发throw new Exception('Exception occurred');
            3、千万不要被shutdown迷惑：register_shutdown_function('_shutdown_handler')
            可以这样理解调用条件：当页面被用户强制停止时、当程序代码运行超时时、当php代码执行完成时。
            ```
    * 六、如果index.php有硬编码的话，重新设置子类前缀
    * 七、加载composer
    * 八、基准时间记录
    * 九、加载核心类并实例化：这些都是核心类core里的文件
        ```
        钩子类 $EXT =& load_class('Hooks'， 'core');
        配置类：$CFG =& load_class('Config'， 'core');  如果在index.php有手工设置的配置选项，也加载进来
        utf8类：$UNI =& load_class('Utf8'， 'core');
        URL类：$URI =& load_class('URI'， 'core');
        路由类：$RTR =& load_class('Router'， 'core'， isset($routing) ? $routing : NULL);//$routing变量index.php可设置
               重点说明：Router在实例化时，构造函数调用$this->_set_routing()进行路由设置。实际上在这里生成$RTR时已经完成路由解析。
        OUTPUT类：$OUT =& load_class('Output'， 'core');
        安全类：$SEC =& load_class('Security'， 'core');
        输入及过滤类：$IN    =& load_class('Input'， 'core');
        语言类：$LANG =& load_class('Lang'， 'core');
        ```
    * 十、缓存调用：
        ```
        $EXT->call_hook('cache_override') === FALSE && $OUT->_display_cache($CFG， $URI) === TRUE
        正常没有写cache_override这个构子方法，所以会去执行$OUT->_display_cache($CFG， $URI)。如果缓存命中则输出，并结束整个CI的单次生命周期。
        如果没有命中缓存，或没有启用缓存，那么将继续向下执行。
        $OUT类是一个重要的核心类，负责了整个系统向浏览器终端呈现的内容输出，包括缓存的创建和过期删除
        ```
    * 十一、加载Controller类：
        ```
        require_once BASEPATH.'core/Controller.php';
        function &get_instance();//创建实例化controller函数
        require_once APPPATH.'core/'.$CFG->config['subclass_prefix'].'Controller.php'; //引入自定义扩展controller
        ```
    * 十二、路由判断
        ```
        CI认为下面这几种情况认为是404，如果找不到就调用show_404()函数：
        1) 请求的class不存在：! class_exists($class)
        2) 请求私有方法：!$method[0] === '_'
        3) 请求基类方法：method_exists('CI_Controller'， $method)
        4)请求的方法不存在：! in_array(strtolower($method)， array_map('strtolower'， get_class_methods($class))， TRUE)
        ```
    * 十三、404处理
    * 十四、解析请求的类，并调用请求的方法
    * 十五、输出



### Work <div id='work_design'></div>
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
    * 隐藏程序 `RunHiddenConsole`[download](http://redmine.lighttpd.net/attachments/660/RunHiddenConsole.zip)，特点：隐藏程序
    


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



### 高内聚、低耦合 <div id='module_design'></div>
* [资料](https://blog.csdn.net/kingscoming/article/details/78836229)
    ```
    举个例子：一个程序有50个函数，这个程序执行得非常好；
    然而一旦你修改其中一个函数，其他49个函数都需要做修改，这就是高耦合的后果。
    一旦理解了它，你编写概要设计的时候设计类或者模块自然会考虑到“高内聚，低耦合”。
    ```

* 通常程序结构中各模块的内聚程度越高，模块间的耦合程度就越低，目的：使得模块的“可重用性”、“移植性”大大增强。
 将软件系统划分模块时，尽量做到高内聚低耦合，提高模块的独立性，为设计高质量的软件结构奠定基础。
    ```
    高内聚低耦合，是软件工程中的概念，是判断设计好坏的标准，主要是面向对象的设计，主要是看类的内聚性是否高，耦合度是否低。
    
    模块独立性指每个模块只完成系统要求的独立子功能，并且与其他模块的联系最少且接口简单，两个定性的度量标准――耦合性和内聚性。
    
    耦合性：也称块间联系。指软件系统结构中各模块间相互联系紧密程度的一种度量。模块之间联系越紧密，其耦合性就越强，模块的独立性则越差。
    模块间耦合高低取决于模块间接口的复杂性、调用的方式及传递的信息。
    
    内聚性：又称块内联系。指模块的功能强度的度量，即一个模块内部各个元素彼此结合的紧密程度的度量。
    若一个模块内各元素（语名之间、程序段之间）联系的越紧密，则它的内聚性就越高。
    
    模块粒度： 
    『函数』 
        高内聚：尽可能类的每个成员方法只完成一件事（最大限度的聚合） 
        低耦合：减少类内部，一个成员方法调用另一个成员方法 
    『类』 
        高内聚低耦合：减少类内部，对其他类的调用 
    『功能块』 
        高内聚低耦合：减少模块之间的交互复杂度（接口数量，参数数据）
        
    【多聚合、少继承】 
    聚合：事物A由若干个事物B组成，体现在类与类之间的关系就是：“类B的实例”作为“类A”的“成员对象”出现。 
    继承：顾名思义，体现在类与类之间的关系就是：“类B”被类A所继承 
    显然，当观察类B所具有的行为能力时，“聚合”方式更加清晰。 
    典型应用：java适配器模式中，优选“对象适配器”，而不是“类适配器”
    ```

    * 内聚：每个模块尽可能独立完成自己的功能，不依赖于模块外部的代码。 
        ```
        内聚是从功能角度来度量模块内的联系，一个好的内聚模块应当恰好做一件事；它描述的是模块内的功能联系。
        
        内聚性匪类(低――高): 偶然内聚;逻辑内聚;时间内聚;通信内聚;顺序内聚;功能内聚;
        1 偶然内聚: 指一个模块内的各处理元素之间没有任何联系。
        2 逻辑内聚: 指模块内执行几个逻辑上相似的功能，通过参数确定该模块完成哪一个功能。
        3 时间内聚: 把需要同时执行的动作组合在一起形成的模块为时间内聚模块。
        4 通信内聚: 指模块内所有处理元素都在同一个数据结构上操作（有时称之为信息内聚），或者指各处理使用相同的输入数据或者产生相同的输出数据。
        5 顺序内聚: 指一个模块中各个处理元素都密切相关于同一功能且必须顺序执行，前一功能元素输出就是下一功能元素的输入。
        6 功能内聚: 这是最强的内聚，指模块内所有元素共同完成一个功能，缺一不可。与其他模块的耦合是最弱的。
        ```
    
    * 耦合：模块与模块之间接口的复杂程度，模块之间联系越复杂耦合度越高，牵一发而动全身。
        ```
        耦合是软件结构中各模块之间相互连接的一种度量，耦合强弱取决于模块间接口的复杂程度、进入或访问一个模块的点以及通过接口的数据。
        
        耦合性分类(低――高): 无直接耦合;数据耦合;标记耦合;控制耦合;公共耦合;内容耦合;
        1 无直接耦合:
        2 数据耦合: 指两个模块之间有调用关系，传递的是简单的数据值，相当于高级语言的值传递;
        3 标记耦合: 指两个模块之间传递的是数据结构，如高级语言中的数组名、记录名、文件名等这些名字即标记，其实传递的是这个数据结构的地址;
        4 控制耦合: 指一个模块调用另一个模块时，传递的是控制变量（如开关、标志等），被调模块通过该控制变量的值有选择地执行块内某一功能;
        5 公共耦合: 指通过一个公共数据环境相互作用的那些模块间的耦合。公共耦合的复杂程序随耦合模块的个数增加而增加。
        6 内容耦合: 这是最高程度的耦合，也是最差的耦合。当一个模块直接使用另一个模块的内部数据，或通过非正常入口而转入另一个模块内部。
        ``` 



### 进程、线程、协程 <div id='pid'></div>
* 概括，[资料](https://www.cnblogs.com/lxmhhy/p/6041001.html)
    ```
    一、概念
    
    1、进程
    
    进程是具有一定独立功能的程序关于某个数据集合上的一次运行活动，进程是系统进行资源分配和调度的一个独立单位。
    每个进程都有自己的独立内存空间，不同进程通过进程间通信来通信。由于进程比较重量，占据独立的内存，
    所以上下文进程间的切换开销（栈、寄存器、虚拟内存、文件句柄等）比较大，但相对比较稳定安全。
    
    2、线程
    
    线程是进程的一个实体，是CPU调度和分派的基本单位，它是比进程更小的能独立运行的基本单位。
    线程自己基本上不拥有系统资源，只拥有一点在运行中必不可少的资源(如程序计数器，一组寄存器和栈)，
    但是它可与同属一个进程的其他的线程共享进程所拥有的全部资源。线程间通信主要通过共享内存，上下文切换很快，
    资源开销较少，但相比进程不够稳定容易丢失数据。
    
    3、协程
    
    协程是一种用户态的轻量级线程，协程的调度完全由用户控制。协程拥有自己的寄存器上下文和栈。
    协程调度切换时，将寄存器上下文和栈保存到其他地方，在切回来的时候，恢复先前保存的寄存器上下文和栈，
    直接操作栈则基本没有内核切换的开销，可以不加锁的访问全局变量，所以上下文的切换非常快。
    
    
    二、区别：
    
    1、进程多与线程比较
    
        线程是指进程内的一个执行单元，也是进程内的可调度实体。
        线程与进程的区别:
        1) 地址空间:线程是进程内的一个执行单元，进程内至少有一个线程，它们共享进程的地址空间，而进程有自己独立的地址空间。
        2) 资源拥有:进程是资源分配和拥有的单位，同一个进程内的线程共享进程的资源。
        3) 线程是处理器调度的基本单位，但进程不是。
        4) 二者均可并发执行。
        5) 每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口，但是线程不能够独立执行，
        必须依存在应用程序中，由应用程序提供多个线程执行控制。
    
    2、协程多与线程进行比较
    
        1) 一个线程可以多个协程，一个进程也可以单独拥有多个协程。
        2) 线程进程都是同步机制，而协程则是异步。
        3) 协程能保留上一次调用时的状态，每次过程重入时，就相当于进入上一次调用的状态。
    ```

* 协程，[资料](https://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/0013868328689835ecd883d910145dfa8227b539725e5ed000)
    ```
    协程，又称微线程，纤程。英文名Coroutine。
    
    协程的概念很早就提出来了，但直到最近几年才在某些语言（如Lua）中得到广泛应用。
    
    子程序，或者称为函数，在所有语言中都是层级调用，比如A调用B，B在执行过程中又调用了C，C执行完毕返回，B执行完毕返回，最后是A执行完毕。
    
    所以子程序调用是通过栈实现的，一个线程就是执行一个子程序。
    
    子程序调用总是一个入口，一次返回，调用顺序是明确的。而协程的调用和子程序不同。
    
    协程看上去也是子程序，但执行过程中，在子程序内部可中断，然后转而执行别的子程序，在适当的时候再返回来接着执行。
    
    注意，在一个子程序中中断，去执行其他子程序，不是函数调用，有点类似CPU的中断。
    ```

    * 优势
        ```
        最大的优势就是协程极高的执行效率。因为子程序切换不是线程切换，而是由程序自身控制，因此，没有线程切换的开销，
        和多线程比，线程数量越多，协程的性能优势就越明显。
        
        第二大优势就是不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，
        只需要判断状态就好了，所以执行效率比多线程高很多。
        
        因为协程是一个线程执行，那怎么利用多核CPU呢？最简单的方法是多进程+协程，既充分利用多核，又充分发挥协程的高效率，可获得极高的性能。
        ```



### 客户端、服务器 <div id='c_s'></div>
* 创建TCP连接时，主动发起连接的叫客户端，被动响应连接的叫服务器。
    ```
    举个例子，当我们在浏览器中访问谷歌时，我们自己的计算机就是客户端，浏览器会主动向谷歌的服务器发起连接。
    如果一切顺利，谷歌的服务器接受了我们的连接，一个TCP连接就建立起来的，后面的通信就是发送网页内容了。
    ```
    * 通常意义的`c/s`就是指 客户端/服务器



### Socket <div id='socket'></div>
* Socket是网络编程的一个抽象概念。
* Socket是对TCP/IP协议的封装，它把复杂的TCP/IP协议族隐藏在Socket接口后面，提供一个易用的接口，所以Socket本身并不是协议，而是一个调用接口（API）。
    ```
    套接字之间的连接过程分为三个步骤：
        服务器监听
        客户端请求
        连接确认
    
    Socket的基本操作：
        3.1、socket()函数
        3.2、bind()函数
        3.3、listen()、connect()函数
        3.4、accept()函数
        3.5、read()、write()函数等
        3.6、close()函数
    ```
* [资料](https://zh.wikipedia.org/wiki/%E7%B6%B2%E8%B7%AF%E6%8F%92%E5%BA%A7)
    ```
    在计算器科学中，网上套接字（英语：Network socket），又译网络套接字、网络接口、网上插槽，
    是计算机网上中进程间数据流的端点。使用以网际协议（Internet Protocol）为通信基础的网上套接字，
    称为网际套接字（Internet socket）。因为网际协议的流行，现代绝大多数的网上套接字，都是属于网际套接字。
    
    socket是一种操作系统提供的进程间通信机制。
    
    在操作系统中，通常会为应用程序提供一组应用程序接口（API），称为套接字接口（英语：socket API）。
    应用程序可以通过套接字接口，来使用网上套接字，以进行数据交换。
    ```
    
* Socket与Http，[资料](https://www.jianshu.com/p/5a1bfc528fdc)
    ```
    通常情况下Socket连接就是TCP连接
    不同点：
    1.连接长度
        Socket：长连接,连接一旦建立，通信双方即可开始相互发送数据内容，直到双方连接断开
        HTTP：短连接,连接使用的是“请求—响应”的方式
    
    2.连接响应
        Socket：实际情况中,客户端到服务器之间的通信防火墙默认会关闭长时间处于非活跃状态的连接而导致 Socket 连接断连，
            因此需要通过轮询告诉网络，该连接处于活跃状态
        HTTP：在请求时需要先建立连接，而且需要客户端向服务器发出请求后，服务器端才能回复数据(被动)
    ```



### Sql <div id='sql'></div>
* 主键、外键和索引的区别
    ```
    定义：    
     主键-- 唯一标识一条记录，不能有重复的，不允许为空    
     外键-- 表的外键是另一表的主键, 外键可以有重复的, 可以是空值    
     索引-- 该字段没有重复值，但可以有一个空值
    
    作用：    
     主键-- 用来保证数据完整性    
     外键-- 用来和其他表建立联系用的    
     索引-- 是提高查询排序的速度
    
    个数：    
     主键-- 主键只能有一个    
     外键-- 一个表可以有多个外键    
     索引-- 一个表可以有多个唯一索引
    ```



### 负载均衡 <div id='load_balancing'></div>
* 不能狭义地理解为分配给所有实际服务器一样多的工作量，因为多台服务器的承载能力各不相同，这可能体现在硬件配置、网络带宽的差异，也可能因为某台服务器身兼多职，我们所说的“均衡”，也就是希望所有服务器都不要过载，并且能够最大程序地发挥作用。

* 一、http重定向
    * 当http代理（比如浏览器）向web服务器请求某个URL后，web服务器可以通过http响应头信息中的Location标记来返回一个新的URL。这意味着HTTP代理需要继续请求这个新的URL，完成自动跳转。
    * 性能缺陷：
        * 1、吞吐率限制
            * 主站点服务器的吞吐率平均分配到了被转移的服务器。现假设使用RR（Round Robin）调度策略，子服务器的最大吞吐率为1000reqs/s，
            那么主服务器的吞吐率要达到3000reqs/s才能完全发挥三台子服务器的作用，那么如果有100台子服务器，那么主服务器的吞吐率可想而知得有大？
            相反，如果主服务的最大吞吐率为6000reqs/s，那么平均分配到子服务器的吞吐率为2000reqs/s，而现子服务器的最大吞吐率为1000reqs/s，因此就得增加子服务器的数量，增加到6个才能满足。
        * 2、重定向访问深度不同
            * 有的重定向一个静态页面，有的重定向相比复杂的动态页面，那么实际服务器的负载差异是不可预料的，而主站服务器却一无所知。因此整站使用重定向方法做负载均衡不太好。
        * 我们需要权衡转移请求的开销和处理实际请求的开销，前者相对于后者越小，那么重定向的意义就越大，例如下载。你可以去很多镜像下载网站试下，会发现基本下载都使用了Location做了重定向。

* 二、DNS负载均衡
    * DNS负责提供域名解析服务，当访问某个站点时，实际上首先需要通过该站点域名的DNS服务器来获取域名指向的IP地址，
    在这一过程中，DNS服务器完成了域名到IP地址的映射，同样，这样映射也可以是一对多的，这时候，DNS服务器便充当了负载均衡调度器，
    它就像http重定向转换策略一样，将用户的请求分散到多台服务器上，但是它的实现机制完全不同。
    * 相比http重定向，基于DNS的负载均衡完全节省了所谓的主站点，或者说DNS服务器已经充当了主站点的职能。但不同的是，
    作为调度器，DNS服务器本身的性能几乎不用担心。因为DNS记录可以被用户浏览器或者互联网接入服务商的各级DNS服务器缓存，
    只有当缓存过期后才会重新向域名的DNS服务器请求解析。也说是DNS不存在http的吞吐率限制，理论上可以无限增加实际服务器的数量。
    * 特性：
        * 1、可以根据用户IP来进行智能解析。DNS服务器可以在所有可用的A记录中寻找离用记最近的一台服务器。
        * 2、动态DNS：在每次IP地址变更时，及时更新DNS服务器。当然，因为缓存，一定的延迟不可避免。
    * 不足：
        * 1、没有用户能直接看到DNS解析到了哪一台实际服务器，加服务器运维人员的调试带来了不便。
        * 2、策略的局限性。例如你无法将HTTP请求的上下文引入到调度策略中，而在前面介绍的基于HTTP重定向的负载均衡系统中，调度器工作在HTTP层面，
        它可以充分理解HTTP请求后根据站点的应用逻辑来设计调度策略，比如根据请求不同的URL来进行合理的过滤和转移。
        * 3、如果要根据实际服务器的实时负载差异来调整调度策略，这需要DNS服务器在每次解析操作时分析各服务器的健康状态，对于DNS服务器来说，这种自定义开发存在较高的门槛，更何况大多数站点只是使用第三方DNS服务。
        * 4、DNS记录缓存，各级节点的DNS服务器不同程序的缓存会让你晕头转向。
        * 5、基于以上几点，DNS服务器并不能很好地完成工作量均衡分配，最后，是否选择基于DNS的负载均衡方式完全取决于你的需要。

* 三、反向代理负载均衡
    * 这个肯定大家都有所接触，因为几乎所有主流的Web服务器都热衷于支持基于反向代理的负载均衡。它的核心工作就是转发HTTP请求。
    * 相比前面的HTTP重定向和DNS解析，反向代理的调度器扮演的是用户和实际服务器中间人的角色：
        * 1、任何对于实际服务器的HTTP请求都必须经过调度器
        * 2、调度器必须等待实际服务器的HTTP响应，并将它反馈给用户（前两种方式不需要经过调度反馈，是实际服务器直接发送给用户）
    * 特性：
        * 1、调度策略丰富。例如可以为不同的实际服务器设置不同的权重，以达到能者多劳的效果。
        * 2、对反向代理服务器的并发处理能力要求高，因为它工作在HTTP层面。
        * 3、反向代理服务器进行转发操作本身是需要一定开销的，比如创建线程、与后端服务器建立TCP连接、接收后端服务器返回的处理结果、
        分析HTTP头部信息、用户空间和内核空间的频繁切换等，虽然这部分时间并不长，但是当后端服务器处理请求的时间非常短时，转发的开销就显得尤为突出。例如请求静态文件，更适合使用前面介绍的基于DNS的负载均衡方式。
        * 4、反向代理服务器可以监控后端服务器，比如系统负载、响应时间、是否可用、TCP连接数、流量等，从而根据这些数据调整负载均衡的策略。
        * 5、反射代理服务器可以让用户在一次会话周期内的所有请求始终转发到一台特定的后端服务器上（粘滞会话），这样的好处一是保持session的本地访问，二是防止后端服务器的动态内存缓存的资源浪费。

* 四、IP负载均衡(LVS-NAT)
    * 因为反向代理服务器工作在HTTP层，其本身的开销就已经严重制约了可扩展性，从而也限制了它的性能极限。那能否在HTTP层面以下实现负载均衡呢？
    * NAT服务器:它工作在传输层，它可以修改发送来的IP数据包，将数据包的目标地址修改为实际服务器地址。

* 五、直接路由(LVS-DR)
    * NAT是工作在网络分层模型的传输层（第四层），而直接路由是工作在数据链路层（第二层）。它通过修改数据包的目标MAC地址（没有修改目标IP），将数据包转发到实际服务器上，不同的是，实际服务器的响应数据包将直接发送给客户羰，而不经过调度器。

* 六、IP隧道(LVS-TUN)
    * 基于IP隧道的请求转发机制：将调度器收到的IP数据包封装在一个新的IP数据包中，转交给实际服务器，然后实际服务器的响应数据包可以直接到达用户端。目前Linux大多支持，可以用LVS来实现，称为LVS-TUN，
    与LVS-DR不同的是，实际服务器可以和调度器不在同一个WAN网段，调度器通过IP隧道技术来转发请求到实际服务器，所以实际服务器也必须拥有合法的IP地址。
    * 总体来说，LVS-DR和LVS-TUN都适合响应和请求不对称的Web服务器，如何从它们中做出选择，取决于你的网络部署需要，因为LVS-TUN可以将实际服务器根据需要部署在不同的地域，并且根据就近访问的原则来转移请求，所以有类似这种需求的，就应该选择LVS-TUN。



### SQL Injection <div id='sql_injection'></div>
* SQL注入（SQL Injection）：就是攻击者把SQL命令插入到Web表单的输入域或页面请求的查询字符串等方式，最终使得欺骗服务器执行恶意的SQL命令。
    * 假设：
        * http://system.com?select_user/id=123
        * 这条url命令是提交参数并执行查询数据库操作
        * select * from user where id=123
    * 则sql注入只需要修改查询参数以达到恶意操作数据库目的
        * http://system.com?select_user/id=123'or'1'=1'
        * select * from user where id=123 or 1=1
    * 后果是由于sql注入判断条件达到恒真，使user表数据被恶意全部暴露窃取



### [RESTful API](https://en.wikipedia.org/wiki/Representational_state_transfer) <div id='restful_api'></div>
```
REST全称是Representational State Transfer，中文意思是表述（编者注：通常译为表征）性状态转移。 
它首次出现在2000年Roy Fielding的博士论文中，Roy Fielding是HTTP规范的主要编写者之一。 他在论
文中提到："我这篇文章的写作目的，就是想在符合架构原理的前提下，理解和评估以网络为基础的应用
软件的架构设计，得到一个功能强、性能好、适宜通信的架构。REST指的是一组架构约束条件和原则。" 
如果一个架构符合REST的约束条件和原则，我们就称它为RESTful架构。

REST本身并没有创造新的技术、组件或服务，而隐藏在RESTful背后的理念就是使用Web的现有特征和能力，
更好地使用现有Web标准中的一些准则和约束。虽然REST本身受Web技术的影响很深， 但是理论上REST架
构风格并不是绑定在HTTP上，只不过目前HTTP是唯一与REST相关的实例。 所以我们这里描述的REST也是
通过HTTP实现的REST。
```



### [SDK](https://en.wikipedia.org/wiki/Software_development_kit) <div id='sdk'></div>
```
软件开发工具包（Software Development Kit, SDK）一般是一些被软件工程师用于为特定的软件包、软件框架、
硬件平台、操作系统等创建应用软件的开发工具的集合。

它或许只是简单的为某个编程语言提供应用程序接口的一些文件，但也可能包括能与某种嵌入式系统通讯的复杂
的硬件。一般的工具包括用于调试和其他用途的实用工具。SDK还经常包括示例代码、支持性的技术注解或者其
他的为基本参考资料澄清疑点的支持文档。
```



### [GNU](https://en.wikipedia.org/wiki/GNU) <div id='gnu'></div>
```
GNU通用公共许可协议（英语：GNU General Public License，简称 GNU GPL、GPL）是广泛使用的免费软件许可
证，可以保证终端用户得自由运行，学习，共享和修改软件。[6]许可证最初由GNU项目的自由软件基金会 （FSF）
的理查德·斯托曼（Richard Matthew Stallman）撰写，并授予计算机程序的收件人自由软件定义的权利。GPL是
一个Copyleft许可证，这意味着派生作品只能以相同的许可条款分发。
```

### [Copyleft](https://en.wikipedia.org/wiki/Copyleft) <div id='copyright'></div>
```
Copyleft是一由自由软件运动所发展的概念，是一种利用现有著作权体制来挑战该体制的许可方式，在自由软件
许可协议方式中增加copyleft条款之后，该自由软件除了允许用户自由使用、散布、改作之外，copyleft条款更
要求用户改作后的派生作品必须要以同等的许可方式发布以反馈社群。

“版权”（Copyright）的概念是借由赋予对著作的专有权利的方式提供作者从事创作之经济动机，但相对的此种
赋予作者专有权利的方式同时也限制了他人任意使用创作物的自由。

Copyleft作品是有版权的，但它们加入了法律上的分发条款，保障任何人都拥有对该作品及其派生品的使用、修
改和重新发布的权力，惟前提是这些发布条款不能被改变。
```



### [依赖注入](https://zh.wikipedia.org/wiki/%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5) <div id='dependency_injection'></div>
```
在软件工程中，依赖注入是种实现控制反转用于解决依赖性设计模式。一个依赖关系指的是可被利用的一种对象
（即服务提供端） 。依赖注入是将所依赖的传递给将使用的从属对象（即客户端）。该服务是将会变成客户端的
状态的一部分。 传递服务给客户端，而非允许客户端来建立或寻找服务，是本设计模式的基本要求。
```



