### Node.js

Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine.

[V8 JavaScript engine](https://v8.dev/): Google的开源高性能JavaScript和WebAssembly引擎

[Node.js 官方API文档](https://nodejs.org/api/)

### 包管理工具 - npm

npm 是 node 的包管理器
Mac下通过Homebrew安装npm
```
brew install npm
```


使用npm安装 Node.js 模块:
```
npm install <Module Name>
```

npm 包安装 按文件保存位置分类
* 本地安装 local - 将安装包放在当前目录下的node_modules目录 (无该目录则创建)
  * `npm install <Module Name>`
  * 代码中通过`require()`引入本地安装的包
* 全局安装 global - 将安装包放在 /usr/local 下或者你 node 的安装目录
  * `npm install <Module Name> -g`
  * 可在命令行里直接使用模块
  * 查看所有全局安装的模块`npm list -g`


例1 本地安装
```
#local install - 使用npm本地安装express包 保存到"当前目录"下的 node_modules 目录中
npm install express

#local - 使用 express包 只需要直接引用 而无需指定包的路径
var express = require('express');
```


例2 指定npm仓库(国内镜像)
```
指定仓库 从国内镜像下载 cnpm
npm install -g cnpm --registry=https://registry.npm.taobao.org

#以后都可使用cnpm代替npm  如下载安装以下常用包 (i 是 install 的缩写)
cnpm install -g vue-cli
cnpm i -g vue
cnpm i -g webpack
cnpm i -g webpack-cli
```

### node软件

例1 fkill-cli （一个不错的结束进程的命令行程序)
```
#安装
npm install --global fkill-cli

#命令行下运行
fkill
```


例2 live-server(实现文件下载服务)
```
#安装
例2 下载安装live-server(实现文件下载服务)
npm i live-server -g

#命令行下运行
live-server
```


### 包管理工具 - Yarn

更强大的包管理器包管理器安装[Yarn](https://yarnpkg.com/)

Mac下通过Homebrew安装yarn
```
brew install yarn
```

如果你用了node多版本管理软件，如nvm(Node Version Manager)，则应排除安装Node.js(以便使用 nvm 的 Node.js 版本)
```
brew install yarn --without-node
```
