### Python代码审计

### 实例1 Python代码-系统命令执行漏洞

#### 设计

程序设计时就需要尽量避免使用命令执行函数。

#### 函数【执行系统命令的函数】

https://stackoverflow.com/questions/89228/calling-an-external-command-in-python

传入【执行系统命令的函数】的参数如果来自用户输入，必须进行严格的过滤/转义（如果此处没有正确实现，则可能存在命令注入）。

#### 修复方案

(Python没有像PHP中自带的2个函数专门对命令字符串进行转义:escapeshellcmd和escapeshellarg）

所以Python需要自己写 过滤/转义 函数。

* 白名单法 (安全) 参考代码 https://github.com/1135/python_demos/blob/master/web_filter_whiteList.py
* 黑名单法 (不推荐 可能被绕过)

#### 修复原理

白名单法
