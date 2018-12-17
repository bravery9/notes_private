# Phishing

#### 钓鱼的意义
直接进内网（国外很多高明的apt攻击都从钓鱼开始，掌握了技巧成功率很高）

#### 钓鱼方式

|攻击payload|方式|针对目标环境|描述|
|:-------------:|--|--|-----|
|含恶意宏的Office文档|邮件|目标用windows读邮件|Microsoft Office documents containing malicious macros|

#### 钓鱼的工具

|名称|属性|运行环境|描述|
|:-------------:|--|--|-----|
|[gophish](https://github.com/gophish/gophish)|Golang|all|2k★ Open-Source Phishing Toolkit. 用于对企业进行定期的钓鱼测试. 启动后先在`Sending Profiles`中配置真实可用的`mail server`,发起一个Campaign:钓鱼邮件(内含钓鱼网站) [使用视频](https://www.youtube.com/watch?v=knc6Iq-hNcw)|
|[blackeye](https://github.com/flagellantX/blackeye)|.sh .php|/| 钓鱼页面Phishing Pages 含各大网站`Facebook,Google,Twitter,Microsoft`等|
|[kgretzky/evilginx2](https://github.com/kgretzky/evilginx2)|Golang|linux|MITM attack framework used for phishing.|

其他辅助工具

|名称|描述|
|:-------------:|-----|
|https://www.irongeek.com/homoglyph-attack-generator.php|外观相似的替换字符 常用于钓鱼|
|https://emkei.cz/|在线发送伪造邮件(可用于钓鱼 如伪造邮件发送者xx@163.com发送到目标的qq邮箱)|
