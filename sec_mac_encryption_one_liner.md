>参考作者介绍[Hacking macOS: How to Create an Undetectable Payload « Null Byte :: WonderHowTo](https://null-byte.wonderhowto.com/how-to/hacking-macos-create-undetectable-payload-0189715/)

### 工具

|名称|属性|针对目标|描述|
|:-------------:|--|---|----|
|[tokyoneon/Armor](https://github.com/tokyoneon/Armor)|shell|MacOS|Armor is a simple Bash script designed to create encrypted macOS payloads capable of evading antivirus scanners.|

### 原理概述

准备工作：预先创建一个MacOS下可执行的常规raw payload(shell command)

|顺序|攻击机(使用Armor工具)|网络|靶机|
|:-------------:|--|--|-----|
|1加密|使用密钥对payload进行加密，得到enc.txt|攻击机的443端口与victim无通信流量|/|
|2待解密|Ncat监听443端口 以供victim访问并获取解密密钥enc.key|攻击机的443端口与victim无通信流量|/|
|3解密|Ncat监听443端口|攻击机的443端口与victim通信该过程是https(抓到流量也无法得到解密密钥)|执行enc.txt 第1步获取解密密钥:连接到攻击机的443端口 获取解密密钥enc.key 第2步:解密成功并执行 常规raw payload(shell command)|
|4结束|Ncat结束监听443端口 用于密钥传输的端口(默认443)只会被连接1次 传输完密钥后立即关闭|攻击机的443端口与victim无通信流量|/|

### 效果

* 网络 流量分析无法获取密钥(https通信)
* 终端 杀软无法解密得到加密命令的含义(无密钥)
