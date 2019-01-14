### 简介

Cross-Site Request Forgery(CSRF)，跨站请求伪造攻击。
缩写为 CSRF/XSRF，也被称为one click attack/session riding

### 分类及攻击方式

* CSRF漏洞 - http method - get请求
  * `http://pay.com/to?name=hacker&moneynumb=100`设法让victim发出该get请求(社工诱骗访问该url等方法)
* CSRF漏洞 - http method - other
  * POST 参数写在post body中`name=hacker&moneynumb=100` 设法让victim发出该post请求(社工诱骗访问含有js的页面发出该post请求、其他漏洞如XSS构造并发送该请求等方法)

### 防御

* web框架 - 启动CSRF防御
* CSRF_TOKEN
* 验证来源域名 referer/Origin
  * 这种办法只能防御来自"非同域"发起的CSRF攻击请求，如果来自 相关域名(父、子域名) 的XSS漏洞构造产生了一个CSRF攻击请求 referer有效 此时该方法无法防御
