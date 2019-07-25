### APT案例 - 钓鱼邮件中的payload载体

* 附件形式
  * 直接 - 邮件附件
  * 间接 - 共有云盘链接
* 附件名称
  * 反转字符

|payload载体|文件后缀|原理与案例|
|:----:|-|----|
|快捷方式文件|`.lnk`|蓝宝菇APT 内嵌PowerShell脚本的LNK文件|
|内嵌恶意Marco的offcie文档|`.doc`|内嵌恶意宏的Word文档 需要目标点击"启动宏"才能使payload执行|
|word文档|`.doc`|利用漏洞(如CVE-2017-11882)实现打开`.doc`文件即可无交互地执行payload|
|HTA文件|`.hta`|APT|
|使用Office软件图标的可执行文件|/|VBA Marco - [蔓灵花(BITTER) APT](https://s.tencent.com/research/report/615.html)的邮件中投递了使用word图标的exe自解压文件|
|.rar压缩文件|`.rar`|如果目标使用的rar程序存在CVE-2018-20250漏洞,当打开evil.rar常常会把恶意程序解压到"启动"`startup`文件夹|
|SFX自解压文件|/|APT|

### 实践 - 构建包含payload的Office文档

* 工具 - [outflanknl/EvilClippy](https://github.com/outflanknl/EvilClippy)
