### 权限提升

提权(Privilege Escalation)

本地提权(LPE, Local Privilege Escalation)


### Windows提权


|名称|类型|适用环境|描述|
|:-------------:|--|--|-----|
|Token Impersonation (RottenPotato)|/|/|/|
|MS08-067 (NetAPI)|系统漏洞提权|/|/|
|MS16-032|系统漏洞提权|/|/|
|MS17-010 (Eternal Blue)|系统漏洞提权|/|/|
|[Win10Pcap-Exploit](https://github.com/Rootkitsmm/Win10Pcap-Exploit)|应用程序提权|win10|Exploit Win10Pcap Driver to enable some Privilege in our process token ( local Privilege escalation)|

详细参考 [PayloadsAllTheThings/Windows - Privilege Escalation.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md#token-impersonation-rottenpotato)


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
