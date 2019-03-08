### 简介

跨站脚本攻击(Cross Site Scripting)

### 分类

* 反射型(Reflected XSS)
  * XSSpayload位置:必然每次都在HTTP request 和 HTTP response中
  * 原理:后端未严格过滤,导致HTTP response中的部分内容由HTTP request中的参数值(XSSpayload)控制
* 存储型(Stored XSS)
  * XSSpayload位置:初次攻击时XSSpayload必然会在HTTP request中(后端将含有XSSpayload的数据保存到数据库) 初次的HTTP response中未必有XSSpayload
  * 原理:XSSpayload已经存储到数据库中,再次被读取时将XSSpayload输出到浏览器前端触发。存储型XSS通常影响更大
* DOM型(DOM Based XSS)
  * XSSpayload位置:只发生在浏览器端 (一定不会在HTTP request 和 HTTP response中)
  * 原理:手工构造XSSpayload并由 浏览器的DOM解析过程 触发(该触发过程与http的请求响应没有直接关系 HTTP request 与HTTP response 一定看不到payload)

### DOM Based XSS - 实例

构造DOM Based XSS场景 只需要前端即可

1.html
```
<a id=a></a>
<title id=b></title>
<div id=c></div>

<script>
document.getElementById("a").innerHTML="<img src=@ onerror=alert(1) />";
document.getElementById("b").innerHTML="<img src=@ onerror=alert(2) />";
document.getElementById("c").innerHTML="<img src=@ onerror=alert(3) />";
</script>
```

打开1.html 经过浏览器解析得到:
```
<html><head></head>
<body>
<a id="a"><img src="@" onerror="alert(1)"></a>
<title id="b">&lt;img src=@ onerror=alert(2) /&gt;</title>
<div id="c"><img src="@" onerror="alert(3)"></div>

<script>
document.getElementById("a").innerHTML="<img src=@ onerror=alert(1) />";
document.getElementById("b").innerHTML="<img src=@ onerror=alert(2) />";
document.getElementById("c").innerHTML="<img src=@ onerror=alert(3) />";
</script>
</body>
</html>
```
可发现会弹出`alert(3)` `alert(1)`(无顺序)

没有`alert(2)`

因为使用`.innerHTML`写入以下这些HTML标签中时，不会解析`Html实体(Html Entity)` :

如它们不会将 `&lt;` 解析为`<`
```
<textarea>
<title>
<iframe>
<noscript>
<noframes>
<xmp>
<plaintext>
...
```

### 利用方式

XSS可以插入Javascript代码，所以JavaScript能实现的任何功能都是XSS的利用方式。

* 盗用身份 执行操作
  * 管理员 发起高权限操作 - 创建新账号 修改密码...
  * 普通用户 发起普通用户权限操作 - 评论、发帖、支付转账、刷量...
* 构造钓鱼页面 - 窃取用户及管理员其他的凭证
* 影响浏览器客户端行为 - 挖矿等
* DOS攻击 - 自动注销 让用户无法登录 严重影响业务使用
* 访问量大的网站 - 利用XSS对其他站点发起请求 DDoS攻击
* 信息搜集 - 获取管理员网页前端代码
* SSRF - 对内网发起攻击
* 传播非法内容 - 低俗 诋毁 广告 等

### SDL防御与修复

防御/修复方案
* 需求与设计阶段(了解产品背景和技术架构 并给出相应的建议)
  * 建议使用成熟的前端框架 `Vue` `React`
  * 建议使用内容安全策略(Content Security Policy) - [Content Security Policy CSP Reference & Examples](https://content-security-policy.com/) CSP本质是白名单
  * API接口 - 建议显式规定response的MIME类型 即 `Content-Type` header 的值. 如json格式 则设置为`Content-type: application/json`
  * 在`Set-Cookie: `中增加
    * `; secure` 仅允许HTTPS协议中传输cookie
    * `; HttpOnly` 当客户端脚本代码读取cookie 则浏览器返回一个空字符串
* 测试阶段
  * 基本防御方案：为了保证输入的原生态，通常在输出点做实体编码、必要的过滤。
  * XSS详细防御[Cross_Site_Scripting_Prevention_Cheat_Sheet.md](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.md)
  * DOM_based_XSS详细防御[DOM_based_XSS_Prevention_Cheat_Sheet.md](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.md)
    * 前端增加html实体编码处理 将`输出`编码为只能用来显示的`Html实体(Html Entity)` `.innerHTML=encodeHTML(<img src=@ onerror=alert(1) />)`
  * API接口 - 显式规定response的MIME类型 即`Content-Type` header 的值. 如json格式 则设置为`Content-type: application/json`

XSS filter:浏览器自带的XSS防御机制(被动防御),搜索绕过方法 `chrome xss filter bypass`
