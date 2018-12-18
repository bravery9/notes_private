>[ Google Hacking Database - exploit-db.com](https://www.exploit-db.com/google-hacking-database)

# Google Hacking

### Google常规语法

```
使用引号搜索完全匹配的结果
"tallest building"

搜索未知字词
"largest * in the world"

搜素具体域名站点下的url
site:so.com

url中存在具体字符的url
inurl:"php?id="
inurl:.edu

url中存在具体关键字(所有关键字都必须存在 忽略符号 如/ )的url
allinurl:foo bar

在html body中存在特定关键字(多个关键字之一的都符合)的url
intext:seo blog

在html body中存在特定关键字(allintext 多个关键字同时存在才行)的url
allintext:seo blog

搜索社交媒体 如
@twitter

找相似的网站
related:bilibili.com

查找具体文件
filetype:pdf
filetype:xls inurl:pass user
```

```
用减法排除具有某些关键字的结果
site:apple.com -paper -music -inurl:https -inurl:.edu
site:apple.com -site:www.apple.com -inurl:support
```

[优化网页搜索 - Google 搜索帮助](https://support.google.com/websearch/answer/2466433)

[Advanced Searching in Google - Resources and Search Strategies](https://sites.google.com/site/resourcesandsearchstrategies/google/advanced-searching-in-google)

[Google高级语法（非官方） - Google Guide](http://www.googleguide.com/or_operator.html)

### 例1 找web后台

```
管理 后台 登陆 用户名 帐号 密码 验证码 系统 inurl:manage|admin|login|system|portal
site:github.com inurl:manage|admin|login|system|portal
```
