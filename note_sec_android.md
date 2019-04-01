### 常见漏洞

[android漏洞列表 安全知识库](http://appscan.360.cn/vulner/list/)

* AES/DES硬编码密钥
  * 风险描述: 使用AES或DES加解密时，采用硬编码在程序中的密钥
  * 危害描述: 通过反编译拿到密钥可以轻易解密APP"通信数据"
  * 修复建议: 密钥加密存储 或 用过变形后进行加解密运算
* 程序可被任意调试
  * 风险描述: 安卓AndroidManifest.xml文件中`android:debuggable`为`true`
  * 危害描述: app可以被任意调试。
  * 修复建议: AndroidManifest.xml文件中设置为`android:Debugable="false"`
* 程序数据任意备份
  * 风险描述: 安卓AndroidManifest.xml文件中`android: allowBackup`为`true`
  * 危害描述: app数据可以被备份导出
  * 修复建议: AndroidManifest.xml文件中设置为`android:allowBackup="false"`
* 随机数不安全使用
  * 风险描述: 调用SecureRandom类中的setSeed方法
  * 危害描述: 生成的随机数具有确定性，存在被破解的可能性
  * 修复建议: 使用`/dev/urandom`或者`/dev/random`来初始化伪随机数生成器
* 动态链接库中包含执行命令函数
  * 风险描述: 
  * 危害描述: 
  * 修复建议: 
* 未使用地址空间随机化技术
  * 风险描述: PIE(Position Independent Executables)是一种地址空间随机化技术，当so被加载时，在内存里的地址是随机分配的
  * 危害描述: 不使用PIE，将会使得shellcode的执行难度降低，攻击成功率增加
  * 修复建议: NDK编译so时 加入`LOCAL_CFLAGS := -fpie -pie` 开启对PIE的支持
* 1
  * 风险描述: 
  * 危害描述: 
  * 修复建议: 
* 2
  * 风险描述: 
  * 危害描述: 
  * 修复建议: 
* 3
  * 风险描述: 
  * 危害描述: 
  * 修复建议: 


#### 工具 - 反编译工具

|名称|属性|描述|
|:-------------:|--|-----|
|[jadx](https://github.com/skylot/jadx)|Java|17k★ Dex to Java decompiler [参考文章posts.specterops.io](https://posts.specterops.io/dont-you-forget-about-re-e2c92d67c641)|
|[Luyten](https://github.com/deathmarine/Luyten)|Java|2k★ Java Decompiler [Gui] |


#### APP自动化检测平台

>参考文章[移动APP漏洞自动化检测平台建设](https://security.tencent.com/index.php/blog/msg/109)

|名称|属性|描述|
|:-------------:|--|-----|
|[360显危镜](http://appscan.360.cn/)|web|自动扫描多种漏洞类型 完全免费|
