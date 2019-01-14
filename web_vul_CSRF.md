### 简介

Cross-Site Request Forgery(CSRF)，跨站请求伪造攻击。
缩写为 CSRF/XSRF，也被称为one click attack/session riding

### 分类及攻击方式

*  http method - get请求
  * `http://pay.com/to?name=hacker&moneynumb=100`
  * 攻击:设法让victim发出该get请求(社工诱骗访问该url等方法)
* http method - other
  * POST 请求的参数在post body中:`name=hacker&moneynumb=100`
  * 攻击:设法让victim发出该post请求(社工诱骗访问含有js的页面发出该post请求、其他漏洞如XSS构造并发送该请求等方法)

危害:
* 未授权操作(数据增删改操作)
* CSRF蠕虫(社交网络)

所有敏感操作都应防御CSRF(转账过程、数据增删改操作、账号信息设置 等)

### 防御方法

* web框架 - 启动CSRF防御
* CSRF_TOKEN
* 验证来源域名 referer/Origin 这种办法有缺陷,只能防御"不被信任"的域 发起的CSRF攻击请求
  * 如果来自 **父、子、兄弟域名** 或 **CORS中被信任的域名** 的XSS漏洞构造产生了一个CSRF攻击请求 此时referer和Origin的值都是被信任的域 此时该方法无法防御
