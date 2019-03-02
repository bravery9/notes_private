### Redis简介

Redis(REmote DIctionary Server) 是一个使用ANSI C语言编写的（BSD协议开源）高性能 key-value 数据库。

* Redis特点
  * 基于内存且可**数据持久化** 可将内存中的数据保存在磁盘中便于再次加载
  * Redis支持多种key-value类型的数据，还有**list，set，hash等数据结构**的存储
  * Redis支持数据的备份 即master-slave模式的数据备份
  * 提供多种语言的API


所以redis通常被称为数据结构服务器，适应不同场景下的存储需求，并借助许多高级的接口使其可以胜任如缓存、队列系统等不同的角色。

### Redis 优势
* 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s
* 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作
* 原子 – Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行
* 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性


### 存储结构

数据结构：字典/映射/关联数组

`dict["key"]="value"`

字典结构变量 `dict`
键名 字符串`key`
键值 字符串`value`


在字典中我们可以获取或设置键名对应的键值，也可以删除一个键。

Redis是REmote DIctionary Server（远程字典服务器）的缩写，redis以字典结构存储数据，并允许其他应用通过TCP协议读写字典中的内容

同大多数脚本语言中的字典一样，Redis字典中的 value 除了字符串，还可以是其他数据类型。

* Redis支持的 value键值 数据类型如下：
  * 字符串类型 String
  * 散列类型 hash
  * 列表类型 list
  * 集合类型 sets
  * 有序集合类型 sorted sets
