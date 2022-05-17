## 一、概念
高阶函数就是函数的参数也是一个函数。
```bash
function add(x, y, f) {
    return f(x) + f(y);
}

var result = add(-5, 9, Math.abs)

console.log(result) // output: 14
```
`f` 是一个函数，但它是 `add` 函数的一个参数。
## 二、map
map() 方法会创建一个新数组，这个新数组是由原来的数组的每一个元素调用一次提供的函数后的返回值组成的。
对 `arr` 数组的每一个值执行 `pow` 函数操作，省去编写 `for` 循环。
```bash
'use strict';

function pow(x) {
    return x * x;
}
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var results = arr.map(pow); // output: [1, 4, 9, 16, 25, 36, 49, 64, 81]
console.log(results);
```
数组所有值转为 `String`
```bash
var arr = [1, 2, 3, 4, 5];
var strArr = arr.map(String);
console.log(strArr); // output: ["1", "2", "3", "4", "5"]
```
## 三、reduce
`reduce()` 接受两个参数，进行一次运算后，把这次运算的结果作为下一次运算的第一个参数。
`[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4) `
`x1` 和 `x2` 进行运算后比如说生成  `y1`，`y1` 作为新的第一个参数，之后  `y1`  与 `x3` 进行运算生成 `y2` ，依次进行。
 
接下来做一些练习。
### 3.1 求和
```bash
var arr = [1, 2, 3, 4, 5]
var value = arr.reduce(function(x, y) {
    // 实现数组的总和
		return x + y;
})
console.log(value); // output: 15
```
### 3.2 求积
```bash
var arr = [1, 2, 3, 4, 5]
var value = arr.reduce(function(x, y) {
    // 求积
		return x * y;
})
console.log(value); // output: 120
```
### 3.3 字符串转数字
```bash
/**
 * 将字符串转为数字
 * @param s 字符串
 * @returns 
 */
function string2int(s) {
    if (s.length == 1) {
        let k = s - 0;
        return k;
    }

    // 先把字符串转为数组
    var strArr = s.split("")

    // 将数组转为 number
    var k = strArr.reduce(function(x, y) {
        return x + y - 0;
    })
    // console.log(typeof(k)); // number
    // console.log(k);
    return k;
}
console.log(string2int('123456')); // output: 123456
console.log(string2int('12300'));  // output: 12300
console.log(string2int('0'));      // output: 0
console.log(string2int('0123'));   // output: 123
```
### 3.4 规范英文名称
```bash
/**
 * 使英文名称规范，第一个字母大写，其余小写
 * @param {* } arr 
 * @returns 
 */
function normalize(arr) {
    // 获取第一个字母然后转为大写，获取剩下的字母转为小写，然后将两个拼接
    return arr.map(item =>
        item.slice(0, 1).toUpperCase() + item.slice(1).toLowerCase()
    )
}

console.log(normalize(["abCD", "abcD", "AbcD"])); // output:["Abcd","Abcd","Abcd"]
```
### 3.5 字符串转整数
```javascript
'use strict';

var arr = ['1', '2', '3'];
var r = arr.map(item =>
    parseInt(item)
);

console.log(r); // output: [1, 2, 3]
```
```javascript
'use strict';

var arr = ['1', '2', '3'];
var r = arr.map(Number);

console.log(r); // output: [1, 2, 3]
```
## 三、参考资料
[map/reduce](https://www.liaoxuefeng.com/wiki/1022910821149312/1024322552460832)
