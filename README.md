# Content

________________________________________________/

- [MyProject我的项目](#myproject)
- [MyNotes我的笔记](#mynotes)
- [Security安全](#security)
    + [工具_防御与分析](#工具_防御与分析)
    + [工具_扫描自动化](#工具_扫描自动化)
    + [website_mail匿名邮箱](#website_mail匿名邮箱)
    + [website_exp引擎](#website_exp引擎)
    + [website_网络空间引擎](#website_网络空间引擎)
    + [website_威胁分析引擎_防御](#website_威胁分析引擎_防御)
    + [website_破解hash](#website_破解hash)
- [Development开发](#development)
    + [必备网站](#必备网站)
    + [Awesome & cheat sheets](#awesome_and_cheat_sheets)
    + [开源图书](#开源图书)
    + [web应用开发-基础框架](#web应用开发-基础框架)
    + [web前端](#web前端)
    + [web系统](#web系统)
    + [学习项目](#学习项目)
    + [知识_books](#知识_books)
    + [其他工具](#其他工具)
    + [CMS](#cms)
    
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
|[net - 流量分析 wireshark](net_wireshark.md)|net_wireshark.md|
|[net - 流量分析 suricata 规则](net_suricata)|net_suricata.md|
|[web - 代码审计 - PHP_例1_命令执行漏洞](web_code_audit_PHP.md)|web_code_audit_PHP.md|
|[web - 代码审计 - Python_例1_命令执行漏洞](web_code_audit_Python.md)|web_code_audit_Python.md|
|[web - vul - SQLi_原理_利用方式_修复方案](web_vul_sqli.md)|web_vul_sqli.md|
|[web - vul - SSRF_原理_利用方式_修复方案](web_vul_SSRF.md)|web_vul_SSRF.md|
|[web - vul - XSS  原理 利用方式 修复方案](web_vul_XSS.md)|web_vul_XSS.md|
|[web - vul - CSRF 原理 利用方式 修复方案](web_vul_CSRF.md)|web_vul_CSRF.md|
|[web - 中间件常见问题修复方案 SSL slowhttpdos Tomcat](web_vul_mid_SSL_slowhttpdos.md)|web_vul_mid_SSL_slowhttpdos.md|
|[web - 靶场 - WebGoat8搭建及WriteUp](z_web_webgoat.md)|z_web_webgoat.md|
|[host - 主机扫描思路_各种服务的攻击与防御](host_sec_port_service_exp.md)|host_sec_port_service_exp.md|
|[Proxy - 代理工具_tcp_udp_http_https_VPN_Socks5](sec_proxy.md)|sec_proxy.md #socks5 #内网穿透|
|[blockchain - ETH 智能合约 基本概念](blockchain_SmartContracts.md)|blockchain_SmartContracts.md|
|[blockchain - 虚拟币挖矿的各种方式](blockchain_mining.md)|blockchain_mining.md|
|[SDL - 安全开发生命周期 实践与意义](sdl.md)|sdl.md|
|[实践 - 钓鱼Phishing_无文件攻击](sec_Phishing.md)|sec_Phishing.md|
|[实践 - MacOS下的one_liner执行加密的payload](sec_mac_encryption_one_liner.md)|sec_mac_encryption_one_liner.md|
|[实践 - github泄露扫描工具的搭建过程](sec_github_scan.md)|sec_github_scan.md|
|[实践 - 信息搜集 Domain_IP_Google](sec_info_gathering.md)|sec_info_gathering.md|
|[实践 - SQLi sqlmap常用参数 tamper详解](sec_sqlmap.md)|sec_Sqlmap.md|
|[实践 - msfvenom - msf的payload生成器](sec_msfvenom.md)|sec_msfvenom.md|
|[实践 - WEB应用安全部署架构_及 WAFbypass通用方式](web_WAF_bypass.md)|web_WAF_bypass.md|
|[实践 - burp基础_设置_插件_技巧](web_x_burp.md)|web_x_burp.md|
|[实践 - burp基础 Intruder的4种攻击方式](web_x_burp_Intruder.md)|web_x_burp_Intruder.md|
|[red - 构建高适应性的C2基础设施](sec_C2.md)|sec_C2.md|
|[red - RAT - Windows - 远控_域渗透](sec_RAT.md)|sec_RAT.md #windows #RedTeam|
|[red - 多维度实现免杀](sec_evasion.md)|sec_evasion.md #FUD #Evasion #python|
|[red - 后渗透 - 持久化 信息搜集](sec_RAT_post_exploitation.md)|sec_RAT_post_exploitation.md #linux #MacOS #backdoor|
|[red - webshell主控端及后渗透操作](sec_webshell.md)|sec_webshell.md|
|[基础 - 非对称加密_RSA_TLS_HTTPS_数字证书](web_x_https_tls.md)|#密码学 web_x_https_tls.md|
|[基础 - 浏览器的同源策略 跨域方案 CORS JSONP](web_x_SOP.md)|web_x_SOP.md|
|[笔记 - pwn linux下的二进制程序pwn](note_bin_pwn.md)|note_bin_pwn.md|
|[笔记 - GDB](note_bin_GDB.md)|note_bin_GDB.md|
|[笔记 - Docker](note_Docker.md)|note_Docker.md|
|[笔记 - Redis](note_DB_redis.md)|note_DB_redis.md|
|[笔记 - android应用安全](note_sec_android.md)|note_sec_android.md|
|[爬虫分类 及 静态爬虫内容提取利器GraphQuery](z_web_crawl.md)|z_web_crawl.md|
|[MacOS下的好用软件、快捷键、命令、技巧](z_command_Mac.md)|z_command_Mac.md|
|[各系统的自动化操作](z_auto_operation.md)|z_auto_operation.md|

---

# Security

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
|[cea-sec/ivre](https://github.com/cea-sec/ivre)|端口扫描|web界面友好|
|[Yuki-Chan-The-Auto-Pentest](https://github.com/Yukinoshita47/Yuki-Chan-The-Auto-Pentest)|主机扫描|python+shell / kali_linux|Automate Pentest Tool|
|[Sn1per](https://github.com/1N3/Sn1per)|主机扫描|python+shell / kali_linux docker|Automated Pentest Recon Scanner.  kali`./install.sh`|
|[ttarvis/glasgo](https://github.com/ttarvis/glasgo)|Golang代码静态分析|golang / all | A Security Scanner for Go|
|[ajinabraham/NodeJsScan](https://github.com/ajinabraham/NodeJsScan)|node.js代码静态分析|node.js|Node.js应用程序的静态安全代码扫描程序|
|[jeffzh3ng/Fuxi-Scanner](https://github.com/jeffzh3ng/Fuxi-Scanner)|端口扫描 PoC扫描 资产管理|python3|Network Security Vulnerability Scanner|

#### Payload_dictionary_字典

|名称|描述|
|:-------------:|-----|
|https://github.com/danielmiessler/SecLists/tree/master/Passwords|弱口令字典|
|https://weakpass.com/|弱口令字典|
|[dictionary/web_upload_filename_all.txt](https://github.com/1135/dictionary/blob/master/web_upload_filename_all.txt)|fuzz字典_web上传_文件名|
|[Unleashing an Ultimate XSS Polyglot · 0xSobky/HackVault Wiki](https://github.com/0xsobky/HackVault/wiki/Unleashing-an-Ultimate-XSS-Polyglot)|XSS payload|
|[dictionary/web_markdown_xss_payload.txt](https://github.com/1135/dictionary/blob/master/web_markdown_xss_payload.txt)|markdwon XSS payload|

#### website_mail匿名邮箱

|名称|描述|
|:-------------:|--|
|https://emkei.cz/|在线发送伪造邮件(可用于钓鱼 如伪造邮件发送者xx@163.com发送到目标的qq邮箱)|
|http://www.yopmail.com/zh/|临时邮箱 可随意进入xxx@yopmail.com收发邮件|
|http://www.mailinator.com/|临时邮箱 可随意进入asd@mailinator.com收邮件|
|https://10minutemail.net/|临时邮箱 十分钟邮箱 短期 每次邮箱地址不同 适用于注册小号|
|https://protonmail.com/|匿名邮箱 瑞士的加密邮箱 长期 google验证码认证后可注册.[福布斯评价：唯一NSA 无法接近的邮件服务](http://www.forbes.com/sites/hollieslade/2014/05/19/the-only-email-system-the-nsa-cant-access/#5060612155ed) |


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
[ThreatMiner.org](https://www.threatminer.org/)|威胁分析引擎|威胁情报的数据挖掘（有2008至今的APT报告pdf） Search for domains, IPs, MD5|SHA1|SHA256, email address or APTnotes(aptnotes:), ssl(ssl:), user-agent(ua:), AV family(av:), filename (filename:), URI (uri:), registry (reg:), mutex (mutex:)...|

#### website_破解hash
|名称|描述|
|:-------------:|-----|
|[Ophcrack](https://www.objectif-securite.ch/en/ophcrack.php)|[瑞士] 在线破解 crack NTLM. 不错|
|[onlinehashcrack.com](https://www.onlinehashcrack.com/) |破解 Hashes(NTLM/wordpress/MySQL). Wifi WPA(2). MS Office.|

---

## Development

#### 必备网站

|名称|属性|描述|
|:-------------:|--|-----|
|[regex101.com](https://regex101.com/)|regex|在线调试正则 支持php/javascript/python/golang. 代码生成器:直接生成多种语言代码java/golang等|
|[graphemica.com/](http://graphemica.com/)|characters|同一字符的各种编码情况下的表示 各种格式|
|https://codebeautify.org/|encoder|进制转换 json xml Escape/Unescape sql...|
|https://httpbin.org/|http debug|A simple HTTP Request & Response Service.|

#### Awesome_and_cheat_sheets

|名称|描述|
|:-------------:|-----|
|[Awesome-Hacking](https://github.com/Hack-with-Github/Awesome-Hacking)|Awesome-Hacking|
|[Awesome-CheatSheet](https://github.com/wxyyxc1992/Awesome-CheatSheet)|全栈 多语言基本语法 CheatSheet|
|[Awesome-Go](https://github.com/avelino/awesome-go)|37k★ Awesome-Go|
|[BruceDone/awesome-crawler](https://github.com/BruceDone/awesome-crawler)|3k★ 各语言爬虫 web crawler|
|[mre/awesome-static-analysis](https://github.com/mre/awesome-static-analysis)|4k★ 各语言静态分析工具Static analysis tools for all programming languages|
|[coreb1t/awesome-pentest-cheat-sheets](https://github.com/coreb1t/awesome-pentest-cheat-sheets)|1k★ #pentesting |
|[rshipp/awesome-malware-analysis](https://github.com/rshipp/awesome-malware-analysis)|4k★ #流量分析 A curated list of awesome malware analysis tools and resources.|
|[a13xp0p0v/linux-kernel-defence-map](https://github.com/a13xp0p0v/linux-kernel-defence-map)|linux内核防御图|
|[niklongstone/regular-expression-cheat-sheet](https://github.com/niklongstone/regular-expression-cheat-sheet)|Regular Expression Cheat Sheet - PCRE|

#### 开源图书

|Golang-Books|Description|
|:-------------:|-----|
|[Go语言圣经](https://books.studygolang.com/gopl-zh/)|[备份链接](https://www.ctolib.com/docs//sfile/gopl-zh-github-com-master/preface.html)|
|[advanced-go-programming-book](https://github.com/chai2010/advanced-go-programming-book/blob/master/SUMMARY.md)| 《Go语言高级编程》涵盖CGO、Go汇编语言、RPC实现、Protobuf插件实现、Web框架实现、分布式系统等|
|[go2-book](https://github.com/chai2010/go2-book/blob/master/SUMMARY.md)|《Go2编程指南》重点讲解Go2新特性|


#### web应用开发-基础框架

|框架名称|属性|描述|
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
|[phacility/phabricator](https://github.com/phacility/phabricator)|CodeReview系统 PHP|11k★ Open software engineering platform(可用作CodeReview系统)|

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
|[CyC2018/CS-Notes](https://github.com/CyC2018/CS-Notes)|md|40k★ Computer Science Learning Notes 算法 操作系统 网络 面向对象 数据库 Java 系统设计 工具 编码实践 后记|
|[free-programming-books](https://github.com/EbookFoundation/free-programming-books/blob/master/free-programming-books-zh.md)|md|110k★ WEB服务器 操作系统 数据库 大数据 编程语言|

#### 其他工具

|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[transfer.sh](https://github.com/dutchcoders/transfer.sh)|Golang|all|7k★ 命令行中上传文件`curl --upload-file ./1.txt https://yoursite.com/1.txt`|
|[MailHog](https://github.com/mailhog/MailHog)|Golang|all|3k★ 仅供SMTP测试使用（基于Web和API） Mac下安装:`brew install mailhog`|
|[GoRequest](https://github.com/parnurzeal/gorequest)|Golang|all| 2k★ Simplified HTTP client (inspired by nodejs SuperAgent)|
|[asciimoo/searx](https://github.com/asciimoo/searx)|python3|Debian/Ubuntu| 4k★ About 70 supported search engines. 搜索引擎. No user tracking/profiling.  https://asciimoo.github.io/searx/|
|[httplib2](https://github.com/httplib2/httplib2)|python|/|#基本库 社区维护的HTTP client library 特性包括:所有Methods 授权Authentication 持久连接Keep-Alive 缓存Caching  压缩Compression(gzip/deflate)|
|[puppeteer](https://github.com/GoogleChrome/puppeteer)|js|/| 38k★ [google] Headless Chrome Node API |

#### CMS

|名称|属性|描述|
|:-------------:|--|-----|
|[WordPress](https://github.com/WordPress/WordPress)|php|11k★ 31% of the web uses [WordPress](https://wordpress.org/), from hobby blogs to the biggest news sites online.|
|[joomla](https://github.com/joomla/joomla-cms)|php|3k★ Joomla! Content Management System.|
