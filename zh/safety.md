##安全
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
    
### 跨域
* <a href='http.md#cross_domain'>http跨域</a>