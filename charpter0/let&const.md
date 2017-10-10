##let、const与块级作用域

###let基本用法
ES6 新增了`let`命令，用来声明变量。它的用法类似于`var`，但是所声明的变量，只在`let`命令所在的代码块内有效。
JavaScript中`var`声明的作用域像是Photoshop中的油漆桶工具，从声明处开始向前后两个方向扩散，直到触及函数边界才停止扩散。
```js
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```
#####例子1：循环内变量过度共享
```js
var messages = ["嗨！", "我是一个web页面！", "alert()方法非常有趣！"];
for (var i = 0; i < messages.length; i++) {
    alert(messages[i]);
}
  ```  
结果为三次弹窗，分别为"嗨！", "我是一个web页面！", "alert()方法非常有趣！"
```js
var messages = ["喵！", "我是一只会说话的猫！", "回调（callback）非常有趣!"];
    for (var i = 0; i < messages.length; i++) {
    setTimeout(function () {
        alert(messages[i]);
    }, i * 1500);
}
```
结果三次弹窗都为`undefined`：事实上，这个问题的答案是，循环本身及三次`timeout`回调均共享唯一的变量i。当循环结束执行时，`i`的值为3（因为messages.length的值为3），此时回调尚未被触发

 ```js
var messages = ["喵！", "我是一只会说话的猫！", "回调（callback）非常有趣!"];
for (let i = 0; i < messages.length; i++) {  // var改为let
    setTimeout(function() {
        alert(messages[i]);
    }, i * 1500);
}
``` 
将上面的`var`改为`let`就能正确的输出 "喵！", "我是一只会说话的猫！", "回调（callback）非常有趣!" 
#####同上的还有：
```js
var buttons = document.querySelectorAll(".button"); // 获取按钮组
var output = document.querySelector("#output"); // 获取输出容器元素

for(var i = 0;  i< buttons.length; ++i) { // 循环为每个按钮绑定点击事件
    buttons[i].addEventListener('click', function(evt){
        output.innerText = buttons[i].innerText; // 期望显示按钮自身的值
    }, false);
}
```
上面这段代码看上去貌似没问题，当我们点击任意一个按钮时，程序都会报错，提示Cannot read property 'innerText' of undefined。因为循环完成后i的值是3，buttons[3]并不存在。计数器i存在于上一层作用域中，就意味着在对它的引用被全部解除之前，它会一直保存着循环结束后的值，即按钮的个数。

 
```js
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
``` 
```js
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```
上面代码正确运行，输出了3次abc。这表明函数内部的变量i与循环变量i不在同一个作用域，有各自单独的作用域。

###let和var有什么不同呢？

这一规则可以帮助你捕捉bug，除了NaN错误以外，每一个异常都会在当前行抛出。
1.  **let声明的变量拥有块级作用域。** 也就是说用let声明的变量的作用域只是外层块，而不是整个外层函数。
2.  **let声明的全局变量不是全局对象的属性。** 这就意味着，你不可 以通过window.变量名的方式访问这些变量。它们只存在于一个不可见的块的作用域中，这个块理论上是Web页面中运行的所有JS代码的外层块。
3.  **形如for (let x...)的循环在每次迭代时都为x创建新的绑定。**
  如果一个for (let...)循环执行多次并且循环保持了一个闭包，那么每个闭包将捕捉一个循环变量的不同值作为副本，而不是所有闭包都捕捉循环变量的同一个值。
4.  **let声明的变量直到控制流到达该变量被定义的代码行时才会被装载，所以在到达之前使用该变量会触发错误.**
```js
function update() {
  console.log("当前时间:", t);  // 引用错误（ReferenceError）
  ...
  let t = readTachymeter();
}
```
5.  **用let重定义变量会抛出一个语法错误（SyntaxError）。**

#####例子2：变量提升
```js
function test () {
  console.log(value);       //=>undefined ,如果是let会直接报错
  var value = 'something';
  console.log(value);       //=>'something'
}

test();

function test2 () {
  console.log(fn);        //=>fn() {}
  function fn () {
  }
  console.log(fn);        //=>fn() {}
}

test2();
```
#####例子3：？？？？？
```js
var foo = true , baz = 10;
if (foo) {
    var bar = 3;
    if (baz > bar) {
        console.log(baz);   //=>10
    }
}    

var foo = true, baz = 10;
if (foo) {
    var bar = 3;
}
if (baz > bar) {
    console.log(baz);   //=>10
}

var foo = true, baz = 10;
if (foo) {
    let bar = 3;
    if (baz > bar) {
        console.log(baz);   //=>10
    }
}

var foo = true , baz = 10;
if (foo) {
    let bar = 3;
}
if (baz > bar) {
    console.log(baz);   //=>Uncaught ReferenceError:bar is not defined
}
```
#####例子4：
```js
function foo() {
    function bar(a) {
        i = 3; // 在外围的for循环的作用域中改变`i`
        console.log( a + i );
    }

    for (var i=0; i<10; i++) {
        bar( i * 2 ); // 噢，无限循环！
    }
}

foo()
```
```js

for (var i = 1; i <= 5; i++) {
    (function(i){setTimeout(function timer() {
        console.log(i);
    }, i * 1000);})(i);
}
// 1 2 3 4 5
```
###const基本用法

`const`声明的变量与`let`声明的变量类似，它们的不同之处在于，`const`声明的变量只可以在声明时赋值，不可随意修改，否则会导致`SyntaxError`（语法错误）。
```js
const MAX_CAT_SIZE_KG = 3000; // 正确

MAX_CAT_SIZE_KG = 5000; // 语法错误（SyntaxError）
MAX_CAT_SIZE_KG++; // 虽然换了一种方式，但仍然会导致语法错误
``` 
当然，规范设计的足够明智，用`const`声明变量后必须要赋值，否则也抛出语法错误。
```js
const theFairest;  // 依然是语法错误，你这个倒霉蛋
``` 


#####参考资料:  
[深入浅出ES6（十四）：let和const](http://www.infoq.com/cn/articles/es6-in-depth-let-and-const)  
[let、const与块级作用域](http://www.jianshu.com/p/c54b765b97a8)  
[每天学点ES6－let和const](http://cookfront.github.io/2015/05/28/es6-let-const/)

