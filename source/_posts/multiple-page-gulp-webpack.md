title: multiple-page-gulp-webpack
date: 2016-06-20 16:50:37
tags:
---

# 入口文件配置：entry参数
~~~javascript
entry: {
    'home': './src/js/pages/home.js',
    'page': './src/js/pages/page.js'
}
~~~

~~~javascript
{
  "scripts": {
    "build": "webpack",
    "dev": "webpack-dev-server --devtool eval --progress --colors --hot --content-base build"
  }
}
~~~

当你在命令行里运行 `npm run dev` 的时候他会执行 dev 属性里的值。这是这些指令的意思：

`webpack-dev-server` - 在 `localhost:8080` 建立一个 Web 服务器
`--devtool eval` - 为你的代码创建源地址。当有任何报错的时候可以让你更加精确地定位到文件和行号
`--progress` - 显示合并代码进度
`--colors` - Yay，命令行中显示颜色！
`--content-base` build - 指向设置的输出目录
总的来说，当你运行 `npm run dev` 的时候，会启动一个 Web 服务器，然后监听文件修改，然后自动重新合并你的代码。真的非常简洁！

#javascript预处理


#css预处理


#文件处理

#开发效率

#数据mock

#域名代理



[webpack在PC项目中的应用](https://github.com/icepy/we-writing/issues/25)