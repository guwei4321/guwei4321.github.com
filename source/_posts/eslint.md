title: Eslint 小结
date: 2017-08-04 15:28:56
tags:
- JavaScript
- JS工具
---

一般团队内为了促进团队协作和降低维护成本，都会制定一套代码规范。而Eslin能帮我们辅助编码规范的执行，幼儿有效控制项目的质量。
<!-- more -->
## 什么是Eslint
ESLint最初是由[Nicholas C. Zakas](https://www.nczonline.net/)于2013年6月创建的开源项目。它的目标是提供一个插件化的javascript代码检测工具。
### Eslint的优势
- 可以自定义规则
- 规则可以配置成off、warn、error三种状态
- 支持插件扩展


## 如何使用
首先得安装
````bash
npm install -g eslint
````

使用：
````bash
eslint file1.js file2.js
````

有两种主要的方式来配置ESlint：
- 在JavaScript中添加注释格式的配置信息
- 使用 `.eslintrc` 文件或者在 `package.json` 添加配置

PS:  `.eslintrc` 放在根目录，则会应用到整个项目；如果子目录也包含 `.eslintrc` 文件，则子目录会忽略根目录的配置文件，使用该目录中的配置文件。

## 配置规则

- env：定义JS的使用环境。譬如：`browser `表示在浏览器中使用；`node `表示在node中使用；`commonjs `表示使用了CommonJS模块规范;`es6 `表示支持es6特性;`amd`表示使用`require()`以及`define()`;
- globals： 定义未在文件中定义的全局变量，譬如在代码中使用了 `WeixinJSBridge`,但是我们不可能在文件中定义 `WeixinJSBridge`，所以这种情况下只需要定义在globals里。
- plugins： 使用第三方插件。
- rules： 核心部分。具体哪些可以配置可查看[官网说明](http://eslint.org/docs/rules/)ESlint的规则定义
    - "off" 或 0 - 关闭规则
    - "warn" 或 1 - 开启规则，使用警告级别的错误：warn (不会导致程序退出)
    - "error" 或 2 - 开启规则，使用错误级别的错误：error (当被触发的时候，程序会退出)

市面上很多编辑器都有ESlint的插件，各种JS的打包工具也有各自版本，所以使用起来很方便。如果你想得到Js报告的话，可以使用命令导出，譬如 `eslint -f html --ext .js File -o ./test.html` 就导出html格式的File文件夹内所有.js结尾的文件的报告。
