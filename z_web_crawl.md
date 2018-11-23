### 爬虫的分类

按架构
* 单任务版
* 并发版
* 分布式版

按类型
* 静态页面爬虫 (不能解析JavaScript)
* 动态页面爬虫 (能够解析JavaScript 如selenium模拟浏览器)

### 静态爬虫 - 内容提取利器 - GraphQuery

项目地址https://github.com/storyicon/graphquery

参考文章https://juejin.im/entry/5bdab426f265da393d4af97e


GraphQuery 是一个文本查询语言 特点：

* 不依赖于任何后端语言，可以被任何后端语言调用

* 统一解析结果:某一段 GraphQuery 查询语句，在任何语言中有相同的解析结果

* 内置多种解析器: xpath选择器，css选择器，jsonpath 选择器 和 正则表达式 ，以及足量的文本处理函数，结构清晰易读，能够保证 数据结构、解析代码、返回结果 结构的一致性


### 小结

GraphQuery让开发者从这些重复繁琐的解析逻辑中解脱（不同的语言、不同的库的解析结果存在差异），写出高可读性、高可移植性、高可维护性的代码。

欢迎实践、持续关注与代码贡献，一起见证 GraphQuery 与开源社区的发展！

