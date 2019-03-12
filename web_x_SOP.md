### 简介

[浏览器的同源策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)[(Same-origin policy)](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
规定：非同源的 客户端脚本(主要指JavaScript) 在没明确授权的情况下，不能读写非同源的资源。

* 资源包括(客户端安全威胁都是围绕这些资源进行)
  * HTTP消息头
  * 整个DOM树(Document Object Model)
  * 浏览器存储 如Cookies、Flash Cookies、localStorage...
* “例外”
  * `<link>`标签 `<img>`标签 `<script>`标签 中的`href`属性/`src`属性 可"获取展示"异域的CSS/JavaScript/图片(其实并不能真正读取资源的内容)
  
### 同源策略的意义

同源策略限制了非同源的客户端脚本的行为。

* 如果浏览器没有同源策略:
  * 例1 当你登录Gmail网站同时也打开这另一个网站E，网站E的JavaScript可以跨域读取你的Gmail网站的cookie，即获取到了Gmail的登录权限。
  * 例2 当你访问了Evil.com 它使用JavaScript拦截其他站点的表单从而捕获数据,发出request到receive.com 从而盗取信息)

### 判断是否同源

Two URLs have the same origin if the protocol, port (if specified), and host.

* 三者必须相同才能算同源
  * protocol `http`与`https`互为非同源
  * host 域名完全相同`www.a.com`与`sub.a.com`与`a.com`互为非同源
  * port 端口完全相同 `:80`与`:443`与`:8080`互为非同源


### 如何允许跨源访问-方案

* 根本解决方法(W3C标准)
  * [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)跨域资源共享 - 允许任何http method的跨源AJAX请求. [CORS详情详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)
* 其他解决办法
  * JSONP - 只支持GET method (padding指的就是callback函数)
  * Web Sockets
  * HTML5 API `window.postMessage`方法 允许跨窗口通信 不论这两个窗口是否同源

### 实例 - CORS

Request:

foo -> bar

`Origin`表示该CORS Request是`Origin`中的网站(通常通过JavaScript)发起的。

从该请求可看出http://foo.example 的客户端脚本JavaScript 想要操作 http://bar.other 的资源

从 `Referer`/`Origin` 中的 `http://foo.example` 对 目标主机`Host: bar.other`发起请求：
```
GET /resources/public-data/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.5; en-US; rv:1.9.1b3pre) Gecko/20081130 Minefield/3.1b3pre
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
Connection: keep-alive
Referer: http://foo.example/examples/access-control/simpleXSInvocation.html
Origin: http://foo.example
```


Response:

foo <- bar

```
HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 00:23:53 GMT
Server: Apache/2.0.61 
Access-Control-Allow-Origin: *
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Transfer-Encoding: chunked
Content-Type: application/xml

[XML Data]
```

可见`bar.other`响应中的`Access-Control-Allow-Origin: *` 代表它的资源可以被任何域（包括foo.example）发出的requset访问。

如果`bar.other`响应中是`Access-Control-Allow-Origin: http://foo.example` 代表它的资源只可以被来自`http://foo.example`发出的requset访问。

只有 `http://bar.other` 返回的HTTP响应头`Access-Control-Allow-Origin` 明确指定允许 `http://foo.example` 操作 `http://bar.other` 的资源时，`http://foo.example` 网站的客户端脚本JavaScript 才有权(通过AJAX技术)对 `http://bar.other` 上的资源进行读写操作。

### 实例 - JSON with Padding
 
* 理解
  * json是目的(json返回的是一串数据)
  * JSONP只是手段(JSONP返回的脚本代码 符合js语法 随后调用了回调函数)
  * JSONP可以“跨域”的本质: `<script>`标签可以请求不同域名下的资源.
 
首先在站点A的前端 内容如下:

```
//在body中动态添加一个 <script>标签
var script = document.createElement('script');
script.setAttribute("type","text/javascript");
script.src = 'http://B.com/ip?callback=foo';//告诉外部的服务器B 请返回指定格式的JS脚本  如foo({"ip": "8.8.8.8"}); 
//为什么不直接写成json数据`{"ip": "8.8.8.8"}`呢? 因为这是JSONP的根本原理要求的 因为JSONP在js文件中 需要符合js语法
document.body.appendChild(script);

//foo 为站点A中的一个本地方法. 当外部的服务器B 响应数据后 foo为接受数据的回调函数(callback)
function foo(data) {
console.log('Your public IP address is: ' + data.ip);
};
```

JSONP步骤分析:
* A站点会向B站点发送GET请求 `http://B.com/ip?callback=foo` 其中参数名callback的参数值为foo 三个字母组成的字符串
* 非同域服务器B的后端 接收到A的请求 得到来自A的字符串foo (foo其实是A页面中的回调函数的函数名)
* B进行响应 目的是把数据返回给A站点的前端JavaScript. 响应内容为`foo({"ip": "8.8.8.8"});`
* A站点的前端JavaScript执行响应内容 也就是把json数据`{"ip": "8.8.8.8"}`作为实参 传入A站点的前端本来就有的函数foo

就这样，A站点的前端就拿到了来自“非同域”的B站点的json格式的数据 `{"ip": "8.8.8.8"}`

通过JSONP的方式实现了跨域请求


### 实例 - jQuery的JSON with Padding

jQuery可以使用匿名函数

```
$.ajax({
    url:"http://www.B.com/open.php?callback=?",
    dataType:"jsonp",
    success:function(data){
        console.log(data);
        //ToDo..
    }
});    
```
