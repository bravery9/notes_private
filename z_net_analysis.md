### 流量分析

流量分析(traffic analysis)：根据"流量"发现 恶意软件特征:mining/APT/..malware

分析工具
* Wireshark
* Security Onion

[rshipp/awesome-malware-analysis: A curated list of awesome malware analysis tools and resources.](https://github.com/rshipp/awesome-malware-analysis)

### Wireshark筛选

* 常用筛选示例
  * http GET数据包`http.request.method==GET`
  * 关注某主机 发到 / 来自 10.3.1.1 的数据包`host 10.3.1.1`
  
  
* 逻辑条件组合(适用于对各种筛选条件进行逻辑组合)
  * 与 `and`
  * 或 `or`
  * 非 `!(表达式)` `not(表达式)`
  * 等于`eq` `==`
* 筛选条件 - ip地址
  * 目的ip地址 `ip.dst==192.168.101.8` `dst host 192.168.101.8`
  * 源ip地址`ip.src==1.1.1.1` `src host 1.1.1.1`
  * 发到 / 来自 10.3.1.1 的数据包 `host 10.3.1.1`
  * 发到 / 来自所有解析为 www.1.com 的 IP 地址的数据包 `host www.1.com`
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

### Wireshark插件

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
