### 简介

[浏览器的同源策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)[(Same-origin policy)](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
规定：非同源的 客户端脚本(主要指JavaScript) 在没明确授权的情况下，不能读写非同源的资源。

* 非同源的资源(客户端安全威胁都是围绕这些资源进行)
  * HTTP header
  * 整个DOM树(Document Object Model)
  * 浏览器存储 如Cookies、Flash Cookies、localStorage...
* "例外" 跨域资源嵌入
  * 说明 - 可以在当前域下进行"跨域资源嵌入" 来"展示"非当前域的资源 如CSS/JavaScript/图片
  * 原理 - 因为浏览器限制了非同域资源的权限 如非同域JavaScript不能真正 读 写 非同域的资源
  * 举例
    * `<link>`标签 通常使用href属性来指定css资源. (因为浏览器下载css文件的同时 不会停止对当前Document的处理) 如`<link href="common.css" rel="stylesheet"/>`
    * `<img>`标签 嵌入跨域图片. 通常使用src属性来指定资源 支持图片格式PNG,JPEG,GIF,BMP,SVG...
    * `<script>`标签 嵌入跨域脚本. 通常使用src属性来指定资源. 浏览器解析到该元素时 会暂停其他资源的下载和处理(直到该资源加载且执行结束后才会继续) 下载资源并应用到当前Document中. 如`<script src ="test.js"></script>`
    * `<iframe>`标签 通常使用src属性来指定资源 会下载资源并应用到当前Document中

### 同源策略的意义

同源策略限制了非同源的客户端脚本的权限。

* 如果浏览器没有同源策略:
  * 例1 访问mail.com并登录mailA账户 的同时也访问了另一个网站E.com，网站E.com的JavaScript可以跨域读取mail.com的cookie，即获取到了mailA账户在mail.com的登录权限
  * 例2 访问Evil.com 该站点使用JavaScript获取mail.com的表单输入框的内容 从而捕获用户输入的数据 并很容易将数据发到Evil.com 盗取对应内容

### 判断是否同源

* 两个url有相同的protocol host port则同源
  * protocol 协议必须完全相同 如`http`与`https`互为非同源
  * host 域名必须完全相同 如`www.a.com`与`sub.a.com`与`a.com` 三者互为非同源
  * port 端口必须完全相同 如`:80`与`:443`与`:8080` 三者互为非同源

### 如何允许跨源访问-方案

* 实现跨域的根本方法(W3C标准)
  * [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)跨域资源共享 - 允许任何http method的跨源AJAX请求. [CORS详情详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)
* 实现跨域的其他办法
  * JSONP - 只支持GET method (padding指的就是callback函数) 且无法双向传输数据
  * `window.postMessage(message，targetOrigin)` - 该HTML5方法可从当前window对象向其他的window对象发送消息 不论这两个窗口是否同源
  * Web Sockets
  * `location.hash`与iframe 跨域传值 - 数据容量有限制 数据类型有限制
  * `window.name`与iframe 跨域数据传输 - window对象的name参数可以在多标签内共享
  * flash


### 实例1 - CORS

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

### 实例2 - JSON with Padding
 
* 理解
  * json是目的(json返回的是一串数据)
  * JSONP只是手段(JSONP返回的脚本代码 符合js语法 随后调用了回调函数)
  * JSONP可以"跨域"的本质: `<script>`标签可以请求不同域名下的资源.
 
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


### 实例3 - JSON with Padding(jQuery实现)

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

### 实例4 - 利用window.name与iframe实现跨域

跨域传输数据原理：window对象的name参数可以在多标签内共享

如a.com的a.html中创建iframe标签并设置地址外域，在外域设置window.name，再跳转回本域，此时还能获取到外域设置的window.name，此时就达到了跨域的目的。

a.com的a.html的内容:
```
<script type="text/javascript">
    var state = 0, 
    iframe = document.createElement('iframe'),//创建iframe元素
    loadfn = function() {
        if (state === 1) {
            var data = iframe.contentWindow.name; // 读取到外域b.com的资源、数据
            alert(data);    //弹出'happy'
        } else if (state === 0) {
            state = 1;
            iframe.contentWindow.location = "http://a.com/proxy.html"; // proxy.html(内容为空 仅起中转作用)
        }  
    };
    iframe.src = 'http://b.com/b.html';//设置a.com页面下的iframe元素 src属性的值为http://b.com/b.html
    if (iframe.attachEvent) {
        iframe.attachEvent('onload', loadfn);
    } else {
        iframe.onload  = loadfn;
    }
    document.body.appendChild(iframe);
</script>
```

a.com的proxy.html的内容:
(内容为空 仅起中转作用)


b.com的b.html的内容:
```
<script type="text/javascript">
    // 需要传输的数据
    // 数据格式可以自定义:json 字符串...
    // 数据量：一般为2M，Firefox下约32M
    window.name = 'happy';
</script>
```
### 实例5 - window.postMessage

`window.postMessage(message，targetOrigin)` - 该HTML5方法可从当前window对象向其他的window对象发送消息 不论这两个窗口是否同源

`message` 为要发送的消息，类型只能为字符串
`targetOrigin` 用来限定接收消息的那个 window 对象所在的域 (可以使用通配符`*`不限定域)


a.com的a.html发送数据:
```
<iframe src="http://b.com/b.html" id="myIframe" onload="test()" style="display: none;">
<script>
    function test() {
        var iframe = document.getElementById('myIframe');
        var win = iframe.contentWindow; // 获取 http://b.com/b.html 页面的 window 对象
        win.postMessage('a.com a.html says: hello............', '*');   // 通过 postMessage 向 http://b.com/b.html 发送消息
    }
</script>
```


b.com的b.html接收数据:
接收消息的 window 对象，监听自身的 message 事件，消息内容储存在该事件对象的 data 属性中
`window.onmessage`
```
<script type="text/javascript">
    // 注册 message 事件用来接收消息
    window.onmessage = function(e) {
        e = e || event; // 获取事件对象
        console.log(e.data); // 通过 data 属性得到发送来的消息
    }
</script>
```


### 实例6 - location.hash与iframe

html中的锚链接
```
<a href='#简介'>click</a>
```

如在本页面中跳转到"简介": 开发者模式输入 `location.hash="#简介"`


