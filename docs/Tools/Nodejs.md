# Nodejs

## 安装

```bash
# 安装 nvm
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
# 列出 版本
nvm ls-remote --lts
# 安装 node
nvm install 12.7.0
# 安装 docsify
npm i -g docsify-cli 
```

使用缓存安装 `npm --cache-min Infinity install <package>`

Windows 安装包：<https://nodejs.org/download/release/v6.9.0/node-v6.9.0-x64.msi>

**Node.js:** 一门后端语言，提供了JavaScript的运行时环境

**nvm(node version manager):** node的版本管理器

**npm(node package manager):** node的包管理器

* 引入第三方模块
* 封装并发布模块

**权限问题：**NodeJS监听80/443端口提供HTTP\(S\)服务时需要ROOT权限：`sudo node server.js`

**模块：**每个文件就是一个模块，文件路径就是模块名

```bash
require 在当前模块中加载和使用其他模块，传入一个模块名，返回一个模块导出对象
        模块名使用绝对路径/相对路径，.js后缀可以省略，可以加载json文件获得json对象
    var obj = require('obj.js');
    var data = require('data.json');
exports  当前模块的导出对象，用于导出当前模块的公共方法和属性，其他模块require的就是其exports对象
    exports.hello = function(){
        console.log("Hello World");
    }
module   通过module对象可以访问当前模块的相关信息，常用于替换当前模块的导出对象
// 模块导出对象默认是普通对象，使用module将其替换成函数
    module.exports = fucntion(){
        console.log("Hello World");
    }
```

**模块初始化：**模块中的JS代码仅在模块第一次被使用时执行一次，并在执行过程中初始化模块的导出对象。之后，缓存起来的导出对象被重复利用。

**主模块：**通过命令行参数传递给node以启动程序的模块称为主模块，主模块用于调度其他模块完成工作

Node应用的配置文件`package.json`描述工程 _本身的基本信息_ 和 _依赖的模块的信息_

```bash
{
    "name": "application-name"
    "version": "0.1.0"
    "dependencies": {
        // 生产环境依赖
    },
    "devDependencies" : {
        // 开发环境依赖
    }
}
```

新建Node应用程序：`npm init`

```bash
// 默认参数配置文件.npmrc

// 自定义 npm init 参数
npm set init.author.name "s3kj-mwy"
npm set init.author.email "s3kj.mwy@gmail.com"
npm set init.license "MIT"
npm set init.version "0.1.0"
// 自定义 npm init 提问的问题
module.exports = prompt("coustom question")

// 完全自定义 npm init 过程
vim ~/.npm-init.js
```

安装并添加生产环境第三方依赖：`npm install <package> --save`

安装并添加开发环境第三方依赖：`npm install <package> --save-dev`

安装`package.json`记录的所有依赖到`node_modules`目录：`npm install`

安装`package.json`记录的所有依赖到用户主目录：`npm install -g`

仅仅安装depencies模块：`npm install --production`

`npm install npm@latesst -g`

适时修改`npm link`

登录`npm login`

发布`npm publish`

**版本管理**

```bash
MAJOR.MINOR.PATCH
MAJOR: 进行了不兼容的API改动
MINOR: 添加了向后兼容的新特性
PATCH: 进行了向后兼容的bug修复
```

```bash
npm install npm@latesst -g
```