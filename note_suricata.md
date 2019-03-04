### Suricata默认规则集 - Emerging Threats Ruleset

* Emerging Threats Ruleset
  * BSD协议开源 由安全社区维护
* Emerging Threats Pro Ruleset
  * 收费规则集 由Proofpoint/ET研究团队维护

ET官方对规则类型的[解释说明](https://doc.emergingthreats.net/bin/view/Main/EmergingFAQ#What_is_the_general_intent_of_ea)





|规则名|更新频率|描述|
|:-------------:|-----|-----|
|Attack-Response Rules||捕获成功攻击后的响应. 如"id=root" 或 错误信息 |
|BotCC Rules||僵尸网络及其C2主机. |
|Compromised Rules|daily|已知被感染主机. 因为会明显增加传感器的负载 所以建议只使用BotCC规则. |
|DOS Rules||inbound DOS 以及 outbound指标(IoC)|
|DROP Rules|daily|主要是专业垃圾邮件发送者|
|DShield Rules||[DShield](http://www.dshield.org)公布的顶级攻击者|
