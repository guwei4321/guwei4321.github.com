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
// 或者
new Schema({}, {autoIndex: false});
````

### 虚拟字段
虚拟字段不会写入数据库中，可以利用它来格式化和组合属性值。
````javascript
var addressSchema = new Schema({
    address: {   
        city: String,   
        street: String 
    } 
});
// 设置获取虚拟字段的get方法
addressSchema.virtual('address.full').get(function(){
    return this.address.city + '/' + this.address.street
})
// 设置赋值虚拟字段的set方法
addressSchema.virtual('address.full').set(function(address){
    var split = address.split('/');
    this.address.city = split[0];
    this.address.street = split[1];
})
var Address = mongoose.model('Address', addressSchema);

var address1 = new Address({
    address: {
        city: 'shanghai',
        street: 'jinzhong road'
    }
})
// 通过虚拟字段获取地址详情
console.log(address1.address.full) // shanghai/jinzhong road
// 通过虚拟字段设置地址详情
address1.address.full = "beijing/gugong";
console.log(address1.address.city) // beijing
console.log(address1.address.street) // gugong
address1.save() // address.full将不会被保存到数据库中

````

### 配置项
`Schema`可以使用配置项来设置，有两种方式设置：
````javascript
new Schema({…}, options)

var schema = new Schema({...});
schema.set(option, value);
````
#### 有效的配置项
* autoIndex（默认true），创建默认索引（_id）。
* capped，限制一次操作的量。
* collection，定义集合的名字，默认是model名+s
* id _id（默认true），如果`schema`中没有定义id，`mongoose`默认分配一个id域
* read 
* safe（默认true），在操作的时候要等待返回的MongoDB返回的结果；否则则不等待
* shardKey 让mongodb做分布式操作 
* strict（默认enabled）如果实例中的字段没在schema中定义过，那么这个字段就不会被保存进数据库
* toJSON，实例调用toJson方法后，针对返回的数据做处理
* toObject，实例调用toJson方法后，针对返回的数据做处理
* versionKey 版本锁，设置在每个文档上，默认键名为 `_v`。
* typeKey 
* validateBeforeSave 保存数据库时会自动验证
* skipVersioning 
* timestamps 在schema设置这个选项，会被自动添加createdAt和updatedAt，默认的名字是createdAt和updatedAt
* useNestedStrict 是否检查childSchem的配置
* retainKeyOrder，是否改变键的顺序

## 验证
`Schmema`里只设置属性的话，缺少字段也能保存；多了属性的话，虽然不会把多余的属性保存，但是还是进入数据库了。所以可以需要增加验证属性来限制下，就能避免像上面这些情况的发生了。常用的验证如下：
````javascript
required  数据必须填写
default  默认值
validate  自定义匹配
min  最小值(只适用于数字)
max  最大值(只适用于数字)
match  正则匹配(只适用于字符串)
enum   枚举匹配(只适用于字符串)
````
### required
将某个字段设置为必填字段，如果没有这个必填字段，则不会被保存。

### default
如果不设置某个字段，则会取默认值

### min | max
将某个字段设置取值范围

### match
字段必须包含指定字符

### enum
枚举值必须在枚举的范围内

### validate
定义一个函数，返回`true`则通过验证；`false`则不通过验证。

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
### 实例的save方法
先实例化，然后再调用`save`方法。
语法：`save([options], [options.safe], [options.validateBeforeSave], [fn])`，例子如下：
````JavaScript
var personSchema = Schema({
    name: String
})
var Person = mongoose.model('Person', personSchema)
var felyne = new Person({name: 'lan'})
felyne.save((err, doc) => {
    if(err) return console.error(err);
    console.log(doc);
})
````

### Model的create方法
直接用`Model`调用`create`方法。
语法：`Model.create(doc(s), [callback])`，例子如下：
````javascript
var personSchema = Schema({
    name: String
})
var Person = mongoose.model('Person', personSchema)
Person.create({name: 'lan'},{name: 'van'}, (err, doc) => {
    if(err) return console.error(err);
    console.log(doc);
})
````

### Model的insertMany方法
也是用`Model`调用，跟`create`相比就是多了`operation`参数。
语法：`Model.insertMany(doc(s), [options], [callback])`，例子如下：
````javascript
var personSchema = Schema({
    name: String
})
var Person = mongoose.model('Person', personSchema)
Person.insertMany({name: 'lan'},{name: 'van'}, (err, doc) => {
    if(err) return console.error(err);
    console.log(doc);
})
````

## 删
### 实例的remove方法
删除实例。
语法：`document.remove([callback])`。


### Model的remove方法
删除符合条件的所有数据。
语法：`Model.insertMany(doc(s), [options], [callback])`。


### findOneAndRemove方法
只删除符合条件的第一条数据

### findByIdAndRemove方法
通过ID删数据

## 改
### update
更改符合条件的数据
options有如下选项
* safe (boolean)： 默认为true。安全模式。
* upsert (boolean)： 默认为false。如果不存在则创建新记录。
* multi (boolean)： 默认为false。是否更新多个查询记录。
* runValidators： 如果值为true，执行Validation验证。
* setDefaultsOnInsert： 如果upsert选项为true，在新建时插入文档定义的默认值。
* strict (boolean)： 以strict模式进行更新。
* overwrite (boolean)： 默认为false。禁用update-only模式，允许覆盖记录。
### updateMany
只能更新多个的update，就是options为`{multi:true}`的update。
### updateMany
只能更新一个的update，就是options为`{multi:false}`的update。

## 查
### find
查询所有数据
#### 查询条件
* $or　　　　或关系
* $nor　　　 或关系取反
* $gt　　　　大于
* $gte　　　 大于等于
* $lt　　　　小于
* $lte　　　 小于等于
* $ne　　　　不等于
* $in　　　　在多个值范围内
* $nin　　　 不在多个值范围内
* $all　　　 匹配数组中多个值
* $regex　　 正则，用于模糊查询
* $size　　　匹配数组大小
* $maxDistance　范围查询，距离（基于LBS）
* $mod　　　　取模运算
* $near　　　 邻域查询，查询附近的位置（基于LBS）
* $exists　　 字段是否存在
* $elemMatch　匹配内数组内的元素
* $within　　　范围查询（基于LBS）
* $box　　　　 范围查询，矩形范围（基于LBS）
* $center　　　范围醒询，圆形范围（基于LBS）
* $centerSphere　范围查询，球形范围（基于LBS）
* $slice　　　　查询字段集合中的元素（比如从第几个之后，第N到第M个元素
* $where       可以使用任意JavaScript作为条件

### findById
通过id查询数据

### findOne
返回查询到的第一个数据


### 查询后处理
查询后处理的方法如下：
* sort     排序
* skip     跳过
* limit    限制
* select   显示字段
* exect    执行
* count    计数
* distinct 去重


## 前后钩子
中间件函数，在执行某些操作之前执行某些函数，在schema上定义。
以下函数可以设置前后钩子：
````javascript
    validate
    save
    remove
    count
    find
    findOne
    findOneAndRemove
    findOneAndUpdate
    insertMany
    update
````

### pre()
`pre()`是在执行方法之前执行的方法。

### post()
`post()`是执行某些操作前最后执行的方法。
