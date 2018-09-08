### 官方仓库

Dante socks5 server
https://github.com/Lozy/danted

### CentOS - 下载&安装
```
wget —no-check-certificate https://raw.github.com/Lozy/danted/master/install.sh -O install.sh
```
```
chmod +x install.sh && ./install.sh –port=端口 –user=自己设置用户名 –passwd=设置密码
```

安装成功提示`Dante Server Install Successfuly!`


### Ubuntu - 安装
`apt install dente-server`
但配置文件需要手动修改`sudo vim /etc/danted.conf`.

### Usage
更多用法
```
/etc/init.d/sockd {start|stop|restart|reload|status|state|adduser|deluser|tail|conf|update}
```

uninstall
```
bash install.sh --uninstall
```

add user
```
/etc/init.d/sockd adduser USERNAME PASSWORD
```
