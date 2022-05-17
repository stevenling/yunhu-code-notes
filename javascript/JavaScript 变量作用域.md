## 一、var 的作用域
### 1.1 var 变量在函数体外使用
变量在函数体内声明，作用域范围是在函数体内。
```javascript
'use strict';

function foo() {
	var x = 1;
	x = x + 1;
}

x = x + 2; // ReferenceError! 无法在函数体外引用变量x
```
### 1.2 不同函数同名变量
```javascript
'use strict';

function foo() {
	var x = 1;
	x = x + 1;
}

function bar() {
	var x = 'A';
	x = x + 'B';
}
```
不同函数内的同一个变量是可以的，互相独立，互不影响。
### 1.3 外部函数变量与内部函数变量重名
```javascript
function foo() {
    var x = 1;
    function bar() {
        var x = 'A';
        console.log('x in bar() = ' + x); // 'A'
    }
    console.log('x in foo() = ' + x); // 1
    bar();
}

foo(); // x in foo() = 1  x in bar() = A
```
内部函数变量会屏蔽掉外部函数变量的值。
## 二、变量提升
```javascript
'use strict';

function foo() {
    var x = 'Hello, ' + y;
    console.log(x);
    var y = 'Bob';
}

foo(); // Hello, undefined
```
`JavaScript` 会扫描整个函数体，然后将所有声明的变量提升到函数的顶部，此时 `y` 被声明因此不会报错，但是 `y` 没有被赋值，因此值是 `undefined`。
## 三、全局作用域
`JavaScript` 默认有一个全局对象 `window`，全局作用域的变量实际上被绑定到 `window` 的一个属性。
```javascript
'use strict';

var course = 'Learn JavaScript';
console.log("1: " + course);
console.log("2: " + window.course)
console.log("3: " + (course === window.course))
```
output:
```bash
1: Learn JavaScript
2: Learn JavaScript
3: true
```
以变量方式 `var foo = function () {}` 定义的函数实际上也是一个全局变量。
```bash
'use strict';

function foo() {
    alert('foo test');
}

foo(); // 直接调用foo()
window.foo(); // 通过window.foo()调用
```
弹出两次 `foo test`。

这说明 `JavaScript` 实际上只有一个全局作用域。

任何变量，包括函数，如果没有在当前的作用域上找到，那么会向上继续寻找，直到全局作用域，如果全局作用域之中也没有，则会报出`ReferenceError` 错误。
## 四、局部作用域
### 4.1 var 定义的局部作用域
```bash
'use strict';

function foo() {
    for (var i = 0; i < 100; i++) {
        //
    }
    i += 100; // 仍然可以引用变量i
		console.log("i = " + i);
}

foo(); // i = 200
```
`var`声明的变量 `i`的作用域不是在 `for` 循环之中，而是在 `foo()`  之中，因此依然可以引用到。
### 4.2 let 定义的块级作用域
使用 `let` 可以拥有块级的作用域。
```bash
'use strict';

function foo() {
    for (let i = 0; i < 100; i++) {}
    i += 100; // 不可以引用变量i
    console.log(i)
}
foo();
```
 报错，`let` 声明的 ` i`，只能在 `for` 循环里面使用。

`ES6` 还引入了 `const` 来表示常量，也是块级作用域。
