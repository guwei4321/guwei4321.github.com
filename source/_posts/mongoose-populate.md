title: mongoose联表查询
date: 2019-08-01 14:07:19
tags:
- mongoose
- nodeJs
- mongoDB
categories:
- mongoDB
---

很多情况下，不可能单独只查询一个表来获取数据，经常会多表联查。这里我们只讲`populate`方法和聚合查询。
<!--more-->

## Schema增加外键增加联系
增加一张书籍和人物的表，通过外键指向需要关联的对象。
````javascript
var personSchema = Schema({
    _id: Schema.Types.ObjectId,
    name: String,
    age: Number,
    stories: [{ type: Schema.Types.ObjectId, ref: 'Story' }] // 以id作为外键值
})

var storySchema = Schema({
    author: { type: Schema.Types.ObjectId, ref: 'Person' },  // 以id作为外键值
    title: String
})
var Story = mongoose.model('Story', storySchema)
var Person = mongoose.model('Person', personSchema)
````

## 保存数据
保存时，只要把关联的文档的`_id`值赋给它就好了:
````javascript
var author = new Person({
    _id: new mongoose.Types.ObjectId(),
    name: 'Ian Flemin',
    age: 50
})
var story = new Story({
    title: 'Casino Royale',
    author: author._id    // 将author的_id赋值给story实例
});
author.save(); // 保存
story.save(); // 保存
````

## 获取数据
使用`populate()`方法查询
````javascript
Story.
    find().
    populate('author').
    exec( (err, stories) => {
        if (err) return console.log(err)
        console.log(stories) // [ { fans: [],_id: 5d4a65c2fa5ac0f1403b9e86,title: 'Casino Royale',author:{ stories: [],_id: 5d4a65c2fa5ac0f1403b9e85, name: 'Ian Flemin',age: 50,__v: 0 },__v: 0 } ] } )
    } )
````

##聚合方法

### 定义Schema
````javascript
var bankSchema = Schema({
    bank_id: String, // 定义表之间关联的键
    name: String,
})

var BankModel = mongoose.model('Bank',bankSchema,'banks');

var projectSchema = Schema({
    bank_id: String, // 定义表之间关联的键
    title:String,
    rate:String,
})

var ProjectModel = mongoose.model('Project',projectSchema,'projects'); // 定义集合名称，聚合查询会用到
````

### 保存数据
````javascript
var insetArray = [
    {"bank_id":"1","name":"众邦银行"},
    {"bank_id":"2","name":"营口银行"},
    {"bank_id":"2","name":"金城金行"},
]
BankModel.create(insetArray, (err, doc) => {
    if(err) return console.error(err);
    console.log(doc);
})

var insetArrayP = [
    {"bank_id":"1","title":'众邦宝活期',"rate":"4.6%"},
    {"bank_id":"1","title":'众邦宝定期',"rate":"4.6%"},
    {"bank_id":"2","title":'祥云宝1号',"rate":"4.6%"},
    {"bank_id":"3","title":'金慧存1号',"rate":"4.6%"},
    {"bank_id":"3","title":'金慧存2号',"rate":"4.6%"},
    {"bank_id":"3","title":'金慧存3号',"rate":"4.6%"},
]
ProjectModel.create(insetArrayP, (err, doc) => {
    if(err) return console.error(err);
    console.log(doc);
})
````

### 聚合查询
````javascript
 BankModel.aggregate([{
            $lookup:{
                from:"projects",
                localField:"bank_id",
                foreignField:"bank_id",
                as:"projects_item"
            }
        }],(err,docs)=>{
            if(err){
                console.log(err);
                return;
            }
            console.log(docs) // [{"_id":"5d4a972f70be25ec68f35f6b","bank_id":"1","name":"众邦银行","__v":0,"projects_item":"众邦宝定期","rate":"4.6%","__v":0}]},{"_id":"5d4a972f70be2:[{"_id":"5d4a972f70be25ec68f35f6e","bank_id":"1","title":"众邦宝活期","rate":"4.6%","__v":0},{"_id":"5d4a972f70be25ec68f35f6f","bank_id":"1","title":"众邦宝定期","rate":"4.6%","__v":0}]},{"_id":"5d4a972f70be25ec68f35f6c","bank_id":"2","name":"营口银行","__v":0,"projects_item":[{"_id":"5d4a972f70be25ec68f35f70","bank_id":"2","title":"祥云宝1号","rate":"4.6%","__v":0}]},{"_id":"5d4a972f70be25ec68f35f6d","bank_id":"2","name":"金城金行","__v":0,"projects_item":[{"_id":"5d4a972f70be25ec68f35f70","bank_id":"2","title":"祥云宝1号","rate":"4.6%","__v":0}]}]
        })
````