### WAF简介

[Web application firewall](https://en.wikipedia.org/wiki/Web_application_firewall)

* Appliance - 设备 硬件WAF
  * Barracuda Networks WAF
  * Citrix Netscaler Application Firewall
  * F5 Big-IP ASM
  * Fortinet FortiWeb
  * Imperva SecureSphere
  * Penta Security WAPPLES
  * Radware AppWall
  * Sophos XG Firewall
* Cloud - 云WAF
  * Alibaba Cloud
  * Amazon Web Services AWS WAF
  * Cloudbric
  * Cloudflare
  * F5 Silverline
  * Fastly
  * Imperva Incapsula
  * Radware
* Open-source 开源WAF
  * ModSecurity

### WEB应用-安全部署架构

```
高防IP（DDoS防护） ->  CDN（静态资源加速） -> Web应用防火墙（中间层，应用层防护） -> 源站（ECS/SLB/VPC/IDC…）
```

缺省任何产品时顺序不变 如:

缺少WAF时的架构为：`高防IP -> CDN -> ECS`

缺少CDN时的架构为：`高防IP -> WAF -> ECS`

**具体配置步骤**:
* 1、配置Web应用防火墙，在源站IP中填写SLB公网IP、ECS公网IP或本地服务器IP，并在"是否已使用了高防、CDN、云加速等代理？" 选项中选择是。
  * 该步骤得到了 WAF提供的`CNAME`
* 2、配置CDN（如果未启用CDN则跳过此步骤）将在CDN中配置的源站，改为WAF提供的`CNAME`
  * 该步骤得到了 CDN提供的`CNAME`
* 3、配置DDoS高防IP，在源站域名中填入CDN提供的CNAME，或者（未启用CDN，高防IP直接回源至WAF情况下）Web应用防火墙提供的CNAME。
  * 该步骤得到了 高防IP服务提供的`CNAME`
* 4、修改DNS解析的配置，将域名解析指向 高防IP服务提供的`CNAME`
  * 配置完成

### 基础概念

接入WAF后，WAF代理了公网所有的客户端请求(过滤访问流量)，将过滤后的访问流量导向源站业务端口

所以源站业务端口收到的访问流量都是来自WAF的IP(而真实的客户端IP地址在HTTP请求头部的X-Forwarded-For字段中)

其中"WAF的源IP"就是回源IP地址(Return Source IP Address)

在源站上正确设置"回源IP防护" 以避免攻击者直接攻击源站IP 策略如下:

1.允许WAF的IP段访问源站的业务端口(80/443)
```
规则方向：入方向
授权策略：允许
协议类型：TCP
授权类型：地址段访问
端口范围：80/443
授权对象：所有Web应用防火墙回源IP段
优先级：1
```

2.禁止公网IP访问源站的业务端口(80/443)
```
规则方向：入方向
授权策略：拒绝
协议类型：TCP
端口范围：80/443
授权类型：地址段访问
授权对象：0.0.0.0/0
优先级：100
```

此时则只有先经过WAF，从WAF出口的流量才可以访问到源站的业务端口。

### WAF/CDN 根本绕过方式 - 攻击源站IP

* 针对没有设置"回源IP防护"的站点 可找到源站IP 直接攻击
  * 找到源站IP - 直接对源站的真实ip发起请求,流量不经过WAF/CDN
    * 方式1 DNS history records - 该域名DNS解析的历史记录中的A记录 可能是源站IP. 参考工具[vincentcox/bypass-firewalls-by-DNS-history](https://github.com/vincentcox/bypass-firewalls-by-DNS-history)
    * 方式2 查找相关域名 如子域名、该公司其他域名 的IP和历史IP
    * 方式3 与其他服务(非web服务)进行交互 如邮件服务可能获取到源站IP
* 修复方案: 在源站上正确设置"回源IP防护" 以避免攻击者直接攻击源站IP 策略如下
  * 1.允许WAF的IP段访问源站的业务端口(80/443)
  * 2.禁止公网IP访问源站的业务端口(80/443)

### WAF规则的绕过

[Ekultek/WhatWaf: Detect and bypass web application firewalls and protection systems](https://github.com/Ekultek/WhatWaf)
