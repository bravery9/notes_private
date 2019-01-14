### 简介

Cross-Site Request Forgery(CSRF)，跨站请求伪造攻击。
缩写为 CSRF/XSRF，也被称为one click attack/session riding

原理:CSRF通过构造get/post等请求，使已登录用户在不知情的情况下执行某种动作。

危害:CSRF攻击主要用来以受害者victim的身份发送请求，让后端执行增删改操作(单一的CSRF攻击，攻击者看不到CSRF发出的请求所对应的响应结果,难以回显查看数据)。
在社交网站中还可能产生CSRF蠕虫导致连环的攻击。

### 分类及攻击方式

* http method - get请求
  * `http://pay.com/to?name=hacker&moneynumb=100`
  * 攻击:设法让victim的浏览器发出该get请求(社工诱骗访问该url等方法)
* http method - other
  * POST 请求的参数在post body中:`name=hacker&moneynumb=100`
  * 攻击:设法让victim的浏览器发出该post请求(社工诱骗访问含有js的页面发出该post请求、其他漏洞如XSS构造并发送该请求等方法)


常见攻击场景:转账过程、新建账号、修改密码、账号信息设置 等

### 防御方法

* web框架 - 启动CSRF防御功能
* CSRF_TOKEN 如`One-time Token`
* CAPTCHA 在表单里增加验证码并后端首先判断验证码是否正确（适用于仅对敏感功能处的修复）
* 验证来源域名 referer/Origin 这种办法有缺陷,只能防御"不被信任"的域 发起的CSRF攻击请求
  * 如果来自 **父、子、兄弟域名** 或 **CORS中被信任的域名** 的XSS漏洞构造产生了一个CSRF攻击请求 此时referer和Origin的值都是被信任的域 此时该方法无法防御

[Cross-Site Request Forgery (CSRF) Prevention Cheat Sheet - OWASP](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet)
