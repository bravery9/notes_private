# Content

_________________________________/

- [MyProject](#myproject)我的项目
- [MyNotes](#mynotes)我的笔记
- [Security](#security)安全
    + [全能框架](#全能框架)
    + [windows后渗透](#windows后渗透)
    + [二进制](#二进制)
    + [Proxy代理](#proxy代理)
    + [扫描自动化](#扫描自动化)
    + [爬虫](#爬虫)
    + [github泄露扫描](#github泄露扫描)
    + [Phishing钓鱼](#phishing钓鱼)
    + [免杀](#免杀)
    + [CMS](#cms)
    + [website_技术知识](#website_技术知识)
    + [website_exp引擎](#website_exp引擎)
    + [website_网络空间引擎](#website_网络空间引擎)
    + [website_威胁分析引擎](website_威胁分析引擎)
    + [website_安全工具online](website_安全工具online)
- [Development](#development)开发
    + [必备网站](#必备网站)
    + [web应用开发-基础框架](#web应用开发-基础框架)
    + [web前端](#web前端)
    + [web系统](#web系统)
    + [学习项目](#学习项目)
    + [学习资源](#学习资源)
    + [其他](#其他)
    
_________________________________/

## MyProject
|项目名称|属性|描述|
|:-------------:|--|-----|
|[EquationExploit](https://github.com/1135/EquationExploit)|Java C++|在Windows下针对网段批量利用永恒之蓝漏洞(MS-17010 EternalBlue) |
|[Applescript_demos](https://github.com/1135/Applescript_demos)|applescript|苹果脚本示例，在Mac下使用苹果脚本实现发送iMessage短信|
|[VulSpider](https://github.com/1135/VulSpider)|python2|后台持续运行，获取最新漏洞及每日简报，发送邮件给安全人员|

## MyNotes

|笔记标题|属性|描述|
|:-------------:|--|-----|
|[wireshark使用总结](z_net_wireshark.md)|net|z_net_wireshark.md|
|[MacOS下的好用软件、快捷键、命令、技巧](z_command_Mac_软件_快捷键_命令_技巧.md)|Mac|z_command_Mac_软件_快捷键_命令_技巧.md|
|[2008年的远控软件介绍](z_history_RAT.md)|RAT|z_history_RAT.md|
|[Ubuntu/CentOS一键安装Socks5代理服务器](linux_proxy_Ubuntu_CentOS_socks5_Server.md)|Proxy|linux_proxy_Ubuntu_CentOS_socks5_Server.md|
|[web中间件修复方案 SSL slowhttpdos Tomcat](web___fix_中间件漏洞修复方案_SSL_slowhttpdos.md)|web|web___fix_中间件漏洞修复方案_SSL_slowhttpdos.md|
|[虚拟币挖矿的多种方式](z_blockchain_mining.md)|blockchain|z_blockchain_mining.md|

---

## Security

#### 全能框架

|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[metasploit-framework](http://github.com/rapid7/metasploit-framework)|Ruby #RAT|Linux/Mac OS/windows|必备的功能强大的渗透测试框架|

#### windows后渗透

|名称|属性tags|运行环境|描述|
|:-------------:|--|--|-----|
|[Empire](https://github.com/EmpireProject/Empire)|python|Kali/Debian/Ubuntu|#域渗透 #RAT 域渗透利器Empire is a post-exploitation framework|
|[QuasarRAT](https://github.com/quasar/QuasarRAT)|C#|windows|#RAT 传统远控 Remote Administration Tool for Windows|
|[sRDI](https://github.com/monoxgas/sRDI)|powershell|windows|Shellcode implementation of Reflective DLL Injection. Convert DLLs to position independent shellcode|

#### 二进制

|名称|属性|描述|
|:-------------:|--|-----|
|[gdbgui](https://github.com/cs01/gdbgui)|/|6k★ 基于浏览器可视化gdb调试 [下载安装ubuntu14.04+](https://gdbgui.com/downloads.html)|
|[peda](https://github.com/longld/peda)|python|6k★ Python Exploit Development Assistance for GDB|
|[pwntools](https://github.com/Gallopsled/pwntools)|python|4k★ CTF framework and exploit development library|



#### Proxy代理

|名称|属性|描述|
|:-------------:|--|-----|
|[frp](https://github.com/fatedier/frp)|golang|14k★ 高性能的反向代理(reverse proxy). 可用于内网穿透,支持协议:tcp, udp, http, https.[中文文档](https://github.com/fatedier/frp/blob/master/README_zh.md)|
|[brook](https://github.com/txthinking/brook)|golang|6k★ 【Server端】(Linux/MacOS/Windows/Android/iOS)开启VPN/Socks5/Shadowsocks 【Client端】可连接，以及Socks5 to HTTP|

#### 扫描自动化
|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[Yuki-Chan-The-Auto-Pentest](https://github.com/Yukinoshita47/Yuki-Chan-The-Auto-Pentest)|python+shell|kali_linux|Automate Pentest Tool|
|[Sn1per](https://github.com/1N3/Sn1per)|python+shell|kali_linux docker|Automated Pentest Recon Scanner.  kali`./install.sh`|


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

#### mail
|名称|描述|
|:-------------:|--|
|https://emkei.cz/|在线发送钓鱼邮件 可伪造邮件发送者 (如伪造xx@163.com发送到qq邮箱)|
|http://www.yopmail.com/zh/|临时邮箱 可随意进入xxx@yopmail.com收发邮件|
|https://10minutemail.net/|临时邮箱 十分钟邮箱 短期 每次邮箱地址不同 适用于注册小号|
|https://protonmail.com/|匿名邮箱 瑞士的匿名邮箱 长期 google验证码认证后可注册|



#### 免杀

FUD:Fully undetectable

|名称|属性|描述|
|:-------------:|--|-----|
|[ASWCrypter](https://github.com/AbedAlqaderSwedan1/ASWCrypter)|shell+python|An Bash&Python Script For Generating Payloads that Bypasses All Antivirus so far FUD.实测无法过360ZhuDongFangyu|


#### CMS
|名称|属性|描述|
|:-------------:|--|-----|
|[WordPress](https://github.com/WordPress/WordPress)|php|11k★ 31% of the web uses [WordPress](https://wordpress.org/), from hobby blogs to the biggest news sites online.|
|[joomla](https://github.com/joomla/joomla-cms)|php|3k★ Joomla! Content Management System.|


#### website_技术知识

|名称|属性|描述|
|:-------------:|--|-----|
|[OWASP](https://www.owasp.org/index.php/Main_Page)|知识库|自由开放的软件安全社区|
|-|知识库|OWASP web测试指南V4 双语 [owasp-testing-guide-v4](/https://kennel209.gitbooks.io/owasp-testing-guide-v4/content/)|
|-|知识库|#反序列化安全# [Deserialization Cheat Sheet](https://www.owasp.org/index.php/Deserialization_Cheat_Sheet)|
|[www.seebug.org](https://www.seebug.org/)|/|知道创宇|
|[paper.seebug.org](https://paper.seebug.org/)|Papers|知道创宇|


#### website_exp引擎

|名称|属性|描述|
|:-------------:|--|-----|
|[exploit-db](https://www.exploit-db.com/)|exp引擎|Offensive Security’s Exploit Database Archive.|
|-|exp引擎|[Remote Exploits](https://www.exploit-db.com/remote/)|
|-|exp引擎|[Web Application Exploits](https://www.exploit-db.com/webapps/)|
|-|exp引擎|[Local & Privilege Escalation Exploits](https://www.exploit-db.com/local/)|
|-|exp引擎|[Denial of Service & Proof of Concept Exploits](https://www.exploit-db.com/dos/)|
|-|/|[Exploit Shellcode Archive](https://www.exploit-db.com/shellcode/)|
|-|Papers|[Archived Security Papers](https://www.exploit-db.com/papers/)|
|[sploitus.com](https://sploitus.com/)|exp引擎|Exploits.|


#### website_网络空间引擎

|名称|属性|描述|
|:-------------:|--|-----|
|[Shodan.io](https://www.shodan.io/)|网络空间引擎|Shodan is the world's first search engine for Internet-connected devices.|
|[Fofa.so](https://fofa.so/)|网络空间引擎|白帽汇 [规则列表](https://fofa.so/library)|
|[Zoomeye](https://www.zoomeye.org/)|网络空间引擎|知道创宇|

#### website_威胁分析引擎

|名称|属性|描述|
|:-------------:|--|-----|
|[360威胁分析平台](https://ti.360.net/)|威胁分析引擎|威胁情报 ip domain file url email|
|[微步在线威胁情报](https://x.threatbook.cn/nodev4/vb4/list)|威胁分析引擎|威胁情报 ip domain file url email|

#### website_安全工具online

|名称|描述|
|:-------------:|-----|
|[findsubdomains.com](https://findsubdomains.com/) |domain信息收集 子域名查找 Find subdomains online.|
|[dnsdumpster.com](https://dnsdumpster.com/) |domain信息收集 FREE domain research tool that can discover hosts related to a domain.|
|[wappalyzer.com](https://www.wappalyzer.com/) |信息收集 web技术栈 web frameworks, server software, analytics tools and many more. |
|[onlinehashcrack.com](https://www.onlinehashcrack.com/) |crack在线破解 Hashes(NTLM/wordpress/MySQL). Wifi WPA(2). MS Office.|

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
|[gitea](https://github.com/go-gitea/gitea)|git系统 golang|9k★ 独立二进制包,无障碍开启self-hosted git service(自托管git服务). all platforms which Go supports!|


#### 学习项目
|名称|属性|描述|
|:-------------:|--|-----|
|[Linux kernel map](http://www.makelinux.net/kernel_map/)|linux|linux内核学习 交互式操作 美国网站|
|[Annotated Nginx Source（中文）](https://github.com/chronolaw/annotated_nginx)|C web中间件|源代码学习 《Nginx完全开发指南》的作者 |

#### 学习资源
|名称|属性|描述|
|:-------------:|--|-----|
|[Java-Guide](https://github.com/Snailclimb/Java-Guide)|非代码|Java学习指南 一份涵盖大部分Java程序员所需要掌握的核心知识|
|[Project-Based-Tutorials-in-C](https://github.com/rby90/Project-Based-Tutorials-in-C)|C|2k★ C语言 基于项目的教程|
|[tensorflow_cookbook](https://github.com/nfmcclure/tensorflow_cookbook)|python #ML|3k★ Code for Tensorflow Machine Learning Cookbook |

#### 其他
|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[MailHog](https://github.com/mailhog/MailHog)|golang|all|3k★ 仅供SMTP测试使用（基于Web和API） Mac下安装:`brew install mailhog`|

