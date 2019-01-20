### 简介

"免杀"(AntiVirus Evasion)

完全不被检测 FUD(Fully undetectable)

[github.com Topic: antivirus-evasion](https://github.com/topics/antivirus-evasion)


### 免杀的维度

* 对抗-终端安全-反病毒软件
  * 逃避原理:自主开发的新样本程序,从未被捕获、分析 通常不会被杀（尽量确保样本仅对单一目标攻击,可以实现源码级的修改实现"变种",避免杀软报毒)
  * 局限:彻底改写源码难度大
* 对抗-流量分析
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

### 工具

|名称|属性|描述|
|:-------------:|--|-----|
|[avet](https://github.com/govolution/avet)|C+shell+python|[仅针对Windows exe] AntiVirus Evasion Tool 运行于kali2|
|[Veil-Evasion](https://github.com/Veil-Framework/Veil-Evasion)|shell+C+python2.7|[可针对Windows exe] generate metasploit payloads that bypass common anti-virus solutions|
|[ASWCrypter](https://github.com/AbedAlqaderSwedan1/ASWCrypter)|shell+python|An Bash&Python Script For Generating Payloads that Bypasses All Antivirus so far FUD.实测无法过360ZhuDongFangyu|
|[tokyoneon/Armor](https://github.com/tokyoneon/Armor)|shell|[仅针对MacOS]Armor is a simple Bash script designed to create encrypted macOS payloads capable of evading antivirus scanners.|
|[csvoss/onelinerizer](https://github.com/csvoss/onelinerizer)|python|把Python代码给编译成一句话的形式工具 Convert any Python file into a single line of code.|
