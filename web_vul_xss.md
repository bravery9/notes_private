### 简介

跨站脚本攻击(Cross Site Scripting)

### 分类

* 反射型 - XSSpayload必然会在HTTP request中,后端未严格过滤,导致HTTP response中的部分内容由请求中的参数值XSSpayload控制
* 存储型 - 通常影响更大(已经保存到数据库中)
* DOM型 - 手工构造XSSpayload并由浏览器的DOM解析过程中触发(该触发过程与http的请求响应没有直接关系 HTTP request 与HTTP response 一直无变化 无payload)
