### 简介

"免杀"(AntiVirus Evasion)

完全不被检测 FUD(Fully undetectable)

[github.com Topic: antivirus-evasion](https://github.com/topics/antivirus-evasion)


### 免杀的维度

* 对抗-终端安全-反病毒软件
  * 逃避原理:自主开发的新样本程序,从未被捕获、分析 且行为不过分时，通常不会被杀（尽量确保样本仅对单一目标攻击,可以实现源码级的修改实现"变种",避免杀软报毒)
  * 局限:彻底改写源码难度大
* 对抗-流量分析
  * Domain Fronting - 百域名
    * 逃避原理:借用CDN实现  C2流量 -> CDN -> C2 Server
    * 局限:?
  * DNS - dga域名生成算法
    * 逃避原理:使用dga可以逃避已有的IoC(恶意域名黑名单) 避免流量告警
    * 局限:dga 可能被机器学习识别
  * HTTP - 白域名
    * 逃避原理:使用知名的域名作为C2 数据加密传输 流量角度上很隐蔽 避免流量告警
      * 国内 可被公开访问的页面 `weibo` `users.qzone.qq.com:80/fcg-bin/cgi_get_portrait.fcg?uins=550067654`
      * 国外 可被公开访问的页面 `google、twitter、pastbin、telegram ...`
  * 其他协议
* 对抗-逆向分析
  * 逃避原理:通过加壳等方式延长被逆向分析人员彻底分析的时间
  * 局限:没有绝对的反逆向保护，只能增加逆向分析难度
* 对抗-行为分析
  * 逃避原理:在指定条件下才会运行恶意行为，从而避免该程序在VM、沙箱、逆向人员的眼皮底下进行恶意操作。
  * 局限:可能会缩小它的目标范围，如只能在某个指定域中计算机才能运行该程序。
  
### 免杀工具

|名称|属性|针对目标|描述|
|:-------------:|--|---|----|
|[EgeBalci/Amber](https://github.com/EgeBalci/Amber)|go+asm|仅针对 Windows| Reflective PE packer. 将PE文件打包成reflective payloads 可以无文件执行 像shellcode一样加载并执行自身|
|[avet](https://github.com/govolution/avet)|C+shell+python|仅针对Windows| AntiVirus Evasion Tool 运行于kali2|
|[Veil-Evasion](https://github.com/Veil-Framework/Veil-Evasion)|shell+C+python2.7|可针对Windows exe| generate metasploit payloads that bypass common anti-virus solutions|
|[tokyoneon/Armor](https://github.com/tokyoneon/Armor)|shell|MacOS|Armor is a simple Bash script designed to create encrypted macOS payloads capable of evading antivirus scanners.|
|[GreatSCT](https://github.com/GreatSCT/GreatSCT)|python3|/|/|
|[ASWCrypter](https://github.com/AbedAlqaderSwedan1/ASWCrypter)|shell+python|/|An Bash&Python Script For Generating Payloads that Bypasses All Antivirus so far FUD.实测无法过360ZhuDongFangyu|
|[tokyoneon/Armor](https://github.com/tokyoneon/Armor)|shell|仅针对MacOS|Armor is a simple Bash script designed to create encrypted macOS payloads capable of evading antivirus scanners.|
|[csvoss/onelinerizer](https://github.com/csvoss/onelinerizer)|python|/|把Python代码给编译成一句话的形式工具 Convert any Python file into a single line of code.|


#### 无文件攻击

* "无文件"的优势 - 不落地 样本难以被捕获

|名称|描述|针对目标|
|--|--|-----|
|[无文件攻击的现实实例FCL(Fileless Command Lines)](https://github.com/chenerlich/FCL)|包括:恶意软件名称/执行过程/恶意命令行/正则表达式用于检测/技术报告/沙箱报告链接/笔记|windows|


### 免杀实例1 - 全面免杀

[渗透利器Cobalt Strike - 第2篇 APT级的全面免杀与企业纵深防御体系的对抗 - 先知社区](https://xz.aliyun.com/t/4191)

### 免杀实例2 - 从远程获取加密payload

简介：使用Armor工具生成加密的payload，MacOS下的one_liner从远程地址获取加密payload，解密，执行。

参考作者介绍[Hacking macOS: How to Create an Undetectable Payload « Null Byte](https://null-byte.wonderhowto.com/how-to/hacking-macos-create-undetectable-payload-0189715/)

**原理解析**

预先创建一个MacOS下可执行的常规raw payload(shell command)

|顺序|攻击机(使用Armor工具)|网络|靶机|
|:-------------:|--|--|-----|
|1加密|使用密钥对payload进行加密，得到enc.txt|攻击机的443端口与victim无通信流量|/|
|2待解密|Ncat监听443端口 以供victim访问并获取解密密钥enc.key|攻击机的443端口与victim无通信流量|/|
|3解密|Ncat监听443端口|攻击机的443端口与victim通信该过程是https(抓到流量也无法得到解密密钥)|执行enc.txt 第1步获取解密密钥:连接到攻击机的443端口 获取解密密钥enc.key 第2步:解密成功并执行 常规raw payload(shell command)|
|4结束|Ncat结束监听443端口 用于密钥传输的端口(默认443)只会被连接1次 传输完密钥后立即关闭|攻击机的443端口与victim无通信流量|/|


* 实现效果
  * 流量分析 流量分析无法获取密钥(https通信)
  * 终端安全 杀软无法解密得到加密命令的含义(无密钥)
