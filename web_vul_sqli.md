# SQL注入漏洞

### 原理

SQL injection主要是对请求的参数过滤不严格造成的，如动态拼接字符串。


### 漏洞影响

（以时间型盲注为例）

**时间型盲注(Time-Based Blind SQL Injection Attacks)**：可根据不同响应时间确定是否存在SQL注入漏洞

* Test
  * 如对某个使用MySQL数据库的WEB系统测试时发送的请求含payload `(select*from(select(sleep(20)))a)` 得到response的时间为20秒

* Attacks
  * 时间型盲注漏洞 只需利用Condition syntax（条件语法）可判断True/False 即可得到数据库中具体数据
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
package main
 import (
 _ "github.com/go-sql-driver/mysql"
 "database/sql"
 "fmt"
 "log"
 )
 
 func main(){
 db, err := sql.Open("mysql", "root:xxxx@tcp(127.0.0.1:3306)/employeedb")
 if err != nil {
 log.Fatal(err)
 }else{
 fmt.Println("Connection Established")
 }
 var (
 id int
 name string
 )
 
 rows,err:=db.Query("select id, username from user where id = ?", 2)
 if err!=nil{
 log.Fatal(err)
 }
 
 for rows.Next(){
 err:=rows.Scan(&id,&name)
 if err != nil {
 log.Fatal(err)
 }
 fmt.Println(id, name)
 }
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

* 3. 白名单方法

* 4. 数据库用户权限最小化

OWASP参考
https://www.owasp.org/index.php/SQL_Injection_Prevention_Cheat_Sheet
