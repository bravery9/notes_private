### 简介

跨站脚本攻击(Cross Site Scripting)

[HackerOne上已公开的各种XSS漏洞](https://hackerone.com/hacktivity?order_direction=DESC&order_field=popular&filter=type%3Apublic&querystring=XSS)

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

### 基础知识 - DOM

* DOM (Document Object Model)  html中每一个元素都是"节点":
  * 文档是一个文档节点
  * 所有的HTML元素都是元素节点
  * 所有的HTML属性都是属性节点
  * 文本插入到HTML元素是文本节点
  * 注释是注释节点

### DOM-XSS详细分析

输入 -> 触发

#### 1.输入源

参考Google Code [domxsswiki](https://github.com/wisec/domxsswiki)

外部可控参数 即用户输入能够影响以下参数(如果处理不当可能会有"DOM型XSS")
```
# URL
document.URL
document.documentURI
document.baseURI

# URL各部分
location
location.href
location.search
location.hash
location.pathname

# 其他
document.cookie
document.referrer
window.name
window.postMessage
history.pushState()
history.replaceState()
localStorage
sessionStorage
```

```
实例：浏览器开启开发者模式 访问 https://cn.bing.com/search?q=location#abc

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

实际测试发现

* 输入:chrome浏览器的`document.URL`是经过url编码的字符.即`location.pathname` `location.search` `location.hash`都是经过URL编码的字符.
* 处理:可以使用`xx.com/pathname#javascript:alert(1)`不需要进行URL解码.
* 触发:HTML标签的属性 有些属性支持 `javascipt:` 伪协议
   * 1.chrome测试通过 - 写到a标签的`href`属性 点击触发. `<a href=javascript:alert(1)>click</a>`
   * ...


#### 2.触发

可能导致DOM based XSS漏洞的函数

```
eval
Function
setTimeout
setInterval
setImmediate
execScript
crypto.generateCRMFRequest
ScriptElement.src
ScriptElement.text
ScriptElement.textContent
ScriptElement.innerText
anyTag.onEventName
document.write
document.writeln
anyElement.innerHTML
Range.createContextualFragment
window.location
document.location
```


#### DOM based XSS 实例1 - 从anyElement.innerHTML触发

* 对某元素内的"HTML代码"的读`outerHTML` 写`innerHTML` 
* 对某元素内的"纯文本"的读 `outerText` 写 `innerText`

```
<div id="test">
   <span style="color:red">test1</span> test2
</div>

//使用 .innerHTML .outerHTML 能够读取/写入元素中的html代码
//在console下测试：

//test.innerHTML
//输出为<span style="color:red">test1</span> test2

//test.innerText
//输出为test1 test2

//test.outerHTML
//输出为<div id="test"><span style="color:red">test1</span> test2</div>
```

可知 `anyElement.innerHTML`可以将html代码写入该元素


构造poc1.html
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


打开poc1.html

通过浏览器的开发者工具 查看经过DOM解析(javascript执行后)的页面代码:
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

发现 弹出`alert(3)` `alert(1)`(无顺序) 而没有`alert(2)`

因为使用`.innerHTML`写进以下这些HTML标签中时，不会解析`Html实体(Html Entity)`  如不会将 `&lt;` 解析为`<` :

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

### XSS绕过方式

BypassXSS
* [s0md3v/AwesomeXSS](https://github.com/s0md3v/AwesomeXSS#awesome-bypassing)
* [MyPapers/Bypassing-XSS-detection-mechanisms](https://github.com/s0md3v/MyPapers/tree/master/Bypassing-XSS-detection-mechanisms)

### XSS利用方式

XSS可以插入Javascript代码，所以JavaScript能实现的任何功能都是XSS的利用方式。

XSS proxy - 与XSS受害者的浏览器实时交互.  工具 [JShell](https://github.com/s0md3v/JShell)、[xsshell](https://github.com/raz-varren/xsshell)、[JSShell](https://github.com/Den1al/JSShell)

举例如下
* 获取Cookie - 如果没有`HttpOnly`则可获取Cookie 从而盗用账户身份(读取账户特有的信息/执行账户特有的操作)
  * 管理员 发起高权限操作 - 创建新账号 修改密码...
  * 普通用户 发起普通用户权限操作 - 评论、发帖、支付转账、刷量...
* 探测内网 - 利用实时通信标准WebRTC 获取存活主机ip列表 甚至端口 进而识别服务、web系统类型与版本（如发现内网confluence系统）
* 攻击内网 - 根据探测结果(或对所有内网ip)发起漏洞利用攻击流量（利用web系统漏洞：confluence系统命令执行等；利用常见服务漏洞：redis未授权Getshell)
* XSStoRCE - 使用node.js作为web后端 或 基于node.js的桌面应用框架(如Electron) 都可能通过XSS实现RCE
* XSS蠕虫 - 在社交网站上可创建蠕虫式的XSS攻击 传播速度极快 影响极大
* 漏洞联合 - 使用XSS绕过CSRF保护机制 无交互地利用CSRF漏洞
   * 1.利用自身域名的XSS漏洞绕过CSRF防御机制 - 有的anti-CSRF机制为后端判断CSRFtoken的值，使用JavaScript找到CSRFtoken参数值并构造出"合法的"GET/POST请求 全程不存在跨域问题
   * 2.利用自身/兄弟/父子域名的XSS漏洞绕过CSRF防御机制 - 有的anti-CSRF机制是后端通过判断Referer的值，如果Referer的值 是自身/兄弟/父子域名下的url 就是"合法"请求
* 漏洞联合 - 通过已有的CSRF漏洞 利用self-XSS漏洞(变废为宝)
  * 利用过程 - 事实上self-XSS漏洞无法直接使对方触发，然而通过已有的CSRF漏洞构造"触发该self-XSS漏洞的"请求，对方触发CSRF漏洞即触发XSS漏洞
* 键盘记录 - 记录所有按键
* 强制下载文件(渗透内网) - 通过社工方法进行欺骗 编造理由 使其下载并打开恶意程序("xx程序必须更新才能使用")
* 构造钓鱼页面 - 通过社工方法进行欺骗 编造理由 窃取各种凭证("WiFi固件更新，请重新输入您的凭据以进行身份验证" "重新登录域账号")
* 污染页面内容 - 直接修改页面内容为恶意内容. 如 广告(利用存储型XSS漏洞实现Ad-Jacking) 诋毁 等
* 获取表单输入 - 窃取表单输入框的内容(如口令输入框)
* 获取前端代码 - 如管理员后台系统的前端代码中 有对应的字段名 可根据代码构造请求 以构造请求 新增管理员账号
* 重定向 - Redirecting
* 虚拟币挖矿 - 利用javascript实现Crypto Mining
* DOS攻击 - 利用javascript发起注销请求 使用户cookie失效从而无法登录 严重影响业务
* DDoS攻击 - 对其他站点进行应用层DDoS攻击 如持续发送HTTP请求
* 获取浏览器信息 - 获取浏览器名称及版本 已安装的插件及版本
* 获取系统信息 - 系统类型 语言 屏幕分辨率
* 获取web storage数据 - (HTML5) 通过`window.localStorage()`和`window.webStorage()`访问该站点的`web storage`数据 
* 获取网页截图 - (HTML5)
* 获取录音数据 - (HTML5) 需要授权 Recording Audio
* 获取摄像数据 - (HTML5) 需要授权 webcam
* 获取地理位置 - (HTML5) 需要授权 访问受害者的Geo-location
* 结合具体功能 - 通过“动态生成PDF”功能的XSS 通过XMLHTTPRequest对`file://`协议发起异步请求 读取本地文件内容 [Local File Read via XSS in Dynamically Generated PDF](https://www.noob.ninja/2017/11/local-file-read-via-xss-in-dynamically.html)
* ...

### SDL - 防御与修复方案

* 需求与设计阶段(了解产品背景和技术架构 并给出相应的建议)
  * 建议使用成熟的现代javascript框架 它们内置了非常好的XSS保护 注意规范使用
    * ReactJS - 禁止使用[`dangerouslySetInnerHTML`](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml)函数
    * Angular (2+) - 禁止使用这样的函数[`bypassSecurityTrust{something}`](https://angular.io/guide/security#bypass-security-apis) (如 `bypassSecurityTrustHtml`, `bypassSecurityTrustStyle`, 等).  
    * Angular (2+) - 构建Angular模板必须使用`-prod`参数  即`ng build --prod`  以避免模板注入(template injection)
  * 建议使用内容安全策略(Content Security Policy)
    * CSP本质 - 浏览器端的白名单机制(为Web应用的客户端资源创建源白名单，如JavaScript, CSS, images等)
    * 设置CSP - Response Header 如`Content-Security-Policy: default-src: 'self'; script-src: 'self' static.domain.tld` 更多参考[Content Security Policy CSP Reference & Examples](https://content-security-policy.com/)
    * CSP安全评估工具 - [CSP Evaluator](https://csp-evaluator.withgoogle.com/)由Google开发
  * 建议使用 自动转义模版系统(Auto-Escaping Template System) - Web应用框架的自动上下文转义功能(automatic contextual escaping functionality)
    * 如 [AngularJS strict contextual escaping](https://docs.angularjs.org/api/ng/service/$sce)
    * 如 [Go Templates](https://golang.org/pkg/html/template/)
  * 建议使用X-XSS-Protection响应头 - 现代浏览器默认开启 使用该Response Header使浏览器为当前页面强制重新开启XSS filter(如果用户禁用了XSS filter)
  * API接口 - 显式规定response的MIME类型 即`Content-Type` header 的值. 如json格式 则设置为`Content-type: application/json`
  * Cookie安全设计参考 - [Security Cookies Whitepaper](https://www.netsparker.com/security-cookies-whitepaper//?utm_source=twitter.com&utm_medium=social&utm_content=security+cookies+whitepaper&utm_campaign=netsparker+social+media)
    * 如在`Set-Cookie: `中增加  `; secure` 仅允许HTTPS协议中传输cookie
    * 如在`Set-Cookie: `中增加  `; HttpOnly` 当客户端脚本代码读取cookie 则浏览器返回一个空字符串


#### XSS防御方法

> 参考[Cross_Site_Scripting_Prevention_Cheat_Sheet.md](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.md)
>[DOM_based_XSS_Prevention_Cheat_Sheet.md](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.md)

* HTML实体编码(HTML entity encoding) 用来防御XSS是远远不够的!!
  * 适用情况1. 将"不受信任的数据"放在 HTML文档中时(the body of the HTML document)，如`<div>`标签内，需要做HTML实体编码
  * 适用情况2. 将"不受信任的数据"放在 HTML属性中时，需要做HTML实体编码(开发人员最好给HTML标签的属性的值,前后都加上引号)
  * 无效情况1. 将"不受信任的数据"放在 `<script>`标签内的任何位置时，不用做HTML实体编码，因为对防御XSS无效!!
  * 无效情况2. 将"不受信任的数据"放在 事件处理属性(event handler attribute)中时，如`onmouseover`，不用做HTML实体编码，因为对防御XSS无效!!
  * 无效情况3. 将"不受信任的数据"放在 `CSS`中时，不用做HTML实体编码，因为对防御XSS无效!!
  * 无效情况4. 将"不受信任的数据"放在 `URL`中时，不用做HTML实体编码，因为对防御XSS无效!!

* 防御XSS的安全编码库(Security Encoding Library)
  * [OWASP Java Encoder Project](https://www.owasp.org/index.php/OWASP_Java_Encoder_Project) - 内含了各种位置下的正确编码处理函数
  

* XSS防御规则#0 - 拒绝所有(deny all) 除了规则#1-5中允许的位置，其他全部禁止放入"不受信任的数据" 如以下位置
  * `<script>...script标签中 永不放入不受信任的数据...</script>`
  * `<!--...HTML注释中 永不放入不受信任的数据...-->`
  * `<div ...HTML标签的属性的名称中 永不放入不受信任的数据...=test />`
  * `<HTML标签名称中永不放入不受信任的数据... href="/test" />`
  * `<style>...CSS中 永不放入不受信任的数据...</style>`
  * 注意 永远禁止接受来自不受信任来源的JavaScript代码并运行它 没有任何防御方法可以解决这种情况 (如 名为`callback`的参数包含JavaScript代码段)



* XSS防御规则#1 - 将"不受信任的数据"放在 HTML元素的Content之前，需要做HTML实体编码
  * 将"不受信任的数据"放在 常见标签中时，需要做HTML实体编码 `div`, `p`, `b`, `td` ...
    * 如 `<body>...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...</body>`
    * 注意 推荐使用十六进制格式的HTML实体 除了XML中重要的5个字符(`&` `<` `>` `"` `'`) 以及正斜杠`/`  见附表1
    * 注意 在其他html上下文中 本规则远远不够 需参考其他规则!!

附表1

|字符|HTML实体|描述|
|:---:|-----|-----|
| & | `&amp;`|XML中重要的5个字符之一|
| < | `&lt;`|XML中重要的5个字符之一|
| > | `&gt;`|XML中重要的5个字符之一|
| " | `&quot;`|XML中重要的5个字符之一|
| ' | `&#x27;`|XML中重要的5个字符之一   更推荐使用`&#x27;`  因为`&apos;`不在HTML规范 而在XML和XHTML规范中|
| / | `&#x2F;`|`/`有助于结束一个HTML实体|


* XSS防御规则#2 - 将"不受信任的数据"放在 HTML元素的常见的属性的值(如`width`,`name`,`value`等)之前，需要做HTML实体编码
  * 反例 属性的值没有用引号包裹 很不安全 使用"能够跳出属性值的字符"跳出属性的值 `[space]` `%` `*` `+` `,` `-` `/` `;` `<` `=` `>` `^` `|` 
  * 正例 所有属性的值都应该被单引号或双引号`"`包裹 并对属性的值做HTML实体编码 `<div attr='...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...'>content`
  * 防御方案 - 必须用引号包裹属性的值,并且属性的值需要做HTML实体编码(用`&#xHH;`转义"除了字母数字字符之外的"所有`ASCII < 256`的字符 即可转义"能够跳出属性值的字符")
  * 注意 本规则不应被用于 复杂的属性(如`href`,`src`,`style`等) 和 任何事件处理属性(如`onmouseover`等) 参考规则#3
* XSS防御规则#3 - 将"不受信任的数据"放在 JavaScript的"数据值"之前,需要做`JavaScript Escape`
  * 针对动态生成的JavaScript代码的情况: `<script>`脚本块 和 事件处理属性(event-handler attributes)
  * 防御方案 - 将"不受信任的数据"放在 JavaScript代码中 只有放在被引号括起来的"数据值"部分并将数据进行HTML实体编码(用`&#xHH;`转义"除了字母数字字符之外的"所有`ASCII < 256`的字符 即可转义"能够跳出属性值的字符")
    * 不能使用`\"`这种可被逃逸的简单的转义(1.运行时引号字符可能被HTML属性解析器首先匹配 2.输入的`\"`可能被转义为`\\"`使双引号逃脱转义)
    * 注意例外 将"不受信任的数据"放在 某些JavaScript函数中 永远不安全!! 即使做了`JavaScript Escape`也不行 如`<script>window.setInterval('...在此处做转义无法防御XSS 仍然会被XSS...');</script>`
  * 正例 传入实参 `<script>alert('...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...')</script>`
  * 正例 变量赋值 `<script>x='...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...'</script>`
  * 正例 事件处理属性`<div onmouseover="x='...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...'"</div>`
* XSS防御规则#3.1 - HTML转义HTML上下文中的JSON值并使用JSON.parse读取数据
  * API接口 - 显式规定response的MIME类型 即`Content-Type` header 的值. 如json格式 则设置为`Content-type: application/json`
  * anti-pattern - 不能直接这么写`<script>var initData = <%= data.to_json %>; </script>` 而必须用以下的编码方式:
    * 编码方式1 JSON serialization - 一个安全的JSON serializer能够使开发人员将`JSON`序列化为"绝对的JavaScript文本字符串"(string of literal JavaScript) 嵌入到HTML的`<script>`中是安全的. 注意 HTML characters 和 JavaScript line terminators 需要被转义.参考 [Yahoo JavaScript Serializer](https://github.com/yahoo/serialize-javascript)
    * 编码方式2 HTML实体编码
      * 数据源(HTML某个元素) - 将`JSON block`作为普通元素放在页面上，标签中的内容是经过HTML实体编码的内容
      * 数据使用方(javascript) - 调用方通过该元素的`.innerHTML`获取标签中的内容

```html
<!-- 数据源(HTML某个元素) -->

<div id="init_data" style="display: none">
 <%= html_escape(data.to_json) %>
</div>
```

```javascript
// 数据使用方(javascript)

// external js file - 读取数据的javascript可以保存在外部文件中，从而更容易实现CSP
var dataElement = document.getElementById('init_data');
// decode and parse the content of the div
var initData = JSON.parse(dataElement.textContent);
// 在JavaScript中直接转义/反转义JSON的另一种方法:JSON服务器端规范化输出(将Response中的JSON数据`<`转换为`\u003c`形式  再返回给浏览器)
```

* XSS防御规则#4 - 将"不受信任的数据"放在 HTML样式属性值(Style Property Values)之前，进行CSS转义并严格验证
  * 防御方案 - 将"不受信任的数据"放在stylesheet或`<style>`标签中时,只能放在属性的值中 (其他位置都不能放 尤其是复杂的属性 如`url`, `behavior`, 和自定义的`-moz-binding`),并用`\HH`转义"除了字母数字字符之外的"所有`ASCII < 256`的字符
    * 注意 不能使用`\"`这种可被逃逸的简单的转义 (1.运行时引号字符可能被HTML属性解析器首先匹配 2.输入的`\"`可能被转义为`\\"`使双引号逃脱转义)
    * 注意 所有属性的值都需要被引号包裹 - 否则很不安全 使用"能够跳出属性值的字符"跳出属性的值 `[space]` `%` `*` `+` `,` `-` `/` `;` `<` `=` `>` `^` `|` 
    * 注意 建议使用严格的CSS编码(CSS encoding)和验证来防止"被引号包裹的属性"和"没被引号包裹的属性"的XSS攻击
    * 注意 在带引号的字符串中的`</style>`也能够闭合样式块(style block)，因为HTML解析器比JavaScript解析器先运行
  * 注意 将"不受信任的数据"放在 某些CSS内容中永远不安全!!即使做CSS Escape 也对防御XSS无效.
    * 1. `URLs`必须保证只能以`http`开头(不能以`javascript`开头)
      * 反例`{ background-url : "javascript:alert(1)"; }  // and all other URLs`
    * 2. 属性的值不能以`expression`开头  因为IE的表达式属性(expression property)允许在此执行JavaScript
      * 反例`css{ text-size: "expression(alert('XSS'))"; }   // only in IE.`


正例
```html
<style>
selector { property : ...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...; }
</style>
```

```html
<style>
selector { property : "...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE..."; }
</style>
```

```html
<!-- 内联(Inline Styles) -->
<span style="property : ...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...">text</span>

<h1 style="color:blue;margin-left:30px;">This is a heading</h1>
```

* 防御规则#5 - 将"不受信任的数据"放在 HTML URL Parameter 之前进行URL转义(URL Escape)
  * 防御方案 - 将"不受信任的数据"放在 HTML的GET参数的值之前，需要进行URL转义(URL Escape):使用`%HH`转义"除了字母数字字符之外的"所有`ASCII < 256`的字符
    * 注意 所有属性的值都需要被引号包裹 - 否则很不安全 使用"能够跳出属性值的字符"跳出属性的值 `[space]` `%` `*` `+` `,` `-` `/` `;` `<` `=` `>` `^` `|` 
    * 注意 URLs中禁止使用`data:` - 因为转义无法禁止攻击者使用`data:`使"数据"跳出URLs变为"代码"
    * 注意 不要使用URL编码对"完整URL"或"相对URL"进行编码
      * 输入验证 - 如果将"不受信任的数据"放在`href`, `src`等基于URL的属性(URL-based attributes)中，需要验证输入数据以确保它没有指向不期望的协议，尤其是`javascript:`
      * 输出编码 - 像其他数据一样，URL的编码应该基于显示内容(输出)  例如`HREF=`中的值是用户驱动的URL 它应该被编码


正例
```html
<a href="http://www.somesite.com?test=...ESCAPE UNTRUSTED DATA BEFORE PUTTING HERE...">link</a >
```

```java
// 输出编码 - 后端对href属性值中的URL进行编码
String userURL = request.getParameter( "userURL" )
boolean isValidURL = Validator.IsValidURL(userURL, 255); 
if (isValidURL) {  
    <a href="<%=encoder.encodeForHTMLAttribute(userURL)%>">link</a>
}
```

#### XSS防御规则总结

| Data Type | Context                                  | Code Sample                                                                                                        | Defense                                                                                                                                                                                        |
|-----------|------------------------------------------|--------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| String    | HTML Body                                |  `<span>UNTRUSTED DATA </span>`                                                                          | HTML Entity Encoding (rule \#1).                                                                                                                                                               |
| String    | Safe HTML Attributes                     | `<input type="text" name="fname" value="UNTRUSTED DATA ">`                                               | Aggressive HTML Entity Encoding (rule \#2), Only place untrusted data into a whitelist of safe attributes (listed below), Strictly validate unsafe attributes such as background, id and name. |
| String    | GET Parameter                            | `<a href="/site/search?value=UNTRUSTED DATA ">clickme</a>`                                               | URL Encoding (rule \#5).                                                                                                                                                                       |
| String    | Untrusted URL in a SRC or HREF attribute | `<a href="UNTRUSTED URL ">clickme</a> <iframe src="UNTRUSTED URL " />`                                   | Canonicalize input, URL Validation, Safe URL verification, Whitelist http and https URL's only (Avoid the JavaScript Protocol to Open a new Window), Attribute encoder.                        |
| String    | CSS Value                                | `html <div style="width: UNTRUSTED DATA ;">Selection</div>`                                                   | Strict structural validation (rule \#4), CSS Hex encoding, Good design of CSS Features.                                                                                                        |
| String    | Javascript Variable                      | `<script>var currentValue='UNTRUSTED DATA ';</script> <script>someFunction('UNTRUSTED DATA ');</script>` | Ensure JavaScript variables are quoted, JavaScript Hex Encoding, JavaScript Unicode Encoding, Avoid backslash encoding (`\"` or `\'` or `\\`).                                                 |
| HTML      | HTML Body                                | `<div>UNTRUSTED HTML</div>`                                                                             | HTML Validation (JSoup, AntiSamy, HTML Sanitizer...).                                                                                                                                          |
| String    | DOM XSS                                  | `<script>document.write("UNTRUSTED INPUT: " + document.location.hash );<script/>`                        | [DOM based XSS Prevention Cheat Sheet](DOM_based_XSS_Prevention_Cheat_Sheet.md)                                                                                                                |


下面的HTML代码片段演示了"如何在各种不同的上下文中安全地渲染不受信任的数据"。

**Safe HTML Attributes:** `align`, `alink`, `alt`, `bgcolor`, `border`, `cellpadding`, `cellspacing`, `class`, `color`, `cols`, `colspan`, `coords`, `dir`, `face`, `height`, `hspace`, `ismap`, `lang`, `marginheight`, `marginwidth`, `multiple`, `nohref`, `noresize`, `noshade`, `nowrap`, `ref`, `rel`, `rev`, `rows`, `rowspan`, `scrolling`, `shape`, `span`, `summary`, `tabindex`, `title`, `usemap`, `valign`, `value`, `vlink`, `vspace`, `width`.


#### 输出编码规则总结

输出编码规则(Output Encoding Rules):目的是将"不受信任的输入数据"转换为"安全格式"，从而输出(展示"数据")给用户,避免了在浏览器中执行"代码"


| Encoding Type           | Encoding Mechanism                                                                                                                                                                                                                                                                                                               |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HTML Entity Encoding    | Convert `&` to `&amp;`, Convert `<` to `&lt;`, Convert `>` to `&gt;`, Convert `"` to `&quot;`, Convert `'` to `&#x27;`, Convert `/` to `&#x2F;`                                                                                                                                                                                  |
| HTML Attribute Encoding | Except for alphanumeric characters, escape all characters with the HTML  Entity `&#xHH;` format, including spaces. (**HH** = Hex Value)                                                                                                                                                                                              |
| URL Encoding            | Standard percent encoding, see [here](http://www.w3schools.com/tags/ref_urlencode.asp). URL encoding should only be used to encode parameter values, not the entire URL or path fragments of a URL.                                                                                                                              |
| JavaScript Encoding     | Except for alphanumeric characters, escape all characters with the `\uXXXX` unicode escaping format (**X** = Integer).                                                                                                                                                                                                               |
| CSS Hex Encoding        | CSS escaping supports `\XX` and `\XXXXXX`. Using a two character escape can  cause problems if the next character continues the escape sequence.  There are two solutions (a) Add a space after the CSS escape (will be  ignored by the CSS parser) (b) use the full amount of CSS escaping  possible by zero padding the value. |


#### 浏览器自带的XSS防御机制 XSS filter

搜索绕过方法`chrome xss filter bypass`
