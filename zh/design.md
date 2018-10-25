## 设计、架构
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
            1、设置错误处理：set_error_handler('_error_handler')。处理函数原型：function _error_handler($severity, $message, $filepath, $line)。程序本身原因或手工触发trigger_error("A custom error has been triggered");
            2、设置异常处理：set_exception_handler('_exception_handler')。处理函数原型：function _exception_handler($exception)。当用户抛出异常时触发throw new Exception('Exception occurred');
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
        正常没有写cache_override这个构子方法，所以会去执行$OUT->_display_cache($CFG, $URI)。如果缓存命中则输出，并结束整个CI的单次生命周期。如果没有命中缓存，或没有启用缓存，那么将继续向下执行。
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
