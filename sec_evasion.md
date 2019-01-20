### 简介

"免杀"(AntiVirus Evasion)

完全不被检测 FUD(Fully undetectable)

[github.com Topic: antivirus-evasion](https://github.com/topics/antivirus-evasion)


### 免杀的维度

* 对抗-终端安全-反病毒软件
  * 逃避:自主开发程序实现源码级修改代码实现 且尽量确保样本仅对单一目标攻击 避免杀软报毒
  * 原理:刚开发的新样本程序,从未被捕获分析，通常不会被杀
  * 局限:想彻底改写源码难度大
* 对抗-流量分析
  * HTTP - 白域名流量
    * 国内 可被公开访问的页面 `weibo` `users.qzone.qq.com:80/fcg-bin/cgi_get_portrait.fcg?uins=550067654`
    * 国外 可被公开访问的页面 `google、twitter、pastbin、telegram`...
  * DNS - dga 域名生成算法
    * 逃避:使用dga可以逃避已有的IoC(恶意域名黑名单)
    * 原理:dga 域名生成算法
    * 局限:dga 可能被机器学习识别
  * 其他协议
* 对抗逆向分析
  * 逃避:通过加壳等方式延长被逆向分析人员彻底分析的时间
  * 局限:没有绝对的反逆向保护，只能增加逆向分析难度

### 工具

|名称|属性|描述|
|:-------------:|--|-----|
|[avet](https://github.com/govolution/avet)|C+shell+python|[仅针对Windows exe] AntiVirus Evasion Tool 运行于kali2|
|[Veil-Evasion](https://github.com/Veil-Framework/Veil-Evasion)|shell+C+python2.7|[可针对Windows exe] generate metasploit payloads that bypass common anti-virus solutions|
|[ASWCrypter](https://github.com/AbedAlqaderSwedan1/ASWCrypter)|shell+python|An Bash&Python Script For Generating Payloads that Bypasses All Antivirus so far FUD.实测无法过360ZhuDongFangyu|
|[tokyoneon/Armor](https://github.com/tokyoneon/Armor)|shell|[仅针对MacOS]Armor is a simple Bash script designed to create encrypted macOS payloads capable of evading antivirus scanners.|
|[csvoss/onelinerizer](https://github.com/csvoss/onelinerizer)|python|把Python代码给编译成一句话的形式工具 Convert any Python file into a single line of code.|
