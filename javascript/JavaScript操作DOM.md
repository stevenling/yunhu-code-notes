## 一、获取 DOM 节点
```javascript
<!-- HTML结构 -->
<div id="test-div">
<div class="c-red">
    <p id="test-p">JavaScript</p>
    <p>Java</p>
  </div>
  <div class="c-red c-green">
    <p>Python</p>
    <p>Ruby</p>
    <p>Swift</p>
  </div>
  <div class="c-green">
    <p>Scheme</p>
    <p>Haskell</p>
  </div>
</div>
```
```javascript
/**
 * 页面加载后执行
 */
window.onload = function(){
  // 获取 <p>JavaScript</p> 
  // 1. 通过 id 获取节点， id 是唯一的
  var jsOne = document.getElementById('test-p')
    
  // 2. 返回文档中与指定选择器或选择器组匹配的第一个 Element 对象。 
  var jsTwo = document.querySelector('#test-p');
  
  // 获取 <p>Python</p>,<p>Ruby</p>,<p>Swift</p>
  // 1. 通过 class 名称，然后获取所有子节点
  var arrOne = document.getElementsByClassName('c-red c-green')[0].children;
  
  // 2. 返回文档中匹配指定 CSS 选择器的所有元素 class name 用 . 的形式，返回 NodeList 对象
  var arrTwo = document.querySelectorAll('.c-red.c-green > p');
  
  // 获取 <p>Haskell</p>
  // 1. c-green 的 class 名称有两个，因此我们获取后面那个，再获取最后一个子节点
  var haskellOne = document.getElementsByClassName("c-green")[1].lastElementChild;
  
  // 2. 使用 querySelectorAll 的方式
  var haskellTwo = document.querySelectorAll('.c-green')[1].lastElementChild;
}
```
## 二、更新 DOM 节点
```html
<!-- HTML结构 -->
<div id="test-div">
  <p id="test-js">javascript</p>
  <p>Java</p>
</div>
```
修改 `DOM` 节点 `css` 样式
```javascript
var js = document.querySelector('#test-js');

// js.innerHtml = 'JavaScript'
js.innerText = 'JavaScript'

// 修改 CSS 为: color: #ff0000, font-weight: bold

js.style.color = '#ff0000';
js.style.fontWeight = 'bold';
```
`innerText` 与 `innerHtml` 都可以写入当前节点的值，区别在于 `innerHtml` 还可以写入 `html` 标签，如果在网络上获取，可能会被 `XSS` 攻击。
## 三、插入 DOM 节点
### 3.1 插入已经存在的 DOM 节点
如果原来的节点为空，可以用 `innerHTML`来增加节点。
如果原来的节点不为空，那么就不能用 `innerHTML`，因为它会直接替换掉原来的所有子节点，原来的就没了。
```html
<!-- HTML结构 -->
<p id="js">JavaScript</p>
<div id="list">
  <p id="java">Java</p>
  <p id="python">Python</p>
  <p id="scheme">Scheme</p>
</div>
```
插入已经有的节点
```javascript
window.onload = function() {
    var js = document.getElementById('js');
    var list = document.getElementById('list');
    list.appendChild(js);
}
```
会将原来的 `<p id="js">JavaScript</p>`删除，然后添加到 `list` 中。
执行后 `html` 结构如下：
```html
<!-- HTML结构 -->
<div id="list">
    <p id="java">Java</p>
    <p id="python">Python</p>
    <p id="scheme">Scheme</p>
    <p id="js">JavaScript</p>
</div>
```
大部分我们并不需要这样操作，而是插入新的节点。
### 3.2 插入新的节点
创建一个新节点，然后插入。
```javascript
window.onload = function() {
    var list = document.getElementById('list');
    
    // 创建一个新节点
    var haskell = document.createElement('p');
    haskell.id = 'haskell';
    haskell.innerText = 'Haskell';
    list.appendChild(haskell);

```
执行后 `html` 文档如下:
```html
<!-- HTML结构 -->
<div id="list">
    <p id="java">Java</p>
    <p id="python">Python</p>
    <p id="scheme">Scheme</p>
    <p id="haskell">Haskell</p>
</div>
```
### 3.3 动态增加样式
```javascript
    var d = document.createElement('style');
    d.setAttribute('type', 'text/css');
    d.innerHTML = 'p { color: red }';
    document.getElementsByTagName('head')[0].appendChild(d);
```
### 3.3 插入节点到指定位置
```html
    <div id="list">
        <p id="java">Java</p>
        <p id="python">Python</p>
        <p id="scheme">Scheme</p>
    </div>
```
```javascript
window.onload = function () {
    var list = document.getElementById('list');
    var ref = document.getElementById('python');

    var haskell = document.createElement('p');
    haskell.id = 'haskell';
    haskell.innerText = 'Haskell';
    
    // 将节点插入到 ref 节点之前
    list.insertBefore(haskell, ref);
}
```
执行后，新的 `html` 文档如下：
```html
<!-- HTML结构 -->
<div id="list">
    <p id="java">Java</p>
    <p id="haskell">Haskell</p>
    <p id="python">Python</p>
    <p id="scheme">Scheme</p>
</div>
```
### 3.4 遍历子节点
```javascript
window.onload = function () {
  var i;
  var c;
  var list = document.getElementById('list');
  
  for (i = 0; i < list.children.length; i++) {
    c = list.children[i]; // 拿到第 i 个子节点
    console.log(c.innerHTML); 
  }
}
```
### 3.5 练习
对以下`list` 按 `li` 文本的字符串顺序排列 `DOM`。
```html
<!-- HTML结构 -->
<ol id="test-list">
  <li class="lang">Scheme</li>
  <li class="lang">JavaScript</li>
  <li class="lang">Python</li>
  <li class="lang">Ruby</li>
  <li class="lang">Haskell</li>
</ol>
```
思路：先获取所有的字符串，排序后，再修改文本值。
```javascript
window.onload = function () {
  var ol = document.getElementById("test-list");
  var arr = [];
  var li = ol.getElementsByClassName("lang");
  
  // 遍历
  for(let i = 0; i < li.length; i++){
    let liElementText = li[i].innerHTML;
    arr.push(liElementText);
  }
  console.log(arr); // output: ["Scheme",  "JavaScript", "Python", "Ruby", "Haskell"]
  arr.sort();
  
  // 修改值
  for(let i = 0; i < arr.length; i++){
    let liElementCollection = document.getElementsByClassName("lang")
    liElementCollection[i].innerText = arr[i];
  }
}
```
执行后，新的 `html`如下：
```html
<ol id="test-list">
    <li class="lang">Haskell</li>
    <li class="lang">JavaScript</li>
    <li class="lang">Python</li>
    <li class="lang">Ruby</li>
    <li class="lang">Scheme</li>
</ol>
```
## 四、删除 DOM 节点
删除节点时需要获取当前节点和它的父节点。
调用父节点的 `removeChild`方法删除节点。
```html
<div id="parent">
    <p>First</p>
    <p>Second</p>
</div>
```
```javascript
var parent = document.getElementById('parent');
parent.removeChild(parent.children[0]);
parent.removeChild(parent.children[1]); // <-- 浏览器报错
```
`getElementById`方法的返回值 `Element` 是实时更新的，删除第一个节点后，`length` 就变为 `1`，因此第二个删除就越界了。
### 练习
```html
    <!-- HTML结构 -->
    <ul id="test-list">
        <li>JavaScript</li>
        <li>Swift</li>
        <li>HTML</li>
        <li>ANSI C</li>
        <li>CSS</li>
        <li>DirectX</li>
    </ul>
```
目的：留下 `<li>JavaScript</li>`、`<li>HTML</li>`、`<li>CSS</li>`，其他删除。
```javascript
window.onload = function () {
    let ulElement = document.getElementById('test-list');
    ulElement.removeChild(ulElement.children[1]);
    ulElement.removeChild(ulElement.children[2]);
    ulElement.removeChild(ulElement.children[3]);
}
```
执行后，新的 `html`如下：
```html
    <ul id="test-list">
        <li>JavaScript</li>
        <li>HTML</li>
        <li>CSS</li>
    </ul>
```
## 五、小结
### 5.1 获取节点

- getElementById 返回一个匹配特定 `ID` 的元素， `ID` 是唯一的
- getElementsByClassName 返回一个**类数组对象**
- querySelector 返回文档中与指定选择器匹配的第一个 `Element` 对象。 
- querySelectorAll 返回 `NodeList` 对象
### 5.2 更新节点

- innerText 更新文本值
- style 更新 `css`
### 5.3 插入节点

- appendChild 在父节点尾部位置插入节点。
- insertBefore(newNode, referenceNode) 插入 `newNode`节点到 `referenceNode`节点之前。
### 5.4 删除节点
removeChild() 方法从 `DOM` 中删除一个子节点。返回删除的节点。



