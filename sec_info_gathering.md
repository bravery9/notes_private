>[ Google Hacking Database - exploit-db.com](https://www.exploit-db.com/google-hacking-database)

# Google Hacking

### Google常规语法

```
搜素具体站点的url
site:so.com

url中存在具体字符的url
inurl:"php?id="

找相似的网站
related:bilibili.com

查找具体文件
filetype:pdf
```

```
用减法排除具有某些关键字的结果
site:apple.com -paper -siri -music -phone
```

[Google高级语法 - Google Guide](http://www.googleguide.com/or_operator.html)

### 例1 找web后台

```
site:github.com intext:管理|后台|登陆|用户名|密码|验证码|系统|帐号|manage|admin|login|system|portal
site:github.com inurl:manage|admin|login|system|portal
```
