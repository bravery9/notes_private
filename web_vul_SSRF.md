### 简介

SSRF (Server Side Request Forgery) 

### 原理

Server1:存在SSRF漏洞的主机
```
Attacker --req1-payload-->  Server1(SSRF) ---req2-->  Server2
     /\                      |    /\                   \/ 
     |                       \/    |                    | 
     '----<----resp1----<----'     '---<---resp2---<----' 
```
攻击者通过控制Requset1中的参数值，发送Requset1到存在SSRF漏洞的Server1，以Server1为"跳板"发出Requset2,通常根据判断Response2的情况(内容、响应时间等),来实现探测内网主机(IP/port/service...)



### 漏洞影响/利用方式

[SSRF Tips](http://blog.safebuff.com/2016/07/03/SSRF-Tips/) `SSRF PHP function` `SFTP Dict gopher TFTP file ldap`


SSRF测试工具/利用工具

|名称|属性|描述|
|:-------------:|--|-----|
|[cujanovic/SSRF-Testing](https://github.com/cujanovic/SSRF-Testing)|python|生成不同形式的SSRF payload (IP obfuscator ...)|
|[D4Vinci/Cuteit(IP obfuscator)](https://github.com/D4Vinci/Cuteit)|python2|IP obfuscator 将恶意IP改为各种格式以绕过安全检测|
|[samhaxr/XXRF-Shots](https://github.com/samhaxr/XXRF-Shots)|node.js|生成payload 发送众多请求 并使用phantomJS实现网页截图|
|[swisskyrepo/SSRFmap](https://github.com/swisskyrepo/SSRFmap)|python3|Automatic SSRF fuzzer and exploitation(RCE...) tool|
|[tarunkant/Gopherus](https://github.com/tarunkant/Gopherus)|python2|生成gopher link 以利用SSRF实现RCE. This tool generates gopher link for exploiting SSRF and gaining RCE in various servers |
