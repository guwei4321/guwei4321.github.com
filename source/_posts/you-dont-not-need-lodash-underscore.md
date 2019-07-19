title: 使用原生函数代替underscore/lodash
date: 2019-07-16 09:23:26
tags: 
- JavaScript
categories:
- JavaScript
---

为了更好的理解函数式变成，我们可以先从替换`underscore/lodash`开始。从[You-Dont-Need-Lodash-Underscore](https://github.com/you-dont-need/You-Dont-Need-Lodash-Underscore)代替方法的学习过程中，我们发现有些`underscore/lodash`来得通俗易懂，所以实际开发中我们根据实际情况取舍，像使用原生`reduce`代替group方法，此篇文章学习重点是理解函数式编程和ES6/S7语法。

## Arrays

### _.each

````javascript
// Underscore/Lodash
_.each(['a', 'b', 'c'], function(value,index){
    console.log(index  + value)
})
// output: 0a 1b 2c

// Native 
['a', 'b', 'c'].forEach( (value, index) => console.log(index + value))
// output: 0a 1b 2c
````

### _.map

````javascript
// Underscore/Lodash
var array1 = ['a', 'b', 'c']
var array2 = _.map(array1, function (value, index) {
    return 'Hi ' + value 
})
console.log(array2)
// output: ["Hi a", "Hi b", "Hi c"]

// Native 
var array1 = ['a', 'b', 'c']
var array2 = array1.map( (value, index) => 'Hi ' + value )
console.log(array2)
// output: ["Hi a", "Hi b", "Hi c"]
````

### _.reduce

````javascript
// Underscore/Lodash
var array = [0, 1, 2, 3, 4]
var result = _.reduce(array, function(previousValue, currentValue, currentIndex, array){
    return previousValue + currentValue
})
console.log(result)
// output: 10

// Native
var array = [0, 1, 2, 3, 4]
var result = array.reduce( (previousValue, currentValue, currentIndex, array) => previousValue + currentValue )
console.log(result)
// output: 10
````

### _.reduceRight

````javascript
// Underscore/Lodash
var array = [0, 1, 2, 3, 4]
var result = _.reduceRight(array, function(previousValue, currentValue, currentIndex, array){
    return previousValue - currentValue
})
console.log(result)
// output: -2

// Native
var array = [0, 1, 2, 3, 4]
var result = array.reduceRight( (previousValue, currentValue, currentIndex, array) => previousValue - currentValue )
console.log(result)
// output: -2
````

### _.every
每一个值是否满足要求
````javascript
// Underscore/Lodash
var array = [10, 20, 30]
var result = _.every(array, function(element, index, array){
    return element >= 10
})
console.log(result)
// output: true

// Native
var array = [10, 20, 30]
var result = array.every( (element, index, array) => element >= 10 )
console.log(result)
// output: true
````

### _.some

````javascript
// Underscore/Lodash
var array = [10, 20, 30]
var result = _.some(array, function(element, index, array){
    return element >= 30
})
console.log(result)
// output: true

// Native
var array = [10, 20, 30]
var result = array.some( (element, index, array) => element >= 30 )
console.log(result)
// output: true
````

### _.find

````javascript
// Underscore/Lodash
 var users = [
    { 'user': 'xiaoming',  'age': 36, 'active': true },
    { 'user': 'xiaohong',    'age': 40, 'active': false },
    { 'user': 'xiaogang', 'age': 1,  'active': true }
]
var result = _.find(users,  function (o) { return o.age < 40; })
console.dir(result)
// output: active: true age: 36 user: "xiaoming"
// Native
var users = [
    { 'user': 'xiaoming',  'age': 36, 'active': true },
    { 'user': 'xiaohong',    'age': 40, 'active': false },
    { 'user': 'xiaogang', 'age': 1,  'active': true }
]
var result = users.find( (o) =>  o.age < 40 )
console.dir(result)
// output: active: true age: 36 user: "xiaoming"
````

### _.pluck
`Lodash` v4.0 已经移除`pluck`函数。
````javascript
var users = [{name: "Alice"}, {name: "Bob"}, {name: "Jeremy"}]
var names = _.pluck(users, "name")
console.log(names)
// output: ["Alice", "Bob", "Jeremy"]
// Native
var users = [{name: "Alice"}, {name: "Bob"}, {name: "Jeremy"}]
var names = users.map( (o) => o.name )
console.log(names)
// output:["Alice", "Bob", "Jeremy"]
````

### _.includes

````javascript
// Underscore/Lodash
var array = [1, 2, 3]
// Underscore/Lodash - also called _.contains
var isIncludes = _.includes(array, 1)
console.log(isIncludes)
// output: true

// Native
var array = [1, 2, 3]
var isIncludes = array.includes(1)
console.log(isIncludes)
var isIndexOf = array.indexOf(1) > -1;
// output: true
console.log(isIndexOf)
// output: true
````

### _.toArray

````javascript
// Underscore/Lodash
_.toArray('abc')
// output: ["a", "b", "c"]
_.toArray([1, 2])
// output: [1, 2]
_.toArray({ 'a': 1, 'b': 2 })
// output: [1, 2]
// Native
[...'abc']
// output: ["a", "b", "c"]
Array.from([1, 2])
// output: [1, 2]
Array.from(Object.values({ 'a': 1, 'b': 2 }))
// output: [1, 2]
````

### _.object

````javascript
// Underscore/Lodash
_.object(['moe', 'larry', 'curly'], [30, 40, 50])
// output: {curly: 50, larry: 40, moe: 30}
_.object([['moe', 30], ['larry', 40], ['curly', 50]])
// output: {curly: 50, larry: 40, moe: 30}

// Native
var array = [['moe', 30], ['larry', 40], ['curly', 50]];
array.reduce((result, [key, val]) => Object.assign(result, {[key]: val}), {})
// output: {curly: 50, larry: 40, moe: 30}
var array = [['moe', 'larry', 'curly'], [30, 40, 50]];
array.slice(0, 1)[0].reduce( (result, key, index, _array) => Object.assign(result, {[key]: array[1][index]}) ,{})
// output: {curly: 50, larry: 40, moe: 30}
````

### _.compact

````javascript
// Underscore/Lodash
_.compact([0, 1, false, 2, '', 3])
// output: [1, 2, 3]

// Native
[0, 1, false, 2, '', 3].filter(Boolean)
// output: [1, 2, 3]
[0, 1, false, 2, '', 3].filter(x => !!x)
// output: [1, 2, 3]
````

### _.uniq

````javascript
// Underscore/Lodash
var result = _.uniq([1, 2, 1, 4, 1, 3])
console.log(result)
// output: [1, 2, 4, 3]

// Native
var result = [...new Set([1, 2, 1, 4, 1, 3])]
console.log(result)
// output: [1, 2, 4, 3]
````

### _.indexOf

````javascript
// Underscore/Lodash
var result = _.indexOf([2, 9, 9], 2)
console.log(result)
// output: 0

// Native
var result = [2, 9, 9].indexOf(2)
console.log(result)
// output: 0
````

### _.findIndex

````javascript
// Underscore/Lodash
var users = [
    { 'user': 'xiaoming',  'age': 36, 'active': true },
    { 'user': 'xiaohong',    'age': 40, 'active': false },
    { 'user': 'xiaogang', 'age': 1,  'active': true }
]
var index = _.findIndex(users, function (o) { return o.age >= 40; })
console.log(index)
// output: 0

// Native
var users = [
    { 'user': 'xiaoming',  'age': 36, 'active': true },
    { 'user': 'xiaohong',    'age': 40, 'active': false },
    { 'user': 'xiaogang', 'age': 1,  'active': true }
]
var index = users.findIndex( (o) => o.age >=40 )
console.log(index)
// output: 0
````

### _.range

````javascript
// Underscore/Lodash
_.range(10)
// output: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
_.range(1, 11)
// output: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
_.range(0, 30, 5)
// output: [0, 5, 10, 15, 20, 25]
_.range(0, -10, -1)
// output: [0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
_.range(0)
// output: []

// Native
Array.from({length:10}, (_, i) => i)
// output: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
Array.from({length:10}, (_, i) => i+1)
// output: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
Array.from({length:6}, (_, i) => i*5)
// output: [0, 5, 10, 15, 20, 25]
Array.from({length:10}, (_, i) => -i)
// output: [0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
Array.from({length:0})
// output: []

[...Array(10).keys() ] )
// output: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
[...Array(10).keys()].map(k => k + 1)
// output: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
[...Array(6).keys()].map(k => k*5 )
// output: [0, 5, 10, 15, 20, 25]
[...Array(10).keys()].map(k => -k)
// output: [0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
[]
// output: []
````

## Objects
### _.keys

````javascript
// Underscore/Lodash
_.keys({one: 1, two: 2, three: 3})
// output: ["one", "two", "three"]

// Native
var result = Object.keys({one: 1, two: 2, three: 3})
console.log(result)
// output: ["one", "two", "three"]
````

### _.size

````javascript
// Underscore/Lodash
_.size({one: 1, two: 2, three: 3})
// output:3

// Native
var result = Object.keys({one: 1, two: 2, three: 3}).length
console.log(result)
// output: 3
````

### _.allKeys
`Reflect.enumerate(..)`：返回一个产生所有（直属和“继承的”）非symbol、可枚举的键的迭代器
````javascript
// Underscore/Lodash
console.log(__.allKeys(new Stooge("Hi")))
// output:["name", "silly"]

// Native
console.log( [...Reflect.enumerate( new Stooge("Hi") )] )
// output: ["name", "silly"]
````

### _.values

````javascript
// Underscore/Lodash
_.values({one: 1, two: 2, three: 3})
// output: [1, 2, 3]

// Native
Object.values({one: 1, two: 2, three: 3})
// output: [1, 2， 3]
var obj = {one: 1, two: 2, three: 3}
Object.keys(obj).map(key => obj[key])
// output: [1, 2， 3]
````

### _.create

````javascript
function Stooge(name) {
    this.name = name;
}
Stooge.prototype.silly = true;
// Underscore/Lodash
 var hi = _.create(Stooge.prototype, {name: "hi"})
console.log(hi)
// output {name: "hi"}

// Native
var hi = Object.assign(Object.create(Stooge.prototype), {name: "hi"})
console.log( hi )
````

### _.assign
通过合并来创建新对象
````javascript
// Underscore/Lodash
_.assign({}, {b: 1,c: 2}, { a: false })
// output {b: 1, c: 2, a: false}
// Native
Object.assign({}, {b: 1,c: 2}, { a: false })
// output {b: 1, c: 2, a: false}
{  ...{b: 1,c: 2}, a: false }
// output {b: 1, c: 2, a: false}
````
### _.isArray

````javascript
// Underscore/Lodash
var array = []
console.log(_.isArray(array))
// output: true

// Native
var array = []
console.log(Array.isArray(array))
// output: true
````

### _.isFinite

````javascript
// Underscore/Lodash
_.isFinite(-101)
// output true
_.isFinite(-Infinity)
// output false
_.isFinite('a')
// output false

// Native
var array = []
Number.isFinite(-101)
// output true
Number.isFinite(-Infinity)
// output true
Number.isFinite('a')
// output false
````

## Functions
### _.bind

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````

### _.each

````javascript
// Underscore/Lodash

// Native
````