### 构建高适应性的C2基础设施
Building resilient C2(command and control) infrastructures

* 前提 已经实现了对目标的 持久访问(Persistent access)
  * 备用低频方式(long-haul beacons)stage 1
    * 功能极简 - 主要做备用. 当所有的 日常高频方式(short-haul beacons) 都失败时 恢复对目标网络的访问
    * 隐蔽性高 - 低频率的回调(calls back) 以尽量不让防御者发现的方式运作的方式
  * 日常高频方式(short-haul beacons)stage 2
    * 功能完整 - 功能强大的RAT`Cobalt Strike``Empire``Slingshot`...
    * 隐蔽性低 - 攻击者日常使用的、较频繁的对目标网络进行操作的方式
* 健壮的C2基础设施 特征
  * channel多样性 - 多个channel更可靠（避免丢失目标网络的权限）
  * channel独立性 - 如果一个channel被发现，不会直接暴露其他channel（避免关联发现其他channel）
  * channel隐蔽性 - 避免各种方式的调查
