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

### DOM Based XSS - demo

构造场景

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


防御:

将可能的输出进行 html实体字符编码 变为 只能用来显示的`Html实体(Html Entity)`

`.innerHTML=encodeHTML(<img src=@ onerror=alert(1) />)`

