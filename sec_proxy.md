
#### Proxy代理工具

|名称|属性|描述|
|:-------------:|--|-----|
|[frp](https://github.com/fatedier/frp)|Golang|18k★ 高性能的反向代理(reverse proxy). 用于内网穿透(将NAT或防火墙后面的本地服务器暴露给Internet),支持协议:tcp, udp, http, https.[中文文档](https://github.com/fatedier/frp/blob/master/README_zh.md)|
|[inlets](https://github.com/alexellis/inlets)|Golang|3k★ 将本地服务器暴露给Internet. 可使用SSL over websockets进行加密传输 `wss://`|
|[brook](https://github.com/txthinking/brook)|Golang|6k★ 【Server端】(Linux/MacOS/Windows/Android/iOS)开启VPN/Socks5/Shadowsocks 【Client端】可连接，以及Socks5 to HTTP|

不太建议使用ngrok - (闭源、可能被企业防火墙禁止、对arm支持有限)

#### ProxyPool代理池

|名称|描述|
|:-------------:|-----|
|http://proxylist.fatezero.org/proxy.list | 在线代理池 每15分钟更新. 实测很多高匿代理会暴露真实ip(具体在http header的X-Forwarded-For中最后一个ip)   即https://github.com/fate0/proxylist/blob/master/proxy.list|


### socks5 Server - [Dante](https://github.com/Lozy/danted)

* CentOS下安装Dante
  * `wget —no-check-certificate https://raw.github.com/Lozy/danted/master/install.sh -O install.sh`
  * `chmod +x install.sh && ./install.sh –-port=端口 –-user=自己设置用户名 –-passwd=设置密码`
  * 安装成功提示 `Dante Server Install Successfuly!`
* Ubuntu下安装Dante
  * `apt install dente-server`
  * 但配置文件需要手动修改`sudo vim /etc/danted.conf`.
* Dante - Usage
  * uninstall `bash install.sh --uninstall`
  * add user `/etc/init.d/sockd adduser USERNAME PASSWORD`
  * 更多用法 `/etc/init.d/sockd {start|stop|restart|reload|status|state|adduser|deluser|tail|conf|update}`
