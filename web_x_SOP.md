### 简介

[Same-origin policy | Mozilla](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
[浏览器的同源策略 | Mozilla](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)

浏览器的同源策略规定：非同源的 客户端脚本(主要指JavaScript) 在没明确授权的情况下，不能读写对方的资源。
### 同源策略的意义

同源策略限制了非同源的客户端脚本的行为。

如果浏览器没有同源策略:当你登录Gmail网站同时也打开这另一个网站E，网站E的JavaScript可以跨域读取你的Gmail网站的cookie，即获取到了Gmail的登录权限。

### 判断是否同源

Two URLs have the same origin if the protocol, port (if specified), and host.

* 三者必须相同才能算同源
  * protocol `http`与`https`互为非同源
  * host 域名完全相同`www.a.com`与`sub.a.com`与`a.com`互为非同源
  * port 端口完全相同 `:80`与`:443`与`:8080`互为非同源

### 授权

某浏览器访问了两个网站:http://evil.com 与 http://foo.com

http://evil.com的客户端脚本JavaScript想要操作http://foo.com的资源,

只有http://foo.com明确返回HTTP响应头允许http://evil.com操作http://foo.com的资源时:

```
Access-Control-Allow-Origin: http://evil.com
或
Access-Control-Allow-Origin: *
```

http://evil.com网站的客户端脚本JavaScript 才有权(通过AJAX技术)对 http://foo.com上的资源进行读写操作。


### 如何允许跨源访问

使用 CORS 允许跨源访问

[Cross-Origin Resource Sharing (CORS) | Mozilla](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)
