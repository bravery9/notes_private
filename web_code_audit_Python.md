### Python代码审计

### 实例1 Python代码-系统命令执行漏洞

#### 设计

程序设计时就需要尽量避免使用命令执行函数。

#### 函数【执行系统命令的函数】

https://stackoverflow.com/questions/89228/calling-an-external-command-in-python

传入【执行系统命令的函数】的参数如果来自用户输入，必须进行严格的过滤/转义（如果此处没有正确实现，则可能存在命令注入）。

#### 修复方案

(Python没有像PHP中自带的2个函数专门对命令字符串进行转义:escapeshellcmd和escapeshellarg）
所以需要自己写 过滤/转义 函数。

* 白名单法 更为安全，以下代码供参考
* 黑名单法 可能被绕过

```
#! /usr/bin/env python
# coding=utf-8

import re

#白名单法【安全】

def Check(input_from_user):
    regex_ = '[^0-9|a-z|A-Z|._]' # 如果存在 白名单字符 0-9|a-z|A-Z|._ 以外的字符则return 0
    result_list2 = re.findall(regex_,long_str_from_user,re.MULTILINE)# re.MULTILINE多行模式

    if len(result_list2):
        print '发现特殊字符'+str(result_list2)
        return 0
    else:
        return 1


if __name__ == '__main__':

    # 如 含有换行符号 不符合白名单规则
    long_str_from_user = """
    test\/
    ~!@$%^*(#%&*
    """

    if Check(long_str_from_user) == 0:
        print "不符合白名单规则"
```
运行结果为：
```
发现特殊字符['\n', ' ', ' ', ' ', ' ', '\\', '/', '\n', ' ', ' ', ' ', ' ', '~', '!', '@', '$', '%', '^', '*', '(', '#', '%', '&', '*', '\n', ' ', ' ', ' ', ' ']
不符合白名单规则
```

#### 修复原理

白名单法
