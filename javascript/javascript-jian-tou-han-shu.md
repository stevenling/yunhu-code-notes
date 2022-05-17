# JavaScript 箭头函数

### 一、概述

#### 1.1 定义一个简单的箭头函数：

```js
x => x * x
```

等价于：

```js
function (x) {
    return x * x;
}
```

#### 1.2 不同参数的箭头函数

```js
// 没有参数
var f1 = () => 3.14;
console.log(f1()); // output: 3.14

// 一个参数
var f2 = x => x * x;
console.log(f2(5)); // output: 25

// 两个参数
var f3 = (x, y) => x * x + y * y;
console.log(f3(6, 8)); // output: 100

// 可变参数
var f4 = (x, y, ...rest) => {
    var i, sum = x + y;
    for (i = 0; i < rest.length; i++) {
        sum += rest[i];
    }
    return sum;
}

console.log("sum = " + f4(1, 2, 3, 4, 5)); // output: sum = 15

// 返回对象字面量表达式，细节外层大括号变为小括号，然后对象用大括号表示
var f5 = x => ({ foo: x+2 });
console.log(f5(20)); // output: {"foo": 22}
```

### 二、箭头函数 this 绑定问题

#### 2.1 this 绑定有误

```js
var obj = {
    birth: 1997,
    getAge: function () {
        var b = this.birth; // 这个 this 指向 obj 所以获取到 1997
        var fn = function () {
            return new Date().getFullYear() - this.birth; // 这个 this 指向 window, 获取不到值
        };
        return fn();
    }
};

// fn 函数中 this 指向 window 
console.log(obj.getAge()); // output: NaN 
```

普通函数 `fn`的 `this`会指向 `window`，因此获取不到我们想要的结果。

#### 2.2 通过引入一个 that 变量保存 this 值

```js
var obj = {
    birth: 1997,
    getAge: function () {
        var b = this.birth; // 这个 this 指向 obj 所以获取到 1997
        var that = this;    // 用 that 保存 this 的值
        var fn = function () {
            return new Date().getFullYear() - that.birth; // 这个 that 指向 obj，可以获取到值
        };
        return fn();
    }
};

console.log(obj.getAge()); // output: 25 
```

#### 2.3 通过箭头函数

```js
var obj = {
    birth: 1997,
    getAge: function () {
        var b = this.birth; // 1997
        var fn = () => new Date().getFullYear() - this.birth; // this 指向 obj 对象
        return fn();
    }
};

console.log(obj.getAge()); // 25
```

箭头函数不会创建自己的 `this`, 它只会从自己的作用域链的上一层继承 `this`，这个例子中就是 `obj` 对象。
