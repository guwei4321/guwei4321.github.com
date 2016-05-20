title: hexo博客相关
date: 2014-02-07 22:44:12
updated : 2016-05-20 14:44:12
permalink: hexo
tags:
- hexo
categories:
- hexo
---

hexo相关笔记
<!--more-->

## Markdown语法

1、 __分段__ 一个或多个回车
2、 __换行__ 一个回车
3、 __标题__ `#~######` 井号的个数表示几级标题
4、 __引用__ `>`
5、 __列表__ `*`，`+`，`-`，`1.`，选其中之一，注意后面有个空格
6、 __链接__ `[文字](链接地址)`
7、 __图片__ `![图片说明](图片地址)`，图片地址可以是本地路劲，也可以是网络地址
8、 __强调__ `**文字**`，`__文字__`，`_文字_`，`*文字*`
9、 __行内代码__ `` `代码` ``
10、 __代码区块__ 四个空格开头 三个\`\`\` 三个 \~\~\~
[更多markdown语法](http://markdown.tw/)
[hexo扩充标签](https://hexo.io/zh-cn/docs/tag-plugins.html)

## hexo常用命令

+ **新建**

    hexo new "my blog"
新建的文件在hexo/source/_posts/my-blog.md

+ **编译**

    hexo generate
部署前需要编译一下，编译后，会出现一个public文件夹，将所有的md文件编译成html文件

+ **开启本地服务**

    hexo server
开启本地hexo服务

+ **部署**

    hexo deploy
部署到github和gitcoffe上

+ **清除public**

    hexo clean
清除source内多余的文件。

+ **一般部署命令**
{% codeblock lang:bash %}
hexo clean
hexo g
hexo d
// 合并 hexo d -g
{% endcodeblock %}

[官方文档](https://hexo.io/zh-cn/docs/commands.html)

##  其他笔记
_ 同时部署到 github 和 coding上 `_config.yml` 配置
~~~
deploy:
  type: git
  repo:
    github: git@github.com:guwei4321/guwei4321.github.io.git,master
    coding: https://git.coding.net/guwei1989/guwei1989.git,coding-pages
~~~

## 其他同学的笔记
<http://sfau.lt/b5lc0k>