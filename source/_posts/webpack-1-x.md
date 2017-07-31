title: webpack-1.x 总结
date: 2016-02-15 17:25:04
tags:
- webpack
- webpack1.x
categories:
- 前端构建工具
---


## webpack

webpack在前端工程中越来越多见，当前流行的vue、react、weex等都推荐webpack作为打包工具。所以在这前端打包工具众多，但是没有一个最好用的时代，这应该是最值得去学习的前端打包工具。
<!--more-->
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

```
├── index.html      // 入口 HTML  
├── main.js         // 入口 JS
```

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


## Chunk
### Chunk是什么？
webpack中 Chunk 实际上就是输出的 .js 文件，可能包含多个模块，主要的作用是为了优化异步加载。
### Chuck包含了哪些内容
* 同步情况下：一个 Check 会把模块中的所有依赖都加载到 Chunk 中
* 异步情况下：所有被切割点分开的依赖被加载到一个 Chunk

**require.ensure跟require都会被加载到一个 Chunk中**

### Chunk 分类
第三方库不需要打包到发布的文件中，这是几需要vendor，将第三方库打包成一个chunk。

webpack将chunk类型分为三种**Entry chunk**，**Normal chunk**，**Initial chunk**。
**Entry Chunk**
包括两部分代码：webpack运行代码（如webpackJsonp, __webpack_require__ 等函数）和模块代码。

**Normal Chunk**
只包含模块代码

**Initial  Chunk**
本质上为Normal Chunk。但是他计算载入时间，比Normal Chunk更重要。一般在使用 CommonsChunkPlugin 时出现。

webpack 可以将代码切割成不同的 chunk，实现按需加载。


## loaders


### 什么是loaders 
{% blockquote %}
Loaders are transformations that are applied on a resource file of your app. They are functions (running in node.js) that take the source of a resource file as the parameter and return the new source.
{% endblockquote %}

意思就是在webpack中，通过loader可以显示静态资源的转换。

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
    loader: String | Array,
    query: String | Object
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

## 常用Loaders
### 加载 CSS
加载css需要 `css-loader`和`style-loader`，分别做以下两件事：
1. css-loader 会遍历 CSS 文件，然后找到 url() 表达式然后处理他们
2. style-loader 会把原来的 CSS 代码插入页面中的一个 style 标签中
````js
{
  // loader配置
    test: /\.css$/,
    loader: 'style!css' // 如果同时使用多个加载器，中间用 ! 连接，加载器的执行顺序是从右向左
  }
````
### 图片处理
图片处理需要 `url-loader` 和 `file-loader`
````js
{
  // loader配置
  test: /\.(png|jpg|gif|jpeg)$/,
  loader: 'url?limit=25000'
}
````
传入的 limit 参数是告诉它图片如果不大于 25KB 的话要自动在它从属的 css 文件中转成 BASE64 字符串。

#### eslint
````js
  module : {
    preLoaders: [
        {test: /\.js$/, loader: "eslint-loader", exclude: /node_modules/}
    ],
  }
````

## 常用Plugin
###UglifyJsPlugin webpack自带的插件
一般配置如下：
````js
  plugins: [
    new webpack.optimize.UglifyJsPlugin({
          compress: {
              warnings: false
          }
      })
  ]
````

### extractTextWebpackPlugin
在webpack中，可以通过require引入css，通过loader对文件自动解析并打包文件。通常会将css以在页面的header切入style形式加载样式。但是我们如果你想通过外链形式加载css的话，通过extract-text-webpack-plugin就可以办到。
````js
var ExtractTextPlugin = require("extract-text-webpack-plugin");
plugins: [
  new ExtractTextPlugin("app.css")
]
````

### htmlWebpackPlugin
生成HTML

````js
const path = require('path');

const HTMLWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: {
    index: './pages/index.js',
    page1: './pages/page1.js',
    page2: './pages/page2.js'
  },
  output: {
    path: path.join(__dirname, 'dist'),
    filename: '[name].js'
  },
  plugins: [
    new HTMLWebpackPlugin({
      filename: 'index.html',
      template: 'templates/index.html',
      inject: true,
      chunks: ['index']
    }),
    new HTMLWebpackPlugin({
      filename: 'page1.html',
      template: 'templates/page1.html',
      inject: true,
      chunks: ['page1']
    }),
    new HTMLWebpackPlugin({
      filename: 'page2.html',
      template: 'templates/page2.html',
      inject: true,
      chunks: ['page2']
    })
  ]
};
````


#### 提取公共Js插件
通过 `CommonsChunkPlugin` 可以将个模块的公共依赖单独打包成一个 chunk，这时webpack的运行代码会被移到`common chunk` 中，原来的 `entry chunk` 也降变为 `initial chunk`。

`entry vendor`配合`CommonsChunkPlugin`使用，可以分离第三方库和app代码。

````js
entry: {
   app: './app.js',
   vendor: ['jquery', 'lodash']
},
plugins: {
    new webpack.optimize.CommonsChunkPlugin('vendor', 'vendor.bundle.js')   
}
````

这样子的话，app.js 只包含依赖的JS，但是对第三方依赖的都被排除掉了。第三方库被打包成 `vendor.bundle.js`。

**CommonsChunkPlugin配置项：**

- names: chunk的名称，字符串或数组。
- filename: chunk文件名称，默认为output.filename或者output.chunkFilename
- minChunks 被几个chunk调用的moudule才会加入common chunk中，最小值为2。如果设置为Infinity，则不会有module加入到common chunk中
chunks: 需要提前common的源文件，默认为全部入口文件。
- children: 如果设置为 `true`，所有  公共chunk 的子模块都会被选择
- async:  如果设置为 `true`，一个异步的  公共chunk 会作为 `options.name` 的子模块，和 `options.chunks` 的兄弟模块被创建。 它会与 `options.chunks` 并行被加载。可以通过提供想要的字符串，而不是 `true` 来对输出的文件进行更换名称。
- minSize: 在 公共chunk 被创建立之前，所有 公共模块 (common module) 的最少大小。

#### ProvidePlugin插件
将模块暴露到全局

````js
new webpack.ProvidePlugin({
    "R": "report",
}),
````

#### 删除目录插件
clean-webpack-plugin
````js

  var CleanPlugin = require("clean-webpack-plugin");
  plugins: [
    new CleanPlugin(['dist']),
  ]
````
#### 拷贝文件插件

copy-webpack-plugin
````js
var CopyWebpackPlugin = require('copy-webpack-plugin');
plugins: [
  new CopyWebpackPlugin([{
    from: __dirname + '/src/public'
  }])
]
````


#### 优化第三方包插件
````js
plugins: [
  new webpack.DefinePlugin({
      //去掉react中的警告，react会自己判断
      'process.env': {
          NODE_ENV: '"production"'
      }
  })
]
````

#### 自动打开浏览器插件
open-browser-webpack-plugin
````js
  // 自动打开浏览器插件
  var OpenBrowserPlugin = require('open-browser-webpack-plugin');
  plugins: [
      new OpenBrowserPlugin({url: 'http://localhost:8080/', browser: 'chrome'})
  ]
````

plugin 为 webpack 提供了更多的自定义功能。
就不一一列举了，点击
 {% link webpack-plugins https://github.com/webpack-contrib/awesome-webpack#webpack-plugins %}


### Resolve属性
webpack 在构建包的时候会按配置进行模块的查找
````js
 resolve: {
      //查找module的话从这里开始查找
      root: '/pomy/github/flux-example/src', //绝对路径
      //自动扩展文件后缀名，意味着我们require模块可以省略不写后缀名
      //注意一下, extensions 第一个是空字符串! 对应不需要后缀的情况.
      extensions: ['', '.js', '.json', '.scss',’jsx’],

      //模块别名定义，方便后续直接引用别名，无须多写长长的地址
      alias: {
          AppStore : 'js/stores/AppStores.js',//后续直接 require('AppStore') 即可
          ActionType : 'js/actions/ActionType.js',
          AppAction : 'js/actions/AppAction.js'
      }
  }
````



### Externals属性
外部依赖不需要打包进 bundle，当我们想在项目中 require 一些其他的类库或者 API ，而又不想让这些类库的源码被构建到运行时文件中，这在实际开发中很有必要。 比如：在页面里通过 script 标签引用了 jQuery：`<script src="//code.jquery.com/jquery-1.12.0.min.js"></script>`，所以并不想在其他 js 里再打包进入一遍，比如你的其他 js 代码类似：

其实就是不是通过require或者import引入的，而是直接写在html中的js地址。

````js
    // 配置了这个属性之后 react 和 react-dom 这些第三方的包都不会被构建进 js 中，那么我们就需要通过 cdn 进行文件的引用了
    // 前边的这个名称是在项目中引用用的，相当于 import React from 'react1' 中的 react
    externals: {
        'react1': 'react',
        'react-dom1': 'react-dom',
        '$1': 'jQuery'
    }
````

这样用了 externals 属性时不用分离插件了，作用是这里引的插件不会被 webpack 所打包。要么用 cdn 要么需要 webpack 打包。

### noParse属性
module.noParse 是 webpack 的另一个很有用的配置项，如果确定一个模块中没有其他新的依赖项就可以配置这个像，webpack 将不再扫描这个文件中的依赖。
````js
  module: {
    noParse: [/moment-with-locales/]
  }
````