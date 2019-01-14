### 简介

SSRF (Server Side Request Forgery) 

### 原理


```
Attacker --【1】req1-payload-->  Server1(存在SSRF漏洞) ---【2】req2--> Server2
     /\                           |        /\                        \/ 
     |                           \/        |                          | 
     '----<----【4】resp1----<----'        '---<---【3】resp2----<-----' 
```

攻击者通过控制Requset1中的参数值，发送Requset1到存在SSRF漏洞的Server1，以Server1为"跳板"发出Requset2,通常根据判断Response2的情况(内容、响应时间等),来实现探测内网主机(IP/port/service...)等利用方式


造成SSRF的前提:服务端提供了与其他服务器交互的功能
造成SSRF的原因:交互时 没有对目标地址做过滤与限制

造成SSRF的常见场景:
* 从指定URL地址获取资源(下载、分享、url跳转)
  * 资源存在形式:图片.png、图标.ico、网页.html、文本.txt

　　

　　
  
  
### 漏洞影响

* 内网
  * 探测内网(IP/port/service...)
    * ip 探测内网存活主机
    * service 如数据库类的服务
  * 攻击内网其他主机
    * web漏洞(SQLi,XSS...)
  * 读取文件
    * 类型1 支持了file协议读取文件
      * 实例 `/click.jsp?url=http://127.0.0.1:8082/config/dbconfig.xml` [21CN某站SSRF(可读取本地数据库配置文件、探测内网)](https://www.secpulse.com/archives/29452.html)
      * 解析
    * 类型2
      * ImageMagick CVE-2016-3718
      * discuz x2.5/x3.0/x3.1/x3.2 SSRF漏洞
      * Ffmpeg文件读取漏洞 CVE-2016-1897  CVE-2016-1898
        * 实例 [新浪微盘存在Ffmpeg文件读取漏洞-SSRF](https://www.secpulse.com/archives/49510.html)
        * 解析 [FFmpeg任意文件读取漏洞分析 - 知乎](https://zhuanlan.zhihu.com/p/28255225)
* 外网
  * 对外发起请求(攻击其他网站等恶意行为)

[SSRF Tips](http://blog.safebuff.com/2016/07/03/SSRF-Tips/) `SSRF PHP function` `SFTP Dict gopher TFTP file ldap`


### 利用方式
  
绕过方式 
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
