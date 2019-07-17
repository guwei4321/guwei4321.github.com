title: 使用原生函数代替underscore/lodash
date: 2019-07-16 09:23:26
tags: 
- JavaScript
categories:
- JavaScript
---

为了更好的理解函数式变成，我们可以先从替换`underscore/lodash`开始。从[You-Dont-Need-Lodash-Underscore](https://github.com/you-dont-need/You-Dont-Need-Lodash-Underscore)代替方法的学习过程中，我们发现有些`underscore/lodash`来得通俗易懂，所以实际开发中我们根据实际情况取舍，像使用原生`reduce`代替group方法，此篇文章学习重点是理解函数式编程和ES6/S7语法。

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
    { 'user': 'barney',  'age': 36, 'active': true },
    { 'user': 'fred',    'age': 40, 'active': false },
    { 'user': 'pebbles', 'age': 1,  'active': true }
]
var result = _.find(users,  function (o) { return o.age < 40; })
console.dir(result)
// output: active: true age: 36 user: "barney"
// Native
var users = [
    { 'user': 'barney',  'age': 36, 'active': true },
    { 'user': 'fred',    'age': 40, 'active': false },
    { 'user': 'pebbles', 'age': 1,  'active': true }
]
var result = users.find( (o) =>  o.age < 40 )
console.dir(result)
// output: active: true age: 36 user: "barney"
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
var isIncludes = array.includes(1);
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