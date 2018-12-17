# Content

________________________________________________/

- [MyProject我的项目](#myproject)
- [MyNotes我的笔记](#mynotes)
- [Security安全](#security)
    + [知识_SDL](#知识_sdl)
    + [知识_RedTeam](#知识_redteam)
    + [全能框架](#全能框架)
    + [工具_后渗透windows_post_exploitation](#工具_后渗透windows_post_exploitation)
    + [工具_后渗透](#工具_后渗透)
    + [工具_Webshell管理](#工具_Webshell管理)
    + [工具_CTF](#工具_ctf)
    + [工具_web](#工具_web)
    + [工具_反编译_Decompiler](#工具_反编译_Decompiler)
    + [工具_二进制_debug_pwn](#工具_二进制_debug_pwn)
    + [工具_二进制_fuzz](#工具_二进制_fuzz)
    + [工具_Proxy代理](#工具_proxy代理)
    + [ProxyPool](#proxypool)
    + [工具_防御与分析](#工具_防御与分析)
    + [工具_扫描自动化](#工具_扫描自动化)
    + [爬虫](#爬虫)
    + [工具_免杀AntiVirusEvasion](#工具_免杀antivirusevasion)
    + [CMS](#cms)
    + [样本](#样本)
    + [website_mail匿名邮箱](#website_mail匿名邮箱)
    + [website_知识_安全技术](#website_知识_安全技术)
    + [website_exp引擎](#website_exp引擎)
    + [website_网络空间引擎](#website_网络空间引擎)
    + [website_威胁分析引擎_防御](#website_威胁分析引擎_防御)
    + [website_安全工具_防御](#website_安全工具_防御)
    + [website_安全工具](#website_安全工具)
    + [website_encoder](#website_encoder)
    + [website_ip](#website_ip)
- [Development开发](#development)
    + [必备网站](#必备网站)
    + [其他网站](#其他网站)
    + [Awesome系列](#awesome)
    + [web应用开发-基础框架](#web应用开发-基础框架)
    + [web前端](#web前端)
    + [web系统](#web系统)
    + [学习项目](#学习项目)
    + [知识_books](#知识_books)
    + [其他工具](#其他工具)
    
________________________________________________/

## MyProject
|项目名称|属性|描述|
|:-------------:|--|-----|
|[1135/EquationExploit](https://github.com/1135/EquationExploit)|Java C++|在Windows下针对网段批量利用永恒之蓝漏洞(MS-17010 EternalBlue) |
|[1135/VulSpider](https://github.com/1135/VulSpider)|python2|后台持续运行，获取最新漏洞及每日简报，发送邮件给安全人员|
|[1135/Applescript_demos](https://github.com/1135/Applescript_demos)|applescript|苹果脚本示例，在Mac下使用苹果脚本实现发送iMessage短信|
|[1135/python_demos](https://github.com/1135/python_demos)|python2|python code|
|[1135/Go_demos](https://github.com/1135/Go_demos)|golang|golang code|
|[1135/dictionary](https://github.com/1135/dictionary)|txt|字典收集 包括user/name/pass/web|

## MyNotes

|笔记标题|描述|
|:-------------:|-----|
|[实践 - 获取最新安全资讯的意义、模式和具体方式](sec_get_news.md)|sec_get_news.md|
|[web - 静态爬虫 - 内容提取利器 - GraphQuery](z_web_crawl.md)|z_web_crawl.md|
|[web - 代码审计 - PHP_例1_命令执行漏洞](web_code_audit_PHP.md)|web_code_audit_PHP.md|
|[web - 代码审计 - Python_例1_命令执行漏洞](web_code_audit_Python.md)|web_code_audit_Python.md|
|[web - 中间件修复方案 SSL slowhttpdos Tomcat](web___fix_中间件漏洞修复方案_SSL_slowhttpdos.md)|web___fix_中间件漏洞修复方案_SSL_slowhttpdos.md|
|[web - 靶场 - WebGoat8搭建及WriteUp](z_web_webgoat.md)|z_web_webgoat.md|
|[host - 主机扫描思路_各种服务的攻击与防御](host_sec_port_service_exp.md)|host_sec_port_service_exp.md|
|[net - wireshark使用总结](z_net_wireshark.md)|z_net_wireshark.md|
|[Mac - MacOS下的好用软件、快捷键、命令、技巧](z_command_Mac_软件_快捷键_命令_技巧.md)|z_command_Mac_软件_快捷键_命令_技巧.md|
|[Proxy - Ubuntu/CentOS一键安装Socks5代理服务器](linux_proxy_Ubuntu_CentOS_socks5_Server.md)|linux_proxy_Ubuntu_CentOS_socks5_Server.md|
|[blockchain - ETH 智能合约 基本概念](blockchain_SmartContracts.md)|blockchain_SmartContracts.md|
|[blockchain - 虚拟币挖矿的各种方式](blockchain_mining.md)|blockchain_mining.md|
|[安全开发生命周期（Security Development Lifecycle）](sdl.md)|sdl.md|
|[工具 - 钓鱼Phishing直接进内网](sec_Phishing.md)|sec_Phishing.md|
|[实践 - MacOS下的one_liner执行加密的payload](sec_mac_encryption_one_liner.md)|sec_mac_encryption_one_liner.md|
|[实践 - github泄露扫描工具的搭建过程](sec_github_scan.md)|sec_github_scan.md|
|[实践 - 信息搜集_Google Hacking](sec_info_gathering.md)|sec_info_gathering.md|
|[RAT - 远控软件历史](z_history_RAT.md)|z_history_RAT.md|
|[各系统的自动化操作](z_auto_operation.md)|z_auto_operation.md|

---

## Security

#### 知识_SDL

|名称|描述|
|:-------------:|--|
|[sdlchina-web](https://github.com/SecurityPaper/SecurityPaper-web/tree/master/_posts)|SDL流程简介 SDL规范文档 SDL落地方案|


#### 知识_RedTeam

|名称|描述|
|:-------------:|--|
|[Red-Team-Infrastructure-Wiki](https://github.com/bluscreenofjeff/Red-Team-Infrastructure-Wiki)|红队基础设施Wiki|
|[infosecn1nja/AD-Attack-Defense](https://github.com/infosecn1nja/AD-Attack-Defense)|Active Directory Security For Red & Blue Team|
|红队攻防揭秘|微信公众号|

#### 全能框架

|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[metasploit-framework](http://github.com/rapid7/metasploit-framework)|Ruby #RAT|Linux/Mac OS/windows|必备的功能强大的渗透测试框架|

#### 工具_后渗透windows_post_exploitation

|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[Empire](https://github.com/EmpireProject/Empire)|python|Kali/Debian/Ubuntu|#域渗透 #RAT 域渗透利器Empire is a post-exploitation framework|
|[CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec)|python+powershell|[Installation](https://github.com/byt3bl33d3r/CrackMapExec/wiki/Installation)| 2k★ #域渗透 域渗透利器|
|[QuasarRAT](https://github.com/quasar/QuasarRAT)|C#|windows|#RAT 传统远控 Remote Administration Tool for Windows|
|[sRDI](https://github.com/monoxgas/sRDI)|powershell|windows|Shellcode implementation of Reflective DLL Injection. Convert DLLs to position independent shellcode|

#### 工具_后渗透

Remote Administration Tool

|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[EggShell](https://github.com/neoneggplant/EggShell)|python|macOS/Linux|#RAT iOS(Jailbroken)/macOS/Linux(OnlyFileManage)|
|[LaZagne](https://github.com/AlessandroZ/LaZagne)|python|windows/Linux/macOS|#密码恢复 Credentials recovery project. 系统口令/浏览器软件/邮箱软件/wifi|
[NYAN-x-CAT/Lime-RAT](https://github.com/NYAN-x-CAT/Lime-RAT)|VB|windows|#RAT 额外功能:勒索(加密文件)、xmr挖矿、DDOS|
|[backdoors](https://github.com/iamckn/backdoors)|sh|linux|#backdoor [利用SSH日志触发的后门分析](http://www.freebuf.com/articles/system/185942.html)|
|[dnscat2](https://github.com/iagox86/dnscat2)|C++ C Ruby|linux Win32|#backdoor #DNS|
|[fbkcs/ThunderDNS](https://github.com/fbkcs/ThunderDNS)|python3/PowerShell/bash|linux/Win|#backdoor #DNS This tool can forward TCP traffic over DNS protocol. Non-compile clients + socks5 support.|
|[m0nad/Diamorphine](https://github.com/m0nad/Diamorphine)|C|linux|#backdoor 需要ROOT权限 隐藏指定进程与通信 LKM rootkit for Linux Kernels 2.6.x/3.x/4.x (x86 x86_64)|

#### 工具_Webshell管理

|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[epinna/weevely3](https://github.com/epinna/weevely3)|python|php|文件管理/HTTP(S) proxy/Bruteforce SQL accounts/Port scan|
|[rebeyond/Behinder](https://github.com/rebeyond/Behinder)|Java|php/Java/.NET/“冰蝎”动态二进制 webshell管理端|

#### 工具_CTF

|名称|描述|
|:-------------:|--|
|https://github.com/zardus/ctf-tools|3k★ CTF工具集 包含类型:binary forensics crypto web stego misc|


知识_CTF

|名称|描述|
|:-------------:|--|
|https://ctf-wiki.github.io/ctf-wiki/|#CTF知识 wiki知识库|
|http://www.garykessler.net/library/file_sigs.html|#CTF知识 各种文件头部hex值 真实文件格式|

#### 工具_web

|名称|描述|
|:-------------:|--|
|[sqlmap](https://github.com/sqlmapproject/sqlmap)| Automatic SQL injection and database takeover tool|
|[upload-fuzz-dic-builder](https://github.com/c0ny1/upload-fuzz-dic-builder)| #fuzz 生成字典 尝试突破文件上传限制|

#### 工具_反编译_Decompiler

|名称|属性|描述|
|:-------------:|--|-----|
|[Luyten](https://github.com/deathmarine/Luyten)|Java|2k★ Java Decompiler [Gui] |


#### 工具_二进制_debug_pwn

|名称|属性|描述|
|:-------------:|--|-----|
|[gdbgui](https://github.com/cs01/gdbgui)|/|6k★ 基于浏览器可视化gdb调试 [下载安装ubuntu14.04+](https://gdbgui.com/downloads.html)|
|[peda](https://github.com/longld/peda)|python|6k★ Python Exploit Development Assistance for GDB|
|[pwntools](https://github.com/Gallopsled/pwntools)|python2.7(best Ubuntu)|4k★ CTF framework and exploit development library|
|[heap-viewer](https://github.com/danigargu/heap-viewer)|python| [IDA Pro plugin] examine the glibc heap, focused on exploit development|

|名称|属性|描述|
|:-------------:|--|-----|
|http://syscalls.kernelgrok.com/|syscalls|系统调用参考 主要用sys_execve|
|http://shell-storm.org/shellcode/|shellcode|各种平台下的shellcode包括:BSD/Cisco/FreeBSD/Linux/OSX/Windows|


#### 工具_二进制_fuzz

|名称|属性|描述|
|:-------------:|--|-----|
|[halfempty](https://github.com/googleprojectzero/halfempty)|C|[projectZero] testcase minimization tool.|


#### 工具_Proxy代理

|名称|属性|描述|
|:-------------:|--|-----|
|[frp](https://github.com/fatedier/frp)|Golang|18k★ 高性能的反向代理(reverse proxy). 用于内网穿透(将NAT或防火墙后面的本地服务器暴露给Internet),支持协议:tcp, udp, http, https.[中文文档](https://github.com/fatedier/frp/blob/master/README_zh.md)|
|[brook](https://github.com/txthinking/brook)|Golang|6k★ 【Server端】(Linux/MacOS/Windows/Android/iOS)开启VPN/Socks5/Shadowsocks 【Client端】可连接，以及Socks5 to HTTP|

#### ProxyPool

|名称|描述|
|:-------------:|-----|
|http://proxylist.fatezero.org/proxy.list | 在线代理池 每15分钟更新. 实测很多高匿代理会暴露真实ip(具体在http header的X-Forwarded-For中最后一个ip)   即https://github.com/fate0/proxylist/blob/master/proxy.list|


#### 工具_防御与分析

|名称|属性|描述|
|:-------------:|-----|-----|
|[suricata](https://github.com/OISF/suricata) |流量解析| #强大的网络威胁检测引擎 real time IDS, IPS, network security monitoring (NSM) and offline pcap processing.|
[Security-Onion-Solutions/security-onion](https://github.com/Security-Onion-Solutions/security-onion)|流量解析|#一个liunx系统镜像 内含 Elasticsearch, Logstash, Kibana, Snort, Suricata, Bro, Wazuh, Sguil, Squert, CyberChef, NetworkMiner, and many other security tools. |
|[cuckoo](https://github.com/cuckoosandbox/cuckoo)|文件分析引擎|3k★ #分析行为 an automated dynamic malware analysis system|
|[objective-see/LuLu](https://github.com/objective-see/LuLu)|终端防御|3k★ MacOS防火墙 LuLu is the free macOS firewall that aims to block unauthorized (outgoing) network traffic|
[Eloston/ungoogled-chromium](https://github.com/Eloston/ungoogled-chromium#software-repositories)|终端防御|6k★ 无Google的chrome/chromium浏览器[Downloads for ungoogled-chromium](https://ungoogled-software.github.io/ungoogled-chromium-binaries/)|
|[Tencent/bk-cmdb](https://github.com/Tencent/bk-cmdb)|资产管理|2k★ 蓝鲸配置平台（蓝鲸CMDB）是一个基于运维场景设计的企业配置管理服务|
|[SpiderLabs/owasp-modsecurity-crs](https://github.com/SpiderLabs/owasp-modsecurity-crs)|Web攻击检测规则|OWASP ModSecurity核心规则集 Web应用程序防火墙的通用攻击检测规则|

#### 工具_扫描自动化

|名称|属性|编程语言/运行环境|描述|
|:-------------:|--|--|-----|
|[Yuki-Chan-The-Auto-Pentest](https://github.com/Yukinoshita47/Yuki-Chan-The-Auto-Pentest)|主机扫描|python+shell / kali_linux|Automate Pentest Tool|
|[Sn1per](https://github.com/1N3/Sn1per)|主机扫描|python+shell / kali_linux docker|Automated Pentest Recon Scanner.  kali`./install.sh`|
[ttarvis/glasgo](https://github.com/ttarvis/glasgo)|Golang代码静态分析|golang / all | A Security Scanner for Go|
[ajinabraham/NodeJsScan](https://github.com/ajinabraham/NodeJsScan)|node.js代码静态分析|node.js|Node.js应用程序的静态安全代码扫描程序|

#### Payload_dictionary_字典
|名称|描述|
|:-------------:|-----|
|https://github.com/danielmiessler/SecLists/tree/master/Passwords|弱口令字典|
|https://weakpass.com/|弱口令字典|
|[dictionary/web_upload_filename_all.txt](https://github.com/1135/dictionary/blob/master/web_upload_filename_all.txt)|fuzz字典_web上传_文件名|
|[Unleashing an Ultimate XSS Polyglot · 0xSobky/HackVault Wiki](https://github.com/0xsobky/HackVault/wiki/Unleashing-an-Ultimate-XSS-Polyglot)|XSS payload|
|[dictionary/web_markdown_xss_payload.txt](https://github.com/1135/dictionary/blob/master/web_markdown_xss_payload.txt)|markdwon XSS payload|

#### 爬虫

|名称|属性|描述|
|:-------------:|--|-----|
|https://github.com/NikolaiT/GoogleScraper|python3.7| 2k★ 异步搜索引擎爬虫 A Python module to scrape several search engines.|
|[chromeless](https://github.com/prisma/chromeless)|js|动态爬虫 Chrome自动化|
|[Anti-Anti-Spider](https://github.com/luyishisi/Anti-Anti-Spider)|python|反反爬虫|

#### 基本库

|名称|属性|描述|
|:-------------:|--|-----|
|[httplib2](https://github.com/httplib2/httplib2)|python|社区维护的HTTP client library 特性包括:所有Methods 授权Authentication 持久连接Keep-Alive 缓存Caching  压缩Compression(gzip/deflate)|
|[puppeteer](https://github.com/GoogleChrome/puppeteer)|js| 38k★ [google] Headless Chrome Node API |


#### 工具_免杀AntiVirusEvasion

FUD:Fully undetectable

|名称|属性|描述|
|:-------------:|--|-----|
|[avet](https://github.com/govolution/avet)|C+shell+python|[仅针对Windows exe] AntiVirus Evasion Tool 运行于kali2|
|[Veil-Evasion](https://github.com/Veil-Framework/Veil-Evasion)|shell+C+python2.7|[可针对Windows exe] generate metasploit payloads that bypass common anti-virus solutions|
|[ASWCrypter](https://github.com/AbedAlqaderSwedan1/ASWCrypter)|shell+python|An Bash&Python Script For Generating Payloads that Bypasses All Antivirus so far FUD.实测无法过360ZhuDongFangyu|
|[tokyoneon/Armor](https://github.com/tokyoneon/Armor)|shell|[仅针对MacOS]Armor is a simple Bash script designed to create encrypted macOS payloads capable of evading antivirus scanners.|

#### CMS
|名称|属性|描述|
|:-------------:|--|-----|
|[WordPress](https://github.com/WordPress/WordPress)|php|11k★ 31% of the web uses [WordPress](https://wordpress.org/), from hobby blogs to the biggest news sites online.|
|[joomla](https://github.com/joomla/joomla-cms)|php|3k★ Joomla! Content Management System.|


#### 样本
|名称|描述|针对目标|
|--|--|-----|
|[chenerlich / FCL(Fileless Command Lines)](https://github.com/chenerlich/FCL)|无文件攻击实例(fileless malicious executions)|windows用户|


#### website_mail匿名邮箱

|名称|描述|
|:-------------:|--|
|https://emkei.cz/|在线发送伪造邮件(可用于钓鱼 如伪造邮件发送者xx@163.com发送到目标的qq邮箱)|
|http://www.yopmail.com/zh/|临时邮箱 可随意进入xxx@yopmail.com收发邮件|
|http://www.mailinator.com/|临时邮箱 可随意进入asd@mailinator.com收邮件|
|https://10minutemail.net/|临时邮箱 十分钟邮箱 短期 每次邮箱地址不同 适用于注册小号|
|https://protonmail.com/|匿名邮箱 瑞士的加密邮箱 长期 google验证码认证后可注册.[福布斯评价：唯一NSA 无法接近的邮件服务](http://www.forbes.com/sites/hollieslade/2014/05/19/the-only-email-system-the-nsa-cant-access/#5060612155ed) |


#### website_知识_安全技术

|名称|属性|描述|
|:-------------:|--|-----|
|[OWASP](https://www.owasp.org/index.php/Main_Page)|知识库|自由开放的软件安全社区|
|-|知识库|OWASP web测试指南V4 [owasp-testing-guide-v4(中文)](https://kennel209.gitbooks.io/owasp-testing-guide-v4/content/zh/index.html)|
|-|知识库|#反序列化安全# [Deserialization Cheat Sheet](https://www.owasp.org/index.php/Deserialization_Cheat_Sheet)|
|https://attack.mitre.org/|知识库|真实世界用到的、系统化的网络攻击技术|
|[www.seebug.org](https://www.seebug.org/)|/|知道创宇|
|[paper.seebug.org](https://paper.seebug.org/)|Papers|知道创宇 Papers|
|[每日安全 - 腾讯安全玄武实验室出品](https://sec.today/)|/|腾讯安全玄武实验室出品|

#### website_exp引擎

|名称|属性|描述|
|:-------------:|--|-----|
|[exploit-db](https://www.exploit-db.com/)|exp引擎|Offensive Security’s Exploit Database Archive. /remote/webapps/local/dos/papers|
|[sploitus.com](https://sploitus.com/)|exp引擎|Exploits.|

#### website_网络空间引擎

|名称|属性|描述|
|:-------------:|--|-----|
|[Shodan.io](https://www.shodan.io/)|网络空间引擎|Shodan is the world's first search engine for Internet-connected devices.|
|[Fofa.so](https://fofa.so/)|网络空间引擎|白帽汇 [规则列表](https://fofa.so/library)|
|[Zoomeye](https://www.zoomeye.org/)|网络空间引擎|知道创宇|
|[censys.io](https://censys.io/ipv4)|网络空间引擎|censys.io|

#### website_威胁分析引擎_防御

|名称|属性|描述|
|:-------------:|--|-----|
|[360威胁分析平台](https://ti.360.net/)|威胁分析引擎|威胁情报 ip domain file url email|
|[微步在线威胁情报](https://x.threatbook.cn/nodev4/vb4/list)|威胁分析引擎|威胁情报 ip domain file url email|
|[在线沙箱app.any.run](https://app.any.run/tasks/defe1b39-b4b6-4573-ba46-de2c425f670f)|威胁分析引擎|在线沙箱:提取ioc 下载样本 下载pcap文件 运行线程 DNS解析 http通信|
|[VirusTotal](https://www.virustotal.com/)|威胁分析引擎|分析可疑文件和URL 以检测恶意软件类型|


#### website_安全工具_防御
|名称|描述|
|:-------------:|-----|
|http://spf.myisp.ch/|#防御 邮件服务器SPF配置检查|

#### website_安全工具

|名称|描述|
|:-------------:|-----|
|[findsubdomains.com](https://findsubdomains.com/) |domain信息收集 子域名查找 Find subdomains online.|
|[dnsdumpster.com](https://dnsdumpster.com/) |domain信息收集 DNS枚举子域名 FREE domain research tool that can discover hosts related to a domain.|
|[wappalyzer.com](https://www.wappalyzer.com/) |信息收集 web技术栈 web frameworks, server software, analytics tools and many more. |
|[onlinehashcrack.com](https://www.onlinehashcrack.com/) |crack在线破解 Hashes(NTLM/wordpress/MySQL). Wifi WPA(2). MS Office.|
|[Objectif Sécurité - Ophcrack](https://www.objectif-securite.ch/en/ophcrack.php)|crack在线破解 NTLM|

#### website_encoder

|名称|描述|
|:-------------:|-----|
|https://codebeautify.org/|进制转换 json xml Escape/Unescape sql...|


#### website_ip

|名称|描述|
|:-------------:|-----|
|https://whoer.net/ | ip匿名性自测 可获取内网ip(通过webRTC等技术) |
|https://www.ipip.net/ip.html | 查国内ip 相对精确的物理位置|
|http://www.ifconfig.io/ | my ip address |

---

## Development

#### 必备网站

|名称|属性|描述|
|:-------------:|--|-----|
|[regex101.com](https://regex101.com/)|regex|在线调试正则 支持php/javascript/python/golang. 代码生成器:直接生成多种语言代码java/golang等|
|[graphemica.com/](http://graphemica.com/)|characters|同一字符的各种编码情况下的表示 各种格式|

#### 其他网站
|名称|属性|描述|
|:-------------:|--|-----|
|https://httpbin.org/|http debug|A simple HTTP Request & Response Service.|

#### Awesome

|名称|描述|
|:-------------:|-----|
|[Awesome-Hacking](https://github.com/Hack-with-Github/Awesome-Hacking)|Awesome-Hacking|
|[Awesome-CheatSheet](https://github.com/wxyyxc1992/Awesome-CheatSheet)|全栈 多语言基本语法 CheatSheet|
|[Awesome-Go](https://github.com/avelino/awesome-go)|37k★ Awesome-Go|
|[BruceDone/awesome-crawler](https://github.com/BruceDone/awesome-crawler)|3k★ 各语言爬虫 web crawler|

#### web应用开发-基础框架

|名称|属性|描述|
|:-------------:|--|-----|
|[Spring Framework](https://github.com/spring-projects/spring-framework)|Java|23k★ create enterprise applications in a wide range of scenarios and architectures. |
|[Spring Boot](https://github.com/spring-projects/spring-boot)|Java|28k★ Spring Boot makes it easy to create Spring-powered, production-grade applications and services with absolute minimum fuss. |
|[ThinkPHP5 Framework](https://github.com/top-think/framework)|php|1k★ 中文web应用开发框架  |
|[Vue.js](https://github.com/vuejs/vue)|js前端框架|120k★ A progressive, incrementally-adoptable JavaScript framework for building UI on the web.|


#### web前端

|名称|属性|描述|
|:-------------:|--|-----|
|[Quill Rich Text Editor](https://github.com/quilljs/quill)|富文本编辑器|19k★ 现代编辑器 专为兼容性和可扩展性而构建 安全好用  |
|[PDF.js](https://mozilla.github.io/pdf.js/)|pdf浏览|23k★ https://mozilla.github.io/pdf.js/|
|[clipboard.js](https://clipboardjs.com)|clipboard操作|23k★ Modern copy to clipboard. |

#### web系统

|名称|属性|描述|
|:-------------:|--|-----|
|[filebrowser](https://github.com/filebrowser/filebrowser)|云盘系统 Golang|4k★ A stylish web-based file manager|
|[nextcloud](https://github.com/nextcloud/server)|云盘系统 PHP|5k★ a safe home for all your data. 多客户端：web、ios、android |
|[gitea](https://github.com/go-gitea/gitea)|git系统 Golang|9k★ 独立二进制包,无障碍开启self-hosted git service(自托管git服务). all platforms which Go supports!|

#### 中间件
|名称|属性|描述|
|:-------------:|--|-----|
[mholt/caddy](https://github.com/mholt/caddy)|Golang|20k★ 跨平台 易部署的中间件 自动开启https配置 HTTP/2 web server|

#### 学习项目
|名称|属性|描述|
|:-------------:|--|-----|
|[Go Data Structures](https://github.com/emirpasic/gods)|Golang|5k★ Go数据结构 Containers/Sets/Lists/Stacks/Maps/Trees/Comparators/Iterators/…|
|[radovskyb/go-packages](https://github.com/radovskyb/go-packages)|Golang| Go的标准包库的大型代码示例、文档  适合自学Golang 2016年的项目|
|[Project-Based-Tutorials-in-C](https://github.com/rby90/Project-Based-Tutorials-in-C)|C|2k★ C语言 基于项目的教程|

#### 知识_books

|名称|属性|描述|
|:-------------:|--|-----|
|[Java-Guide](https://github.com/Snailclimb/Java-Guide)|非代码|Java学习指南 一份涵盖大部分Java程序员所需要掌握的核心知识|
|[tensorflow_cookbook](https://github.com/nfmcclure/tensorflow_cookbook)|python #ML|3k★ Code for Tensorflow Machine Learning Cookbook |
|[Linux kernel map](http://www.makelinux.net/kernel_map/)|linux|linux内核学习 交互式操作 美国网站|
|[Annotated Nginx Source（中文）](https://github.com/chronolaw/annotated_nginx)|C web中间件|源代码学习 《Nginx完全开发指南》的作者 |
|[PDFs_books](https://github.com/tongxurt/pdfs)|pdf|网络是怎样连接的_户根勤.pdf Linux内核设计与实现(第三版中文高清带目录).pdf|

#### 其他工具

|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[transfer.sh](https://github.com/dutchcoders/transfer.sh)|Golang|all|7k★ 命令行中上传文件`curl --upload-file ./1.txt https://yoursite.com/1.txt`|
|[MailHog](https://github.com/mailhog/MailHog)|Golang|all|3k★ 仅供SMTP测试使用（基于Web和API） Mac下安装:`brew install mailhog`|
|[GoRequest](https://github.com/parnurzeal/gorequest)|Golang|all|2k★  Simplified HTTP client (inspired by nodejs SuperAgent)|

