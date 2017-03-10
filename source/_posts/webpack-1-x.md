title: webpack-1.x
date: 2017-02-15 17:25:04
tags:
---


## webpack

webpack在前端工程中越来越多见，当前流行的vue、react、weex等都推荐webpack作为打包工具。所以在这前端打包工具众多，但是没有一个最好用的时代，这应该是最值得去学习的前端打包工具。

### webpack是什么

{% blockquote  官方解释 https://webpack.github.io/docs/what-is-webpack.html %}
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.
{% endblockquote %}

{% img [what is webpack] http://om64pi295.bkt.clouddn.com/what-is-webpack.png %}

Webpack是一个模块打包工具，将包含有依赖关系的模块集打包合并。Webpack 不仅支持 CommonJs 和 AMD 的模块定义方式的Js，还可以将css、图片、文本等前端资源视为模板。

### 为什么要webpack
网站进化成Web app，交互越来越复杂，JavaScript文件体积越来越大。通过 `<script>`标签加载js容易引起冲突、阻塞加载等问题，虽然之后出现了RequireJs、Seajs等模块载入框架解决了以上问题，随着定义模块以及模块依赖的方法层出不穷，Webpack获得追捧 。Webpack不仅支持支持多种模块系统风格，而且也支持分段加载、延迟加载等功能，可谓集大成者。

## Webpack配置
**Webpack 的三个核心概念**

1.**loader**：通过各种资源转换器，将它们转换成对应模块引入
2.**chunk**：实现按需加载，避免Js文件过大导致阻塞加载。

### 安装配置
**第一步：Node.js**

webpack 是 Node 实现，首先需要到 Node.js 下载安装最新版本的 Node.js

**第二步：全局安装webpack-和webpack-dev-server**

```bash
// -g 参数表示全局安装
$ npm i -g webpack webpack-dev-server
```

**第三步：新建前端项目以及安装webpack**

````
├── index.html      // 入口 HTML  
├── main.js         // 入口 JS
````

````html
<html>
  <body>
    <script type="text/javascript" src="bundle.js"></script>
  </body>
</html>
````

````js
document.write('<h1>Hello World</h1>');
````

**第四步：在项目中安装webpack**
````bash
// 初始化 package.json,  根据提示填写 package.json 的相关信息
$ npm init

// 下载 webpack 依赖 
// --save-dev 表示将依赖添加到 package.json 中的 'devDependencies' 对象中
$  npm install webpack --save-dev
````

**第五步 调用**
**命令行调用**
````bash
webpack main.js
````

````bash
Hash: 000934e5d93f498db0f5
Version: webpack 1.14.0
Time: 49ms
    Asset     Size  Chunks             Chunk Names
bundle.js  1.57 kB       0  [emitted]  main
   [0] multi main 40 bytes {0} [built]
   [1] ./main.js 41 bytes {0} [built]
````

执行后，可在浏览器打开 index.html

**通过配置文件执行**

````bash
module.exports = {
  entry: './main.js',
  output: {
    filename: 'bundle.js'
  }
};

````

文件编译执行

````bash
webpack
````

内存编译执行

````bash
webpack-dev-server
````

一般我们都是通过配置文件投入生产，我们可以在配置指定多个入口文件、代码分离、暴露JS全局变量、编译CSS、压缩图片等等。阮老师做了一个 {% link webpack-demo https://github.com/ruanyf/webpack-demos webpack-demo %}写了很多简单的例子，是份不错的学习资料。所以这篇文章就不再介绍基本用法了。


## loaders


### 什么是loaders 
{% blockquote %}
Loaders are transformations that are applied on a resource file of your app. They are functions (running in node.js) that take the source of a resource file as the parameter and return the new source.
{% endblockquote %}

意思就是在webpack中，用过loader可以显示静态资源的转换。

### loader 功能

1. loader 管道：在同一种类型的源文件上，可以同时执行多个 loader ， loader 的执行方式可以类似管道的方式，管道执行的方式是从右到左的方式loader 可以支持同步和异步
2. loader 可以接收配置参数

3. loader 可以通过正则表达式或者文件后缀指定特定类型的源文件

4. 插件可以提供给 loader 更多功能

5. loader 除了做文件转换以外，还可以创建额外的文件

### loader 配置
在webpack.config.js 的module.loaders数组中新增一个loader配置。

一个 loader 的配置：
```` js
{
    // 通过扩展名称和正则表单时来匹配资源文件
    test: String,
    loader: String | Array
}

````
### 使用 loader

**第一步：安装**
loader 和 webpack 一样都是Node.js实现，发布到 npm 当中，需要使用loader的时候，只需要如下安装
````bash
$ npm install xx-loader --save-dev

// eg css loader
$ npm install css-loader style-loader --save-dev
````
**第二步：修改配置**
````js
{
    entry: {
        index: './src/index.js',
        a: './src/a.js'
    },
    output: {
        path: './dist/',
        filename: '[name].js'
    },
    module: {
        loaders: [{
            test: /\.js$/,
            exclude: /node_modules/,
            loader: 'babel',
            query: {
                presets: ['es2015', 'stage-0', 'react']
            }
        }, {
            test: /\.css$/, 
            loader: "style-loader!css-loader" 
        }]
    }
}
````

**第三步：使用**

前面我们已经使用过 jsx loader 了， loader 的使用方式有多种

1. 在配置文件中配置

2. 显示的通过 require 调用

3. 命令行调用

__显示的调用 require 会增加模块的耦合度，应尽量避免这种方式__


src/style.css

````css
body {
    background: red;
    color: white;
}
````
修改 webpack 配置 entry 添加
````js
entry: {
    index: ['./index.js', './style.css']
}
````
最终的编译结果会将  css 被转化为了 javascript。

另一种方法是直接 require，修改./index.js:
````js
var css = require("css!./style.css");
````
结果一样

### chunk
