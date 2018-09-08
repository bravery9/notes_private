# Content

* [MyNotes](#MyNotes)
* [Security](#security)
* [Development](#development)

---

## MyNotes

|我的项目|属性|描述|
|:-------------:|--|-----|
|[EquationExploit](https://github.com/1135/EquationExploit)|Java C++|在Windows下针对网段批量利用永恒之蓝漏洞(MS-17010 EternalBlue) |
|[Applescript_demos](https://github.com/1135/Applescript_demos)|applescript|苹果脚本示例，在Mac下使用苹果脚本实现发送iMessage短信|
|[VulSpider](https://github.com/1135/VulSpider)|python2|后台持续运行，获取最新漏洞及每日简报，发送邮件给安全人员|

|我的笔记|属性|描述|
|:-------------:|--|-----|
|[z_net_wireshark.md](z_net_wireshark.md)|net|wireshark使用总结|
|[z_command_Mac_软件_快捷键_命令_技巧.md](z_command_Mac_软件_快捷键_命令_技巧.md)|Mac|MacOS下的好用软件、快捷键、命令、技巧|
|[z_history_RAT.md](z_history_RAT.md)|RAT|2008年当时的远控介绍|
|[linux_proxy_Ubuntu_CentOS_socks5_Server.md](linux_proxy_Ubuntu_CentOS_socks5_Server.md)|Proxy|在Ubuntu或CentOS下一键安装Socks5代理服务器|


## Security

#### 全能框架

|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[metasploit-framework](http://github.com/rapid7/metasploit-framework)|Ruby #RAT|Linux/Mac OS/windows|必备的功能强大的渗透测试框架|

##### windows后渗透/二进制

|名称|属性tags|运行环境|描述|
|:-------------:|--|--|-----|
|[Empire](https://github.com/EmpireProject/Empire)|python|Kali/Debian/Ubuntu|#域渗透 #RAT 域渗透利器Empire is a post-exploitation framework|
|[QuasarRAT](https://github.com/quasar/QuasarRAT)|C#|windows|#RAT 传统远控 Remote Administration Tool for Windows|
|[sRDI](https://github.com/monoxgas/sRDI)|powershell|windows|Shellcode implementation of Reflective DLL Injection. Convert DLLs to position independent shellcode|

#### 爬虫

|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[chromeless](https://github.com/prisma/chromeless)|js|/|动态爬虫 Chrome自动化|
|[Anti-Anti-Spider](https://github.com/luyishisi/Anti-Anti-Spider)|python|/|反反爬虫|
|[httplib2](https://github.com/httplib2/httplib2)|python|/|社区维护的HTTP client library 特性包括:所有Methods 授权Authentication 持久连接Keep-Alive 缓存Caching  压缩Compression(gzip/deflate)|

#### github泄露扫描
|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[x-patrol](https://github.com/MiSecurity/x-patrol/)|golang|all|MiSecurity|

#### Phishing钓鱼

|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[gophish](https://github.com/gophish/gophish)|golang|all|2k★ Open-Source Phishing Toolkit. 用于对企业进行定期的钓鱼测试. 启动后先在`Sending Profiles`中配置真实可用的`mail server`,发起一个Campaign:钓鱼邮件(内含钓鱼网站) [使用视频](https://www.youtube.com/watch?v=knc6Iq-hNcw)|
|[blackeye](https://github.com/flagellantX/blackeye)|.sh .php|/| 钓鱼页面Phishing Pages 含各大网站`Facebook,Google,Twitter,Microsoft`等|

#### 免杀

FUD:Fully undetectable

|名称|属性|描述|
|:-------------:|--|-----|
|[ASWCrypter](https://github.com/AbedAlqaderSwedan1/ASWCrypter)|shell+python|An Bash&Python Script For Generating Payloads that Bypasses All Antivirus so far FUD.实测无法过360ZhuDongFangyu|

#### Proxy代理

|名称|属性|描述|
|:-------------:|--|-----|
|[frp](https://github.com/fatedier/frp)|golang|14k★ 高性能的反向代理(reverse proxy). 可用于内网穿透,支持协议:tcp, udp, http, https.[中文文档](https://github.com/fatedier/frp/blob/master/README_zh.md)|

#### website安全网站

|名称|属性|描述|
|:-------------:|--|-----|
|[exploit-db](https://www.exploit-db.com/)|exp引擎|Offensive Security’s Exploit Database Archive.|
|-|exp引擎|[Remote Exploits](https://www.exploit-db.com/remote/)|
|-|exp引擎|[Web Application Exploits](https://www.exploit-db.com/webapps/)|
|-|exp引擎|[Local & Privilege Escalation Exploits](https://www.exploit-db.com/local/)|
|-|exp引擎|[Denial of Service & Proof of Concept Exploits](https://www.exploit-db.com/dos/)|
|-|/|[Exploit Shellcode Archive](https://www.exploit-db.com/shellcode/)|
|-|Papers|[Archived Security Papers](https://www.exploit-db.com/papers/)|
|[OWASP](https://www.owasp.org/index.php/Main_Page)|知识库|自由开放的软件安全社区|
|-|知识库|#反序列化安全# [Deserialization Cheat Sheet](https://www.owasp.org/index.php/Deserialization_Cheat_Sheet)|
|[360威胁分析平台](https://ti.360.net/)|威胁分析引擎|威胁情报 ip domain file url email|
|[微步在线威胁情报](https://x.threatbook.cn/nodev4/vb4/list)|威胁分析引擎|威胁情报 ip domain file url email|
|[Shodan.io](https://www.shodan.io/)|网络空间引擎|Shodan is the world's first search engine for Internet-connected devices.|
|[Fofa.so](https://fofa.so/)|网络空间引擎|白帽汇 [规则列表](https://fofa.so/library)|
|[Zoomeye](https://www.zoomeye.org/)|网络空间引擎|知道创宇|
|[www.seebug.org](https://www.seebug.org/)|/|知道创宇|
|[paper.seebug.org](https://paper.seebug.org/)|Papers|知道创宇|
|[dnsdumpster.com](https://dnsdumpster.com/) |/|FREE domain research tool that can discover hosts related to a domain.|



---

## Development

#### 必备网站

|名称|属性|描述|
|:-------------:|--|-----|
|[regex101.com](https://regex101.com/)|regex|在线调试正则 支持php/javascript/python/golang. 代码生成器:直接生成多种语言代码java/golang等|
|[graphemica.com/](http://graphemica.com/)|characters|同一字符的各种编码情况下的表示 各种格式|


#### web应用开发-基础框架

|名称|属性|描述|
|:-------------:|--|-----|
|[Spring Framework](https://github.com/spring-projects/spring-framework)|Java|23k★ create enterprise applications in a wide range of scenarios and architectures. |
|[Spring Boot](https://github.com/spring-projects/spring-boot)|Java|28k★ Spring Boot makes it easy to create Spring-powered, production-grade applications and services with absolute minimum fuss. |
|[ThinkPHP5 Framework](https://github.com/top-think/framework)|php|1k★ 中文web应用开发框架  |
|[Vue.js](https://github.com/vuejs/vue)|js前端框架|112k★ A progressive, incrementally-adoptable JavaScript framework for building UI on the web.|


#### web前端

|名称|属性|描述|
|:-------------:|--|-----|
|[Quill Rich Text Editor](https://github.com/quilljs/quill)|富文本编辑器|19k★ 现代编辑器 专为兼容性和可扩展性而构建 安全好用  |
|[PDF.js](https://mozilla.github.io/pdf.js/)|pdf浏览|23k★ https://mozilla.github.io/pdf.js/|
|[clipboard.js](https://clipboardjs.com)|clipboard操作|23k★ Modern copy to clipboard. |

#### web系统

|名称|属性|描述|
|:-------------:|--|-----|
|[filebrowser](https://github.com/filebrowser/filebrowser)|云盘系统 Golang|3k★ A stylish web-based file manager|
|[nextcloud](https://github.com/nextcloud/server)|云盘系统 PHP|5k★ a safe home for all your data. 多客户端：web、ios、android |

#### 学习项目
|名称|属性|描述|
|:-------------:|--|-----|
|[Linux kernel map](http://www.makelinux.net/kernel_map/)|linux|linux内核学习 交互式操作 美国网站|
|[Annotated Nginx Source（中文）](https://github.com/chronolaw/annotated_nginx)|C web中间件|100★ 源代码学习 《Nginx完全开发指南》的作者 |

#### 学习资源
|名称|属性|描述|
|:-------------:|--|-----|
|[Java-Guide](https://github.com/Snailclimb/Java-Guide)|非代码|Java学习指南 一份涵盖大部分Java程序员所需要掌握的核心知识|


#### 其他
|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[MailHog](https://github.com/mailhog/MailHog)|golang|all|3k★ 仅供SMTP测试使用（基于Web和API） Mac下安装:`brew install mailhog`|

