# Phishing

#### 钓鱼的意义
钓鱼成功即进入办公内网（国外很多高明的apt攻击都从钓鱼开始，掌握了技巧成功率很高）

[2018年全球十大APT攻击事件盘点 - 安全客，安全资讯平台](https://www.anquanke.com/post/id/169007)

>[腾讯安全2018年高级持续性威胁（APT）研究报告 - 腾讯安全](https://s.tencent.com/research/report/623.html)
2018年，鱼叉攻击依然是APT攻击的最主要方式，使用鱼叉结合社工类的方式，投递带有恶意文件的附件，诱使被攻击者打开。虽然该方式攻击成本极低，但是效果却出人意料的好。这也进一步体现了被攻击目标的人员的安全意识亟需加强。从曝光的APT活动来看，2018年使用鱼叉攻击的APT活动比例高达95%以上。

#### 钓鱼方式

|攻击payload|方式|针对目标环境|描述|
|:-------------:|--|--|-----|
|含恶意宏的Office文档|邮件|用windows系统读邮件|Microsoft Office documents containing malicious macros|

#### 钓鱼文案

* 社会高度关注的热点
  * `xx暴恐事件最新通报`
  * `公务员工资收入改革方案`
* 针对行业的信息
  * `xx最新调整`
* 针对个人的社工信息
  * `[xxx.com]您的账号在异地登录 请尽快修改密码`


邮件内容的“样子” 应尽量模拟官方的风格:
>[针对银行钓鱼事件的分析 - FreeBuf互联网安全新媒体平台](https://www.freebuf.com/articles/web/129079.html)
图片链接 点开跳进钓鱼站点
钓鱼页面上所有的联系电话，都为Santander银行的真实电话，整个页面的布局也完全与银行的界面类似
内容是在警告用户，他们的计算机上的安全模块已过期，如果不及时更新到最新版本，银行将向他们发出246.67BRL的罚单（约$80.00）
  
#### 无文件攻击

|名称|描述|针对目标|
|--|--|-----|
|[无文件攻击的现实实例FCL(Fileless Command Lines)](https://github.com/chenerlich/FCL)|包括:恶意软件名称/执行过程/恶意命令行/正则表达式用于检测/技术报告/沙箱报告链接/笔记|windows|


#### 钓鱼的工具

|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[gophish](https://github.com/gophish/gophish)|Golang|all|2k★ Open-Source Phishing Toolkit. 用于对企业进行定期的钓鱼测试. 启动后先在`Sending Profiles`中配置真实可用的`mail server`,发起一个Campaign:钓鱼邮件(内含钓鱼网站) [使用视频](https://www.youtube.com/watch?v=knc6Iq-hNcw)|
|[blackeye](https://github.com/flagellantX/blackeye)|.sh .php|/| 钓鱼页面Phishing Pages 含各大网站`Facebook,Google,Twitter,Microsoft`等|
|[UndeadSec/SocialFish](https://github.com/UndeadSec/SocialFish)|python3|/|终极的 Ultimate phishing tool. 使用xx.ngrok.io得到公网域名 含各大网站`Facebook,Google,Twitter,LinkedIn,GitHub,StackOverflow,Instagram`等|
|[kgretzky/evilginx2](https://github.com/kgretzky/evilginx2)|Golang|linux|MITM attack framework used for phishing.|

其他辅助工具

|名称|描述|
|:-------------:|-----|
|https://www.irongeek.com/homoglyph-attack-generator.php|外观相似的替换字符 常用于钓鱼|
|https://emkei.cz/|在线发送伪造邮件(可用于钓鱼 如伪造邮件发送者xx@163.com发送到目标的qq邮箱)|


防御工具

|名称|描述|
|:-------------:|-----|
|http://spf.myisp.ch/|#防御 SPF Record Checker. 邮件服务器SPF配置检查.|
