### 主机扫描思路

由于被扫描主机的各种设置，具体端口号 与 端口提供的服务 不必然相关（如80通常为web端口 但不是必然)

针对端口的攻击与防御 应该以服务为主：

**Port -> Service -> Exp**

扫描全部端口 - 确定各端口服务 - 对每个服务尝试其对应的漏洞利用办法

### 防御思路

* 资产收集与管理
* 权限最小化（主机权限/主机启动的服务属主权限/网络划分）

### Services

|服务|常见端口|检测方式/攻击工具|防御/修复|
|:-------------:|--|-----|-----|
| 远程连接类服务 | - | 黑盒 | 白盒 |
|SSH|22|暴力枚举 - hydra|基线检查|
|RDP(windows)|3389|hydra - 暴力枚举|基线检查|
| 数据库类服务 | - | 黑盒 | 白盒 |
|MySQL|3306|暴力枚举 - hydra|基线检查|
|MS SQL Server(windows)|1433|暴力枚举 - hydra|基线检查|
|Postgres|5432|暴力枚举 - hydra|基线检查|
|redis|6379|暴力枚举 - hydra|基线检查|
|MongoDB|27017|暴力枚举 - python脚本|基线检查|
| 其他服务 | - | - | - |
|RMI|1099|[BaRMIe](https://github.com/NickstaDB/BaRMIe) (Java) |ACL|

### 相关说明

防御 - 基线检查：使用白盒方法和工具 检查不安全配置

```
# BaRMIe
# enumerating and attacking Java RMI (Remote Method Invocation) services.
java -jar BaRMIe_v1.01.jar -attack ip port
#可以只输入ip自动进行端口枚举
#支持13种payload
Deserialization payloads for: 10.0.0.1:1099
 1) Apache Commons Collections 3.1, 3.2, 3.2.1
 2) Apache Commons Collections 4.0-alpha1, 4.0
 3) Apache Groovy 1.7-beta-1 to 2.4.0-beta-4
 4) Apache Groovy 2.4.0-rc1 to 2.4.3
 5) JBoss Interceptors API
 6) ROME 0.5 to 1.0
 7) ROME 1.5 to 1.7.3
 8) Mozilla Rhino 1.7r2
 9) Mozilla Rhino 1.7r2 for Java 1.4
 10) Mozilla Rhino 1.7r3
 11) Mozilla Rhino 1.7r3 for Java 1.4
 12) Mozilla Rhino 1.7r4 and 1.7r5
 13) Mozilla Rhino 1.7r6, 1.7r7, and 1.7.7.1
 ```
