### 简介

Cross-Site Request Forgery(CSRF)，跨站请求伪造攻击。
缩写为 CSRF/XSRF，也被称为one click attack/session riding

原理:CSRF通过构造get/post等请求，使已登录用户在不知情的情况下执行某种动作。

危害:CSRF攻击主要用来以受害者victim的身份发送请求，让后端执行增删改操作(单一的CSRF攻击，攻击者看不到CSRF发出的请求所对应的响应结果,难以回显查看数据)。

常见攻击场景:转账过程、新建账号、修改密码、账号信息设置、社交网站中的CSRF"蠕虫"实现连环的攻击 等。

### 分类及攻击方式

* http method - get请求
  * `http://csrfurl.com/payto?name=hacker&moneynumb=100`
  * 攻击:设法让victim的浏览器发出该get请求
     * 社工诱骗 - 使victim点击该url. 或 使victim访问第三方网站通过ajax跨域访问该url.
     * 漏洞联合 - 无交互地利用CSRF漏洞
       * XSS
       * html注入 `<img src="http://csrfurl.com/payto?name=hacker&moneynumb=100">`
* http method - other
  * POST 请求的参数在post body中:`name=hacker&moneynumb=100`
  * 攻击:设法让victim的浏览器发出该post请求
    * 转变为get更容易攻击 (可尝试将post请求改成get形式的请求 如果可以正常响应 则可以使用get请求进行csrf攻击)
    * 社工诱骗victim点击该url
    * 漏洞联合 - 无交互地利用CSRF漏洞
      * XSS - 构造并发送该Post请求

### 测试antiCSRF机制

相同请求重放 - 即可判断token是否为每次动态生成 仅一次性有效 用后失效
完全删除CSRFtoken值令其为空 - 即可判断后端是否验证token是否有效
修改CSRFtoken值中一个字符 - 即可判断后端验证机制是否严谨
HTTP方法由POST转换为GET - 即可判断后端验证机制是否严谨

### 防御方法

* web框架 - 启动框架中成熟的CSRF防御功能
* CSRF_TOKEN - 如`One-time Token`其位置不能在Cookie中 需要额外的HTTP请求header标明 如`X-CSRF-token:xxxx`
* 二次验证 安全但有些影响用户体验（适用于仅对敏感功能处进行防御）
  * CAPTCHA 增加验证码机制进行确认
  * 再次输入密码确认
  * 手机验证码确认
* 校验来源域名 referer/Origin 这种办法即使实现严格且无误 也是不推荐的 因为有固有缺陷:
  * 1.只能防御从"不被信任"的域发起的伪造的http请求（如果 **父、子、兄弟域名** 或 **CORS中被信任的域名** 有XSS漏洞 配合构造一个伪造请求 此时referer和Origin的值都是被信任的域 此时“校验来源域名”无法防御）
  * 2.正常业务如果有302跳转 不携带Origin

参考OWASP [Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.md](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.md)
