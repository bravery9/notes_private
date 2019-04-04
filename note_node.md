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

npm 的包安装分为
* 本地安装 local - 将安装包放在当前目录下的node_modules目录 (无该目录则创建)
  * `npm install <Module Name>`
  * 代码中通过`require()`引入本地安装的包
* 全局安装 global - 将安装包放在 /usr/local 下或者你 node 的安装目录
  * `npm install <Module Name> -g`
  * 可在命令行里直接使用模块

如使用 npm 命令安装常用的 Node.js web框架模块 express:
```
#local install - 本地安装express包 保存到"当前目录"下的 node_modules 目录中
npm install express

#local - 使用 express包 只需要直接引用 而无需指定包的路径
var express = require('express');
```


查看所有全局安装的模块:
```
npm list -g
```


### 包管理工具 - Yarn

现在还有更安全更强大的包管理器包管理器安装[Yarn](https://yarnpkg.com/)

Mac下通过Homebrew安装yarn
```
brew install yarn
```

如果你用了node多版本管理软件，如nvm(Node Version Manager)，则应排除安装Node.js(以便使用 nvm 的 Node.js 版本)
```
brew install yarn --without-node
```
