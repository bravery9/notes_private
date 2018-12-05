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

### 实例1 系统命令执行漏洞

#### PHP中 【执行系统命令的函数】

它们只有略微差别：
 ```
+----------------+-----------------+----------------+----------------+
|    Command     | Displays Output | Can Get Output | Gets Exit Code |
+----------------+-----------------+----------------+----------------+
| system()       | Yes (as text)   | Last line only | Yes            |
| passthru()     | Yes (raw)       | No             | Yes            |
| exec()         | No              | Yes (array)    | Yes            |
| shell_exec()   | No              | Yes (string)   | No             |
| backticks (``) | No              | Yes (string)   | No             |
+----------------+-----------------+----------------+----------------+
 ```

传入以上【执行系统命令的函数】的参数如果来自用户，必须进行严格的过滤/转义（如果此处没有正确实现，则可能存在命令注入）。

#### PHP命令执行漏洞修复方案

使用PHP自带的2个函数专门对命令字符串进行转义:escapeshellcmd和escapeshellarg
```
<?php
$arg = "-L";
system(escapeshellcmd('pwd '.escapeshellarg($arg)));
//此时尝试进行命令注入：修改"-L"和'pwd '  会发现关键字符被转义 无法执行命令
?>
```

#### 过滤原理

escapeshellarg和escapeshellcmd 的C具体实现 331行
https://github.com/php/php-src/blob/321c0cc3493998f731f0666127c093eff4e119eb/ext/standard/exec.c

