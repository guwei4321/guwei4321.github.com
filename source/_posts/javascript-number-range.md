title: javascript-number-range
date: 2018-03-09 11:11:52
tags: 
- JavaScript
- JS实际应用
categories:
- JavaScript
---

JavaScript是弱类型语言，所以JavaScript中所有的数字，无论是整数还是小数，起类型均为Number。JavaScript所能表示的数值范围为[5e-324,1.7976931348623157e+308]，这两个边界值可以分别用`Number.MAX_VALUE`和`Number.MIN_VALUE` 获得。对于超出JavaScript数值范围的，将显示 Infinity 和 -Infinity。
<!--more-->

## JavaScript运算
在JavaScript里面，数字使用[IEEE 754](https://zh.wikipedia.org/wiki/IEEE_754)中规定的[双精度浮点类型](https://zh.wikipedia.org/wiki/雙精度浮點數)。在这个规定中，JavaScript能表示并精确算术运算的整数范围为[-(2^53 - 1),2^53 - 1]，也就是 [-9007199254740991,9007199254740991]，这两个边界值可分别使用`Number.MIN_SAFE_INTEGER`,`Number.MAX_SAFE_INTEGER`来获得。 超出这个范围的算术运算将会JavaScript将不保证运算结果的精度。譬如以下代码
````js
    var num = 9007199254740992;
    console.log(num + 5); // 9007199254740996
````
正确的应该是显示 9007199254740997，但是已经超出JavaScript的运算精度范围，所以得到了错误结果。


## JavaScript位运算
JavaScript中对于位运算，JavaScript仅支持32位整数，又有一位要表示正负号，所以其范围：[-(2^31 - 1)，(2^31 - 1)]，超出也JavaScript也不将保证精度。
````js
    var maxSum = Math.pow(2,31);
    console.log(pow&pow) // -2147483648
    console.log(pow>>1) // -1073741824
````
自己与自己正确应该是还是自己，但是他显示`-2147483648`, `pow>>1` 正确结果应该是跟`pow/2`的值一样。因为超出了位运算的范围，以上运算都得出了错误结果。