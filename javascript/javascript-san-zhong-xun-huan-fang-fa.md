# JavaScript 三种循环方法

### 一、for in

for in 遍历的是对象的属性名称，一个 array 数组的索引被视为属性，因此输出索引。

```js
var a = ['A', 'B', 'C'];

for (var x in a) {
    console.log(x); // output: '0' '1' '2'
}

a.name = 'Hello';
for (var x in a) {
    console.log(x); // output: '0', '1', '2', 'name'
}
```

### 二、for of

for of 遍历的是集合里面的元素。

```js
var a = ['A', 'B', 'C'];

for (var x of a) {
    console.log(x); // output: 'A' 'B' 'C'
}
```

### 三、for each 方法

#### 3.1 数组的 for each 方法

```js
var a = ['A', 'B', 'C'];

/**
 * 遍历数组
 * element: 指向当前元素的值
 * index: 指向当前索引
 * array: 指向 Array 对象本身
 */
a.forEach(function (element, index, array) {
    console.log('index = ' + index + ' value = ' + element);
});
```

输出：

> index = 0 value = A
>
> index = 1 value = B
>
> index = 2 value = C

也可以省略不需要的参数，只输出元素的值。

```js
var a = ['A', 'B', 'C'];
a.forEach(function (element) {
    console.log(element); // output: 'A' 'B' 'C'
});
```

#### 3.2 Set 集合的 for each 方法

由于集合本身是无序的，所以 element 和 sameElement 输出的是同样的元素。

```js
var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, sameElement, set) {
    console.log(element);     // A B C
    console.log(sameElement); // A B C 
});
```

#### 3.3 Map 的 for each 方法

Map 的回调函数第一个参数是值，第二个参数是键，第三个参数是 Map 本身。

```js
var m = new Map([[233, 'x'], [0.618, 'y'], ["yunhu", 'z']]);
m.forEach(function (value, key, map) {
    console.log('key = ' + key + ' value = ' + value);
});
```

输出：

> key = 233 value = x
>
> key = 0.618 value = y
>
> key = yunhu value = z
