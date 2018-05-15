title: css基础积累之 attr-content
date: 2017-08-11 16:42:28
tags:
- Css 积累
categories:
- Css
---

Css越来越强大，最近看到attr，发现css都可以获取节点的data属性内容并放入`content`。有了这个属性，我们可以完全只使用css的情况下，写出一些需要JS的效果（譬如tips），而且还很优雅。


<!--more-->

1. 为了优雅，我们再html中加入`data-tips`属性
2. 使用`attr`获取`data-tips`内容，并放入`content`中
3. 使用`white-space: pre`解决空格问题
4. 使用`Unicode `码解决换行问题


[实例](../demo/css-attr-content.html)
