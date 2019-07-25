title: 使用原生函数代替underscore/lodash
date: 2019-07-16 09:23:26
tags: 
- JavaScript
categories:
- JavaScript
---

为了更好的理解函数式变成，我们可以先从替换`underscore/lodash`开始。从[You-Dont-Need-Lodash-Underscore](https://github.com/you-dont-need/You-Dont-Need-Lodash-Underscore)代替方法的学习过程中，我们发现有些`underscore/lodash`来得通俗易懂，所以实际开发中我们根据实际情况取舍，像使用原生`reduce`代替group方法，此篇文章学习重点是理解函数式编程和ES6/S7语法。

如果项目小，也没必要引入经常爆出bug的`loadsh`。
<!--more-->
PS：其中数组方法中的 `concat`(合并生成一个新数组)、`fill`（填充数组）、`reverse`(颠倒数组顺序)、`slice`(切割数组)等等跟原生方法一样就不列举出来了。
## collections
### _.each
按顺序遍历list的所有元素。Es6的forEach只支持循环数组，可以使用`Object.entries`将对象转成数组。有两个参数：
1. 执行的回调函数：三个参数分别是：元素值、元素索引以及原数组。
2. 执行回调函数的 `this` 值
````javascript
// Underscore/Lodash
_.each(['a', 'b', 'c'], function(value,index){
    console.log(index  + value)
})
// output: 0a 1b 2c
_.each({one: 1, two: 2, three: 3}, function(value,key){
    console.log(key  + value)
});
// output: one1 two2 three3

// Native 
['a', 'b', 'c'].forEach( (value, index) => console.log(index + value))
// output: 0a 1b 2c
Object.entries({one: 1, two: 2, three: 3}).forEach( (value, key) =>console.log(value[0] + value[1] ) )
// output one1 two2 three3
````

### _.map
`_.map`跟`_.each`都是遍历list的所有元素，`_.each`只是在原list上操作，而`_.map`会创建一个新的list。所以native中我们使用同名函数`map`来实现就够了。有两个参数：
1. 生成新数组的回调函数三个参数分别是：元素值、元素索引以及原数组。
2. 执行回调函数的 `this` 值
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
`reduce`可以用原生数组的`reduce`方法来模拟。`reduce`提供了两个参数：
1. 累加器函数：四个参数分别是：上一次调用的值或者传入的初始值、正在处理的元素、正在处理元素的索引值以及调用的`reduce`的源数组。
有。
2. 第一次执行累加函数的初始值。
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
`reduce`的从右往左累加的方法。
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
every表示是否每一项都必须都符合条件。换句话说如果全部都符合条件，则返回true；有一项不符合则返回false。有两个参数：
1. 执行的回调函数：三个参数分别是：元素值、元素索引以及原数组。
2. 执行回调函数的 `this` 值
若收到一个空数组，此方法在一切情况下都会返回 true。
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
some则是只要有一项符合条件就返回true。换句话说如果有一项符合条件，则返回true；除非一项都不符合条件才返回false。有两个参数：
1. 执行的回调函数：三个参数分别是：元素值、元素索引以及原数组。
2. 执行回调函数的 `this` 值
对于空数组上的任何条件，此方法返回false。
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
返回第一个满足条件的元素，如果不存在则放回`undefined`。有两个参数：
1. 执行的回调函数：三个参数分别是：元素值、元素索引以及原数组。
2. 执行回调函数的 `this` 值
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
原生JS中没有pluck方法，但是我们可以使用map模拟。
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
判断是否包含某个特定的值，如果有则返回true，否则返回false。
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
`toArray`方法则可以用数组扩展符以及`Array.from`来模拟。`Array.from`跟数组扩展符都是浅拷贝，只能拷贝第一层的值，第二次及其之后的值无法进行深拷贝，他们的引用地址任然是同一个。
其中`Array.from`不仅可以生成转换成数组，也可以运行对每个元素执行回调函数。
`Array.from({length:10})`生成的新数组，及时里面的值都是`undefined`也是可以循环遍历。`Array.from`有三个参数：
1. 想要转为成数组的元素
2. `map`回调函数
3. 回调的执行函数
所以，`Array.from(obj, mapFn, thisArg)` 就相当于 `Array.from(obj).map(mapFn, thisArg)`,

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

## Arrays

### _.object
`_.object`将数组转换为对象。原生js中没有此方法，但是我们可以使用数组的`reduce`方法以及对象的`assign`来模拟。
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
去除数组中的所有false的值。原生中没有，但是可以使用filter来模拟。
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
数组去重。`Set` 对象可以去重，再转成数组来模拟。
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
数组中寻找一个给定元素的第一个索引，如果不存在，则返回-1，跟原生数组中的`indexOf`方法一样。
`indexOf`有两个参数：
1. 要查找的元素
2. 开始查找的索引位置
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
返回数组中满足提供的测试函数的第一个元素的索引，否则返回-1。有两个参数：
1. 执行的回调函数：三个参数分别是：元素值、元素索引以及原数组。
2. 执行回调函数的 `this` 值
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
原生Js中没有`range`。所以直接使用`map`函数来处理，并最终处理成数组。
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

### _.chunk
将数组拆分成多个size长度的小数组，并且将这些小数组组成一个新的大数组，如果最后剩余的元素不够size长度，那么最后剩余的元素组成一个小数组。
````javascript
// Underscore/Lodash
_.chunk(['a', 'b', 'c', 'd'], 2);
// => [['a', 'b'], ['c', 'd']]

_.chunk(['a', 'b', 'c', 'd'], 3);
// => [['a', 'b', 'c'], ['d']]

// Native
const chunk = (input, size) => {
    return input.reduce((previousValue, currentValue, currentIndex, array) => {
        return currentIndex % size === 0
        ? [...previousValue, [currentValue]] //当前索引跟size余数为0时，新增一个小数组
        : [...previousValue.slice(0, -1), [...previousValue.slice(-1)[0], currentValue]]; //当前索引跟size余数不为0时，往length - 1的小数组里推值
    })
};
chunk(['a', 'b', 'c', 'd'], 2);
// => [['a', 'b'], ['c', 'd']]

chunk(['a', 'b', 'c', 'd'], 3);
// => [['a', 'b', 'c'], ['d']]
````

### _.difference
原生中没有此方法，所以得模拟。`difference`会返回一个过滤值不包含排除值的新数组。接受两个参数：
1. 原数组
2. 要排除的值，[values]
````javascript
var arrays = [[1, 2, 3, 4, 5], [5, 2, 10]];
// Underscore/Lodash
var a =_.difference([1, 2, 3, 4, 5], [5, 2, 10]);
// output: [1, 3, 4]

// Native
console.log(arrays.reduce(function(source, target) {
    return source.filter(function(value) { 
        return !target.includes(value); // 如果过滤数组中函数该值includes则返回true，filter则会创建返回值是true的新数组
    });
}));
// output: [1, 3, 4]
console.log(arrays.reduce((source, target) => source.filter(value => !target.includes(value))));
// output: [1, 3, 4]
````

### _.drop
原生`slice`方法的阉割版。
````javascript
// Underscore/Lodash
_.drop([1, 2, 3]);
// => [2, 3]
_.drop([1, 2, 3], 2);
// => [3]

// Native
_.drop([1, 2, 3], 5);
// => []
 
_.drop([1, 2, 3], 0);
// => [1, 2, 3]
````

### _.first
获取数组 array 的第一个元素。
````javascript
// Underscore/Lodash
_.first([1, 2, 3, 4, 5]);
// => 1

_.first([1, 2, 3, 4, 5], 2);
// => [1, 2]
// Native
[1, 2, 3, 4, 5][0];
// => 1
//or
[].concat(1, 2, 3, 4, 5).shift()
// => 1
//or
[].concat([1, 2, 3, 4, 5]).shift()
// => 1

// Native (works even with potentially undefined/null, like _.first)
[].concat(undefined).shift()
// => undefined
[1, 2, 3, 4, 5].slice(0, 2);
// => [1, 2]
````


### _.last
获取array中的最后一个元素。
````javascript
// Underscore/Lodash
const numbers = [1, 2, 3, 4, 5];
_.last(numbers);
// => 5

_.last(numbers, 2);
// => [4, 5]

// Native
const numbers = [1, 2, 3, 4, 5];
numbers[numbers.length - 1];
// => 5
//or
numbers.slice(-1)[0];
// => 5
[].concat(numbers).pop()
// => 5

// Native
[].concat(undefined).pop()
// => undefined

numbers.slice(-2);
// => [4, 5]
````

### _.flatten

````javascript
// Underscore/Lodash
_.flatten([1, [2, [3, [4]], 5]]);
// => [1, 2, [3, [4]], 5]
// Native
const flatten = [1, [2, [3, [4]], 5]].reduce( (a, b) => a.concat(b), []) // 合并一级层级
// => [1, 2, [3, [4]], 5]

// Native(ES2019)
const flatten = [1, [2, [3, [4]], 5]].flat()
// => [1, 2, [3, [4]], 5]
````

### _.flattenDeep

````javascript
// Underscore/Lodash
_.flattenDeep([1, [2, [3, [4]], 5]]);
// => [1, 2, 3, 4, 5]
// Native
const flattenDeep = (arr) => Array.isArray(arr)
  ? arr.reduce( (a, b) => a.concat(flattenDeep(b)) , []) // 如果是数组则继续合并
  : [arr] // 知道不是数组为止

flattenDeep([1, [[2], [3, [4]], 5]])
// => [1, 2, 3, 4, 5]

// Native(ES2019)
[1, [2, [3, [4]], 5]].flat(Infinity) // 使用 Infinity 作为深度，展开为一维数组
// => [1, 2, 3, 4, 5]
````

### _.isArrayBuffer
检查 value 是否是 ArrayBuffer 对象。
````javascript
// Underscore/Lodash
_.isArrayBuffer(new ArrayBuffer(2));
// output: true

// Native
console.log(new ArrayBuffer(2) instanceof ArrayBuffer);
// output: true
````

### _.intersection
创建唯一值的数组
````javascript
// Underscore/Lodash
console.log(_.intersection([1, 2, 3], [101, 2, 1, 10], [2, 1]))
// output: [1, 2]

// Native
var arrays = [[1, 2, 3], [101, 2, 1, 10], [2, 1]];
console.log(array.reduce(function(a, b) {
  return a.filter(function(value) { // 因为是包含共同值，所以很自然的使用filter来过滤并创建新数组
    return b.includes(value); // 为true则保留该元素
  });
})));
// output: [1, 2]

// ES6
let arrays = [[1, 2, 3], [101, 2, 1, 10], [2, 1]];
console.log(arrays.reduce((a, b) => a.filter(c => b.includes(c))));
// output: [1, 2]
````

### _.without
创建一个剔除所有给定值的新数组
````javascript
// Underscore/Lodash
var array = [1, 2, 3]
console.log(_.without(array, 2))
// output: [1, 3]

// Native
var array = [1, 2, 3]
console.log(array.filter(function(value) {
  return value !== 2;
}));
// output: [1, 3]
````

## Objects
### _.keys
返回一个可枚举的直属属性的数组。
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
返回对象的长度，原生没有size方法，用`keys`返回一个数组，再获取长度来模拟。
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
function Stooge(name) {
    this.name = name;
}
Stooge.prototype.silly = true;
// Underscore/Lodash
console.log(__.allKeys(new Stooge("Hi")))
// output:["name", "silly"]

// Native
console.log( [...Reflect.enumerate( new Stooge("Hi") )] )
// output: ["name", "silly"]
````

### _.values
返回对象所有的属性值。原生中没有`values`方法，可使用`Object.keys`和`map`来模拟。
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
使用现有的对象来提供新创建的对象。基本上和`Object.create`一样。
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
通过合并自身属性到目标对象来创建新对象，基本上和`Object.assign`一样。
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
判断是否是一个数组。
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
判断是否是一个有限的数字。只要是数字就返回true，其他值都是false。
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
指定调用函数的作用域，函数里的this都指向传入的`this`。
````javascript
var objA = {
    x: 66,
    offsetX: function(offset) {
        return this.x + offset;
    }
}

var objB = {
    x: 67
};
// Underscore/Lodash
boundOffsetX = _.bind(objA.offsetX, objB, 0);
// output 67
// Native
boundOffsetX = objA.offsetX.bind(objB, 0);
// output 67
````
大部分方法都从以下文章搬过来，觉得很有学习价值，就一个个修修改改搬过来。从中又熟悉了一遍ES6的函数方法，特别是逼格满满的`reduce`方法，^_^

参考文章：
1. [You-Dont-Need-Lodash-Underscore](https://github.com/you-dont-need/You-Dont-Need-Lodash-Underscore)
2. [You don't (may not) need Lodash/Underscore](https://github.com/you-dont-need/You-Dont-Need-Lodash-Underscore)
