### 规则集

|规则集名|描述|
|:-------------:|-----|
|Suricata PT Open Ruleset|[ptresearch/AttackDetection](https://github.com/ptresearch/AttackDetection)|
|Emerging Threats Ruleset|https://rules.emergingthreats.net/open/suricata/rules/|

### Suricata默认规则集 - Emerging Threats Ruleset

* Emerging Threats Ruleset
  * BSD协议开源 由安全社区维护
* Emerging Threats Pro Ruleset
  * 收费规则集 由Proofpoint/ET研究团队维护

ET官方对规则类型的[解释说明](https://doc.emergingthreats.net/bin/view/Main/EmergingFAQ#What_is_the_general_intent_of_ea)


|规则名|更新频率|描述|
|:-------------:|-----|-----|
|Attack-Response Rules||捕获成功攻击后的响应. 如"id=root" 或 错误信息|
|BotCC Rules||僵尸网络及其C2主机. |
|Compromised Rules|daily|已知被感染主机. 因为会明显增加传感器的负载 所以建议只使用BotCC规则|
|DOS Rules||inbound DOS 以及 outbound指标(IoC)|
|DROP Rules|daily|主要是专业垃圾邮件发送者|
|DShield Rules||[DShield](http://www.dshield.org)公布的顶级攻击者|
|Exploit Rules||检测exploits如windows exploit. 而sql注入等漏洞的检测不在这里|
|Game Rules||检测网络游戏 如Starcraft等 都是无害的流量|
|Malware Rules||恶意软件规则 最重要的规则|
|P2P Rules||检测P2P流量 如Bittorrent|
|Scan Rules||检测侦察探测类的流量 如NessusNikto portscanning等|
|Web Rules||检测Web攻击流量 如SQLi等. 负载非常合理|
|Web-SQL-Injection Rules||大型规则集 细化到Web攻击的确切信息. 如 web应用程序 Web服务器 等|
|...|||

### 规则管理

使用Suricata-Update对Suricata的规则集进行统一管理，比如更新、启用、停用等

[suricata-update documentation](https://suricata-update.readthedocs.io/en/latest/)

* 安装
  * `pip install --pre --upgrade suricata-update`
* 更新Suricata规则集的库
  * `suricata-update`
* 更新一下 更新源
  * `suricata-update update-sources`
* 列出源的列表
  * `suricata-update list-sources`
* 启用某规则集
  * `suricata-update enable-source ptresearch/attackdetection` `suricata-update`
* 列出已开启的源
  * `suricata-update list-enabled-sources`
* 关闭某个源
  * `suricata-update disable-source et/pro`
* 删除某个源
  * `suricata-update remove-source et/pro`
  
