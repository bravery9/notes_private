### 权限提升

提权(Privilege Escalation)

本地提权(LPE, Local Privilege Escalation)


### Checklists - linux 提权之前必要信息搜集

Windows Version and Configuration
Windows 版本和配置

```
# ---------------
# 查看综合信息
systeminfo

# 查看系统名称（windows系统 中文版）
systeminfo | findstr /B /C:"OS 名称" /C:"OS Version"

# 查看系统名称（windows系统 英文版）
systeminfo | findstr /B /C:"OS Name" /C:"OS Version"

# ---------------
# Architecture
# 查询架构 不同语言的系统都通用
wmic os get osarchitecture || echo %PROCESSOR_ARCHITECTURE%
如结果
OSArchitecture
64-bit

# ---------------
# List all env variables
# 查看所有环境变量
set

# 第三方powershell脚本 如
Get-ChildItem Env: | ft Key,Value

# ---------------
# List all drives
# 列出所有磁盘
wmic logicaldisk get caption || fsutil fsinfo drives

# 解释
# 命令wmic logicaldisk get caption 会获取:硬盘(本地磁盘)、移动存储设备(软盘驱动器 DVD驱动器)、网络位置(网络驱动器)
# 命令fsutil fsinfo drives 需要管理员权限 只会获取:硬盘(本地磁盘)、移动存储设备(软盘驱动器 DVD驱动器)   不会获取:网络位置(网络驱动器)
```

User Enumeration
```
# ---------------
# Get current username
# 获取当前的用户名
echo %USERNAME% || whoami
$env:username

# ---------------
# List user privilege
# 获取用户权限

whoami /priv

# ---------------
# List all users
# 列出所有用户

net user
whoami /all
Get-LocalUser | ft Name,Enabled,LastLogon
Get-ChildItem C:\Users -Force | select Name

# ---------------
# List logon requirements; useable for bruteforcing
# 列出登录要求; 可用于暴力枚举

net accounts
如结果
强制用户在时间到期之后多久必须注销?:     从不
密码最短使用期限(天):                    0
密码最长使用期限(天):                    42
密码长度最小值:                          0
保持的密码历史记录长度:                  None
锁定阈值:                                从不
锁定持续时间(分):                        30
锁定观测窗口(分):                        30
计算机角色:                              WORKSTATION

# ---------------
# Get details about a user (i.e. administrator, admin, current user)
# 获取某个用户的详细信息（如administrator admin 当前用户）

net user administrator
net user admin
net user %USERNAME%

# ---------------
# List all local groups

net localgroup

# 第三方脚本
Get-LocalGroup | ft Name

# ---------------
# Get details about a group (i.e. administrators)
# 获取一个用户组的的详细信息

net localgroup administrators

# 第三方脚本
Get-LocalGroupMember Administrators | ft Name, PrincipalSource
Get-LocalGroupMember Administrateurs | ft Name, PrincipalSource
```


Network Enumeration
网络枚举
```
# ---------------
# List all network interfaces, IP, and DNS.
# 列出所有网络接口 IP DNS

ipconfig /all

# 第三方脚本
Get-NetIPConfiguration | ft InterfaceAlias,InterfaceDescription,IPv4Address
Get-DnsClientServerAddress -AddressFamily IPv4 | ft
# ---------------
# List current routing table
# 列出当前路由表

route print
# 第三方脚本
Get-NetRoute -AddressFamily IPv4 | ft DestinationPrefix,NextHop,RouteMetric,ifIndex


# ---------------
# List the ARP table
# 列出ARP表

arp -A

# 第三方脚本
Get-NetNeighbor -AddressFamily IPv4 | ft ifIndex,IPAddress,LinkLayerAddress,State

# ---------------
# List all current connections
# 列出所有当前连接

netstat -ano

# ---------------
# List firewall state and current configuration
# 列出防火墙状态和当前配置

netsh advfirewall firewall dump

netsh firewall show state

netsh firewall show config

# ---------------
# List firewall's blocked ports
# 列出防火墙的 封掉的端口 (powershell命令)

$f=New-object -comObject HNetCfg.FwPolicy2;$f.rules |  where {$_.action -eq "0"} | select name,applicationname,localports

# ---------------
# Disable firewall
# 禁用防火墙

netsh firewall set opmode disable
netsh advfirewall set allprofiles state off

# ---------------
# List all network shares
# 列出所有网络共享

net share

# ---------------
# SNMP Configuration
# SNMP配置

reg query HKLM\SYSTEM\CurrentControlSet\Services\SNMP /s

# 第三方脚本
Get-ChildItem -path HKLM:\SYSTEM\CurrentControlSet\Services\SNMP -Recurse

```


### Windows提权


|名称|类型|适用环境|描述|
|:-------------:|--|--|-----|
|Token Impersonation (RottenPotato)|/|/|/|
|MS08-067 (NetAPI)|系统漏洞提权|/|/|
|MS16-032|系统漏洞提权|/|/|
|MS17-010 (Eternal Blue)|系统漏洞提权|/|/|
|[Win10Pcap-Exploit](https://github.com/Rootkitsmm/Win10Pcap-Exploit)|应用程序提权|win10|Exploit Win10Pcap Driver to enable some Privilege in our process token ( local Privilege escalation)|

详细参考 [PayloadsAllTheThings/Windows - Privilege Escalation.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md#token-impersonation-rottenpotato)



------


### Checklists - linux 提权之前必要信息搜集

#### 密码

查找包含密码的文件
Files containing passwords
```
grep --color=auto -rnw '/' -ie "PASSWORD" --color=always 2> /dev/null
find . -type f -exec grep -i -I "PASSWORD" {} /dev/null \;
```

最近编辑过的文件
Last edited files
```
# Files that were edited in the last 10 minutes
find / -mmin -10 2>/dev/null | grep -Ev "^/proc"
```

内存中的密码
In memory passwords
```
strings /dev/mem -n10 | grep -i PASS
```


查找敏感文件
Find sensitive files

```
$ locate password | more           
/boot/grub/i386-pc/password.mod
/etc/pam.d/common-password
/etc/pam.d/gdm-password
/etc/pam.d/gdm-password.original
/lib/live/config/0031-root-password
...
```


#### 计划任务

Cron jobs

检查您是否具有对这些文件具有写入权限的访问权限。
检查文件内部，找到具有写权限的其他路径。
```
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
```


Systemd timers

系统定时器
```
systemctl list-timers --all
NEXT                          LEFT     LAST                          PASSED             UNIT                         ACTIVATES
Mon 2019-04-01 02:59:14 CEST  15h left Sun 2019-03-31 10:52:49 CEST  24min ago          apt-daily.timer              apt-daily.service
Mon 2019-04-01 06:20:40 CEST  19h left Sun 2019-03-31 10:52:49 CEST  24min ago          apt-daily-upgrade.timer      apt-daily-upgrade.service
Mon 2019-04-01 07:36:10 CEST  20h left Sat 2019-03-09 14:28:25 CET   3 weeks 0 days ago systemd-tmpfiles-clean.timer systemd-tmpfiles-clean.service

3 timers listed.
```


SUID

SUID / Setuid代表“执行时设置用户ID”，默认情况下在每个Linux发行版中都启用。
如果某个具有这个bit位的文件被运行，则uid会被属主owner改掉。 
如果这个文件属主owner为`root`，即使是用户`bob`执行了这个文件，uid也将被更改为root。 SUID位被表示为`s`

SUID/Setuid stands for "set user ID upon execution", it is enabled by default in every Linux distributions. 
If a file with this bit is ran, the uid will be changed by the owner one.
If the file owner is `root`, the uid will be changed to `root` even if it was executed from user `bob`.
SUID bit is represented by an `s`.




### linux提权

|名称|类型|适用环境|描述|
|:-------------:|--|--|-----|
|CVE-2016-5195 (DirtyCow)|Kernel Exploits|Linux Kernel <= 3.19.0-73.8|DirtyCow|
|CVE-2010-3904 (RDS)|Kernel Exploits| Linux Kernel <= 2.6.36-rc8|RDS|
|CVE-2010-4258 (Full Nelson)|Kernel Exploits|Linux Kernel 2.6.37 (RedHat / Ubuntu 10.04)|Full Nelson|
|CVE-2012-0056 (Mempodipper)|Kernel Exploits|Linux Kernel 2.6.39 < 3.2.2 (Gentoo / Ubuntu x86/x64)|Mempodipper |

详细参考 [PayloadsAllTheThings/Linux - Privilege Escalation.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md)
