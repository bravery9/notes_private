>参考作者介绍[Hacking macOS: How to Create an Undetectable Payload « Null Byte :: WonderHowTo](https://null-byte.wonderhowto.com/how-to/hacking-macos-create-undetectable-payload-0189715/)

### 原理概述

准备工作：预先创建一个MacOS下可执行的常规raw payload(shell command)

|顺序|网络|攻击机(使用Armor工具)|靶机|
|:-------------:|--|--|-----|
|1加密||使用密钥对payload进行加密，得到enc.txt||
|2待解密|攻击机监听443端口-与victim无通信流量|使用Ncat提供一个端口 让victim访问 victim获取解密密钥enc.key 以实现payload解密||
|3解密|攻击机监听443端口-与victim无通信流量|Ncat监听443端口|执行enc.txt 第1步获取解密密钥:连接到攻击机的443端口 获取解密密钥enc.key 第2步:解密成功并执行 - 常规raw payload(shell command)|
|4结束|攻击机关闭443端口-与victim通信该过程是https(抓到流量也无法得到解密密钥)|用于密钥传输的端口(默认443)只会被连接1次 传输完密钥后立即关闭||

### 工具

|名称|属性|描述|
|:-------------:|--|-----|
|[tokyoneon/Armor](https://github.com/tokyoneon/Armor)|shell|[仅针对MacOS]Armor is a simple Bash script designed to create encrypted macOS payloads capable of evading antivirus scanners.|
