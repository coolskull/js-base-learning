##作用域闭包

闭包：当函数可以记住并访问所在的词法作用域时，就产生闭包，即使函数是在当前词法作用域之外。
```js
function foo() {
    var a = 2;
    function bar() {
        console.log(a);
    }
    return bar;
}
var baz = foo();
baz(); 

```

```js
// === 例子1===
for (var i = 1; i <= 5; i++) {
    setTimeout(function timer() {
        console.log(i);
    }, i * 1000);
}
// =>5个6

// === 例子2===
for (var i = 1; i <= 5; i++) {
    (function() {
        setTimeout(function timer() {
            console.log(i);
        }, i * 1000);
    }
    )();
}
// =>5个6

// === 例子3===
for (var i = 1; i <= 5; i++) {
    (function() {
        var j=i;
        setTimeout(function timer() {
            console.log(j);
        }, j * 1000);
    }
    )();
}
// =>1，2，3，4，5

// === 例子4===
// 对例子3进行改进
for (var i = 1; i <= 5; i++) {
    (function(j) {
        setTimeout(function timer() {
            console.log(j);
        }, j * 1000);
    }
    )(i);
}
// =>1，2，3，4，5
```
对例子1进行调整，变成例子2。如果作用域是空的，那么仅仅将它们进行封闭是不够的。它需要包含一点实质性的内容才能为我们所用。
它需要包含迭代中储存i的值

#####重返块作用域
如果使用let的话一切从简：
```js
for (let i = 1; i <= 5; i++) {
    var j = i; // 闭包块的作用域
    setTimeout(function timer() {
        console.log(j);
    }, j * 1000);
}
// 等价于
for (let i = 1; i <= 5; i++) {
    setTimeout(function timer() {
        console.log(i);
    }, i * 1000);
}
```
#####模块
```js
function foo() {
    var something = 'cool';
    var another = [1, 2, 3];
    function doSomething() {
        console.log(something);
    }
    function doAnother() {
        console.log(another.join('!'));
    }
}
// 改写
function coolModule() {
    var something = 'cool';
    var another = [1, 2, 3];
    function doSomething() {
        console.log(something);
    }
    function doAnother() {
        console.log(another.join('!'));
    }
    return {
        doSomething: doSomething,
        doAnother: doAnother
    };
}
var foo = coolModule();
foo.doSomething();
foo.doAnother();
```
这个模式在JavaScript被成为模块，最常见的实现模块方式被成为模块暴露，这里展示的是其变体。 

首先，coolModule( )只是一个函数，必须通过调用它来创建一个模块实例。如果不执行外部函数，内部作用域和闭包都无法被创建。

其次，coolModule( )返回一个用对象字面量语法 { key: value, ... }来表示的对象。这个对象包含对内部函数而不是内部变量的引用。外面保持内部变量是隐藏且私有的状态。可以将这个对象类型的返回值看作本质上的是模块的公共API。

这个对象类型的返回值最终被赋值给外部的变量foo，然后就可以通过它来访问API的属性方法，比如foo.doSomething()。
    注：从模块返回实际的对象不是必须的，可以返回一个内部函数，如jQuery。
模块模式需要具备两个必要条件：

1. 必须有外部的封闭函数，该函数必须至少被调用一次（每次调用都会创建一个新的模块实例）。
2. 封闭函数必须返回至少一个内部函数，这样内部函数才能在私有作用域中形成闭包，并且可以访问或修改私有的状态。

将上面的改为LIFE,当只需一个实例时，可以实现单例模式：

```js
var foo=(function coolModule() {
    var something = 'cool';
    var another = [1, 2, 3];
    function doSomething() {
        console.log(something);
    }
    function doAnother() {
        console.log(another.join('!'));
    }
    return {
        doSomething: doSomething,
        doAnother: doAnother
    };
})();
foo.doSomething();
foo.doAnother();
```
模块也是普通的函数，因此可以接受参数：
```js
function coolModule(id) {
    function identify() {
        console.log( id );
    }

    return {
        identify: identify
    };
}

var foo1 = coolModule( "foo 1" );
foo1.identify();  // "foo 1"
```
模块模式另一个简单但强大的用法是命名将要作为公共API返回的对象：
```js
var foo = (function coolModule(id){
    function change(){
        // 修改公共API
        publicAPI.identify = identify2;
    }

    function identify1() {
        console.log( id );
    }

    function identify2() {
        console.log( id.toUpperCase() );
    }

    var publicAPI = {
        change: change,
        identify: identify1
    }

    return publicAPI;
}("foo module"));

foo.identify();  //  foo module
foo.change();
foo.identify();  //  FOO MODULE

```
#####现代的模块机制
Page54
#####未来的模块机制
```js
// bar.js
function  hello(who){
    return "let me introduce："+who;
}
export hello;

```
```js
// foo.js
import hello from "bar";
var hungry="hippo";
function  awesome() {
    console.log(hello(hungry).toUpperCase());
}
export awesome;

```
```js
// baz.js
module foo from "foo";
module bar from "bar";

console.log(
  bar.hello("rhino")  
);
foo.awesome();

```
模块文件中的内容会被当作好像包含在作用域闭包中一样来处理，就和前面介绍的函数闭包模块一样。
#####动态作用域
Page58
#####参考资料：
《你不知道的JavaScript上卷》
