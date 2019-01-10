### 简介

[Same-origin policy | Mozilla](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)

[浏览器的同源策略 | Mozilla](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)

浏览器的同源策略规定：非同源的 客户端脚本(主要指JavaScript) 在没明确授权的情况下，不能读写非同源的资源。

* 资源包括(客户端安全威胁都是围绕这些资源进行)
  * HTTP消息头
  * 整个DOM树(Document Object Model)
  * 浏览器存储 如Cookies、Flash Cookies、localStorage...

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

### 授权

某浏览器访问了两个网站:http://evil.com 与 http://foo.com

http://evil.com 的客户端脚本JavaScript 想要操作 http://foo.com 的资源,

只有http://foo.com 明确返回HTTP响应头允许 http://evil.com 操作 http://foo.com 的资源时:

```
Access-Control-Allow-Origin: http://evil.com
或
Access-Control-Allow-Origin: *
```

http://evil.com网站的客户端脚本JavaScript 才有权(通过AJAX技术)对 http://foo.com上的资源进行读写操作。



### 如何允许跨源访问

使用 CORS 允许跨源访问

[Cross-Origin Resource Sharing (CORS) | Mozilla](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)

### CORS实例

Request:

foo -> bar

`Origin`表示该CORS Request是`Origin`中的网站(通常通过JavaScript)发起的。

从该请求可看出是从 `Referer`/`Origin` 中的 `http://foo.example` 对 目标主机`Host: bar.other`发起请求：
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
