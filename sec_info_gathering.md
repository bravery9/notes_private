### 信息搜集 - github高级语法

* 编程语言 `language:java`
* 搜索个人 `user:1135`
* 搜索组织 `org:spring-cloud`
* 仓库的名称 `in:name 关键词`
* 仓库的描述 `in:descripton 关键词`
* 仓库的README文件 `in:readme 关键词`
* star数量筛选 `stars:>300`
* 仓库的size(KB) `size:>=1024`
* 最近一次更新时间(只看积极维护项目) `pushed:>2019-01-03`
* 创建日期 `created:>2019-01-03`
* license类别 `license:apache-2.0`

### 信息搜集 - Google常规语法

以下语法均经过实测 2019.3
```
使用引号搜索完全匹配的结果
"tallest building"

搜索未知字词
"largest * in the world"

搜素具体域名站点下的url
site:so.com

url中存在具体字符的url
inurl:"php?id="
inurl:.edu

url中存在具体关键字(所有关键字都必须存在 忽略符号 如/ )的url
allinurl:foo bar

在html body中存在特定关键字(多个关键字之一的都符合)的url
intext:seo blog

在html body中存在特定关键字(allintext 多个关键字同时存在才行)的url
allintext:seo blog

搜索社交媒体 如
@twitter

找相似的网站
related:bilibili.com
related:alipay.com

查找具体文件
filetype:pdf
filetype:xls inurl:pass user
```

```
用减法排除具有某些关键字的结果
site:apple.com -paper -music -inurl:https -inurl:.edu
site:apple.com -site:www.apple.com -inurl:support
```

* 例1 子域名搜集
  * `site:<qq.com>`
* 例2 web后台地址
  * 语法 `管理 后台 登陆 用户名 帐号 密码 验证码 系统 inurl:manage|admin|login|system|portal`
  * 语法 `site:github.com inurl:manage|admin|login|system|portal`


* 权威参考
  * [优化网页搜索 - Google 搜索帮助](https://support.google.com/websearch/answer/2466433)
  * [Advanced Searching in Google - Resources and Search Strategies](https://sites.google.com/site/resourcesandsearchstrategies/google/advanced-searching-in-google)
  * [Google高级语法（非官方） - Google Guide](http://www.googleguide.com/or_operator.html)
  * [ Google Hacking Database - exploit-db.com](https://www.exploit-db.com/google-hacking-database)


### 信息搜集 - IP/Domain

|域名相关工具|描述|
|:-------------:|-----|
|https://www.virustotal.com/#/domain/qq.com| 综合查询 Whois/子域名/DNS解析历史(支持子域名) |
|https://findsubdomains.com/subdomains-of/qq.com|domain 子域名信息 Find subdomains online.|
|https://dnsdumpster.com/|domain DNS枚举子域名信息 FREE domain research tool that can discover hosts related to a domain.|
|https://viewdns.info/|Reverse Whois/IP/domain/DNS/MS/NS Lookup.|
|https://dnstable.com/ip/203.205.158.53| Reverse 解析为某ip的诸多域名|
|https://viewdns.info/iphistory/?domain=qq.com|DNS解析历史(IP History) 该网站不支持查询子域名的历史IP|
|http://www.domaincrawler.com/qq.com|综合查询 whois/Mailserver(s)/subdomains|
|https://www.threatcrowd.org/ |Open Source Threat Intelligence. IP/Domain/mail|
|https://censys.io/ipv4/104.93.196.220/table| 根据tls证书获取域名 443.https.tls.certificate.parsed.extensions.subject_alt_name.dns_names|
[https://url.fht.im/domain_search?domain=qq.com| 基于大数据 [查看被搜索引擎收录的URL](https://url.fht.im/url_search?domain=v.qq.com)|

---

|其他工具|描述|
|:-------------:|-----|
|https://dns.google.com/query?name=qq.com|Google Public DNS 在线查询dns解析|
|https://www.ipip.net/ip.html | IP Location 国内物理位置|
[https://iplocation.com/?ip=52.186.31.196|IP Location 物理位置|
|[wappalyzer.com](https://www.wappalyzer.com/) |web技术栈信息 web frameworks, server software, analytics tools and many more. |
|https://api.hackertarget.com/nmap/?q=qq.com| 即时进行 端口扫描 仅对8个端口发起nmap扫描|
|https://api.hackertarget.com/dnslookup/?q=vqq.com|即时进行 DNS lookup (A MX ..)|
|https://whoer.net/ | web匿名性自测 (通过webRTC等技术获取内网ip) |
|http://www.ifconfig.io/ | 得到自身外网IP|
|https://osintframework.com/|OSINT Framework( 自动化工具[Photon: Incredibly fast crawler designed for OSINT.](https://github.com/s0md3v/Photon))|


### 更多

[jivoi/awesome-osint](https://github.com/jivoi/awesome-osint)
