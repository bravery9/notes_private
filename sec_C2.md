### 构建高适应性的C2基础设施
Building resilient C2(command and control) infrastructures

* 持久访问方式(Persistent access)
  * 备用低频方式(long-haul beacons)stage 1
    * 功能极简 - 主要做备用. 当所有的 日常高频方式(short-haul beacons) 都失败时 恢复对目标网络的访问
    * 隐蔽性高 - 低频率的回调(calls back) 以尽量不让防御者发现的方式运作的方式
  * 日常高频方式(short-haul beacons)stage 2
    * 功能完整 - 功能强大的RAT`Cobalt Strike` `Empire` `Slingshot`...
    * 隐蔽性低 - 攻击者日常使用的、较频繁的对目标网络进行操作的方式
* 健壮的C2基础设施 特征
  * channel多样性 - 具有多个channel的持久访问 稳定性强（避免丢失目标网络的权限）
  * channel独立性 - 如果一个channel被发现，不会直接暴露其他channel（避免关联发现其他channel）
  * channel隐蔽性 - 加密的通信流量（避免各种方式的调查）

### 具体方案1 - DNS over HTTPS (DoH)

[RFC 8484 - DNS Queries over HTTPS (DoH)](https://tools.ietf.org/html/rfc8484)

DNS over HTTPS (DoH)允许通过https协议进行DNS解析.

DNS over HTTPS (DoH) allows for DNS resolution via the HTTPS protocol.

* 从攻击者的角度看DoH的优势
  * 自定义内容 响应中有部分内容是攻击者可控的
  * “白”域名 如`Google Public DNS` https://dns.google.com/resolve?name=qq.com&type=TXT
  * 流量加密 `Over an SSL-encrypted channel`且许多已实施SSL检查的客户因各种原因（Google产品中的证书，流量负载，隐私等）将所有`Google`排除在检查范围之外。
  

httpGET https://dns.google.com/resolve?name=qq.com&type=TXT
response:
其中data字段中的内容可控
```
{
  "Status": 0,
  "TC": false,
  "RD": true,
  "RA": true,
  "AD": false,
  "CD": false,
  "Question": [
    {
      "name": "qq.com.",
      "type": 16
    }
  ],
  "Answer": [
    {
      "name": "qq.com.",
      "type": 16,
      "TTL": 3329,
      "data": "\"v=spf1 include:spf.mail.qq.com -all\""
    }
  ]
}
```

所以`DoH via Google`是一个触发payload运行的理想channel
