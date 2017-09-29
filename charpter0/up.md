##提升
提升：变量和函数声明从他们再代码中出现的位置被"移动"到最上面。
```js
a = 2;
var a;
console.log(a); //=>2
```
```js
console.log(a);
var a = 2;  //=>2
// ===等价于===
var a;
console.log(a);
a = 2;  //=>2
```

```js
foo();
function foo() {
    console.log(a);   //=>undefined
    var a = 2;
}
// ===等价于===
foo();
function foo() {
    var a;
    console.log(a);   //=>undefined
    a = 2;
}
```
```js
// 函数声明会被提升，但是函数表达式却不会被提升。
foo();  // 不是ReferenceError，而不是TypeError！
var foo=function bar(){
    // ... 
}
// ===等价于===
var foo;
foo();
foo=function bar(){
    // ... 
}
```
只有声明本身会被提升，而赋值或其他运行逻辑会留在原地。如果提升改变了代码执行的顺序，会造成非常严重的破坏。


###函数优先
函数声明和变量声明都会被提升。但是一个值得注意的细节（这个细节可以出现在有多个"重复"声明的代码中）是函数会首先被提升，然后才是变量。

```js
foo();
var foo;
function foo() {
    console.log(1);
}
foo = function() {
    console.log(2);
}
// =>1
// ===等价于===
function foo() {
    console.log(1);
}
foo();    //=>1,只有声明本身会被提升，而赋值或其他运行逻辑会留在原地
foo = function() {
    console.log(2);
}
```
```js
foo();
function foo() {
    console.log(1);
}
var foo = function() {
    console.log(2);
}
function foo(){
  console.log(3);
}
// =>3

foo();
var a = true;
if (a) {
    function foo() {
        console.log('a');
    }
} else {
    function foo() {
        console.log('b');
    }
}
// =>b
```


#####参考资料：
《你不知道的JavaScript上卷》
