## 阿里巴巴集团
### 阿里云 <div id='aliyun'></div>
* 购买`云服务器 ECS`
    ```
    搭建好lnmp后外网无法访问80端口
    解决方式：
    1、关闭防火墙
        Centos7版本或以上默认使用的是firewall作为防火墙，7版本以下默认使用的是iptables作为防火墙。
    2、到 aliyun.com 进入 `云服务器 ECS`->`网络和安全`->`安全组`，然后找到那台服务器->`配置规则`，
        `入方向`添加允许http的规则【核心就是找到你要访问的那台服务器，然后添加安全组规则】。
    ```



