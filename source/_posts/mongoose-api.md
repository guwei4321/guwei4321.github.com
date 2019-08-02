title: 入门 mongoose
date: 2019-08-01 14:07:19
tags:
- mongoose
- nodeJs
- mongoDB
categories:
- mongoDB
---


{% blockquote mongoose https://github.com/Automattic/mongoose  %}
Mongoose is a MongoDB object modeling tool designed to work in an asynchronous environment.

Mongoose是运行在异步环境中对MongoDB进行操作的对象建模工具。
{% endblockquote %}

## 准备工作
1. 首先电脑必须安装 [MongoDB](https://www.mongodb.com/) 和 [NodeJS](http://nodejs.org/)。
2. 项目中使用npm来安装mongoose `npm install mongoose --save`
3. 引入`mongoose`就能使用了。
    ````javascript
        // Using Node.js `require()`
        const mongoose = require('mongoose');

        // Using ES6 imports
        import mongoose from 'mongoose';
    ````

## 连接

## Schema
每一个`schema`都是一个文档的映射结构，无法操作数据库，但是可以对文档定义属性、实例方法、静态方法、索引、虚拟字段以及配置项。

## Model
就是由`Schema`生成的模型，可以对数据进行操作。

## Entity
由`Schema`生成的实例，也可以对数据进行操作。

接着我们介绍下怎么使用`mongoose`来操作“增删改查”。

## 增

## 删

## 改

## 查


