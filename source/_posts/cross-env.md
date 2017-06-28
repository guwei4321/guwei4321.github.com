title: 跨平台设置NODE_ENV
date: 2017-06-28 13:53:51
tags:
- 技术细节 cross-dev
categories:
- nodeJs
---

## 使用cross-env解决跨平台设置NODE_ENV
今天把之前写的webpack打包程序从自己电脑（mac）拷贝到公司电脑（windows）使用，运行 `npm start`，报如下错误：
<!--more-->
````bash
NODE_ENV=development webpack  --progress
'NODE_ENV' 不是内部或外部命令，也不是可运行的程序
或批处理文件。

npm ERR! Windows_NT 6.1.7601
npm ERR! argv "C:\\Program Files\\nodejs\\node.exe" "d:\\Users\\wei.gu\\AppData\\Roaming\\npm\\node_modules\\npm\\bin\\n
pm-cli.js" "run" "dev"
npm ERR! node v6.10.3
npm ERR! npm  v3.4.0
npm ERR! code ELIFECYCLE
npm ERR! webpack@1.0.0 dev: `NODE_ENV=development webpack  --progress`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the webpack@1.0.0 dev script 'NODE_ENV=development webpack  --progress'.
npm ERR! Make sure you have the latest version of node.js and npm installed.
npm ERR! If you do, this is most likely a problem with the webpack package,
npm ERR! not with npm itself.
npm ERR! Tell the author that this fails on your system:
npm ERR!     NODE_ENV=development webpack  --progress
npm ERR! You can get their info via:
npm ERR!     npm owner ls webpack
npm ERR! There is likely additional logging output above.

npm ERR! Please include the following file with any support request:
npm ERR!     D:\learn\webpack2\npm-debug.log
````
意思是说，windows不支持通过 `NODE_ENV`的来设置环境变量（默认为`development`）。
webpack打包系统希望通过检查NODE_ENV来分别对开发环境和生成环境做不同处理，但是windows下报了如上的错误。

### 解决方法
后来网上查了查知道不同系统通过`NODE_ENV`设置环境变量命令是不同的。
- linux && mac:
````bash
NODE_ENV=production
````
- windows:
````bash
set NODE_ENV=production
````
通过google找到了解决方案：{% link cross-env https://www.npmjs.com/package/cross-env  %}
### 使用方法

- 安装cross：`npm install cross-env --save-dev`
- 在`NODE_ENV=xxx`前面加上`cross-env`

这样就OK了。


