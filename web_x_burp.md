### Mac下burpsuite代理HSTS请求

问题：burpsuite无法拦截某些https的请求，浏览器报错信息为 `ERR_SSL_VERSION_OR_CIPHER_MISMATCH.协议不受支持。`

解决方案
* 1.证书 `project options` - `SSL` 将`default`改为`Use custom protocols and ciphers`
* 2.系统导入burpsuite的证书 访问 http://burp/cert 下载证书,打开mac的钥匙串访问`keychain`，导入证书并且修改`PortSwigger CA`为始终信任。

最终效果：
可正常使用burp抓取HSTS请求,浏览器显示信任该证书。[图片参考](http://xdxd.love/2017/03/15/mac%E4%B8%8A%E5%AE%89%E8%A3%85burpsuite%E8%AF%81%E4%B9%A6/)

### burpsuite扩展 - bypassWAF

BApp安装bypassWAF后，

在burp的选项卡`Project options` - `Session Handling Rules` - `Rules Actions` - `Add`-`Invoke a Burp Extension` 选择`bypassWAF`

在`scope`里指定：工具范围 url范围 参数范围

这里只需要在`URL scope`里增加一个`Any`即可

### burpsuite扩展 - NoPE Proxy

安装：

[summitt/Burp-Non-HTTP-Extension: Non-HTTP Protocol Extension (NoPE) Proxy and DNS for Burp Suite.](https://github.com/summitt/Burp-Non-HTTP-Extension)

使用参考：

[Burpsuite抓取非HTTP流量 - FreeBuf](https://www.freebuf.com/articles/network/158589.html)

