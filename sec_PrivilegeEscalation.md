### 权限提升

提权(Privilege Escalation)

本地提权(LPE, Local Privilege Escalation)

### Windows


|名称|类型|适用环境|描述|
|:-------------:|--|--|-----|
|Token Impersonation (RottenPotato)|/|/|/|
|MS08-067 (NetAPI)|系统漏洞提权|/|/|
|MS16-032|系统漏洞提权|/|/|
|MS17-010 (Eternal Blue)|系统漏洞提权|/|/|
|[Win10Pcap-Exploit)](https://github.com/Rootkitsmm/Win10Pcap-Exploit)|应用程序提权|win10|Exploit Win10Pcap Driver to enable some Privilege in our process token ( local Privilege escalation)|

详细参考 [PayloadsAllTheThings/Windows - Privilege Escalation.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md#token-impersonation-rottenpotato)


### linux

|名称|类型|适用环境|描述|
|:-------------:|--|--|-----|
|CVE-2016-5195 (DirtyCow)|Kernel Exploits||Kernel Exploits|
|CVE-2010-3904 (RDS)|Kernel Exploits||RDS|
|CVE-2010-4258 (Full Nelson)|Kernel Exploits||Full Nelson|
|CVE-2012-0056 (Mempodipper)|Kernel Exploits||Mempodipper|

详细参考 [PayloadsAllTheThings/Linux - Privilege Escalation.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md)
