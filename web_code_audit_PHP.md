### 参考
* [PHP危险函数 - Exploitable PHP functions](https://stackoverflow.com/questions/3115559/exploitable-php-functions)
* [PHP安全编码 | WooYun知识库](http://su.xmd5.org/static/drops/tips-135.html#!)

### 思路
(做代码审计 开发能力需过关)
* 搜索 危险函数 - 尝试利用

* 搜索 自定义安全过滤函数 - 尝试绕过
  * 常见的自定义函数名 `RemoveXSS`
  * 正则函数 `preg_replace`

* 搜索 输入输出点 - 跟踪输入点 查看输出点
  * 超全局变量 `$_GET $_POST $_SERVER $_SESSION $_COOKIE`等
