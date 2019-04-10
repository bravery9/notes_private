### 简介

Cross-Site Request Forgery(CSRF)，跨站请求伪造攻击。

缩写为 CSRF/XSRF，也被称为one click attack/session riding

### 原理

"借刀杀人"

CSRF通过构造get/post等请求，设法使已登录用户victim不知情的情况下，以受害者victim的身份发出"非其主观意愿的"HTTP请求。

如通过"点击链接"等方式，以victim的身份将请求发给web后端，让后端执行增删改操作。

* CSRF局限性:单一的CSRF攻击无法回显，即攻击者是无法查看到该伪造请求的响应结果。
* CSRF扩展性:可联合其他漏洞(如XSS)进行攻击最大化。


### CSRF分类+对应攻击方式

* CSRF类型 - 伪造GET请求
  * 构造请求 `http://csrfurl.com/payto?name=hacker&moneynumb=100`
  * 攻击思路 设法让victim的浏览器发出该GET请求
     * 社工诱骗
       * 直接 使victim点击该url发出GET请求
       * 间接 使victim访问第三方网站A 通过A的前端ajax实现跨域 访问该url发出GET请求
     * 漏洞联合 - 无交互地利用CSRF漏洞
       * XSS + CSRF 构造并发送GET请求
       * html注入 实现CSRF `<img src="http://csrfurl.com/payto?name=hacker&moneynumb=100">`
* CSRF类型 - 伪造POST请求
  * 构造请求 注意POST请求的参数在请求的body中 `name=hacker&moneynumb=100`
  * 攻击思路 设法让victim的浏览器发出该POST请求
    * 转变为Get更容易攻击 (可尝试将post请求改成get形式的请求 如果可以正常响应 则可以使用get请求进行csrf攻击)
    * 社工诱骗
      * 间接 使victim访问第三方网站A 通过A的前端ajax实现跨域 访问该url发出POST请求
    * 漏洞联合 - 无交互地利用CSRF漏洞
      * XSS + CSRF  构造并发送该POST请求

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
* CSRF_TOKEN - 如`One-time Token`其位置不能在Cookie中 需要额外的HTTP请求header标明 如`X-CSRF-token:xxxx`
* 二次验证 安全但影响用户体验（适用于仅对敏感功能处进行防御）
  * CAPTCHA 增加验证码机制
  * 再次输入密码
  * 手机验证码
* 后端Set-Cookie使用SameSite属性 (由Google引入的用于缓解CSRF攻击的cookie属性 其值为Strict或Lax)
  * 如`Set-Cookie: JSESSIONID=xxx; SameSite=Strict`则从其他站对目标站web后端发起的http请求(CSRF) 不会携带`JSESSIONID`这一Cookie字段
* 【不推荐】通过`Referer/Origin`校验来源域名
  * 缺陷1.只能防御从"不被信任"的域发起的伪造的http请求（如果 **父、子、兄弟域名** 或 **CORS中被信任的域名** 有XSS漏洞 配合构造一个伪造请求 此时referer和Origin的值都是被信任的域 此时“校验来源域名”无法防御）
  * 缺陷2.正常业务如果有302跳转 不携带Origin

参考OWASP [Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.md](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.md)
