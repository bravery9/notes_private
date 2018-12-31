# Phishing

#### 钓鱼的意义
钓鱼成功即进入办公内网（国外很多高明的apt攻击都从钓鱼开始，掌握了技巧成功率很高）

#### 钓鱼方式

|攻击payload|方式|针对目标环境|描述|
|:-------------:|--|--|-----|
|含恶意宏的Office文档|邮件|用windows系统读邮件|Microsoft Office documents containing malicious macros|

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
