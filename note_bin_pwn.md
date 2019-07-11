
#### 简介

pwn:通过二进制/系统调用等方式获得目标主机的shell.

#### 检测ELF二进制文件的属性


linux下运行[checksec.sh](https://github.com/slimm609/checksec.sh)以检测ELF二进制可执行程序的属性/保护机制

如PIE, RELRO, PaX, Canaries, ASLR, Fortify Source

```
# 检测命令
checksec elf_file

# 输出结果
Arch:     i386-32-little
RELRO:    Partial RELRO
Stack:    No canary found
NX:       NX enabled
PIE:      No PIE (0x8048000)
```

具体解释:

* RELRO
  * Partial RELRO
  * FULL RELRO（这种无法修改got表）
* Stack
  * Canary found - 无法直接用溢出去覆盖栈中返回地址。要进行绕过:通过改写指针与局部变量、leak canary、overwrite canary
* NX
  * NX enabled - NX(即No-eXecute)表示不可执行,则栈中数据没有执行权限，经常用的call esp或者jmp esp的方法就无法使用. 可以利用ROP这种方法绕过NX enabled
* PIE
  * PIE enabled - 开启地址随机化选项. 即程序每次运行的地址都不同
  * No PIE (0x400000) - 没有开PIE，那么程序的基地址就是括号内的数据0x400000
* FORTIFY
  * FORTIFY_SOURCE机制对格式化字符串有2个限制
(1)包含%n的格式化字符串不能位于程序内存中的可写地址。
(2)当使用位置参数时，必须使用范围内的所有参数。所以如果要使用%7$x，你必须同时使用1,2,3,4,5和6



#### 工具_二进制_debug_pwn

|名称|属性|描述|
|:-------------:|--|-----|
|[gdbgui](https://github.com/cs01/gdbgui)|/|6k★ 基于浏览器可视化gdb调试 [下载安装ubuntu14.04+](https://gdbgui.com/downloads.html)|
|[peda](https://github.com/longld/peda)|python|6k★ Python Exploit Development Assistance for GDB|
|[pwntools](https://github.com/Gallopsled/pwntools)|python2.7(best Ubuntu)|4k★ CTF framework and exploit development library|
|[heap-viewer](https://github.com/danigargu/heap-viewer)|python| [IDA Pro plugin] examine the glibc heap, focused on exploit development|
|[cea-sec/miasm](https://github.com/cea-sec/miasm)|python|2k★ (PE/ELF)Reverse engineering framework|

|名称|属性|描述|
|:-------------:|--|-----|
|http://syscalls.kernelgrok.com/|syscalls|系统调用参考 主要用sys_execve|
|http://shell-storm.org/shellcode/|shellcode|各种平台下的shellcode包括:BSD/Cisco/FreeBSD/Linux/OSX/Windows|


#### 工具_二进制_fuzz

|名称|属性|描述|
|:-------------:|--|-----|
|[halfempty](https://github.com/googleprojectzero/halfempty)|C|[projectZero] testcase minimization tool.|
