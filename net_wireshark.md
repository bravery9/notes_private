### 流量分析

流量分析(traffic analysis)：根据"流量"发现 恶意软件特征 mining/APT/..

### 流量分析工具

|名称|描述|
|:-------------:|-----|
|Wireshark||
|[suricata](https://github.com/OISF/suricata) |强大的网络威胁检测引擎 real time IDS, IPS, network security monitoring (NSM) and offline pcap processing.|
[security-onion](https://github.com/Security-Onion-Solutions/security-onion)|一个liunx系统镜像 内含 Elasticsearch, Logstash, Kibana, Snort, Suricata, Bro, Wazuh, Sguil, Squert, CyberChef, NetworkMiner, and many other security tools. |

恶意软件分析工具与资源 [awesome-malware-analysis](https://github.com/rshipp/awesome-malware-analysis)

### Wireshark - 筛选

* 官方文档 [Wireshark User’s Guide](https://www.wireshark.org/docs/wsug_html_chunked/)
* 常用筛选示例
  * 查看所有web流量 `(http.request) or (ssl.handshake.type == 1)` 查看HTTP请求的URL + HTTPS(SSL/TLS)流量中使用的域名
  * http GET数据包`http.request.method==GET`
  * http 包的内容(包含header url responseCode ...) `http contains "User-Agent: "`
  * http URL地址特征 `http.request.uri == "/logo.png"`
  * 关注域名解析 `(dns.qry.name contains microsoft)`
  * 关注某主机 发到 / 来自 某ip 的数据包 `host 10.3.1.1` `ip.addr == 192.168.10.1`
  
* 逻辑条件组合(适用于对各种筛选条件进行逻辑组合)
  * 与 `and`
  * 或 `or`
  * 非 `!(表达式)` `not(表达式)` **注意格式**
  * 等于`eq` `==`
  * 包含某字符串 `contains`
* 筛选条件 - ip地址
  * 目的ip地址 `ip.dst==192.168.101.8` `dst host 192.168.101.8`
  * 源ip地址`ip.src==1.1.1.1` `src host 1.1.1.1`
  * 发到 / 来自 某ip 的数据包 `host 10.3.1.1` `ip.addr == 192.168.10.1`
  * 发到 / 来自 某域名解析的IP地址的数据包 `host www.1.com`
* 筛选条件 - 端口
  * 源端口 或 目的端口为 80 的数据包`tcp.port==80`
  * 目的端口为 80 的数据包`tcp.dstport==80`
  * 源端口为 80 的数据包`tcp.srcport==80`
* 筛选条件 - MAC地址
  * MAC地址是20:dc:e6:f3:78:cc的数据包  包括 源MAC地址 和 目的MAC地址`eth.addr==20:dc:e6:f3:78:cc`
  * 源MAC地址 `eth.src==20:dc:e6:f3:78:cc`
  * 目的MAC地址 `eth.dst==20:dc:e6:f3:78:cc`
* 筛选条件 - 协议
 * 选中某协议 `tcp` `udp` `dns` `http` `icmp` `ssl` `ftp` `smtp` `msnms` `arp` ...
 * 排除某协议 `!ssl` `not ssl`

### Wireshark - 插件

[pentesteracademy/patoolkit: PA Toolkit is a collection of traffic analysis plugins focused on security](https://github.com/pentesteracademy/patoolkit)

* WiFi (WiFi network summary, Detecting beacon, deauth floods etc.)
* HTTP (Listing all visited websites, downloaded files)
* HTTPS (Listing all websites opened on HTTPS)
* ARP (MAC-IP table, Detect MAC spoofing and ARP poisoning)
* DNS (Listing DNS servers used and DNS resolution, Detecting DNS Tunnels)检测DNS隧道

### 知名的流量分析博客

[Malware-Traffic-Analysis.net](https://www.malware-traffic-analysis.net/)

此博客专注于 与恶意软件感染相关的网络流量(network traffic related to malware infections)

其中每个博文基本都有相关的流量文件（.pcap files）和 恶意软件样本(malware samples)
 
[Malware-Traffic-Analysis.net - 流量分析练习Traffic Analysis Exercises](https://www.malware-traffic-analysis.net/training-exercises.html)

下载的zip文件解压密码为`infected`

### 确定受影响主机的信息

* 确定被感染主机的信息(系统信息、IP地址、MAC地址、计算机名、系统帐户名)
  * [对恶意软件Dridex的流量分析 - FreeBuf互联网安全新媒体平台](https://www.freebuf.com/articles/es/195832.html)
  * 通过HTTP中的User-Agent中Windows NT字符串代表不同版本的Microsoft Windows：
    * Windows NT 5.1：Windows XP
    * Windows NT 6.0：Windows Vista
    * Windows NT 6.1：Windows 7
    * Windows NT 6.2：Windows 8
    * Windows NT 6.3：Windows 8.1
    * Windows NT 10.0：Windows 10
