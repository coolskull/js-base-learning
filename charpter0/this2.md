##this全面解析
#####四种绑定规则  
    1.默认绑定  
    2.隐式绑定  
    3.显式绑定  
    4.new绑定   
优先级：硬绑定(bind)>new>显式>隐式>默认
#####2.2.1 默认绑定  
this一般情况绑定到全局，如果strict模式下，默认绑定到undefined
```js
function foo() {
    console.log(this.a);
}

var a=2;

foo();

```
#####2.2.2 隐式绑定  
```js

function foo() {
    console.log(this.a);
}

var obj = {
    a: 2,
    foo:foo
}
obj.foo();
```
#####2.2.3 显式绑定  
```js
function foo() {
    console.log(this.a);
}

var obj = {
    a: 2
}

var bar = function() {
    foo.call(obj)  //强制把foo的this绑定到obj上。
}

bar();  //2
setTimeout(bar, 100);   //2
bar.call(window);   //2

```
#####2.2.4 new绑定
JavaScript中new的机制实际上和面向类的语言完全不同。JavaScript中，构造函数只是一些使用new操作符时被调用的函数。它们并不会属于某个类，也不会实例化一个类。实际上，它们都不能说是一种特殊的函数类型，只是被new操作符调用的普通函数而已。

包括内置对象函数（比如Number()等）在内的所有函数都可以用new来调用，这种函数调用被称为构造函数调用。这里有一个重要但是非常细微的区别：__实际上并不存在所谓的“构造函数”，只有对于函数的“构造调用”__。

使用new来调用函数，或者说发生构造函数调用时，会自动执行下面的操作。

1.创建（或者说构造）一个全新的对象
2.这个新对象会执行[[prototype]]连接。
3.这个新对象会绑定到函数调用的this。
4.如果函数没有返回其他对象，那么new表达式中的函数调用会自动返回这个新对象。

```js
function foo(a) {
    this.a=a;
}

var bar=new foo(2);
console.log(bar.a); // 2

```

```js
// new && 硬绑定 优先级
function foo(something){
    this.a=something;
}
var obj1={};
var bar=foo.bind(obj1)
bar(2);
console.log(obj1.a);// 2

var baz=new bar(3);
console.log(obj1.a);// 2
console.log(baz.a);// 3

```
###判断this

1.由new调用？绑定到新创建的对象。
```js
 var bar =new foo()
```
2.由call或apply（或者bind）调用？绑定到指定的对象。
 ```js
  var bar =foo.call(obj2)
 ```
3.由上下文对象调用？绑定到那个上下文对象。
```js
  var bar =obj1.foo()
 ```
4.默认：在严格模式下绑定到undefined，否则绑定到全局对象。
```js
  var bar =foo()
 ```
###被忽略的this

如果你把null或者undefined作为this的绑定对象传入call apply或者bind, 这些值调用的时会被忽略，实际应用的是默认绑定规则：
```js
function foo(){
    console.log(this.a);
}
var a =2;
foo.call(null); //2
```
什么情况下会传入null呢？
```js
function foo(a, b) {
     console.log("a:"+a+", b:"+ b);
}
//把数组“展开”称参数
foo.apply(null, [2,3]); //a:2,b:3

//使用bind()进行柯里化
var bar = foo.bind(null, 2);
bar(3); //a:2, b:3
```
如果函数并不关心this的话，你仍然需要传入一个占位符，这是null可能是一个不错的选择。
ES6中，可以用...操作符代替apply(..)来展开数组，foo（...[1,2])和foo（1，2）是一样的，这样可以避免不必要的this绑定。
但仍然没有柯里化的相关语法，因此还是需要使用bind(...)
  然而，总是使用null来忽略this绑定可能产生一些副作用，如果某个函数内部确实使用了this(比如第三方库中的一个函数)，
那默认绑定规则会把this绑定到全局对象（在浏览器中这个对象是window),这将导致无法预料的后果（比如修改了全局对象的属性),
显而易见，这种方式可能会导致许多难以分析和追踪的bug.
####更安全的this

```js
function foo(a,b){
    console.log("a:"+a+",b:"+b);
}
// DMZ空对象
const ∅=Object.create(null);
// 把数组展开成参数
foo.apply(∅,[2,3]);  //a:2,b:3
// 用bind(..)进行柯里化
const bar=foo.bind(∅,2);
bar(3);  //a:2,b:3

```
ø表示"我希望this是空"，这比null的含义更清楚

```js
function foo(){
    console.log(this.a);
}

var o = {a: 3, foo: foo};
var p = {a: 4};
var a =2;
o.foo(); //3 为什么不是2
(p.foo = o.foo)() ; //2
```
