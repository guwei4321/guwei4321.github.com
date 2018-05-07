title: 验证身份证号是否正确
date: 2017-07-31 14:22:34
tags: 
- JavaScript
- JS实际应用
categories:
- JavaScript
---



在[正则基础深入应用](../javascript-reg-2#匹配身份证号码)中，我们使用了正则去验证身份证号，虽然根据规律写的那个正则能满足多数情况，但是还是会有一些漏网之鱼。后来发现有一个计算方法可以去判定这个身份证号是否正确。
<!--more-->
验证方法：
1. 将身份证的前17位分别乘以不同的系数。从第一位到第十七位的系数分别为： 7－9－10－5－8－4－2－1－6－3－7－9－10－5－8－4－2。
2. 将这18位数字和系数相乘的结果相加。
3. 用加出来的和除以11，得出余数。
4. 除以11，余数只能是0-10这11个数的一个。
5. 以上得出的余数，对应着 [1,0,X,9,8,7,6,5,4,3,2]。

根据以上结论，使用JS写相应程序，得出如下代码：
````js
/**
 * [checkID 验证身份证号码是否正确]
 * @param  {[String]} strIDCardnumber [必须是字符串，不然会触发大整数精度问题]
 * @return {[Boolean]}                [返回布尔值]
 */
function checkID(strIDCardnumber){
    var _isIDRule = false,
        _sum = 0,
        _strIDCardnumber = strIDCardnumber.toString(),
        _coefficient = [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2],
        _mantissa = ['1', '0', 'X', '9', '8', '7', '6', '5', '4', '3', '2'];

    for (var i = 0; i < 17; i++) {
        _sum += Number(_strIDCardnumber.substring(i, i + 1)) * _coefficient[i];
    }

    if (_strIDCardnumber.substring(17) == _mantissa[_sum % 11]) {
        _isIDRule = true;
    }

    return _isIDRule;
}
````

以上就是验证身份证的程序代码，只要`checkID('430404196710021020')`调用就ok了。
调用时必须要注意传入的值必须是字符类型，不然大于`2^53`时，就会触发大数字的精度问题。
