### WAF简介

[Web application firewall](https://en.wikipedia.org/wiki/Web_application_firewall)

* Appliance - 设备
  * Barracuda Networks WAF
  * Citrix Netscaler Application Firewall
  * F5 Big-IP ASM
  * Fortinet FortiWeb
  * Imperva SecureSphere
  * Penta Security WAPPLES
  * Radware AppWall
  * Sophos XG Firewall
* Cloud - 云
  * Alibaba Cloud
  * Amazon Web Services AWS WAF
  * Cloudbric
  * Cloudflare
  * F5 Silverline
  * Fastly
  * Imperva Incapsula
  * Radware
* Open-source 开源
  * ModSecurity

### WEB应用安全部署架构 - 最佳方式

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

### WAF/CDN 根本绕过方式

找到真实IP - 直接对源站的真实ip发起请求,流量不经过WAF/CDN

* DNS history records - 该域名DNS解析的历史记录中的A记录 可能是真实ip
  * 工具[vincentcox/bypass-firewalls-by-DNS-history](https://github.com/vincentcox/bypass-firewalls-by-DNS-history)
