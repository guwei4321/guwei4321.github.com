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
`connect`用于创建连接数据库，也可以调用使用`disconnect`断开连接。如下代码：
````javascript
const dbPath = 'mongodb://localhost/test';
const database = () => {
    mongoose.set('debug', true)

    const connect = function(){
        mongoose.connect(dbPath, function(err) {
            if(err){
                console.log('连接失败');
            }else{
                console.log('连接成功');
            }
        })
    }

    connect();

    /**
    * 断开重新连接
    */
    mongoose.connection.on('disconnected', () => {
        connect();
    })

    /**
    * 连接成功
    */
    mongoose.connection.on('connected', function () {    
        console.log('已经连接成功');  
    }); 

    /**
    * 连接异常
    */
    mongoose.connection.on('error', err => {
        console.error('异常' + err)
    })

    mongoose.connection.on('open', async () => {
        console.log('Connected to MongoDB ', dbPath)
    })
}

database()
````
如果要指定用户，可在连接的数据库地址上形如 'mongodb://用户名:密码@IP地址/数据库名称' 结构上修改。

## Schema
每一个`schema`都是一个文档的映射结构，无法操作数据库，但是在`schema`上可以定义属性、静态方法、实例方法、查询辅助、索引、虚拟字段以及配置项。
### 允许的类型
    文档中字段允许的属性：
    1. String
    2. Number
    3. Date
    4. Buffer
    5. Boolean
    6. Mixed
    7. ObjectId
    8. Array
    9. Decimal128
    10. Map

````javascript
var schema = new Schema({
    name:    String,
    binary:  Buffer,
    living:  Boolean,
    updated: { type: Date, default: Date.now },
    age:     { type: Number, min: 18, max: 65 },
    mixed:   Schema.Types.Mixed,
    _someId: Schema.Types.ObjectId,
    decimal: Schema.Types.Decimal128,
    array: [],
    ofString: [String],
    ofNumber: [Number],
    ofDates: [Date],
    ofBuffer: [Buffer],
    ofBoolean: [Boolean],
    ofMixed: [Schema.Types.Mixed],
    ofObjectId: [Schema.Types.ObjectId],
    ofArrays: [[]],
    ofArrayOfNumbers: [[Number]],
    nested: {
        stuff: { type: String, lowercase: true, trim: true }
    },
    map: Map,
    mapOfString: {
    type: Map,
    of: String
    }
})
// 如下使用

var Thing = mongoose.model('Thing', schema);

var m = new Thing;
m.name = 'Statue of Liberty';
m.age = 125;
m.updated = new Date;
m.binary = Buffer.alloc(0);
m.living = false;
m.mixed = { any: { thing: 'i want' } };
m.markModified('mixed');
m._someId = new mongoose.Types.ObjectId;
m.array.push(1);
m.ofString.push("strings!");
m.ofNumber.unshift(1,2,3,4);
m.ofDates.addToSet(new Date);
m.ofBuffer.pop();
m.ofMixed = [1, [], 'three', { four: 5 }];
m.nested.stuff = 'good';
m.map = new Map([['key', 'value']]);
m.save(callback);
````

### 索引
`mongoose`自动会给每个`document`增加索引，可以在`connect`设置关闭所有的自动索引，也可以单独针对某个`doucument`设置。
````javascript
schema.set('autoIndex', false);
````

### 虚拟字段
虚拟字段不会写入数据库中，可以利用它来格式化和组合属性值。
````javascript
var UsersSchema = new Schema({
    address:{
        firstName: String,
        lastName: String
    }
});
const address = UsersSchema.virtual('address.full');   



address.get(function () {   

  return this.address.city + ' ' + this.address.street;  

});

````


## Model
就是由`Schema`生成的模型，可以对数据进行操作。
### 自定义静态方法
通过`Schema`的`statics`属性给`model`添加方法，如下使用
````javascript
// 查询所有同名的用户
personSchema.statics.findByName = function(name, callback){
    this.model('Person').find({'name':name}, callback);  
}
var Person = mongoose.model('Person', personSchema)
Person.findByName('lan', function(err, persons) {
    console.log(persons);
});
````

### 查询辅助
通过`Schema`给`model`的查询增加辅助函数，如下使用
````javascript
var personSchema = Schema({
    name: String,
    age: Number,
})
// 查询所有同名的用户
personSchema.query.byName = function(name, callback){
    return this.find({name: name});
}
var Person = mongoose.model('Person', personSchema)
//使用
Person.find().byName('lan').exec(function(err, users){
    console.log(users);
})
````


## Entity
由`Schema`生成的实例，可以定义由生成的实例方法对数据进行操作。
### 自定义实例方法
通过`Schema`的`methods`属性给`document`添加方法，如下使用
````javascript
var personSchema = Schema({
    name: String
})
// 查询所有同名的用户
personSchema.methods.findByName = function(name, callback){
    this.model('Person').find({'name':name}, callback);  
}
// 自定义 showName 方法
personSchema.methods.showName = function(){
    console.log(this.name)
}
var Person = mongoose.model('Person', personSchema)
var felyne = new Person({name: 'lan'})
felyne.showName()
felyne.findByName('lan' ,function(err, result){
    console.log(result)
})
````


接着我们介绍下怎么使用`mongoose`来操作“增删改查”。

## 增

## 删

## 改

## 查


