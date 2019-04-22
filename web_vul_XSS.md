### 简介

跨站脚本攻击(Cross Site Scripting)

### 分类

* 反射型(Reflected XSS)
  * 原理 - 后端未严格过滤,导致HTTP response中的部分内容由HTTP request中的参数值(XSSpayload)控制
  * XSSpayload位置 - 必然每次都在HTTP request 和 HTTP response中
  * 检测方法1 - 自动化方法(脚本) - 构造并发送HTTP request(含有XSSpayload),并查看HTTP response的内容，即可确定是否存在Reflected XSS
  * 检测方法2 - 浏览器(headless) - 构造并发送HTTP request(含有XSSpayload),根据headless调试进行判断，如果执行成功则存在DOM Based XSS
* 存储型(Stored XSS)
  * 原理 - 初次攻击时发送的HTTP request使后端将XSSpayload存储到数据库中,再次访问页面时后端将HTTP response(含有XSSpayload)返回给浏览器前端，XSSpayload执行
  * XSSpayload位置 - 初次攻击时XSSpayload必然会在HTTP request中(后端将含有XSSpayload的数据保存到数据库) 初次的HTTP response中未必有XSSpayload
  * 检测方法1 - 自动化方法(脚本)
    * 存储 - 构造并发送HTTP request(含有XSSpayload)完成第一次输入
    * 判断 - 通过脚本等自动化方法访问可能解析该XSSpayload的URL，根据HTTP response的内容如果含有XSSpayload则存在Stored XSS
  * 检测方法2  - 浏览器(headless)
    * 存储 - 构造并发送HTTP request(含有XSSpayload)完成第一次输入
    * 判断 - 通过浏览器(headless)访问若干个可能解析该XSSpayload的页面，根据headless调试进行判断，如果确认执行成功则存在Stored XSS
* DOM型(DOM Based XSS)
  * 原理:"DOM型XSS"依赖DOM解析过程，所以必须手工构造XSSpayload并使浏览器端解析DOM，才能执行XSSpayload. 检测它也必须解析DOM.
  * XSSpayload位置: 根据漏洞原理可看出 "DOM型XSS"与HTTP的请求响应没有直接关系. (所以HTTP request 与 HTTP response的内容中一定没有XSSpayload)
    * 检测方法1  - 浏览器&开发者工具 - 查看经过js解析过的html代码，输入构造的数据并查看其前端解析，构造的payload执行成功则存在DOM Based XSS
    * 检测方法2  - 使用浏览器(headless) - 根据该页面的前端代码设计缺陷，通常在浏览器(headless)的URL输入框构造XSSpayload并访问，使前端解析并执行该XSSpayload，根据headless调试进行判断，如果确认执行成功则存在DOM Based XSS

### DOM

* DOM (Document Object Model)  html中每一个元素都是"节点":
  * 文档是一个文档节点
  * 所有的HTML元素都是元素节点
  * 所有的HTML属性都是属性节点
  * 文本插入到HTML元素是文本节点
  * 注释是注释节点


浏览器访问 https://cn.bing.com/search?q=location#abc

进入开发者模式 用户输入可控的参数如下(如果处理不当可能会有"DOM型XSS"):

```
document.URL
document.documentURI
document.baseURI
location
location.href
location.search
location.hash
location.pathname
document.cookie
document.referrer
window.name
history.pushState()
history.replaceState()
localStorage
sessionStorage
```

实例：
```
得到完整URL
location.href
"https://cn.bing.com/search?q=location#abc"

返回一个URL的锚部分 即 #号及之后的内容(如果URL中没有#号则返回空)
location.hash
"#abc"

返回URL路径名 即第一个/号到?号之间的内容(如果URL中没有?号则是/号到#号之间的内容 如果URL中没有#号则是到URL结束之间的内容)
location.pathname
"/search"

返回一个URL的查询部分 得到`?`之后到`#`之前的内容(如果URL中没有#号则是到URL结束之间的内容)
location.search
"?q=location"
```


### DOM Based XSS - 实例1 恐怖的innerHTML

DOM Based XSS场景 只需要前端即可实现

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


打开1.html 通过浏览器的开发者工具 查看经过DOM解析(javascript执行后)的页面代码:

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

### BypassXSS

* [s0md3v/AwesomeXSS](https://github.com/s0md3v/AwesomeXSS)
* [MyPapers/Bypassing-XSS-detection-mechanisms](https://github.com/s0md3v/MyPapers/tree/master/Bypassing-XSS-detection-mechanisms)

### 利用方式

XSS可以插入Javascript代码，所以JavaScript能实现的任何功能都是XSS的利用方式。

XSS proxy - 与XSS受害者的浏览器实时交互.  工具 [JShell](https://github.com/s0md3v/JShell)、[xsshell](https://github.com/raz-varren/xsshell)、[JSShell](https://github.com/Den1al/JSShell)
举例如下
* 探测内网(利用实时通信标准WebRTC 获取存活主机ip列表 甚至部分端口 能打到内网则类似SSRF漏洞 可对内网发起许多攻击... 如XSS可以利用redis未授权Getshell)
* 盗用身份 进行读写(读取账户特有的信息/执行账户特有的操作)
  * 管理员 发起高权限操作 - 创建新账号 修改密码...
  * 普通用户 发起普通用户权限操作 - 评论、发帖、支付转账、刷量...
* 窃取表单凭据 - 类似键盘记录 记录或读取表单输入的内容
* 构造钓鱼页面 - 窃取用户及管理员其他的凭证
* 漏洞联合 - 使用XSS无交互地利用CSRF漏洞. 有的anti-CSRF机制只判断Referer的值(自身/兄弟/父子域名 则正常响应) 如果这些站有某处存在XSS则可无交互地利用CSRF漏洞
* XSS蠕虫 - 在社交网站上可创建蠕虫式的XSS攻击 传播速度极快 影响极大
* 获取前端代码 - 如管理员后台系统 修改密码处的html代码中有对应的字段名 可根据代码构造请求 以实现新增或修改管理员账号密码
* DOS攻击 - 自动注销 让用户无法登录 严重影响业务使用
* DDoS攻击 - 对其他站点进行应用层DDoS攻击 如持续发送HTTP请求
* 传播非法内容 - 跳转或直接修改页面内容为非法内容. 如 广告 诋毁 等
* 使浏览器下载文件 - 结合社工方法欺骗用户 使其打开有危害的程序
* 挖矿
* 桌面应用XSStoRCE - 基于node.js的桌面应用框架(如Electron)都可能存在XSStoRCE 
* ...

### SDL - 防御与修复方案

* 需求与设计阶段(了解产品背景和技术架构 并给出相应的建议)
  * 建议使用成熟的前端框架 `Vue` `React`
  * 建议使用内容安全策略(Content Security Policy) - [Content Security Policy CSP Reference & Examples](https://content-security-policy.com/) CSP本质是白名单 (实测 Google开发的CSP安全评估工具[CSP Evaluator](https://csp-evaluator.withgoogle.com/))
  * API接口 - 建议显式规定response的MIME类型 即 `Content-Type` header 的值. 如json格式 则设置为`Content-type: application/json`
  * Cookie安全设计参考 - [Security Cookies Whitepaper](https://www.netsparker.com/security-cookies-whitepaper//?utm_source=twitter.com&utm_medium=social&utm_content=security+cookies+whitepaper&utm_campaign=netsparker+social+media)
    * 如在`Set-Cookie: `中增加  `; secure` 仅允许HTTPS协议中传输cookie
    * 如在`Set-Cookie: `中增加  `; HttpOnly` 当客户端脚本代码读取cookie 则浏览器返回一个空字符串
* 测试阶段
  * 基本防御方案：为了保证输入的原生态，通常在输出点做实体编码、必要的过滤。
  * XSS详细防御[Cross_Site_Scripting_Prevention_Cheat_Sheet.md](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.md)
  * DOM_based_XSS详细防御[DOM_based_XSS_Prevention_Cheat_Sheet.md](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.md)
    * 前端增加html实体编码处理 将`输出`编码为只能用来显示的`Html实体(Html Entity)` `.innerHTML=encodeHTML(<img src=@ onerror=alert(1) />)`
  * API接口 - 显式规定response的MIME类型 即`Content-Type` header 的值. 如json格式 则设置为`Content-type: application/json`

XSS filter:浏览器自带的XSS防御机制(被动防御),搜索绕过方法 `chrome xss filter bypass`
