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

https://github.com/prahladyeri/http-live-simulator/commit/354644525f1626c5921abac10913c0d47f1f1433

可实现任意文件读取
```
http://localhost:8080//../../../../etc/passwd
```

第1次修复 将`/../`替换为空 仅替换一次(绕过方式 `/./.././` )
```
pathname = pathname.replace("/../",""); //fix for path traversal bug

//bypass http://localhost:8080/./../././../././../././.././etc/passwd
```

第2次修复 将`/../`替换为空 循环替换 直到不存在`/../`为止
```
while(pathname.indexOf("/../") != -1) {
		pathname = pathname.replace("/../",""); //fix for path traversal bug
	}
```
