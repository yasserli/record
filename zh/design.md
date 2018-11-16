## 设计、架构、思想
### 单例模式（构造 construct） <div id='single_case'></div>
    * 结构：
        * $_instance必须声明为静态的私有变量
        * __construct()构造函数必须声明为私有，防止外部程序new类从而失去单例模式的意义
        * __clone()函数声明为私有，阻止用户复制对象实例
        * getInstance()方法必须设置为公有的,必须调用此方法以返回实例的一个引用
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
            1、设置错误处理：set_error_handler('_error_handler')。处理函数原型：function _error_handler($severity, $message, $filepath, $line)。
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
        钩子类 $EXT =& load_class('Hooks', 'core');
        配置类：$CFG =& load_class('Config', 'core');  如果在index.php有手工设置的配置选项，也加载进来
        utf8类：$UNI =& load_class('Utf8', 'core');
        URL类：$URI =& load_class('URI', 'core');
        路由类：$RTR =& load_class('Router', 'core', isset($routing) ? $routing : NULL);//$routing变量index.php可设置
               重点说明：Router在实例化时，构造函数调用$this->_set_routing()进行路由设置。实际上在这里生成$RTR时已经完成路由解析。
        OUTPUT类：$OUT =& load_class('Output', 'core');
        安全类：$SEC =& load_class('Security', 'core');
        输入及过滤类：$IN    =& load_class('Input', 'core');
        语言类：$LANG =& load_class('Lang', 'core');
        ```
    * 十、缓存调用：
        ```
        $EXT->call_hook('cache_override') === FALSE && $OUT->_display_cache($CFG, $URI) === TRUE
        正常没有写cache_override这个构子方法，所以会去执行$OUT->_display_cache($CFG, $URI)。如果缓存命中则输出，并结束整个CI的单次生命周期。
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
        3) 请求基类方法：method_exists('CI_Controller', $method)
        4)请求的方法不存在：! in_array(strtolower($method), array_map('strtolower', get_class_methods($class)), TRUE)
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



